---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Trabalhando com parâmetros

Em ações serverless, os dados são fornecidos incluindo parâmetros nas ações. Os parâmetros são declarados como um argumento para a função principal sem servidor.
{: shortdesc}

É possível fornecer valores para parâmetros de duas maneiras:
* **Passar parâmetros para uma ação durante a chamada**: forneça parâmetros quando a ação for chamada por meio de sinalizadores da CLI ou por meio de um arquivo. Os parâmetros fornecidos na chamada substituem quaisquer parâmetros padrão que foram configurados anteriormente.
* **Ligar parâmetros a uma ação ou a um pacote**: configure parâmetros padrão quando uma ação ou um pacote for criado ou atualizado. Essa opção é útil para dados que permanecem os mesmos em cada execução, equivalente a variáveis de ambiente em outras plataformas, ou para valores padrão que podem ser substituídas no tempo de chamada.

## Passando parâmetros para uma ação durante a chamada
{: #pass-params-action}

Os parâmetros podem ser passados para uma ação quando ela é chamada.

1. Salve o código a seguir em um arquivo nomeado `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Crie a ação `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Se você usou anteriormente essa ação, assegure-se de que a ação seja limpa de quaisquer parâmetros configurados anteriormente atualizando-a.
    ```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. Chame a ação, transmitindo os parâmetros `name` e `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **Nota**: em vez disso, é possível passar um arquivo de parâmetros formatados por JSON:
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "payload": "Hello, Dorothy from Kansas" }
    ```
    {: screen}

5. Também é possível passar parâmetros em um objeto estruturado para sua ação. Por exemplo, atualize a ação `hello` para o seguinte:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    Agora, a ação espera que um único parâmetro `person` tenha os campos `name` e `place`.

6. Chame a ação com um único parâmetro `person` que seja um objeto JSON válido.
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "payload": "Hello, Dorothy from Kansas" }
    ```
    {: screen}

## Ligando parâmetros a uma ação
{: #default-params-action}

Ações podem ser chamadas com vários parâmetros denominados. Por exemplo, a ação básica `hello`espera dois parâmetros: o `nome`de uma pessoa e o `local`de onde eles são.

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

Em vez de passar todos os parâmetros para uma ação toda vez, é possível ligar os parâmetros padrão à ação. As etapas a seguir mostram como ligar o parâmetro `place` à ação básica `hello` para que a ação seja padronizada para o local "Kansas".

1. Salve o código a seguir em um arquivo nomeado `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Crie a ação `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Atualize a ação para ligar os valores de parâmetro usando o sinalizador `--param` e um par de chave/valor.

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **Nota**: em vez disso, é possível passar um arquivo de parâmetros formatados por JSON:
    ```
    ibmcloud fn action update hello -- param-file parameters.json
    ```
    {: pre}

    Se você modifica seus parâmetros de credenciais sem serviço, executar um comando `action update` com novos parâmetros remove quaisquer parâmetros que existem atualmente, mas não estão especificados no comando `action update`. Por exemplo, se você executar `action update -p key1 new-value -p key2 new-value`, mas omitir quaisquer outros parâmetros que foram configurados, esses parâmetros não existirão mais após a ação ser atualizada. Quaisquer serviços que foram ligados à ação também são removidos, então, depois de atualizar outros parâmetros, deve-se [ligar os serviços à sua ação](/docs/openwhisk?topic=cloud-functions-binding_services) novamente.
    {: tip}

4. Chame a ação transmitindo apenas o parâmetro `name`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "payload": "Hello, Dorothy from Kansas" }
    ```
    {: screen}

    Como você não especificou o parâmetro `place` quando chamou a ação, o valor do parâmetro padrão de limite, `Kansas`, é usado.

5. Os parâmetros de limite podem ser sobrescritos, especificando o valor de parâmetro no tempo de chamada. Chame a ação, transmitindo `name` e `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "payload": "Hello, Dorothy from Washington, DC"
    }
    ```
    {: screen}

## Ligando parâmetros a um pacote
{: #default-params-package}

Os parâmetros padrão também podem ser configurados no nível do pacote. Os parâmetros de limite servem como os parâmetros padrão para ações no pacote, a menos que:

- a ação em si tenha um parâmetro padrão
- a ação tenha um parâmetro fornecido no momento da chamada

O exemplo a seguir configura um parâmetro padrão de `name` no pacote `MyApp` e mostra uma ação usando isso.

1. Crie um pacote, ligando o parâmetro padrão `name` a ele.
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. Salve o código a seguir em um arquivo denominado `helloworld.js`.

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. Crie a ação no pacote `MyApp`.
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    Se você modifica seus parâmetros de credenciais sem serviço, executar um comando `action update` com novos parâmetros remove quaisquer parâmetros que existem atualmente, mas não estão especificados no comando `action update`. Por exemplo, se você executar `action update -p key1 new-value -p key2 new-value`, mas omitir quaisquer outros parâmetros que foram configurados, esses parâmetros não existirão mais após a ação ser atualizada. Quaisquer serviços que foram ligados à ação também são removidos, então, depois de atualizar outros parâmetros, deve-se [ligar os serviços à sua ação](/docs/openwhisk?topic=cloud-functions-binding_services) novamente.
    {: tip}

3. Chame a ação sem passar nenhum parâmetro.
    ```
    ibmcloud fn action invoke -- result MyApp/hello
    ```
    {: pre}

    Exemplo de Saída:
    ```
       {
           "payload": "Hello, World"
     }
    ```
    {: screen}

    O parâmetro padrão que está ligado ao pacote é usado.

