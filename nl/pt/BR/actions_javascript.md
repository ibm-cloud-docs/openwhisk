---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, javascript, node, node.js

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}


# Criando ações do JavaScript
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

As seções a seguir o orientam pelo trabalho com ações em JavaScript. Inicie a criação e a chamada de uma
ação simples. Em seguida, inclua parâmetros na ação e chame essa ação com parâmetros. Em seguida, configure
parâmetros padrão e chame-os. Finalmente, crie ações assíncronas.
{: shortdesc}

## Criando e chamando uma ação simples JavaScript
{: #single_action_js}
{: #openwhisk_single_action_js}

Revise as etapas e os exemplos a seguir para criar sua primeira ação JavaScript.

1. Salve o código a seguir em um arquivo denominado `hello.js`.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  O arquivo JavaScript pode conter funções adicionais. No entanto, por convenção, uma função chamada `main` deve existir para fornecer o ponto de entrada para a ação.
  {: shortdesc}

2. Crie uma ação chamada `hello` usando a função JavaScript.

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Exemplo de Saída:
  
  ```
  ok: ação hello criada
  ```
  {: screen}

  O tipo de ação é determinado pelo uso da extensão do arquivo de origem. Para arquivos de origem `.js`, a ação é executada usando um tempo de execução Node.js. É possível especificar a versão de runtime do Node.js para sua ação JavaScript configurando o parâmetro `--kind` para `nodejs:10` ou `nodejs:8`. Para obter mais informações sobre o tempo de execução do Node.js, consulte [Tempos de Execução](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).
  {: shortdesc}

3. Verifique se a ação `hello` está em sua lista de ações.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  Exemplo de Saída:
  
  ```
  ações
  hello       privada
  ```
  {: screen}

4. Execute a ação na nuvem ao executar uma chamada de bloqueio. As chamadas de bloqueio usam um estilo de solicitação-resposta e aguardam que o resultado de ativação esteja disponível. O período de espera é o menor de 60 segundos ou o [valor limite de tempo](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits) da ação.

    ```
    ibmcloud fn action invoke -- blocking hello
    ```
    {: pre}

    O comando expõe as informações a seguir:
        * O ID de ativação (`44794bd6aab74415b4e42a308d880e5b`), que pode ser usado
para recuperar os logs ou o resultado da chamada
        * O resultado da chamada se ele estiver disponível dentro do período de espera estimado

    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    
    Para obter dicas sobre ativações de monitoramento, consulte a
[Saída de ação de monitoramento](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output).
    {: tip}

5. Se você não precisa do resultado da ação imediatamente, é possível omitir a sinalização
`--blocking` para executar uma chamada sem bloqueio.

    1. Execute uma chamada sem bloqueio.

        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Exemplo de Saída:

        ```
        ok: hello chamada com id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Use o ID de ativação para obter o resultado da ação.

        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        O resultado da ação é retornado:

        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. Caso você se esqueça de registrar o ID de ativação, é possível obter uma lista de ativações ordenadas da mais para a menos recente.

    ```
    ibmcloud fn activation list
    ```
    {: pre}

    Saída:
    ```
    ativações
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}
    
### Protótipo de função JavaScript
{: #openwhisk_ref_javascript_fnproto}

As ações de JavaScript do {{site.data.keyword.openwhisk_short}} são executadas em um tempo de execução do Node.js.

As ações que são gravadas em JavaScript devem ser confinadas a um único arquivo. O arquivo pode conter múltiplas funções, mas, por convenção, uma função denominada `main` deverá existir e será aquela chamada quando a ação for chamada. Por exemplo, o exemplo a seguir mostra uma ação com múltiplas funções.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

Os parâmetros de entrada de ação são passados como um objeto JSON como um parâmetro para a função `main`. O resultado de uma ativação bem-sucedida também é um objeto JSON, mas é retornado de forma diferente dependendo de se a ação é síncrona ou assíncrona conforme descrito na seção a seguir.

### Comportamento síncrono e assíncrono
{: #openwhisk_ref_javascript_synchasynch}

É comum que as funções JavaScript continuem a execução em uma função de retorno de
chamada mesmo após um retorno. Para acomodar esse comportamento, uma ativação de uma ação JavaScript pode ser *síncrona* ou *assíncrona*.

A ativação de uma ação JavaScript é **síncrona** se a função principal sair sob uma das condições a seguir:

- A função principal sai sem executar uma instrução `return`.
- A função principal sai executando uma instrução `return` que retorna qualquer valor *exceto* uma Promessa.

Veja o exemplo a seguir de uma ação síncrona:

```javascript
// an action in which each path results in a synchronous activation
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

A ativação de uma ação JavaScript será **assíncrona** se a
função principal sair retornando uma Promessa. Nesse caso, o sistema supõe que a ação ainda está em execução até que a promessa será cumprida ou rejeitada.
Comece
instanciando um novo objeto Promessa e passando a ele uma função de retorno de chamada. O
retorno de chamada aceita dois argumentos, resolver e rejeitar, que são ambos funções. Todos
os códigos assíncronos vão dentro desse retorno de chamada.

No exemplo a seguir, é possível ver como cumprir uma promessa chamando a função de resolução.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
                }, 100);
             })
}
```
{: codeblock}

Este exemplo mostra como rejeitar uma promessa chamando a função de rejeição.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

É possível que uma ação seja síncrona em algumas entradas e assíncrona em outras, conforme mostrado no exemplo a seguir.
```javascript
function main(params) {
     if (params.payload) {
        // asynchronous activation
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
                }, 100);
             })
     } else {
        // synchronous activation
         return {done: true};
      }
}
```
{: codeblock}

Independentemente se uma ativação é síncrona ou assíncrona, a chamada da ação pode ser bloqueio ou não bloqueio.

## Criando ações assíncronas
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

As funções JavaScript que são executadas de forma assíncrona podem retornar o resultado da ativação após o retorno da função `main`, retornando uma Promessa em sua ação.

1. Salve o código a seguir em um arquivo denominado `asyncAction.js`.

    ```javascript
    function main(args) {
         return new Promise(function(resolve, reject) {
           setTimeout(function() {
             resolve({ done: true });
         }, 2000);
      })
     }
    ```
    {: codeblock}

    * A função  ` main `  retorna uma Promessa. O Promise indica que a ativação ainda
não foi concluída, mas espera-se que seja no futuro.

    * A função JavaScript `setTimeout()` aguarda por 2 segundos antes de chamar a
função de retorno de chamada do Promise, que representa o código assíncrono.

    * O retorno de chamada do Promise aceita os argumentos `resolve` e
`reject`, em que ambos são funções.

      * A chamada para `resolve()` cumpre a Promessa e indica que a ativação é concluída normalmente.
      * Uma chamada para `reject()` pode ser usada para rejeitar a Promessa e sinalizar que a ativação é concluída de forma anormal.

2. Crie uma ação chamada `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
    ```
    {: pre}

3. Invoque a ação.

    ```
    ibmcloud fn action invoke -- result asyncAction
    ```
    {: pre}

    Exemplo de Saída:

    ```
    {
        "done": true
  }
    ```
    {: screen}

4. Verifique quanto tempo a ativação levou para ser concluída obtendo o log de ativação.

  1. Obtenha o ID de ativação.

      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Exemplo de Saída:
      ```
      ativações
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Obtenha o log para o ID de ativação.

      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      A `duration` mostra que essa ativação demorou um pouco mais de 2 segundos
para ser concluída:

      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

## Usando ações para chamar uma API externa
{: #apicall_action}
{: #openwhisk_apicall_action}

Os exemplos fornecidos até agora são funções JavaScript autocontidas. Também é possível criar uma ação que chama uma API externa.
{: shortdesc}

O exemplo a seguir chama o serviço NASA Astronomy Picture of the Day (APOD) que fornece uma imagem exclusiva do nosso universo todos os dias.

1. Salve o código a seguir em um arquivo denominado `apod.js`.

    ```javascript
    let rp = require('request-promise')

    function main(params) {
        const options = {
            uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
        .then (res = >{
            return { response: res }
        })
    }
    ```
    {: codeblock}

2. Uma chamada é feita para a API APOD da NASA e os campos são extraídos do resultado JSON. Para obter mais informações sobre quais pacotes do Node.js você pode usar em suas ações, consulte [Tempos de execução](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).

3. Crie uma ação chamada  ` apod `.
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
    ```
    {: pre}

3. Chame a ação `apod`.
    ```
    ibmcloud fn action invoke -- result apod
    ```
    {: pre}

    O objeto de exemplo a seguir é retornado:

    ```
    {
      "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? ...",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
    ```
    {: screen}

## Empacotamento de uma ação como um módulo Node.js
{: #packaging_javascript_actions}
{: #openwhisk_js_packaged_action}

Como uma alternativa para gravar todo o seu código de ação em um único arquivo de origem JavaScript, é possível gravar uma ação como um pacote `npm`.

Por exemplo, considere um diretório com os arquivos a seguir:

1. Salve o código a seguir em um arquivo denominado `package.json`.

    ```json
    {
      "name": "my-action",
  "main": "index.js",
  "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. Salve o código a seguir em um arquivo denominado `index.js`.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}

    * A ação é exposta por meio de `exports.main`.
    * O manipulador de ações pode ter qualquer nome, desde que ele esteja em conformidade com a
assinatura convencional de aceitação e de retorno de um objeto (ou com um `Promise` de um
objeto).
    * Deve-se nomear este arquivo `index.js` ou especificar o nome do
arquivo que você preferir como a propriedade `main` em `package.json`.

3. Instale todas as dependências localmente.

    ```
    npm install
    ```
    {: pre}

    **Nota:** embora a maioria dos pacotes `npm` instala origens
do JavaScript no `npm install`, alguns também instalam e compilam artefatos binários. O upload de arquivo de archive suporta apenas dependências do JavaScript. Chamadas de ação poderão falhar se o
archive incluir dependências binárias.

4. Crie um archive `.zip` contendo todos os arquivos, incluindo todas as dependências.

    ```
    zip -r action.zip *
    ```
    {: pre}

    Usar a ação do Windows Explorer para criar o arquivo zip resulta em uma estrutura incorreta. As ações .zip do {{site.data.keyword.openwhisk_short}} devem ter `package.json` na raiz do zip, enquanto o Windows Explorer coloca-o dentro de uma pasta aninhada. A opção mais segura é usar o comando `zip` da linha de comandos.
    {: tip}

5. Crie a ação. Ao criar uma ação por meio de um archive `.zip`, deve-se configurar um valor para o parâmetro `--kind` para especificar a versão de runtime do Node.js. Escolha entre `nodejs:8` ou `nodejs:10`.

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
    ```
    {: pre}

6. Invoque a ação.

    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    Exemplo de Saída:

    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

## Empacotando uma ação como um único pacote configurável
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

Se o empacotamento da ação como um .zip incluir muitos arquivos desnecessários ou se você precisar de uma implementação mais rápida, será possível escrever o código mínimo em um único arquivo `.js` que inclua dependências.
{: shortdesc}

É possível empacotar uma ação usando um bundler de módulo JavaScript, como
[webpack
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://webpack.js.org/concepts/). Quando o webpack processa seu código, ele constrói recursivamente um gráfico de
dependência que inclui cada módulo de que a sua ação precisa.

1. Salve o código a seguir em um arquivo denominado `package.json`. O `webpack` é incluído como uma dependência de desenvolvimento.

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
  "dependencies": {
        "left-pad" : "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Salve o código de configuração do webpack a seguir em um arquivo denominado `webpack.config.js`.

    ```javascript
    var path = require('path');
module.exports = {
      entry: './index.js',
  output: {
        path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
      },
  target: 'node'
    };
    ```
    {: codeblock}

3. Salve o código a seguir em um arquivo denominado `index.js`. A variável
`global.main` é configurada para a função principal da ação.

    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Instale todas as dependências localmente.

    ```
    npm install
    ```
    {: pre}

5. Construa o pacote configurável do webpack.

    ```
    npm run build
    ```
    {: pre}

    O arquivo `dist/bundle.js` é criado e implementado como o código de origem da
ação.

6. Crie a ação usando o script `npm` ou a CLI.

    * Usando o script  ` npm ` :

        ```
        npm run deploy
        ```
        {: pre}

    * Usando a CLI:

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    O arquivo de pacote configurável que é construído pelo `webpack`suporta apenas dependências JavaScript. Chamadas de ação podem falhar se o pacote configurável depender de dependências de arquivo binário porque isso não está incluído com o arquivo `bundle.js`.
    {: tip}
    



