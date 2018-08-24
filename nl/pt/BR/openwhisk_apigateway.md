---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Criando APIs de REST sem servidor
{: #openwhisk_apigateway}

Use APIs para gerenciar diretamente as ações do {{site.data.keyword.openwhisk}}. O API Gateway age como um proxy para [ações da web](./openwhisk_webactions.html) e fornece roteamento de método de HTTP, ID e segredos do cliente, limites de taxa, CORS, visualização do uso da API, visualização de logs de resposta e políticas de compartilhamento de API.
{: shortdesc}

Para obter mais informações sobre gerenciamento de API, é possível ler a [documentação de gerenciamento de API](/docs/api-management/manage_openwhisk_apis.html#manage_openwhisk_apis).

## Criando sua primeira API
{: #create_cli_api}

Antes de iniciar, instale o plug-in da CLI do [ {{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

1. Salve o código a seguir em um arquivo JavaScript denominado `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Crie uma ação da web chamada `hello` usando o arquivo que você criou. **Nota:** certifique-se de incluir a sinalização `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: ação hello criada
  ```
  {: screen}

3. Crie uma API com o caminho base `/hello`, o caminho `/world`, o método `get` e o tipo de resposta `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Uma nova URL é gerada expondo a ação `hello` usando um método GET HTTP.

4. Envie uma solicitação de HTTP de teste para a URL usando o comando cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/ < GENERATED_API_ID> /hello/world?name=OpenWhisk
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

A ação da web `hello` é chamada, que retorna um objeto JSON que inclui o parâmetro **name** no parâmetro de consulta. É possível passar parâmetros para a ação com parâmetros de consulta simples ou usando o corpo da solicitação. As ações da web podem chamar publicamente uma ação sem o uso da chave API de autorização do {{site.data.keyword.openwhisk_short}}.

## Usando o controle integral sobre a resposta de HTTP
{: #full_control}

A sinalização `--response-type` controla a URL de destino da ação da web a ter o proxy efetuado pelo API Gateway. Por exemplo, ao usar a sinalização `--response-type json`, o resultado integral da ação é retornado no formato JSON e o cabeçalho **Content-Type** é configurado automaticamente como `application/json`.

Para retornar tipos de conteúdo diferentes no corpo, use o controle integral sobre as propriedades de resposta de HTTP, como **statusCode** e **headers**. É possível usar a sinalização `--response-type http` para configurar a URL de destino da ação da web com a extensão `http`. É possível mudar o código da ação para obedecer ao retorno de ações da web com a extensão `http` ou incluir a ação em uma sequência para passar seu resultado para uma nova ação. A nova ação pode então transformar o resultado para ser formatado adequadamente para uma resposta de HTTP. É possível ler mais sobre tipos de resposta e extensões de ações da web na documentação [ações da web](./openwhisk_webactions.html).

1. Mude o código para a ação `hello.js` retornando as propriedades JSON `body`, `statusCode` e `headers`.
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

2. Atualize a ação com o resultado modificado.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Atualize o tipo de resposta da API usando a sinalização `-- response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Chame a API atualizada usando o comando cURL a seguir.
  ```
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

## Expondo múltiplas ações da web
{: #multiple_web_actions}

É possível expor múltiplas ações da web para implementar seu backend de app. Por exemplo, para expor um conjunto de ações para um clube do livro, é possível usar uma série de ações para implementar seu backend para o clube do livro:

| Ação | Método HTTP | Descrição |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Obter detalhes do livro  |
| postBooks   | POST | Incluir um livro |
| putBooks    | PUT | Atualizar detalhes do livro |
| deleteBooks | DELETE | Excluir um livro |

Neste exemplo, a API é definida com um parâmetro de caminho. Ao usar parâmetros de caminho, a API deve ser definida com um tipo de resposta de `http`. O valor do caminho, iniciando com o caminho base e incluindo os valores de parâmetro de caminho reais, está disponível no campo `__ow_path` do parâmetro JSON da ação. Para obter mais detalhes sobre os campos de contexto HTTP, consulte a documentação [Contexto HTTP de ações da web](./openwhisk_webactions.html#http-context).

Para experimentar esse exemplo de ações da web do clube do livro:

1. Crie uma API para o clube do livro denominada `Book Club`, com `/club` como seu caminho base de URL HTTP, `books` como seu recurso e `{isbn}` como um parâmetro de caminho que é usado para identificar um livro específico usando seu Número Padrão Internacional de Livro (ISBN).
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  A primeira ação que é exposta com o caminho base `/club` é rotulada com o nome `Book Club`. Quaisquer outras ações expostas em `/club` são agora associadas com `Book Club`.

2. Liste todas as ações de `Book Club` que são expostas.
  ```
  ibmcloud fn api list /club -f
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

3. Inclua um livro intitulado `JavaScript: The Good Parts` usando um POST HTTP.
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

4. Obtenha uma lista de livros usando uma chamada GET HTTP para a ação `getBooks`.
  ```
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

5. Exclua um livro específico usando uma chamada DELETE HTTP para a ação `deleteBooks`. Neste exemplo, o valor do campo `__ow_path` da ação `deleteBooks` é `/club/books/978-0596517748`, em que `978-0596517748` é o valor real `{isbn}` do caminho.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## Exportando e importando a configuração
{: #export_import_config}

Para exportar ou importar uma configuração, é possível continuar usando o exemplo do clube do livro.

1. Exporte a API `Book Club` em um arquivo denominado `club-swagger.json`. Esse arquivo pode ser usado como uma base para recriar as APIs usando um arquivo como entrada.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Teste o arquivo swagger excluindo primeiramente todas as URLs expostas em um caminho base comum.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: deleted API /club
  ```
  {: screen}

  É possível excluir todas as URLs expostas usando o caminho base `/club` ou o rótulo de nome da API `"Book Club"`.
  {: tip}

3. Restaure a API `Book Club` usando o arquivo `club-swagger.json`.
  ```
  ibmcloud fn api create -- config-file club-swagger.json
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

4. Verifique se a API `Book Club` foi recriada.
  ```
  ibmcloud fn api list /club
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

## Modificando a configuração
{: #modify_config}

Depois de ter criado sua configuração, é possível usar a [**Guia de APIs**](https://console.bluemix.net/openwhisk/apimanagement) no painel do {{site.data.keyword.openwhisk_short}} para modificar a configuração das maneiras a seguir.

* [Crie uma API {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/apimanagement) que agrupe um conjunto de ações do {{site.data.keyword.openwhisk_short}}.
* [Proteja sua API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) aplicando as políticas de segurança e de limitação de taxa da API.
* [Gerencie o tráfego](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) visualizando as estatísticas de uso da API e verificando os logs de resposta.
* [Socialize e compartilhe](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) sua API com desenvolvedores tanto dentro quanto fora do {{site.data.keyword.Bluemix_notm}}.

Quando você tiver terminado de atualizar a configuração, será possível fazer download do arquivo de definição em formato JSON e, em seguida, reimportá-lo usando a CLI. O download e a importação da configuração são úteis, por exemplo, para uma implementação não assistida em um pipeline de integração e implementação contínuas (CICD). Você também tem a opção de fazer upload e reimportar o arquivo de definição de API usando a UI.
