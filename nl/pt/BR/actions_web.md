---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# Criando ações da web
{: #actions_web}

Quando você cria uma ação da web, o resultado é uma URL que pode ser usada para acionar a ação por meio de qualquer app da web.
{: shortdesc}

## Por que usar ações da web em vez de ações padrão?

### 1. Executar ações da web anonimamente

As ativações de ação da web estão associadas ao usuário que criou a ação, em vez de ao responsável pela chamada da ação. Geralmente, para chamadas da API para apps como o Github, você incluiria um nome de usuário e um token com a chamada da API para um usuário específico ou um ID funcional. Quando você usa uma ação da web, esses tipos de credenciais não são necessários. Uma ação da web é acessível por meio de uma interface REST sem a necessidade de credenciais.

Embora não seja necessário usar credenciais com ações da web, é possível implementar sua própria autenticação e autorização ou um fluxo OAuth. Para configurar uma ação da web com credenciais, consulte [Protegendo ações da web](#actions_web_secure).

### 2. Usar qualquer tipo de solicitação de HTTP

Por padrão, as ações aceitam somente solicitações `POST`, mas as ações da web podem ser chamadas por meio de qualquer um destes métodos de HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, bem como `HEAD` e `OPTIONS`.

### 3. Acionar uma ação da web de qualquer lugar

Ao criar uma ação da web do {{site.data.keyword.openwhisk}}, você gera uma URL para chamar essa ação de qualquer app baseado na web. As ações que não são ações da web requerem autenticação e devem responder com um objeto JSON. 

Um caminho da API de ação da web pode ser usado com cURL, `wget` ou até mesmo ser inserido diretamente em seu navegador. Uma ação da web pode ser chamada usando uma URL estruturada desta forma: `https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName>.<ext>`.

### 4. Criar menos entidades do {{site.data.keyword.openwhisk_short}}

Como é possível chamar uma ação da web de qualquer lugar, não é necessário criar outras entidades do {{site.data.keyword.openwhisk_short}}, como acionadores ou regras.

## Como as ações da web funcionam?

As ações da web podem ser chamadas sem autenticação e podem ser usadas para implementar manipuladores de HTTP que respondam com o conteúdo `headers`, `statusCode` e `body` de tipos diferentes.

As ações da web devem retornar um objeto JSON. No entanto, o Controller tratará uma ação da web de forma diferente se seu resultado incluir um ou mais dos itens a seguir como [propriedades JSON](#web_action_properties) de nível superior.
{: shortdesc}

## Recursos disponíveis de ações da web
{: #actions_web_extra}

As ações da web suportam os recursos a seguir:

| Recurso | Descrição |
| --- | --- |
| [Extensões de conteúdo](#extra_features) | É possível especificar um tipo de conteúdo para solicitações de HTTP, como `.json`, `.html`, `.http`, `.svg` ou `.text`. Se nenhum tipo de conteúdo for especificado, a extensão `.http` será presumida. É possível especificar um tipo de conteúdo incluindo uma extensão no nome da ação no URI para que uma ação `demo/hello` seja referenciada como `/demo/hello.svg`. As extensões `.json` e `.http` não requerem um caminho de projeção, enquanto as extensões `.html`, `.svg` e `.text` requerem. Presume-se que o caminho padrão corresponda ao nome da extensão. Para chamar uma ação da web e receber uma resposta `.html`, a ação deve responder com um objeto JSON que contém uma propriedade de nível superior chamada `html` (ou a resposta deve estar no caminho explícito). Em outras palavras, `/<namespace>/demo/hello.html` é equivalente a projetar a propriedade `html` explicitamente, como em `/<namespace>/demo/hello.html/html`. O nome completo da ação deverá incluir seu nome do pacote, que é `default` se a ação não estiver em um pacote nomeado. |
| [Projeção de campos por meio do resultado](#projecting_fields) | O caminho que segue o nome da ação é usado para projetar um ou mais níveis da resposta. Por exemplo, `/demo/hello.html/body`. Esse recurso permite que qualquer ação da web que retorne um dicionário, como `{body: "..." }`, projete a propriedade `body` e retorne diretamente seu valor de sequência em vez de seu valor de dicionário. O caminho projetado segue um modelo de caminho absoluto (como em XPath). |
| [Parâmetros de consulta e de corpo como uma entrada](#query_test) | A ação recebe os parâmetros de consulta, bem como os parâmetros no corpo da solicitação. A ordem de precedência para mesclar parâmetros é: parâmetros de pacote, parâmetros de ação, parâmetro de consulta e parâmetros de corpo. Cada um desses parâmetros poderá substituir quaisquer valores anteriores se a sobreposição ocorrer. Como um exemplo, `/demo/hello.http?name=Jane` pode passar o argumento `{name: "Jane"}` para a ação. |
| [Dados de formulário](#form_data) | Além do `application/json` padrão, as ações da web podem receber os dados de formulário codificados por URL `application/x-www-form-urlencoded data` como uma entrada.
| [Ativações usando vários verbos HTTP](#actions_web_options) | Uma ação da web pode ser chamada por meio de qualquer um destes métodos de HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, bem como `HEAD` e `OPTIONS`. |
| [Manipulação de corpo não JSON e de entidade HTTP bruta](#actions_web_raw_enable) | Uma ação da web pode aceitar um corpo de solicitação de HTTP diferente de um objeto JSON e pode optar por sempre receber esses valores como valores opacos (texto simples quando não for um arquivo binário ou, caso contrário, uma sequência codificada em base64). |

## Criando uma ação da web
{: #actions_web_example}

Para criar uma ação da web: 

1. Salve o código JavaScript a seguir como `hello.js`.

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Crie o pacote `demo`. O nome do pacote é `default`, a menos que especificado explicitamente.
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. Crie a ação `hello`. Neste exemplo, o `packageName/actionName` é `demo/hello`. Substitua a variável `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js` e configure o sinalizador `--web` como `true`. 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. Chame ou teste a ação da web `hello` sem nenhum parâmetro. Substitua as variáveis `<apihost>` e `<namespace>`. Para obter o `<apihost>`, execute `ibmcloud fn property get --apihost`. Por exemplo, `<apihost>`: `us-south.functions.cloud.ibm.com`.

  Para namespaces ativados pelo IAM, substitua a variável `<namespace>` pelo ID do namespace. Para obter o ID, execute `ibmcloud fn namespace get <namespace_name>`.
  {: note}

  a. É possível testar a ação da web de uma das seguintes formas: 
    * Abrir uma URL usando a estrutura `https://<apihost>/api/v1/web/<namespace>/demo/hello` em seu navegador.
    * Testar a ação usando um comando cURL.
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * Testar a ação usando um comando `wget`.  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. O código de ação retorna o dicionário a seguir.
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  Também é possível testar a ação retornando apenas a propriedade `body` usando o comando a seguir:
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **Saída de exemplo**

    Como o parâmetro `<name>` não foi especificado, a mensagem a seguir é retornada.
    ```
    <html><body><h3>Você não me disse quem você é.</h3></body></html>
    ```
    {: screen}

5. Agora tente definir o parâmetro `<name>`. Teste a ação com um parâmetro `<name>` de uma das duas formas:
  * Abrir `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane` em seu navegador. 
  * Testar a ação usando um comando cURL.

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * Testar a ação usando um comando `wget`.  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **Saída de exemplo**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**Próximas etapas**

Inclua a URL para sua ação da web `hello` em seu app da web e teste-a lá.

### Propriedades JSON de ação da web
{: #web_action_properties}

O `Content-Type` padrão para uma resposta HTTP é `application/json`, e o corpo pode ser qualquer valor JSON permitido. Se o `Content-Type` não for `application/json`, um `Content-Type` deverá ser especificado nos `headers` de seu código de ação.

Se o [limite de tamanho do resultado](/docs/openwhisk?topic=cloud-functions-limits) para ações for atingido, a resposta falhará. Se você souber que seu resultado de ação é maior que 5 MB, configure um [armazenamento de objeto](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

| Propriedade JSON | Descrição |
| --- | --- |
| `headers`| Um objeto JSON no qual as chaves são nomes de cabeçalho e os valores são valores de sequência, número ou booleano. Para enviar vários valores para um único cabeçalho, o valor do cabeçalho é uma matriz JSON dos diversos valores. Os cabeçalhos não são configurados por padrão. |
| `statusCode` | Um código de status HTTP válido. Se o conteúdo do corpo estiver presente, o padrão será `200 OK`. Se nenhum conteúdo do corpo estiver presente, o padrão será `204 Sem Conteúdo`. |
| `body` | Uma sequência que é texto sem formatação, um objeto JSON ou matriz ou uma sequência codificada em base64 para dados binários. O corpo será considerado vazio se for `null`, a sequência vazia `""` ou indefinida. O padrão é um corpo vazio. |

O [Controller](/docs/openwhisk?topic=cloud-functions-about#about_controller) transmite todos os cabeçalhos especificados pela ação, o código de status ou o corpo para o cliente HTTP que finaliza a solicitação ou a resposta. Se o cabeçalho `Content-Type` não for declarado nos `headers` do resultado da ação, o corpo será interpretado como `application/json` para valores não de sequência e `text/html`, caso contrário. Se o cabeçalho `Content-Type` for definido, o controlador determinará se a resposta será texto simples ou dados binários e decodificará a sequência usando um decodificador de base64 conforme necessário. Se o corpo não for decodificado corretamente, um erro será retornado para o cliente.

O proprietário da ação da web possui todos os registros de ativações e incorre no custo de executar a ação no sistema independentemente de como a ação foi chamada.
{: note}

#### Parâmetros protegidos
Os parâmetros de ação são protegidos e só podem ser mudados atualizando sua ação. Os parâmetros são finalizados automaticamente para ativar ações da web.

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


O resultado dessas mudanças é que o `name` é ligado a `Jane` e não pode ser substituído por parâmetros de consulta ou corpo devido à anotação final. Esse design assegura a ação contra parâmetros de consulta ou corpo que tentam mudar esse valor, seja por acidente ou intencionalmente.

### Executando um redirecionamento de HTTP usando uma ação da web
{: #http_redirect}
Talvez seja possível usar esse recurso em um aplicativo da web para redirecionar um usuário para a nova versão de seu site.

**Antes de iniciar** Crie o pacote `demo` e a ação da web `hello` concluindo as etapas em [Criando uma ação da web](#actions_web_example).

Para criar uma ação da web que execute um redirecionamento de HTTP:

1. Salve o código como `hello.js`.

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão para seu código `hello.js`. Substitua `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js`.

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Teste a ação da web `hello`. Substitua as variáveis `<apihost>` e `<namespace>`. É possível testar a ação da web de uma das seguintes formas:

  * Abrindo a URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` em seu navegador. 
  * Executando o comando cURL a seguir:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Executando o comando `wget` a seguir:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **Resultado de exemplo** 
  
  Essa ação da web de exemplo redireciona seu navegador para o [painel do {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/){: external}.

### Configurando cookies usando uma ação da web
{: #multiple_cookie}
Talvez seja possível usar esse recurso em um aplicativo da web para armazenar um token da web JSON como um cookie de sessão após um login bem-sucedido.

Para criar uma ação da web que configura vários cookies:

**Antes de iniciar** Crie o pacote `demo` e a ação da web `hello` concluindo as etapas em [Criando uma ação da web](#actions_web_example).

1. Salve o código como `hello.js`.
  ```javascript
  function main() {
    return {
      headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
        ],
      'Content-Type': 'text/html'
      }, 
    statusCode: 200,
    body: '<html><body><h3>Olá</h3></body></html>' }
  }
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão para seu código `hello.js`. Substitua `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. Limpe os cookies de seu navegador antes de testar a ação.

4. Teste a ação da web `hello` abrindo a URL em seu navegador. Substitua as variáveis `<apihost>` e `<namespace>` e abra `https://<apihost>/api/v1/web/<namespace>/demo/hello`. Por exemplo, `<apihost>`: `us-south.functions.cloud.ibm.com`.

**Resultado**

Os cookies `UserID=Jane` e `SessionID=asdfgh123456` são configurados nas ferramentas do desenvolvedor de seu navegador.


### Retornando uma imagem usando uma ação da web
{: #return_image}
Talvez seja possível usar esse recurso em um aplicativo da web para retornar a imagem de um sinalizador de país com base no código de idioma do usuário.

**Antes de iniciar** 

Crie o pacote `demo` e a ação da web `hello` concluindo as etapas em [Criando uma ação da web](#actions_web_example).

Para criar uma ação da web que retorne uma `image/png`: 

1. Salve o código como `hello.js`.

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão para seu código `hello.js`. Substitua `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Teste a ação em seu navegador ou usando um comando cURL. Substitua as variáveis `<apihost>` e `<namespace>`. É possível testar a ação da web de uma das seguintes formas:

  * Abrindo a URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` em seu navegador. 
  * Executando o comando cURL a seguir.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Executando o comando `wget` a seguir.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### Retornando o JSON usando uma ação da web
{: #return_json}
Talvez seja possível usar esse recurso em um aplicativo da web para retornar um objeto JSON de informações de IP do usuário.

**Antes de iniciar** 

Crie o pacote `demo` e a ação da web `hello` concluindo as etapas em [Criando uma ação da web](#actions_web_example).

Para criar uma ação da web que retorne `application/json`:

1. Salve o código como `hello.js`.
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão para seu código `hello.js`. Substitua `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js`.
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Teste a ação em seu navegador ou usando um comando cURL. Substitua as variáveis `<apihost>` e `<namespace>`. É possível testar a ação da web de uma das seguintes formas:
  * Abrindo a URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` em seu navegador. 
  * Executando o comando cURL a seguir:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Executando o comando `wget` a seguir:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **Saída de exemplo**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### Contexto de HTTP
{: #actions_web_context}

Todas as ações da web, quando chamadas, recebem detalhes de solicitação de HTTP como parâmetros de entrada para o argumento de ação.

| Parâmetro HTTP | Tipo | Descrição |
| --- | --- | --- |
| `__ow_method` | Sequência | O método de HTTP da solicitação. |
| `__ow_headers` | Mapear sequência para sequência | Os cabeçalhos de solicitação. |
| `__ow_path` | Sequência | O caminho não correspondido da solicitação (a correspondência para depois que a extensão de ação é consumida). |
| `__ow_user` | Sequência | O namespace que identifica o indivíduo autenticado pelo {{site.data.keyword.openwhisk_short}}. |
| `__ow_body` | Sequência | A entidade do corpo da solicitação, como uma sequência codificada em base64 quando o conteúdo é um arquivo binário ou, caso contrário, uma sequência simples. |
| `__ow_query` | Sequência | Os parâmetros de consulta da solicitação como uma sequência não analisada. |

Uma solicitação não pode substituir nenhum dos parâmetros nomeados `__ow_`. Fazer isso resulta em uma solicitação com falha com status igual a 400 Solicitação ruim.

O `__ow_user` estará presente apenas quando a ação da web for [anotada para requerer autenticação](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) e permitirá que uma ação da web implemente sua própria política de autorização. O `__ow_query` estará disponível apenas quando uma ação da web optar por manipular a [solicitação de HTTP "bruto"](#actions_web_raw_enable). O `__ow_query` é uma sequência que contém os parâmetros de consulta que são analisados por meio do URI (separados por `&`). A propriedade `__ow_body` está presente em solicitações de HTTP brutas ou quando a entidade de solicitação de HTTP não é um objeto JSON ou dados de formulário. Caso contrário, as ações da web recebem parâmetros de consulta e de corpo como propriedades de primeira classe no argumento da ação. Os parâmetros de corpo têm precedência sobre os parâmetros de consulta, que por sua vez têm precedência sobre os parâmetros de ação e pacote.

### Suporte ao terminal HTTPS
{: #actions_web_endpoint}

Protocolos SSL suportados: TLS 1.2, TLS 1.3 ([versão de rascunho 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### Alterando o conteúdo de resposta da ação da web
{: #extra_features}
É possível alterar o conteúdo de resposta de uma ação da web para retornar tipos de conteúdo diferentes usando [Extensões de conteúdo](#actions_web_extra).
{: shortdesc}

**Antes de iniciar**

Crie o pacote `demo` e a ação da web `hello` concluindo as etapas em [Criando uma ação da web](#actions_web_example).

Para alterar a resposta de uma ação da web:

1. Salve o código a seguir como `hello.js`.

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão de seu código `hello.js`. Substitua `<filepath>` pelo caminho de arquivo de seu arquivo `hello.js`.

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Teste a ação em seu navegador ou usando um comando cURL. Substitua as variáveis `<apihost>` e `<namespace>`.

  a. Retorne JSON de uma das formas:
    * Abrindo `https://<apihost>/api/v1/web/<namespace>/demo/hello.json` em seu navegador da web. 
    * Executando o comando cURL a seguir.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * Executando o comando `wget` a seguir.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **Saída de exemplo**

      ```
      {
        "response": {
          "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. Teste a ação usando um parâmetro de consulta. É possível testar a ação de uma das formas:
  {: #query_test}

    * Executando o comando cURL a seguir.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * Executando o comando `wget` a seguir.

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **Saída de exemplo**
      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. Também é possível testar a ação da web usando dados de formulário. É possível testar a ação da web de uma das seguintes formas:
  {: #form_data}
  
    * Executando o comando cURL a seguir.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * Executando o comando `wget` a seguir.
        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **Saída de exemplo**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. É possível especificar um objeto JSON executando o comando a seguir. É possível testar a ação da web de uma das seguintes formas:
    * Executando o comando cURL a seguir.
        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * Executando o comando `wget` a seguir.
        ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **Saída de exemplo**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. Também é possível retornar o valor `name` como texto de uma das formas:
  * Executando o comando cURL a seguir.

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * Executando o comando `wget` a seguir.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **Saída de exemplo**

    ```
    Janete
    ```
    {: screen}

    Em ações padrão, os parâmetros de consulta, os dados do formulário e as entidades do corpo do objeto JSON são todos tratados como dicionários e seus valores são diretamente acessíveis como propriedades de entrada de ação. Esse comportamento não é o caso para ações da web, que manipulam entidades de solicitação de HTTP, ou quando a ação da web recebe uma entidade que não é um objeto JSON.
    {: note}

  f. É possível configurar o `Content-Type` de uma das formas:
  * Executando o comando cURL a seguir.  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * Executando o comando `wget` a seguir.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **Saída de exemplo**

    ```
    {
      "response": {
        "__ow_method": "post",
    "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## Protegendo as ações da web
{: #actions_web_secure}

**Antes de iniciar** Crie o pacote `demo` e a ação da web `hello` concluindo as etapas em [Criando uma ação da web](#actions_web_example).

Por padrão, qualquer pessoa pode chamar uma ação da web usando a URL de chamada. É possível usar a [anotação da ação da web](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) do `require-whisk-auth` para proteger a ação da web das formas a seguir:
  1. Configurando a anotação `require-whisk-auth` como `true`. Quando a anotação `require-whisk-auth` estiver configurada como `true`, a ação da web autenticará as credenciais de Autorização básica da solicitação de chamada com relação à chave de autenticação de whisk do proprietário da ação da web. Quando configurada para um número ou uma sequência com distinção entre maiúsculas e minúsculas, a solicitação de chamada da ação da web deve incluir o conjunto de cabeçalho `X-Require-Whisk-Auth` configurado para esse mesmo número ou sequência com distinção entre maiúsculas e minúsculas. As ações da web seguradas retornam a mensagem `Not Authorized` quando a validação de credencial falha.

  2. Permitindo que a anotação `require-whisk-auth` seja configurada automaticamente usando o sinalizador `--web-secure`. Quando o sinalizador `--web-secure` é configurado como `true`, um número aleatório é gerado como o valor de anotação `require-whisk-auth`. Quando configurada para `false`, a anotação `require-whisk-auth` é removida.  Quando configurada para qualquer outro valor, esse valor é usado como o valor de anotação `require-whisk-auth`.

Para testar uma ação da web segura:

1. Salve o código JavaScript a seguir como `hello.js`.
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Atualize sua ação da web `hello` com a nova versão de seu código `hello.js` e configure o sinalizador `--web secure` como `true`.
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. Obtenha a ação da web `hello` para visualizar o valor de `require-whisk-auth` gerado aleatoriamente.

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **Saída de exemplo**

    O valor `require-whisk-auth` foi configurado como `7819991076995522`.
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

Para testar se a autenticação está funcionando:

1. Teste a ação da web `hello` sem configurar o parâmetro `X-Require-Whisk-Auth` para verificar se a autenticação é necessária. Esse teste resultará em um erro. É possível testar a ação da web de uma das seguintes formas:

  * Testando a ação da web usando um comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * Testando a ação da web usando um comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **Saída de exemplo**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    A chamada falha porque o valor `X-Require-Whisk-Auth` não foi fornecido.
    {: note}

2. Agora, teste a ação da web `hello` e forneça o valor de `X-Require-Whisk-Auth` aleatoriamente gerado. Substitua os valores `<apihost>` e `<namespace>`. Substitua o valor de `<my-secret>` pelo número gerado aleatoriamente que você criou na etapa 3. É possível testar a ação da web de uma das seguintes formas:
  * Testando a ação da web usando um comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * Testando a ação da web usando um comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **Saída de exemplo**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

Para testar uma ação da web usando um valor customizado `require-whisk-auth`:

1. Atualize sua ação da web `hello` com seu próprio valor de `require-whisk-auth`. Em seguida, tente testar sua ação da web especificando o valor `X-Require-Whisk-Auth` durante a chamada.

  a. Configure um valor `require-whisk-auth` em que `<my-secret>` é seu token de autenticação com distinção entre maiúsculas e minúsculas.
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. Teste a ação da web e inclua seu valor `<my-secret>`. É possível testar a ação da web de uma das seguintes formas:
  * Testando a ação da web usando um comando cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * Testar a ação usando um comando `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## Manipulação de HTTP bruto
{: #actions_web_raw}

Uma ação da web pode optar por interpretar e processar um corpo HTTP recebido diretamente, sem a promoção de um objeto JSON para as propriedades de primeira classe disponíveis para a entrada de ação da web (por exemplo, `args.name` versus a análise de `args.__ow_query`). Esse processo é feito por meio de uma [anotação](/docs/openwhisk?topic=cloud-functions-annotations) `raw-http`. Usando o mesmo exemplo mostrado anteriormente, mas agora como uma ação da web de HTTP "bruto" que recebe `name`, como um parâmetro de consulta e como um valor JSON no corpo da solicitação de HTTP:
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**Saída de exemplo**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",      
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

O {{site.data.keyword.openwhisk_short}} usa a estrutura [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} para [determinar quais tipos de conteúdo são arquivos binários e quais são texto sem formatação](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}.

### Ativando a manipulação de HTTP bruto
{: #actions_web_raw_enable}

É possível criar ações da web HTTP brutas configurando `--web` como `raw`.
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Decodificando conteúdo do corpo binário do Base64
{: #actions_web_decode}

Quando o conteúdo HTTP bruto é processado, o conteúdo `__ow_body` é codificado em Base64 quando a solicitação `Content-Type` é o tipo binário. As funções a seguir demonstram como decodificar o conteúdo do corpo no Node, Python e Swift.

1. Salve o código de amostra em seu idioma preferencial para um arquivo chamado `decode.<ext>`. Substitua `<ext>` pela extensão de arquivo do código de amostra de seu idioma preferencial.

  **Node**
  {: #actions_web_decode_js}

  ```javascript
  function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
  def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
  extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
        }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. Crie uma ação da web HTTP bruta com o código de amostra executando o comando a seguir. Neste exemplo, a função do Node é salva como `decode.js`. Substitua o caminho de arquivo pelo caminho de arquivo de seu arquivo `decode` e atualize a extensão do arquivo para corresponder à extensão do código de amostra que você usou.

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **Saída de exemplo**
  ```
  ok: created action decode
  ```
  {: screen}

3. Teste a ação `decode` executando o comando cURL a seguir.
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **Saída de exemplo**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## Solicitações de opções
{: #actions_web_options}

Por padrão, uma solicitação `OPTIONS` feita para uma ação da web resulta em cabeçalhos CORS que são automaticamente incluídos nos cabeçalhos de resposta. Esses cabeçalhos permitem todas as origens e os verbos HTTP `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD` e `PATCH`.
{: shortdesc}

Veja os cabeçalhos a seguir:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Como alternativa, as solicitações `OPTIONS` podem ser manipuladas manualmente por uma ação da web. Para ativar essa opção, inclua uma
anotação `web-custom-options` com um valor de `true` para uma ação da web. Quando esse recurso é ativado, os cabeçalhos CORS não são incluídos automaticamente na resposta de solicitação. Em vez disso, deve-se anexar seus cabeçalhos programaticamente.

Para criar respostas customizadas para solicitações `OPTIONS`:

1. Salve o código a seguir em um arquivo `custom-options.js`.

  ```js
  function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
        },
      statusCode: 200
    }
    }
  }
  ```
  {: codeblock}

2. Crie a ação da web. Configure o `--web-custom-options` como `true`.

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. Teste a ação usando o comando cURL a seguir.

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **Saída de exemplo**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## Manipulação de erros
{: #actions_web_errors}

Uma ação do {{site.data.keyword.openwhisk_short}} falha em dois diferentes modos de falha possíveis. O primeiro é conhecido como um _erro de aplicativo_ e é análogo a uma exceção de captura: a ação retorna um objeto JSON que contém uma propriedade `error` de nível superior. O segundo é um _erro de desenvolvedor_, que ocorre quando a ação falha e não produz uma resposta (semelhante a uma exceção de não captura). Para ações da web, o controlador manipula erros de aplicativo conforme a seguir:

- Qualquer projeção de caminho especificada é ignorada e o controlador projeta a propriedade `error` em seu lugar.
- O controlador aplica a manipulação de conteúdo que está implícita pela extensão de ação ao valor da propriedade `error`.

Os desenvolvedores devem saber como as ações da web podem ser usadas e geram respostas de erro apropriadas. Por exemplo, uma ação da web que é usada com a extensão `.http` retorna uma resposta de HTTP como `{error: { statusCode: 400 }`. A falha em fazer isso é uma incompatibilidade entre o `Content-Type` implícito na extensão e o `Content-Type` da ação na resposta de erro. Consideração especial deve ser dada a ações da web que são sequências para que os componentes que compõem uma sequência possam gerar erros adequados quando necessário.



## Desativando ações da web
{: #actions_web_disable}

É possível desativar uma ação da web configurando o sinalizador `--web` como `false` ou `no` na CLI. Substitua `<packageName>/<actionName>` e `<filepath>/<filename>` pelo nome do pacote, nome da ação da web, caminho de arquivo e nome do arquivo de seu arquivo de código.

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



