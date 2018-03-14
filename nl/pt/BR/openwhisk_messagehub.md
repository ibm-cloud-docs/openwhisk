---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote do Hub de mensagens
{: #openwhisk_catalog_message_hub}

Um pacote que permite a comunicação com instâncias do [Message Hub](https://developer.ibm.com/messaging/message-hub) para publicar e consumir mensagens usando a API nativa do Kafka de alto desempenho.
{: shortdesc}

## Criando um Acionador que atende a uma instância do IBM MessageHub
{: #openwhisk_catalog_message_hub_trigger}

Para criar um acionador que reaja quando as mensagens forem postadas em uma instância do Message Hub, será necessário usar o feed denominado `/messaging/messageHubFeed`. Essa ação de feed suporta os parâmetros a seguir:

|Nome|Digite|Descrição|
|---|---|---|
|kafka_brokers_sasl|Matriz JSON de sequência de caracteres|Esse parâmetro é uma matriz de `<host>:<port>` sequências que compõem os brokers na instância do Hub de mensagens|
|usuário|Sequência|Nome do usuário do Message Hub|
|Senha|Sequência|Senha do Message Hub|
|tópico|Sequência|O tópico ao qual gostaria que o Acionador atendesse|
|kafka_admin_url|Sequência URL|A URL da interface REST do administrador do Message Hub|
|isJSONData|Booleano (Opcional - padrão=false)|Quando configurado para `true`, o provedor tenta analisar o valor da mensagem como JSON antes de passá-lo adiante como a carga útil do acionador.|
|isBinaryKey|Booleano (Opcional - padrão=false)|Quando configurado para `true`, o provedor codifica o valor da chave como Base64 antes de passá-lo adiante como a carga útil do acionador.|
|isBinaryValue|Booleano (Opcional - padrão=false)|Quando configurado para `true`, o provedor codifica o valor da mensagem como Base64 antes de passá-lo adiante como a carga útil do acionador.|

Embora essa lista de parâmetros possa parecer assustadora, ela pode ser configurada automaticamente para você usando o comando de atualização de pacote da CLI:

1. Crie uma instância do serviço Message Hub em sua organização e espaço atuais que você está usando para o OpenWhisk.

2. Verifique se o tópico que você deseja receber está disponível no Message Hub ou crie um novo tópico, por exemplo, `mytopic`.

3. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Message Hub que você criou.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```

  Sua ligação de pacote agora contém as credenciais que estão associadas à instância do Message Hub.

4. Agora tudo o que você precisa fazer é criar um acionador que será disparado quando novas mensagens forem postadas em seu tópico do Message Hub.

  ```
  wsk trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Configurando um pacote do Message Hub fora do {{site.data.keyword.Bluemix_notm}}

Se você deseja configurar o Message Hub fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para o serviço Message Hub. Você precisa das credenciais de serviço e das informações de conexão do Message Hub.

1. Crie uma ligação de pacote que esteja configurada para o serviço Message Hub.

  ```
  wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your Message Hub user> -p password <your Message Hub password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Agora é possível criar um acionador usando seu novo pacote que será disparado quando novas mensagens forem postadas em seu tópico do Message Hub.

  ```
  wsk trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}


## Atendendo mensagens
{: #openwhisk_catalog_message_hub_listen}

Quando um acionador é criado, o sistema monitora o tópico especificado em seu serviço de sistema de mensagens. Quando novas mensagens são postadas, o acionador é disparado.

A carga útil desse acionador contém um campo `messages`, que é uma matriz de mensagens que foram postadas na última vez em que o acionador foi disparado. Cada objeto de mensagem na matriz contém os campos a seguir:
- tópico
- Partição
- deslocamento
- Chave
- valor

Em termos do Kafka, os campos são autoevidentes. No entanto, `key` tem um recurso chamado `isBinaryKey` que permite que o `key` transmita dados binários. Além disso, o `value` requer consideração especial. Os campos `isJSONData` e `isBinaryValue` estão disponíveis para manipular mensagens JSON e binárias. Estes campos, `isJSONData` e `isBinaryValue`, não podem ser usados juntos.

Como um exemplo, se `isBinaryKey` foi configurado para `true` quando o acionador foi criado, o `key` será codificado como uma sequência Base64 quando retornado da carga útil de um acionador disparado.

Se um `key` de `Some key` é postado com `isBinaryKey` configurado para `true`, a carga útil do acionador é semelhante ao exemplo a seguir:

```json
{
    "messages": [
    {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```

Se o parâmetro `isJSONData` foi configurado para `false` (ou não configurado) quando o acionador foi criado, o campo `value` será o valor bruto da mensagem postada. No entanto, se `isJSONData` foi configurado para `true` quando o acionador foi criado, o sistema tentará analisar esse valor como um objeto JSON, em uma base de melhor esforço. Se a análise for bem-sucedida, o `value` na carga útil do acionador será o objeto JSON resultante.

Se uma mensagem de `{"title": "Some string", "amount": 5, "isAwesome": true}` for postada com `isJSONData` configurado para `true`, a carga útil do acionador poderá ser semelhante ao exemplo a seguir:

```json
{
  "messages": [
    {
      "partition": 0,
        "key": null,
        "offset": 421760,
        "topic": "mytopic",
        "value": {
          "amount": 5,
            "isAwesome": true,
            "title": "Some string"
        }
    }
  ]
}
```

No entanto, se o mesmo conteúdo da mensagem fosse postado com `isJSONData` configurado para `false`, a carga útil do acionador seria semelhante ao exemplo a seguir:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```

Semelhante a `isJSONData`, se `isBinaryValue` foi configurado para `true` durante a criação do acionador, o `value` resultante na carga útil do acionador será codificado como uma sequência Base64.

Se um `value` de `Some data` é postado com `isBinaryValue` configurado para `true`, a carga útil do acionador pode ser semelhante ao exemplo a seguir:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```

Se a mesma mensagem fosse postada sem `isBinaryData` configurado para `true`, a carga útil do acionador seria semelhante ao exemplo a seguir:

```json
{
  "messages": [
    {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```

### As mensagens são processadas em lote
Observe que a carga útil do acionador contém uma matriz de mensagens. Se essas mensagens forem produzidas em seu sistema de mensagens rapidamente, o feed tentará em lote as mensagens postadas em um único disparo do acionador. O processamento em lote permite que as mensagens sejam postadas para seu acionador de forma mais rápida e eficiente.

Tenha em mente quando codificar as ações que são disparadas por seu acionador que o número de mensagens na carga útil é tecnicamente sem limites, mas é sempre maior que 0. Veja o exemplo a seguir de uma mensagem em lote (observe a mudança no valor *offset*):
 
 ```json
 {
   "messages": [
    {
         "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
             "amount": 5
         }
       },
       {
         "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
             "amount": 1
         }
       },
       {
         "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
             "amount": 999
         }
       }
   ]
 }
 ```

## Produzindo mensagens para o Message Hub
Se você desejar usar que uma ação do OpenWhisk produza de forma conveniente uma mensagem para o Message Hub, será possível usar a ação `/messaging/messageHubProduce`. Essa ação usa os parâmetros a seguir:

|Nome|Digite|Descrição|
|---|---|---|
|kafka_brokers_sasl|Matriz JSON de sequência de caracteres|Esse parâmetro é uma matriz de `<host>:<port>` sequências que compõem os brokers na instância do Hub de mensagens|
|usuário|Sequência|Nome do usuário do Message Hub|
|Senha|Sequência|Senha do Message Hub|
|tópico|Sequência|O tópico ao qual gostaria que o Acionador atendesse|
|valor|Sequência|O valor para a mensagem que você gostaria de produzir|
|Chave|Sequência (opcional)|A chave para a mensagem que você gostaria de produzir|

Embora os três primeiros parâmetros possam ser ligados automaticamente usando `wsk package refresh`, veja o exemplo a seguir que chama a ação com todos os parâmetros necessários:

```
wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your Message Hub user> -p password <your Message Hub password> -p value "This is the content of my message"
```
{: pre}

## Exemplos

### Integrando o OpenWhisk ao IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage e IBM Data Science Experience
O exemplo que integra o OpenWhisk ao serviço IBM Message Hub, Node Red, IBM Watson IoT, IBM Object Storage, IBM Data Science Experience (Spark) pode ser [localizado aqui](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## Referência
- [IBM Message Hub](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
