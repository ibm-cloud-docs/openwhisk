---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Criando APIs de REST sem servidor
{: #openwhisk_apigateway}

As ações do {{site.data.keyword.openwhisk}} podem se beneficiar ao serem gerenciadas por APIs diretamente introduzindo o API Gateway, que age como um proxy para [ações da web](./openwhisk_webactions.html) e fornece a elas recursos adicionais. Os recursos adicionais incluem: roteamento de método de HTTP, ID/segredos de cliente, limites de taxa, CORS, visualizar o uso da API, visualizar logs de respostas e políticas de compartilhamento de API. Para obter mais informações sobre gerenciamento de API, é possível ler a [documentação de gerenciamento de API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Criar APIs de ações da web do OpenWhisk usando seu navegador
{: #create_api_browser}

É possível usar a [**guia APIs**](https://console.bluemix.net/openwhisk/apimanagement) no [Painel do {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) para executar as tarefas a seguir:

* [Criar uma API do Cloud Functions](https://console.bluemix.net/openwhisk/apimanagement) - crie uma API que agrupe um conjunto de ações do OpenWhisk.
* [Proteger sua API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - aplique políticas de segurança e de limitação de taxa de API para proteger sua API.
* [Gerenciar tráfego](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) - visualize estatísticas de uso da API e efetue check-out dos logs de resposta.
* [Socializar e compartilhar](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) - compartilhe sua API com desenvolvedores dentro e fora do {{site.data.keyword.Bluemix_notm}}.

## Criar APIs de ações da web do OpenWhisk usando o plug-in da CLI
{: #create_api_cli}

A seção a seguir conduz você pelas tarefas de gerenciamento de API usando o plug-in da CLI do {{site.data.keyword.openwhisk_short}}. Para criar e gerenciar APIs por meio da CLI, deve-se primeiro instalar o [plug-in da CLI do {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html) para o {{site.data.keyword.Bluemix_notm}}.

Por conveniência, as etapas são divididas em subtópicos menores para os quais você pode ir rapidamente usando a lista de tarefas de API a seguir:

* [Criar primeira API](openwhisk_apigateway.html#create_cli_api)
* [Controle total sobre a resposta de HTTP](openwhisk_apigateway.html#full_control)
* [Expondo múltiplas ações da web](openwhisk_apigateway.html#multiple_web_actions)
* [Exportar a configuração](openwhisk_apigateway.html#export_config)
* [Importar a configuração](openwhisk_apigateway.html#import_config)
* [Modificar a configuração](openwhisk_apigateway.html#modify_config)

### Criar sua primeira API usando a CLI
{: #create_cli_api}

1. Crie um arquivo JavaScript nomeado **hello.js** que contém o conteúdo a seguir:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Crie uma ação da web nomeada **hello** usando o arquivo `hello.js` criado na etapa um. **Nota:** certifique-se de incluir a sinalização `--web true`.
  ```
  ibmcloud wsk action create hello hello.js --web true
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: ação hello criada
  ```
  {: screen}

3. Crie uma API com o caminho base `/hello`, caminho `/world` e método `get`, com o tipo de resposta `json`:
  ```
  ibmcloud wsk api create /hello /world get hello --response-type json
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Uma nova URL é gerada expondo a ação `hello` usando um método de HTTP __GET__.

4. Finalmente, envie uma solicitação de HTTP para a URL usando o comando **curl**:
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

A ação da web **hello** é chamada, que retorna um objeto JSON que inclui o parâmetro **name** enviado por meio do parâmetro de consulta. É possível passar parâmetros para a ação com parâmetros de consulta simples ou usando o corpo da solicitação. As ações da web podem chamar uma ação de uma maneira pública sem usar a chave API de autorização do OpenWhisk.

### Controle total sobre a resposta de HTTP
{: #full_control}

A sinalização `--response-type` controla a URL de destino da ação da web a ter o proxy efetuado pelo API Gateway. Usar `--response-type json` retorna o resultado integral da ação no formato JSON e configura automaticamente o cabeçalho Content-Type para `application/json`.

Você deseja ter o controle total sobre as propriedades de resposta de HTTP como `statusCode` e `headers`, assim é possível retornar tipos de conteúdo diferentes no `body`. A sinalização `--response-type http` torna isso possível configurando a URL de destino da ação da web com a extensão `http`.

É possível escolher mudar o código da ação para obedecer ao retorno de ações da web com a extensão `http` ou incluir a ação em uma sequência para passar seu resultado para uma nova ação. A nova ação pode então transformar o resultado para ser formatado adequadamente para uma resposta de HTTP. É possível ler mais sobre tipos de resposta e extensões de ações da web na documentação [ações da web](./openwhisk_webactions.html).

1. Mude o código para o `hello.js` retornando as propriedades JSON `body`, `statusCode` e `headers`:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Atualize a ação com o resultado modificado:
  ```
  ibmcloud wsk action update hello hello.js --web true
  ```
  {: pre}

3. Atualize o tipo de resposta da API usando a sinalização `--response-type http`:
  ```
  ibmcloud wsk api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Chame a API atualizada usando o comando **curl** a seguir:
  ```bash
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

Agora, você está no controle total de suas APIs e pode controlar o conteúdo. Como retornar HTML ou configurar o código de status para coisas como Não localizado (404), Desautorizado (401) ou até mesmo Erro interno (500).

### Expondo múltiplas ações da web
{: #multiple_web_actions}

Por exemplo, se você deseja expor um conjunto de ações para um clube do livro, é possível usar uma série de ações para implementar seu backend para o clube do livro:

| Ação | Método HTTP | Descrição |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Obter detalhes do livro  |
| postBooks   | POST | Incluir um livro |
| putBooks    | PUT | Atualizar detalhes do livro |
| deleteBooks | DELETE | Excluir um livro |

Neste exemplo, a API é definida com um **parâmetro de caminho**. Ao usar parâmetros de caminho, a API deve ser definida com um tipo de resposta de `http`. O valor do caminho, iniciando com o caminho base e incluindo os valores de parâmetros de caminho reais, está disponível no campo `__ow_path` do parâmetro JSON da ação. Consulte a documentação [Contexto de HTTP de ações da web](./openwhisk_webactions.html#http-context) para obter mais detalhes, incluindo informações sobre mais campos de contexto de HTTP que estão disponíveis para as ações da web chamadas com um tipo de resposta `http`.

1. Crie uma API para o clube do livro, nomeado **Book Club**, com `/club` como seu caminho base de URL de HTTP, `books` como seu recurso e `{isbn}` como um parâmetro de caminho que é usado para identificar um livro específico usando seu International Standard Book Number (ISBN).
  ```bash
  ibmcloud wsk api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud wsk api create /club /books get getBooks                       --response-type http
  ibmcloud wsk api create /club /books post postBooks                     --response-type http
  ibmcloud wsk api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud wsk api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  Observe que a primeira ação que é exposta com o caminho base `/club` obtém o rótulo da API com o nome **Book Club**. Quaisquer outras ações expostas em `/club` são agora associadas com **Book Club**.

2. Liste todas as ações do **Book Club** que são expostas usando o comando a seguir:
  ```
  ibmcloud wsk api list /club -f
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. Por diversão, é possível incluir um livro intitulado **JavaScript: The Good Parts** com um __POST__ HTTP:
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. Obtenha uma lista de livros usando a ação **getBooks** com __GET__ HTTP:
  ```bash
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. É possível excluir um livro específico usando a ação **deleteBooks** com __DELETE__ HTTP. Neste exemplo, o valor do campo `__ow_path` da ação **deleteBooks** é `/club/books/978-0596517748`, em que `978-0596517748` é o valor real `{isbn}` do caminho.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### Exportar a configuração
{: #export_config}

1. Exporte a API chamada **Book Club** para um arquivo que possa ser usado como uma base para recriar as APIs usando um arquivo como entrada.
  ```
  ibmcloud wsk api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Teste o arquivo swagger excluindo primeiramente todas as URLs expostas em um caminho base comum usando o comando a seguir:
  ```
  ibmcloud wsk api delete /club
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: deleted API /club
  ```
  {: screen}

  É possível excluir todas as URLs expostas usando o caminho base `/club` ou o rótulo do nome da API **"Book Club"**:
  {: tip}

### Importar a configuração
{: #import_config}

1. Agora, restaure a API nomeada **Book Club** usando o nome do arquivo `club-swagger.json`:
  ```
  ibmcloud wsk api create --config-file club-swagger.json
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

2. Verifique se a API **Book Club** é recriada:
  ```
  ibmcloud wsk api list /club
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

### Modificar a configuração usando a UI
{: #modify_config}

É possível editar a configuração no Painel do {{site.data.keyword.openwhisk_short}}, clique na [guia APIs](https://console.ng.bluemix.net/openwhisk/apimanagement) para configurar a segurança, limites de taxa e outros recursos. Quando você tiver terminado de atualizar a configuração, será possível fazer download do arquivo de definição em formato JSON e, em seguida, reimportá-lo usando a CLI. Isso pode ser útil, por exemplo, para uma implementação não assistida em um pipeline de integração e implementação contínuas (CICD). Você também tem a opção de fazer upload e reimportar o arquivo de definição de API usando a UI.
