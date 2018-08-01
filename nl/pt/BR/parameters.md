---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Trabalhando com parâmetros

Saiba como configurar parâmetros em pacotes e ações para implementação e como passar parâmetros para ações durante a chamada. Também é possível usar um arquivo para armazenar parâmetros e passar o nome do arquivo para a ação, em vez de fornecer cada parâmetro individualmente na linha de comandos.
{: shortdesc}

Com ações sem servidor, os dados são fornecidos incluindo parâmetros nas ações, que são declarados como um argumento para a função sem servidor principal. Todos os dados chegam dessa maneira e os valores podem ser configurados de algumas maneiras diferentes. A primeira opção é fornecer parâmetros quando uma ação ou um pacote é criado (ou atualizado). Essa opção é útil para dados que permanecem os mesmos em cada execução, equivalente a variáveis de ambiente em outras plataformas, ou para valores padrão que podem ser substituídas no tempo de chamada. A segunda opção é fornecer parâmetros quando a ação é chamada que substituem quaisquer parâmetros que foram configurados anteriormente.

## Passando parâmetros para uma ação durante a chamada
{: #pass-params-action}

Os parâmetros podem ser passados para uma ação quando ela é chamada. Os exemplos que são fornecidos usam JavaScript, mas todas as outras linguagens funcionam da mesma maneira. Para ver exemplos detalhados, verifique os tópicos a seguir em [Ações do Javascript](./openwhisk_actions.html#creating-and-invoking-javascript-actions), [Ações do Swift](./openwhisk_actions.html#creating-swift-actions), [Ações do Python](./openwhisk_actions.html#creating-python-actions), [Ações do Java](./openwhisk_actions.html#creating-java-actions), [Ações do PHP](./openwhisk_actions.html#creating-php-actions), [Ações do Docker](./openwhisk_actions.html#creating-docker-actions) ou [Ações do Go](./openwhisk_actions.html#creating-go-actions).

1. Use parâmetros na ação. Por exemplo, crie um arquivo nomeado **hello.js** com o conteúdo a seguir:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Os parâmetros de entrada são passados como um parâmetro de objeto JSON para a função **main**. Observe como os parâmetros `name` e `place` são recuperados a partir do objeto `params` neste exemplo.

2. Atualize a ação **hello** para que ela esteja pronta para usar:
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

  Se você modifica seus parâmetros de credenciais sem serviço, executar um comando `action update` com novos parâmetros remove quaisquer parâmetros que existem atualmente, mas não estão especificados no comando `action update`. Por exemplo, se você executar `action update -p key1 new-value -p key2 new-value`, mas omitir quaisquer outros parâmetros que foram configurados, esses parâmetros não existirão mais após a ação ser atualizada. Quaisquer serviços que foram ligados à ação também são removidos, então, depois de atualizar outros parâmetros, deve-se [ligar os serviços à sua ação](./binding_services.html) novamente.
  {: tip}

3. Os parâmetros podem ser fornecidos explicitamente usando a linha de comandos ou [fornecendo um arquivo](./parameters.html#using-parameter-files) que contenha os parâmetros desejados.

  Para passar os parâmetros diretamente pela linha de comandos, forneça um par de chave/valor para a sinalização `--param`:
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **
Resposta:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas" }
  ```
  {: screen}

  Observe o uso da opção `--result`: isso significa uma chamada de bloqueio em que a CLI aguarda a ativação ser concluída e, em seguida, exibe somente o resultado. Por conveniência, essa opção pode ser usada sem `--blocking` que é inferido automaticamente.

  Além disso, se os valores de parâmetro que estiverem especificados na linha de comandos forem JSON válido, eles serão analisados e enviados para a sua ação como um objeto estruturado.

  Por exemplo, atualize a ação **hello** para o seguinte:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Agora a ação espera que um único parâmetro `person` tenha os campos `name` e `place`.

  Em seguida, chame a ação com um único parâmetro `person` parâmetro que é um JSON válido, como no exemplo a seguir:
  ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **
Resposta:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas" }
  ```
  {: screen}

  O resultado é o mesmo porque a CLI analisa automaticamente o valor `person` para o objeto estruturado que a ação agora espera.

## Configurando parâmetros padrão em uma ação
{: #default-params-action}

As ações podem ser chamadas com múltiplos parâmetros nomeados. Lembre-se de que a ação **hello** do exemplo anterior espera dois parâmetros: o *name* de uma pessoa e o *place* de onde ela é.

Em vez de passar todos os parâmetros para uma ação toda vez, é possível fazer a ligação de determinados parâmetros. O exemplo a seguir liga o parâmetro *place* para que a ação seja padronizada para o local "Kansas":

1. Atualize a ação usando a opção `--param` para ligar os valores de parâmetros ou passando um arquivo que contenha os parâmetros para `--param-file`. (Para exemplos que usam arquivos, veja a seção em [Usando arquivos de parâmetro](./parameters.html#using-parameter-files).

  Para especificar os parâmetros padrão explicitamente na linha de comandos, forneça um par de chave/valor para a sinalização `param`:
  ```
  ibmcloud fn action update hello --param place Kansas
  ```
  {: pre}

2. Chame a ação passando somente o parâmetro `name` desta vez.
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

  Observe que você não precisou especificar o parâmetro place quando chamou a ação. Os parâmetros ligados ainda podem ser substituídos especificando o valor de parâmetro no momento da chamada.

3. Chame a ação passando os valores `name` e `place` e observe a saída:

  Chame a ação usando a sinalização `--param`:
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

  Os parâmetros configurados em uma ação quando ela foi criada ou atualizada são sempre substituídos por um parâmetro que é fornecido diretamente na chamada.
  {: tip}

## Configurando parâmetros padrão em um pacote
{: #default-params-package}

Os parâmetros podem ser configurados no nível de pacote e servem como os parâmetros padrão para as ações _a menos que_:

- A ação em si tenha um parâmetro padrão.
- A ação tenha um parâmetro que é fornecido no tempo de chamada, que é sempre a "prioridade" quando mais de um parâmetro está disponível.

O exemplo a seguir configura um parâmetro padrão de `name` no pacote **MyApp** e mostra uma ação usando isso.

1. Crie um pacote com um conjunto de parâmetros:

  ```
  ibmcloud fn package update MyApp --param name World
  ```
  {: pre}

2. Crie uma ação no pacote **MyApp**:
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Crie a ação:
  ```
  ibmcloud fn action update MyApp/hello hello.js
  ```
  {: pre}

3. Chame a ação e observar o parâmetro de pacote padrão em uso:
  ```
  ibmcloud fn action invoke --result MyApp/hello
  ```
  {: pre}

  Exemplo de Saída:
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## Usando arquivos de parâmetro
{: #using-parameter-files}

É possível colocar parâmetros em um arquivo no formato JSON e, em seguida, passar os parâmetros fornecendo o nome do arquivo com a sinalização `--param-file`. Esse método pode ser usado para a criação (ou atualizações) de pacote e ação e durante a chamada de ação.

1. Como um exemplo, considere o exemplo **hello** anterior usando `hello.js` com o conteúdo a seguir:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Atualize a ação com os conteúdos atualizados do `hello.js`:

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. Crie um arquivo de parâmetro chamado `parameters.json` contendo parâmetros formatados por JSON:

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. Use o nome do arquivo `parameters.json` ao chamar a ação **hello** e observe a saída:

  ```
  ibmcloud fn action invoke --result hello --param-file parameters.json
  ```

  Exemplo de Saída:
  ```
  {
      "payload": "Hello, Dorothy from Kansas" }
  ```
  {: screen}
