---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Criando APIs de REST sem servidor
{: #apigateway}

É possível usar as APIs para gerenciar diretamente as [ações da web](/docs/openwhisk?topic=cloud-functions-actions_web) do {{site.data.keyword.openwhisk}}.
{: shortdesc}

A criação de APIs com o Gateway de API não é suportada para namespaces baseados no IAM. Em vez disso, use um namespace baseado no Cloud Foundry.
{: important}

## Por que usar as APIs de REST com o {{site.data.keyword.openwhisk_short}}?

É possível usar o Gateway de API como um proxy para suas ações da web. O Gateway de API fornece roteamento de método de HTTP, ID do cliente e segredos, limites de taxa, CORS, visualizando o uso da API, visualizando logs de resposta e políticas de compartilhamento de API.

Para obter mais informações sobre gerenciamento de API, é possível ler a [documentação de gerenciamento de API](/docs/api-management?topic=api-management-manage_openwhisk_apis).

## Criando sua primeira API
{: #api_create}

Deve-se ter as permissões `SpaceDeveloper` em seu espaço do Cloud Foundry para criar APIs de REST. As permissões de espaço podem ser vistas executando `ibmcloud account space-roles <org>`.
{: note}

Antes de iniciar, instale o [plug-in da CLI do {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Salve o código a seguir em um arquivo JavaScript denominado `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. Crie uma ação da web que é denominada `hello` usando o arquivo que você criou. Certifique-se de incluir a sinalização `--web true`. Substitua `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js`.

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Saída de exemplo**
  ```
  ok: ação hello criada
  ```
  {: screen}

3. Crie uma API com o caminho base `/hello`, o caminho `/world`, o método `get` e o tipo de resposta `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Saída de exemplo** Uma nova URL é gerada expondo a ação `hello` usando um método de HTTP `GET`.

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. Envie uma solicitação de HTTP de teste para a URL usando o comando cURL a seguir.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **Saída de exemplo**
  A ação da web `hello` é chamada e retorna um objeto JSON que inclui o parâmetro `name` no parâmetro de consulta. É possível passar parâmetros para a ação com parâmetros de consulta simples ou usando o corpo da solicitação. As ações da web podem chamar publicamente uma ação sem usar a autenticação.

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## Usando o controle integral sobre a resposta de HTTP
{: #api_control}

A sinalização `--response-type` controla a URL de destino da ação da web a ter o proxy efetuado pelo API Gateway. Por exemplo, ao usar a sinalização `--response-type json`, o resultado integral da ação é retornado no formato JSON e o cabeçalho `Content-Type` é configurado automaticamente como `application/json`.

Para retornar tipos de conteúdo diferentes no corpo, use o controle integral sobre as propriedades de resposta de HTTP, como `statusCode` e `headers`. É possível usar a sinalização `--response-type http` para configurar a URL de destino da ação da web com a extensão `http`. É possível mudar o código da ação para obedecer ao retorno de ações da web com a extensão `http` ou incluir a ação em uma sequência para passar seu resultado para uma nova ação. A nova ação pode então transformar o resultado para ser formatado adequadamente para uma resposta de HTTP. É possível ler mais sobre tipos de resposta e extensões de ações da web na documentação [ações da web](/docs/openwhisk?topic=cloud-functions-actions_web).

1. Salve o código a seguir como `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão de seu código `hello.js`.
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Saída**
  ```
  ok: updated action hello
  ```
  {: screen}

3. Atualize o tipo de resposta da API usando a sinalização `-- response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **Saída**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. Chame a API atualizada usando o comando cURL a seguir.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **Saída de exemplo**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## Modificando a configuração
{: #api_modify_config}

Depois de criar sua configuração, é possível usar a [guia APIs](https://cloud.ibm.com/openwhisk/apimanagement){: external} no painel do {{site.data.keyword.openwhisk_short}} para modificar a configuração das maneiras a seguir.

* [Crie uma API do {{site.data.keyword.openwhisk_short}}](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) que agrupe um conjunto de ações do {{site.data.keyword.openwhisk_short}}.
* [Proteja sua API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) aplicando as políticas de segurança e de limitação de taxa da API.
* [Gerencie o tráfego](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) visualizando as estatísticas de uso da API e verificando os logs de resposta.
* [Socialize e compartilhe](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) sua API com desenvolvedores tanto dentro quanto fora do {{site.data.keyword.cloud_notm}}.

</br>
Depois de concluir a atualização da configuração, será possível fazer download do arquivo de definição no formato JSON e, em seguida, reimportá-lo usando a CLI. O download e a importação da configuração são úteis, por exemplo, para uma implementação não assistida em um pipeline de integração e implementação contínuas (CICD). Também será possível fazer upload e reimportar o arquivo de definição de API usando a IU.



