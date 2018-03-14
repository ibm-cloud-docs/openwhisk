---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando as APIs de REST do OpenWhisk
{: #openwhisk_rest_api}

Depois que o ambiente do OpenWhisk for ativado, será possível usar o OpenWhisk com seus apps da web ou apps móveis com as chamadas API de REST.
{: shortdesc}

Para obter mais informações sobre as APIs para ações, ativações, pacotes, regras e acionadores, veja a [documentação da API do OpenWhisk](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json).


Todos os recursos no sistema estão disponíveis por meio de uma API REST. Os terminais de coleção e entidade estão disponíveis para ações, acionadores, regras, pacotes, ativações e namespaces.

Terminais de coleção disponíveis:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

O `{APIHOST}` é o nome do host da API do OpenWhisk (por exemplo, openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13 e assim por diante).
Para o `{namespace}`, o caractere `_` pode ser usado para especificar o *namespace padrão* do
usuário.

É possível executar uma solicitação GET nos terminais de coleção para buscar uma
lista de entidades na coleção.

Os terminais de entidade a seguir estão disponíveis para cada tipo de entidade:
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

Os terminais de namespace e ativação suportam solicitações de GET. Os terminais de ações, acionadores, regras e pacotes suportam solicitações de GET, PUT e DELETE. Os terminais de ações, acionadores e regras também suportam solicitações de POST, que são usadas para chamar ações e acionadores e ativar ou desativar regras. 

Todas as APIs são protegidas com autenticação Básica de HTTP. 
É possível usar a ferramenta [wskadmin ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) para gerar um novo namespace e autenticação.
As credenciais de
autenticação Básica estão na propriedade `AUTH` em seu arquivo
`~/.wskprops`, delimitadas por dois pontos. 
Também é possível recuperar essas credenciais usando a CLI executando `wsk property get --auth`.


No exemplo a seguir, a ferramenta de comando [cURL](https://curl.haxx.se) é usada para obter a lista de todos os pacotes no namespace `whisk.system`:
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

Neste exemplo, a autenticação foi passada usando a sinalização `-u`. Também é possível passar esse valor como parte da URL, como `https://$AUTH@{APIHOST}`.

A API OpenWhisk suporta chamadas de solicitação-resposta de Web clients. O OpenWhisk responde para solicitações de `OPTIONS` com cabeçalhos de Compartilhamento de Recurso de Origem
Cruzada. Atualmente, todas as origens são permitidas (ou seja, a Origem de Permissão de
Controle de Acesso é "`*`") e os Cabeçalhos de Permissão de Controle de
Acesso produzem Autorização e Tipo de Conteúdo.

**Atenção:** como o OpenWhisk suporta atualmente somente uma chave por namespace, não é recomendado usar CORS além de experimentos simples. Use [Ações da web](./openwhisk_webactions.html) ou [API Gateway](./openwhisk_apigateway.html) para expor suas ações ao público e não use a chave de autorização do OpenWhisk para aplicativos clientes que requeiram CORS.

## Usando o modo detalhado da CLI
{: #openwhisk_rest_api_cli_v}

A CLI do OpenWhisk é uma interface para a API de REST do OpenWhisk.
É possível executar a CLI em modo detalhado com a sinalização `-v`, que imprime todas as informações sobre a solicitação e a resposta de HTTP.

Exiba o valor de namespace para o usuário atual executando o comando a seguir:
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

As informações impressas fornecem as propriedades da solicitação de HTTP e executa um método de HTTP `GET` na URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` usando um cabeçalho do agente do usuário `OpenWhisk-CLI/1.0 (<CLI-Build-version>)` e um cabeçalho de Autorização básica `Basic XXXYYYY`.
Observe que o valor de autorização é a sequência de autorização codificada com base64 do OpenWhisk.
A resposta é do tipo de conteúdo `application/json`.

## Ações
{: #openwhisk_rest_api_actions}

Para criar ou atualizar uma ação, envie uma solicitação de HTTP com o método `PUT` na coleção de ações. Por exemplo, para criar uma ação `nodejs:6` com o nome `hello` usando um único conteúdo de arquivo, use o comando a seguir:
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

Para executar uma chamada de bloqueio em uma ação, envie uma solicitação de HTTP com um método `POST` e corpo que contenha o parâmetro de entrada `name`, use o comando a seguir:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

Você obtém a seguinte resposta:
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
Para obter o `response.result`, execute o comando novamente com o parâmetro de consulta `result=true` como no exemplo a seguir:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
Você obtém a seguinte resposta:
```json
{
  "payload": "hello John"
}
```

## Anotações e ações da web
{: #openwhisk_rest_api_webactions}

Para criar uma ação como uma ação da web, é necessário incluir uma [anotação](./openwhisk_annotations.html) de `web-export=true` para ações da web. Como as ações da web são publicamente acessíveis, você deseja proteger os parâmetros predefinidos (ou seja, tratá-los como finais) usando a anotação `final=true`. Se você criar ou atualizar uma ação que usa a sinalização da CLI `--web true`, o comando incluirá ambas as anotações `web-web-export=true` e `final=true`.

Execute o comando curl a seguir para fornecer a lista completa de anotações para configurar na ação.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

Agora é possível chamar essa ação como uma URL pública sem autorização do OpenWhisk. Tente chamar usando a URL pública da ação e incluindo uma extensão como `.json` ou `.http`, por exemplo, no final da URL.
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

Este código-fonte de exemplo não funciona com `.http`, veja a documentação [Ações da web](./openwhisk_webactions.html) sobre como modificar.

## Sequências
{: #openwhisk_rest_api_sequences}

Para criar uma sequência de ações, forneça os nomes das ações que compõem a sequência na ordem desejada. Dessa forma, a saída da primeira ação é passada como entrada para a próxima ação.

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

Crie uma sequência com as ações `/whisk.system/utils/split` e `/whisk.system/utils/sort`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

Quando você especifica os nomes das ações, eles devem ser completamente qualificados.

## disparos
{: #openwhisk_rest_api_triggers}

Para criar um acionador, as informações mínimas necessárias é um nome para o acionador. Também seria possível incluir parâmetros padrão que são passados para a ação por meio de um Tule quando o acionador é disparado.

Crie um acionador com o nome `events` com um parâmetro padrão `type` com o valor `webhook` configurado.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

Agora, sempre que você tem um evento que precise disparar esse acionador, ele apenas toma uma solicitação de HTTP com um método `POST` usando a chave de autorização do OpenWhisk.

Para disparar o acionador `events` com um parâmetro `temperature`, envie a solicitação de HTTP a seguir.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### Acionadores com ações de feed
{: #openwhisk_rest_api_triggers_feed}

Os acionadores especiais podem ser criados usando uma ação de feed. A ação de feed é uma ação que ajuda com a configuração de um provedor de feed que é responsável por disparar o acionador sempre que um evento para o acionador ocorre. Saiba mais sobre esses provedores de feed na documentação [feeds.md].

Alguns dos acionadores disponíveis que alavancam uma ação de feed são periódicos/alarmes, Slack, Github, Cloudant/Couchdb e messageHub/Kafka. Também é possível criar sua própria ação de feed e provedor de feed.

Crie um acionador com o nome `periodic` para ser disparado em uma frequência especificada, a cada 2 horas (por exemplo, 2h, 4h,...).

Usando a CLI, execute o comando a seguir para criar o acionador:
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

Como a sinalização `-v` sinalizador é usada, duas solicitações de HTTP são enviadas. Uma é criar um acionador que é chamado `periodic` e a outra é chamar a ação de feed. A ação de feed, `/whisk.system/alarms/alarm`, envia os parâmetros para configurar o provedor de feed para disparar o acionador a cada 2 horas.

Para alcançar essa atividade com a API de REST, crie o acionador primeiro assim:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

Como é possível ver, a anotação `Feed` é armazenada no acionador. Posteriormente, essa anotação pode ser usada para saber qual ação de feed deve ser usada para excluir o acionador.

Agora que o acionador foi criado, vamos chamar a ação de feed
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

Excluir o acionador é semelhante a criar o acionador. Então, desta vez, exclua o acionador usando a ação de feed para configurar o provedor de feed para também excluir o manipulador para o acionador.

Chame a ação de feed para excluir o manipulador do acionador do provedor de feed, executando o comando a seguir:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

Agora exclua o acionador com uma solicitação de HTTP usando o método `DELETE`:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## Regras
{: #openwhisk_rest_api_rules}

Para criar uma regra que associa um acionador com uma ação, envie uma solicitação de HTTP com um método `PUT` para fornecer o acionador e a ação no corpo da solicitação.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

As regras podem ser ativadas ou desativadas e é possível mudar o status da regra atualizando sua propriedade de status. Por exemplo, para desativar a regra `t2a`, envie no corpo da solicitação `status: "inactive"` um método `POST`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## Pacotes
{: #openwhisk_rest_api_packages}

Para criar uma ação em um pacote, você precisa criar um pacote primeiro. Crie um pacote com o nome `iot` e envie uma solicitação de HTTP com um método `PUT` executando o comando a seguir:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## Ativações
{: #openwhisk_rest_api_activations}

Para obter a lista das últimas três ativações, use uma solicitação de HTTP com um método `GET` passando o parâmetro de consulta `limit=3` assim:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

Para obter todos os detalhes de uma ativação que incluem resultados e logs, envie uma solicitação de HTTP com um método `GET` passando o identificador de ativação como um parâmetro de caminho assim:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
