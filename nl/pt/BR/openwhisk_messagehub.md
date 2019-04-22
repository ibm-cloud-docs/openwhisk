---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-19"

keywords: message hub, event, trigger, messages, batch, listen

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Origem de Eventos do Streams de Eventos
{: #openwhisk_catalog_message_hub}

É possível criar um acionador que reaja quando as mensagens forem postadas em uma instância do {{site.data.keyword.messagehub_full}} usando feeds. Saiba como criar acionadores do {{site.data.keyword.messagehub}} com ou sem o {{site.data.keyword.Bluemix}}, detectar mensagens e manipular mensagens em lote.
{: shortdesc}

## {{site.data.keyword.messagehub}} pacote 

A ação `/messaging/messageHubProduce` foi descontinuada e será removida em uma data futura. Ela já foi removida na região de Tóquio. Para manter o desempenho ideal, migre seu uso da ação `/messaging/messageHubProduce` para usar uma conexão persistente quando dados forem produzidos para o Message Hub/Kafka.
{: deprecated}

Esse pacote permite a comunicação com instâncias do [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) para publicar e consumir mensagens usando a API nativa do Kafka de alto desempenho. Para obter mais informações sobre o pacote do {{site.data.keyword.messagehub}}, como configurá-lo e como produzir mensagens, consulte o tópico [pacote do {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-catalog_message_hub).

## Criando um acionador que atenda a uma instância do {{site.data.keyword.messagehub}}
{: #create_message_hub_trigger}

Para criar um acionador que reaja quando as mensagens são postadas em uma instância do
{{site.data.keyword.messagehub}}, é necessário usar o feed que é denominado
`/messaging/messageHubFeed`. A ação de feed suporta os parâmetros a seguir:

|Nome|Digite|Descrição|
|---|---|---|
|kafka_brokers_sasl|Matriz JSON de sequência de caracteres|Esse parâmetro é uma matriz de sequências `<host>:<port>` que compõem os brokers em sua instância do {{site.data.keyword.messagehub}}|
|usuário|Sequência|Seu nome do usuário do {{site.data.keyword.messagehub}}.|
|Senha|Sequência|Sua senha do {{site.data.keyword.messagehub}}.|
|tópico|Sequência|O tópico que você gostaria que o acionador atendesse.|
|kafka_admin_url|Sequência URL|A URL da interface REST de administrador do {{site.data.keyword.messagehub}}.|
|isJSONData|Booleano (Opcional - padrão=false)|Quando configurado para `true`, o provedor tenta analisar o valor da mensagem como JSON antes de passá-lo adiante como a carga útil do acionador.|
|isBinaryKey|Booleano (Opcional - padrão=false)|Quando configurado para `true`, o provedor codifica o valor da chave como Base64 antes de passá-lo adiante como a carga útil do acionador.|
|isBinaryValue|Booleano (Opcional - padrão=false)|Quando configurado para `true`, o provedor codifica o valor da mensagem como Base64 antes de passá-lo adiante como a carga útil do acionador.|

Embora essa lista de parâmetros possa parecer assustadora, eles podem ser configurados automaticamente
usando o comando de plug-in da CLI `ibmcloud fn package refresh`.

1. Crie uma instância do serviço {{site.data.keyword.messagehub}} em sua organização e espaço atuais que estejam sendo usados para o {{site.data.keyword.openwhisk}}.

2. Verifique se o tópico que você deseja receber está disponível no {{site.data.keyword.messagehub}} ou crie um novo tópico, por exemplo, **mytopic**.

3. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço {{site.data.keyword.messagehub}} criada.
  ```
  Atualização do pacote ibmcloud fn
  ```
  {: pre}

  Exemplo de Saída:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. Liste os pacotes em seu namespace para mostrar que sua ligação de pacote agora está disponível.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Sua ligação de pacote agora contém as credenciais que estão associadas à sua instância
do {{site.data.keyword.messagehub}}.

5. Agora tudo o que você precisa fazer é criar um acionador que é disparado quando novas mensagens são postadas em seu tópico do {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Criando um acionador para um pacote do {{site.data.keyword.messagehub}} fora do {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_trigger_outside}

Caso deseje configurar o {{site.data.keyword.messagehub}} fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para
o seu serviço {{site.data.keyword.messagehub}}. São necessárias as informações das credenciais de serviço e da conexão do
{{site.data.keyword.messagehub}}.

1. Crie uma ligação de pacote configurada para o seu serviço {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Agora é possível criar um acionador usando seu novo pacote que é disparado quando novas mensagens são postadas em seu tópico do {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## Atendendo mensagens
{: #message_hub_listen}

Quando um acionador é criado, o sistema monitora o tópico especificado em seu serviço de sistema de mensagens. Quando novas mensagens são postadas, o acionador é disparado.

A carga útil desse acionador contém um campo `messages`, que é uma matriz de mensagens que são postadas da última vez que o acionador é disparado. Cada objeto de mensagem na matriz contém os campos a seguir:
- tópico
- Partição
- deslocamento
- Chave
- valor

Em termos do Kafka, os campos são autoevidentes. No entanto, `key` tem um recurso chamado `isBinaryKey` que permite que o `key` transmita dados binários. Além disso, o `value` requer consideração especial. Os campos `isJSONData` e `isBinaryValue` estão disponíveis para manipular mensagens JSON e binárias. Estes campos, `isJSONData` e `isBinaryValue`, não podem ser usados juntos.

Como um exemplo, se `isBinaryKey` foi configurado para `true` quando o acionador foi criado, o `key` é codificado como uma sequência Base64 quando retornado da carga útil de um acionador disparado.

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
{: codeblock}

Se o parâmetro `isJSONData` foi configurado para `false` (ou não configurado) quando o acionador foi criado, o campo `value` é o valor bruto da mensagem postada. No entanto, se `isJSONData` foi configurado para `true` quando o acionador foi criado, o sistema tentará analisar esse valor como um objeto JSON, em uma base de melhor esforço. Se a análise for bem-sucedida, o `value` na carga útil do acionador será o objeto JSON resultante.

Se uma mensagem de `{"title": "Some string", "amount": 5, "isAwesome": true}` é postada com `isJSONData` configurado para `true`, a carga útil do acionador pode ser semelhante ao exemplo a seguir:
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
{: codeblock}

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
{: codeblock}

Semelhante a `isJSONData`, se `isBinaryValue` foi configurado para `true` durante a criação do acionador, o `value` resultante na carga útil do acionador é codificado como uma sequência Base64.

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
{: codeblock}

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
{: codeblock}

### As mensagens são processadas em lote
Observe que a carga útil do acionador contém uma matriz de mensagens. Se essas mensagens forem
produzidas para seu sistema de mensagens rapidamente, o feed tentará criar lotes das mensagens postadas em um
único disparo de seu acionador. O processamento em lote permite que as mensagens sejam postadas para seu acionador de forma mais rápida e eficiente.

Tenha em mente ao codificar ações que são disparadas por seu acionador que o número de mensagens na carga útil é tecnicamente sem limites, mas é sempre maior que 0. Veja o exemplo a seguir de uma mensagem em lote (observe a mudança no valor *offset*):
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

## Exemplos
{: #examples}

### Integrando o OpenWhisk ao {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full_notm}} e IBM Data Science Experience
Um exemplo que integra o OpenWhisk ao serviço {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full}}, IBM Data Science Experience (Spark) pode ser [localizado aqui](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0).

## Referência
{: #message_references}
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org)
