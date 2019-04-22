---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: web actions, http requests, functions, extensions, parameters

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Criando ações da web
{: #openwhisk_webactions}

Ações da web são ações do {{site.data.keyword.openwhisk}} que são anotadas para permitir
que os desenvolvedores construam rapidamente aplicativos baseados na web. Essas ações anotadas permitem que os
desenvolvedores programem a lógica de backend que seu aplicativo da web pode acessar anonimamente, sem
a requisição de uma chave de autenticação do {{site.data.keyword.openwhisk_short}}. Cabe ao
desenvolvedor de ação implementar sua própria autenticação e autorização ou fluxo do OAuth desejado.
{: shortdesc}

As ativações de ação da web estão associadas ao usuário que criou a ação. Essa ação adia o custo de uma ativação de ação do responsável pela chamada para o proprietário da ação.

Veja a ação do JavaScript `hello.js` a seguir:
```javascript
function main({name}) {
  var msg = 'you did not tell me who you are.';
  if (name) {
    msg = `hello ${name}!`
  }
  return {body: `<html><body><h3>${msg}</h3></body></html>`}
}
```
{: codeblock}  

É possível criar uma _ação da web_ **hello** no pacote `demo` para o namespace `guest` usando a sinalização `--web` da CLI com um valor de `true` ou `yes`:
```
ibmcloud fn package create demo
```
{: pre}

```
ibmcloud fn action create /guest/demo/hello hello.js --web true
```
{: pre}

Usar a sinalização `--web` com um valor de `true` ou `yes` permite que uma ação seja acessível por meio de uma interface REST sem a necessidade de credenciais. Para configurar uma ação da web com as credenciais, veja a seção [Protegendo ações da web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#securing-web-actions) seção. Uma ação da web pode ser chamada usando uma URL que é estruturada como a seguir:
`https://{APIHOST}/api/v1/web/{namespace}/{packageName}/{actionName}.{EXT}`.

O nome do pacote será **default** se a ação não estiver em um pacote nomeado.

Um exemplo é `guest/demo/hello`. O caminho da API de ação da web pode ser usado com `curl` ou `wget` sem uma chave API. Ele pode até ser inserido diretamente em seu navegador.

Tente abrir `https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello?name=Jane` em seu navegador da web. Ou tente chamar a ação usando `curl`:
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello?name=Jane
```
{: pre}

No exemplo a seguir, uma ação da web executa um redirecionamento de HTTP:
```javascript
function main() {
  return {
    headers: { location: 'http://openwhisk.org' },
    statusCode: 302
  }
}
```
{: codeblock}    

No exemplo a seguir, uma ação da web configura um único cookie:
```javascript
function main() {
  return {
    headers: {
      'Set-Cookie': 'UserID=Jane; Max-Age=3600; Version=',
      'Content-Type': 'text/html'
    }, 
    statusCode: 200,
    body: '<html><body><h3>Olá</h3></body></html>' }
}
```
{: codeblock}  

No exemplo a seguir, uma ação da web configura múltiplos cookies:
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

O exemplo a seguir retorna um `image/png`:
```javascript
function main() {
    let png = <base 64 encoded string>
    return { headers: { 'Content-Type': 'image/png' },
             statusCode: 200,
             body: png };
}
```
{: codeblock}  

The following example returns `application/json`:
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

The default `Content-Type` for an HTTP response is `application/json`, and the body can be any allowed JSON value. The default `Content-Type` can be omitted from the headers.

It is important to be aware of the [limite de tamanho de resposta](/docs/openwhisk?topic=cloud-functions-openwhisk_reference) para ações porque uma resposta que excede os limites predefinidos do sistema falha. Os objetos grandes não são enviados sequencialmente por meio do {{site.data.keyword.openwhisk_short}},
mas, em vez disso, são adiados para um armazenamento de objeto, por exemplo.

## Manipulando solicitações de HTTP com ações
{: #openwhisk_webactions_http}

Uma ação {{site.data.keyword.openwhisk_short}} que não é uma ação da web requer autenticação e deve responder com um objeto JSON.

As ações da web podem ser chamadas sem autenticação e podem ser usadas para implementar manipuladores de HTTP que respondam com o conteúdo `headers`, `statusCode` e `body` de tipos diferentes.
As ações da web devem retornar um objeto JSON. No entanto, o controlador trata uma ação da web de forma diferente se seu resultado incluir um ou mais dos itens a seguir como propriedades JSON de nível superior:

- `headers`: um objeto JSON no qual as chaves são nomes de cabeçalho e os valores são sequência, número ou valores booleanos. Para enviar vários valores para um único cabeçalho, o valor do cabeçalho é uma matriz JSON dos diversos valores. Os cabeçalhos não são configurados por padrão.
- `statusCode`: um código de status HTTP válido. Se o conteúdo do corpo estiver presente, o padrão será `200 OK`. Se nenhum conteúdo do corpo estiver presente, o padrão será `204 Sem Conteúdo`.
- `body`: uma sequência que é texto simples, um objeto JSON ou matriz ou uma sequência codificada em base64 para dados binários. O corpo será considerado vazio se for `null`, a sequência vazia `""` ou indefinida. O padrão é um corpo vazio.

O controlador passa quaisquer cabeçalhos especificados pela ação, código de status ou corpo para o cliente HTTP que finaliza a solicitação ou resposta. Se o cabeçalho `Content-Type` não for declarado nos `headers` do resultado da ação, o corpo será interpretado como `application/json` para valores não de sequência e `text/html`, caso contrário. Se o cabeçalho `Content-Type` for definido, o controlador determinará se a resposta será texto simples ou dados binários e decodificará a sequência usando um decodificador de base64 conforme necessário. Se o corpo não for decodificado corretamente, um erro será retornado para o cliente.


## Contexto de HTTP
{: #http-context}

Todas as ações da web, quando chamadas, recebem detalhes da solicitação de HTTP como parâmetros para o argumento de entrada de ação.

Veja os parâmetros HTTP a seguir:

- `__ow_method` (tipo: sequência). O método de HTTP da solicitação.
- `__ow_headers` (tipo: mapear sequência para sequência): os cabeçalhos da solicitação.
- `__ow_path` (tipo: sequência): o caminho não correspondido da solicitação (a correspondência é parada assim que a extensão de ação é consumida).
- `__ow_user` (tipo: sequência): o Namespace que identifica o sujeito
autenticado pelo {{site.data.keyword.openwhisk_short}}
- `__ow_body` (tipo: sequência): a entidade de corpo da solicitação, como uma sequência codificada com base64 quando o conteúdo é binário ou, caso contrário, sequência simples
- `__ow_query` (tipo: sequência): os parâmetros de consulta da solicitação como uma sequência não analisada

Uma solicitação não pode substituir nenhum dos parâmetros nomeados `__ow_`. Fazer isso resulta em uma solicitação com falha com status igual a 400 Solicitação ruim.

O `__ow_user` estará presente apenas quando a ação da web for [anotada para requerer autenticação](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#annotations-specific-to-web-actions) e permitirá que uma ação da web implemente sua própria política de autorização. O `__ow_query` estará disponível apenas quando uma ação da web optar por manipular a [solicitação de HTTP "bruto"](#raw-http-handling). É uma sequência que contém os parâmetros de consulta que são analisados no URI (separados por `&`). A propriedade `__ow_body` está presente em solicitações de HTTP "brutas" ou quando a entidade de solicitação de HTTP não é um objeto JSON ou dados de formulário. Caso contrário, as ações da web recebem parâmetros de consulta e de corpo como propriedades de primeira classe no argumento da ação. Os parâmetros de corpo têm precedência sobre os parâmetros de consulta, que por sua vez têm precedência sobre os parâmetros de ação e pacote.

## Suporte ao terminal HTTPS

Protocolos SSL suportados: TLS 1.2, TLS 1.3 ([versão de rascunho 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Protocolos SSL não suportados: SSLv2, SSLv3, TLS 1.0, TLS 1.1

## Recursos extras
{: #extra-features}

As ações da web fornecem recursos extras que incluem:

- `Extensões de conteúdo`: a solicitação deve especificar seu tipo de conteúdo desejado como `.json`, `.html`, `.http`, `.svg` ou `.text`. O tipo é especificado incluindo uma extensão no nome da ação no URI para que uma ação `/guest/demo/hello` seja referenciada como `/guest/demo/hello.http`, por exemplo, para receber uma resposta de HTTP de volta. Por conveniência, a extensão `.http` é assumida quando nenhuma extensão é detectada.
- `Projetando campos do resultado`: o caminho que segue o nome da ação é usado para projetar um ou mais níveis da resposta.
`/guest/demo/hello.html/body`. Esse recurso permite que uma ação que retorna um dicionário `{body: "..." }` projete a propriedade `body` e retornar diretamente seu valor de sequência. O caminho projetado segue um modelo de caminho absoluto (como em XPath).
- `Parâmetros de consulta e corpo como entrada`: a ação recebe parâmetros de consulta, bem como parâmetros no corpo da solicitação. A ordem de precedência para mesclar parâmetros é: parâmetros de pacote, parâmetros de ação, parâmetro de consulta e parâmetros de corpo. Cada um desses parâmetros poderá substituir quaisquer valores anteriores se a sobreposição ocorrer. Como um exemplo, `/guest/demo/hello.http?name=Jane` pode passar o argumento `{name: "Jane"}` para a ação.
- `Dados de formulário`: além do `application/json` padrão, as ações da web podem receber URL codificada de dados `application/x-www-form-urlencoded data` como entrada.
- `Ativação usando múltiplos verbos de HTTP`: uma ação da web pode ser chamada por meio de qualquer um destes métodos de HTTP: `GET`, `POST`, `PUT`, `PATCH` e `DELETE`, bem como `HEAD` e `OPTIONS`.
- `Corpo não JSON e manipulação de entidade de HTTP bruto`: uma ação da web pode aceitar um corpo da solicitação de HTTP diferente de um objeto JSON e pode optar por sempre receber esses valores como valores opacos (texto simples quando não binário ou, caso contrário, sequência codificada com base64).

O exemplo abaixo esboça rapidamente como você pode usar esses recursos em uma ação da web. Considere uma ação `/guest/demo/hello` com o corpo a seguir:
```javascript
function main(params) {
    return { response: params };
}
```

Quando essa ação é chamada como uma ação da web, é possível alterar a resposta da ação da web projetando caminhos diferentes do resultado.

Por exemplo, para retornar o objeto inteiro e ver quais argumentos a ação recebe:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json
 ```
{: pre}

Exemplo de Saída:
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

Para executar com um parâmetro de consulta, veja o exemplo de comando a seguir:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json?name=Jane
 ```
{: pre}

Exemplo de Saída:
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

Também é possível executar com dados de formulário:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -d "name":"Jane"
 ```
{: pre}

Exemplo de Saída:
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

Execute o comando a seguir para um objeto JSON:
```
 curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
```
{: pre}

Exemplo de Saída:
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

Execute o comando a seguir para projetar a nome (como texto):
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.text/response/name?name=Jane
```
{: pre}

Exemplo de Saída:
```
Janete
```
{: screen}

Por conveniência, os parâmetros de consulta, dados de formulário e entidades do corpo de objeto JSON são todos tratados como dicionários e seus valores são diretamente acessíveis como propriedades de entrada de ação. Esse comportamento não é o caso para ações da web, que optam por manipular entidades de solicitação de HTTP mais diretamente, ou quando a ação da web recebe uma entidade que não é um objeto JSON.

Veja o exemplo a seguir que usa um tipo de conteúdo "texto", como foi mostrado anteriormente.
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
```
{: pre}

Exemplo de Saída:
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

## Extensões de Conteúdo
{: #openwhisk_webactions_extensions}

Uma extensão de conteúdo é geralmente necessária para chamar uma ação da web. A ausência de uma extensão assume `.http` como o padrão. As extensões `.json` e `.http` não requerem um caminho de projeção, enquanto as extensões `.html`, `.svg` e `.text` requerem. Por conveniência, o caminho padrão deve corresponder ao nome da extensão. Para chamar uma ação da web e receber uma resposta `.html`, a ação deve responder com um objeto JSON que contém uma propriedade de nível superior chamada `html` (ou a resposta deve estar no caminho explícito). Ou seja, `/guest/demo/hello.html` é equivalente a projetar a propriedade `html` explicitamente, como em `/guest/demo/hello.html/html`. O nome completo da ação deverá incluir seu nome do pacote, que é `default` se a ação não estiver em um pacote nomeado.

## Parâmetros protegidos
{: #openwhisk_webactions_protected}

Os parâmetros de ação são protegidos e tratados como imutáveis. Os parâmetros são finalizados automaticamente para ativar ações da web.
```
ibmcloud fn action create /guest/demo/hello hello.js --parameter name Jane --web true
```
{: pre}

O resultado dessas mudanças é que o `name` é ligado a `Jane` e não pode ser substituído por parâmetros de consulta ou corpo devido à anotação final. Esse design assegura a ação contra parâmetros de consulta ou corpo que tentam mudar esse valor, seja por acidente ou intencionalmente.

## Protegendo as ações da web
{: #securing-web-actions}

Por padrão, uma ação da web pode ser chamada por alguém que tem a URL de chamada da ação da web. Use o `require-whisk-auth`[web action annotation](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#annotations-specific-to-web-actions)para proteger a ação da web. Quando a anotação `require-whisk-auth` for configurada para `true`, a ação autenticará as credenciais de autorização básica da solicitação de chamada com relação à chave de aut. do whisk do proprietário da ação. Quando configurada para um número ou uma sequência com distinção entre maiúsculas e minúsculas, a solicitação de chamada da ação deve incluir um cabeçalho `X-Require-Whisk-Auth` tendo esse mesmo valor. As ações da web protegidas retornarão a mensagem `Not Authorized` quando a validação de credencial falhar.

Como alternativa, use a sinalização `--web-secure` para configurar automaticamente a anotação `require-whisk-auth`.  Quando configurada para `true`, um número aleatório é gerado como o valor de anotação `require-whisk-auth`. Quando configurada para `false`, a anotação `require-whisk-auth` é removida.  Quando configurada para qualquer outro valor, esse valor é usado como o valor de anotação `require-whisk-auth`.

Exemplo usando **--web-secure**:
```bash
ibmcloud fn action update /guest/demo/hello hello.js --web true --web-secure my-secret
```
{: pre}

Exemplo usando **require-whisk-auth**:
```bash
ibmcloud fn action update /guest/demo/hello hello.js --web true -a require-whisk-auth my-secret
```
{: pre}

Exemplo usando **X-Require-Whisk-Auth**:
```bash
curl https://${APIHOST}/api/v1/web/guest/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: my-secret"
```
{: pre}

É importante observar que o proprietário da ação da web possui todos os registros de ativações e incorre no custo de execução da ação no sistema independentemente de como a ação foi chamada.

## Desativando ações da web

Para desativar a chamada de uma ação da web por meio da API da web (`https://openwhisk.bluemix.net/api/v1/web/`), passe um valor de `false` ou `no` para a sinalização `--web` para atualizar uma ação com a CLI.
```
ibmcloud fn action update /guest/demo/hello hello.js --web false
```
{: pre}

## Manipulação de HTTP bruto

Uma ação da web pode optar por interpretar e processar um corpo HTTP recebido diretamente, sem a promoção de um objeto JSON para as propriedades de primeira classe disponíveis para a entrada de ação (por exemplo, `args.name` versus `args.__ow_query` de análise sintática). Esse processo é feito por meio de uma [anotação](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations) `raw-http`. Usando o mesmo exemplo mostrado anteriormente, mas agora como uma ação da web de HTTP "bruto" que recebe `name`, como um parâmetro de consulta e como um valor JSON no corpo da solicitação de HTTP:
```
curl https://us-south.functions.cloud.ibm.com/api/v1/web/guest/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}

Exemplo de Saída:
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

O OpenWhisk usa a estrutura [Akka Http](https://doc.akka.io/docs/akka-http/current/?language=scala) para [determinar](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html) quais tipos de conteúdo são binários e quais são texto sem formatação.

### Ativando a manipulação de HTTP bruto

As ações da web de HTTP bruto são ativadas por meio da sinalização `--web` usando um valor de `raw`.
```
ibmcloud fn action create /guest/demo/hello hello.js --web raw
```
{: pre}

### Desativando a manipulação de HTTP bruto

A desativação de HTTP bruto pode ser realizada transmitindo um valor de `false` ou
`no` para a sinalização `--web`.
```
ibmcloud fn update create /guest/demo/hello hello.js --web false
```
{: pre}

### Decodificando conteúdo do corpo binário do Base64

Quando o conteúdo de HTTP bruto é processado, o conteúdo `__ow_body` é codificado em Base64 quando a solicitação `Content-Type` é binária. As funções a seguir demonstram como decodificar o conteúdo do corpo no Node, Python e Swift. Simplesmente salve um método em um arquivo, crie uma ação da web de HTTP bruto que utilize o artefato salvo e, em seguida, chame a ação da web.

#### Node

```javascript
function main(args) {
    decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
    return {body: decoded}
}
```
{: codeblock}

#### Python

```python
def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
```
{: codeblock}

#### Swift

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

Como um exemplo, salve a função do Nó como `decode.js` e executar os seguintes comandos:
```
ibmcloud fn action create decode decode.js --web raw
```
{: pre}

Exemplo de Saída:
```
ok: created action decode
```
{: screen}

```
curl -k -H "content-type: application" -X POST -d "Decoded body" https:// us-south.functions.cloud.ibm.com/api/v1/web/guest/default/decodeNode.json
```
{: pre}

Exemplo de Saída:
```
{
  "body": "Decoded body"
}
```
{: screen}

## Solicitações de opções
{: #options-requests}

Por padrão, uma solicitação OPTIONS feita para uma ação da web resulta em cabeçalhos CORS que são incluídos automaticamente nos cabeçalhos de resposta. Esses cabeçalhos permitem todas as origens e os verbos HTTP options, get, delete, post, put, head e patch.

Veja os cabeçalhos a seguir:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Como alternativa, as solicitações OPTIONS podem ser manipuladas manualmente por uma ação da web. Para ativar essa opção, inclua uma
anotação `web-custom-options` com um valor de `true` para uma ação da web. Quando esse recurso é ativado, os cabeçalhos CORS não são incluídos automaticamente na resposta de solicitação. Em vez disso, é responsabilidade do desenvolvedor anexar seus
cabeçalhos desejados programaticamente.

Veja o exemplo a seguir para criar respostas customizadas para solicitações OPTIONS:
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

Salve a função em `custom-options.js` e execute os comandos a seguir:
```
ibmcloud fn action create custom-option custom-options.js --web true -a web-custom-options true
```
{: pre}

```
$ curl https://${APIHOST}/api/v1/web/guest/default/custom-options.http -kvX OPTIONS
```
{: pre}

Exemplo de Saída:
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
{: #openwhisk_webactions_errors}

Uma ação do {{site.data.keyword.openwhisk_short}} falha em dois diferentes modos de falha possíveis. O primeiro é conhecido como um _erro de aplicativo_ e é análogo a uma exceção de captura: a ação retorna um objeto JSON que contém uma propriedade `error` de nível superior. O segundo é um _erro de desenvolvedor_, que ocorre quando a ação falha catastroficamente e não produz uma resposta (semelhante a uma exceção de não captura). Para ações da web, o controlador manipula erros de aplicativo conforme a seguir:

- Qualquer projeção de caminho especificada é ignorada e o controlador projeta a propriedade `error` em seu lugar.
- O controlador aplica a manipulação de conteúdo que está implícita pela extensão de ação ao valor da propriedade `error`.

Os desenvolvedores devem saber como as ações da web podem ser usadas e geram respostas de erro apropriadas. Por exemplo, uma ação da web que é usada com a extensão `.http` retorna uma resposta de HTTP como `{error: { statusCode: 400 }`. A falha em fazer isso é uma incompatibilidade entre o `Content-Type` implícito na extensão e o `Content-Type` da ação na resposta de erro. Consideração especial deve ser dada a ações da web que são sequências para que os componentes que compõem uma sequência possam gerar erros adequados quando necessário.
