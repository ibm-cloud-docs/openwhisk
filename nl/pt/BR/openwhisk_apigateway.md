---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Gateway da API
{: #openwhisk_apigateway}

As ações do OpenWhisk podem se beneficiar de serem gerenciadas pelo gerenciamento de API.

O API Gateway age como um proxy para [Ações da web](./openwhisk_webactions.html) e fornece a elas recursos adicionais. Os recursos adicionais incluem: roteamento de método de HTTP, ID/segredos de cliente, limites de taxa, CORS, visualizar o uso da API, visualizar logs de respostas e políticas de compartilhamento de API.
Para obter mais informações sobre gerenciamento de API, é possível ler a [documentação de gerenciamento de API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Criar APIs de ações da web do OpenWhisk usando seu navegador.

Com o API Gateway, é possível expor uma ação do OpenWhisk como uma API. Depois de definir a API, é possível aplicar políticas de segurança e de limite de taxa, visualizar logs de uso e resposta da API e definir políticas de compartilhamento de API.
No Painel do OpenWhisk, clique na [Guia APIs](https://console.ng.bluemix.net/openwhisk/apimanagement).


## Criar APIs de ações da web do OpenWhisk usando a CLI

### Configuração da CLI do OpenWhisk

Configure a CLI do OpenWhisk com o host da API.

Estão disponíveis duas regiões do {{site.data.keyword.Bluemix_notm}} que requerem seu host de API e chave de autorização exclusivos.

* Sul dos Estados Unidos
  * Host da API: `openwhisk.ng.bluemix.net`

* 
  * Host da API: `openwhisk.eu-gb.bluemix.net`

Execute o comando a seguir para configurar o host de API para sua região desejada do Bluemix:

Sul dos EUA:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Reino Unido:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Se você precisa alternar regiões, deve-se reconfigurar a CLI com o host de API e a chave de autorização porque a chave de autorização é específica por região.
{: tip}

Artefatos como Ações, Regras e pacotes são específicos da região. Então, se você usar o mesmo artefato em múltiplas regiões, deverá implementá-lo em cada região desejada.

Para poder usar o comando `wsk api`, o arquivo de configuração da CLI `~/.wskprops` precisa conter o Token de acesso do Bluemix.

Para obter o token de acesso, use o comando da CLI a seguir:
```
wsk bluemix login
```
{: pre}

Para obter mais informações sobre esse comando, execute:
```
wsk bluemix login -h
```
{: pre}

Se o comando `wsk bluemix login` falha com o erro `BMXLS0202E: You are using a federated user ID, please use one time code to login with option --sso`, efetue login com a CLI do {{site.data.keyword.Bluemix_notm}} usando `bluemix login`, em seguida, emita `wsk bluemix login --sso`.
{: tip}

### Criar sua primeira API usando a CLI

1. Crie um arquivo JavaScript com o conteúdo a seguir. Para esse exemplo, o nome do arquivo é 'hello.js'.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. Crie uma ação da web por meio da função JavaScript a seguir. Para este exemplo, a ação é chamada 'hello'. Certifique-se de incluir a sinalização `--web true`.
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: ação hello criada
  ```
  
3. Crie uma API com o caminho base `/hello`, caminho `/world` e método `get`, com o tipo de resposta `json`:
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  Uma nova URL é gerada expondo a ação `hello` por meio de um método de HTTP __GET__.
  
4. Finalmente, envie uma solicitação de HTTP para a URL.
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  A ação da web `hello` é chamada, que retorna de volta um objeto JSON que inclui o parâmetro `name` enviado por meio do parâmetro de consulta. É possível passar parâmetros para a ação por meio de parâmetros de consulta simples ou por meio do corpo da solicitação. As ações da web podem chamar uma Ação de uma maneira pública sem a chave API de autorização do OpenWhisk.
  
### Controle total sobre a resposta de HTTP
  
  A sinalização `--response-type` controla a URL de destino da ação da web a ter o proxy efetuado pelo API Gateway. Usar `--response-type json` retorna o resultado integral da ação no formato JSON e configura automaticamente o cabeçalho Content-Type para `application/json`. 
  
  Depois de começar, você deseja ter o controle total sobre as propriedades de resposta de HTTP como `statusCode`, `headers` e retornar tipos de conteúdo diferentes no `body`. A sinalização `--response-type http` torna isso possível configurando a URL de destino da ação da web com a extensão `http`.

  É possível escolher mudar o código da ação para obedecer ao retorno de ações da web com a extensão `http` ou incluir a ação em uma sequência para passar seu resultado para uma nova ação. A nova ação pode então transformar o resultado para ser formatado adequadamente para uma resposta de HTTP. É possível ler mais sobre tipos de resposta e extensões de ações da web na documentação [Ações da web](./openwhisk_webactions.html).

  Mude o código para o `hello.js` retornando as propriedades JSON `body`, `statusCode` e `headers`
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'), 
        statusCode:200, 
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  Observe que o corpo precisa ser retorno codificado em `base64` e não uma sequência.
  
  Atualize a ação com o resultado modificado: 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  Atualize a API com `--response-type http`: 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  Chame a API atualizada: 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  Agora, você está no controle total de suas APIs e pode controlar o conteúdo. Como retornar HTML ou configurar o código de status para coisas como Não localizado (404), Desautorizado (401) ou até mesmo Erro interno (500).

### Expondo múltiplas ações da web

Por exemplo, se você deseja expor um conjunto de ações para um clube do livro, é possível usar uma série de ações para implementar seu backend para o clube do livro:

| Ação | Método HTTP | Descrição |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Obter detalhes do livro  |
| postBooks   | POST | Incluir um livro |
| putBooks    | PUT | Atualizar detalhes do livro |
| deleteBooks | DELETE | Excluir um livro |

Crie uma API para o clube do livro, chamado `Book Club`, com `/club` como seu caminho base da URL de HTTP e `books` como seu recurso.
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

Observe que a primeira ação que é exposta com o caminho base `/club` obtém o rótulo da API com o nome `Book Club`. Quaisquer outras ações expostas em `/club` são associadas ao `Book Club`.

Liste todas as ações que são expostas usando o comando a seguir:
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Por diversão, é possível incluir um livro, `JavaScript: The Good Parts`, com um HTTP __POST__:
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

Obtenha uma lista usando a ação `getBooks` por meio de HTTP __GET__
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### Exportar a configuração
Exporte a API chamada `Book Club` para um arquivo que possa ser usado como uma base para recriar as APIs usando um arquivo como entrada. 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

Teste o arquivo swagger excluindo primeiramente todas as URLs expostas em um caminho base comum.
É possível excluir todas as expostas usando o caminho base `/club` ou o rótulo de nome da API `"Book Club"`:
```
wsk api delete /club
```
```
ok: deleted API /club
```
### Mudar a configuração

É possível editar a configuração no Painel do OpenWhisk, clicar na [guia APIs](https://console.ng.bluemix.net/openwhisk/apimanagement) para configurar a segurança, os limites de taxa e outros recursos.

### Importar a configuração

Agora, restaure a API chamada `Book Club` usando o arquivo `club-swagger.json`
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Verifique se a API foi recriada:
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
