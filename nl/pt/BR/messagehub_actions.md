---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: message hub, package, messages, events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.messagehub}} pacote 

{: #catalog_message_hub}

Um pacote que permite a comunicação com instâncias do [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) para publicar e consumir mensagens usando a API nativa do Kafka de alto desempenho.
{: shortdesc}

## Configurando um pacote do {{site.data.keyword.messagehub}} usando o {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Crie uma instância do serviço {{site.data.keyword.messagehub}} em sua organização e espaço atuais que estejam sendo usados para o {{site.data.keyword.openwhisk}}.

2. Verifique se o tópico que você deseja receber está disponível no {{site.data.keyword.messagehub}} ou crie um novo tópico, por exemplo, chamado **mytopic**.

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

4. Liste os pacotes em seu namespace para mostrar que sua ligação de pacote está agora disponível.
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

## Configurando um pacote do {{site.data.keyword.messagehub}} fora do {{site.data.keyword.Bluemix_notm}}

Caso deseje configurar o {{site.data.keyword.messagehub}} fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para
o seu serviço {{site.data.keyword.messagehub}}. São necessárias as informações das credenciais de serviço e da conexão do
{{site.data.keyword.messagehub}}.

Crie uma ligação de pacote configurada para o seu serviço {{site.data.keyword.messagehub}}.
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Recebendo mensagens usando eventos

Para obter informações detalhadas sobre como usar acionadores no {{site.data.keyword.messagehub}}para detectar mensagens, consulte o seguinte
Tópico[{{site.data.keyword.messagehub}}origem de eventos](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub), em que as seguintes tarefas são abordadas:
* [Criando um acionador que atenda a uma instância do {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger)
* [Criando um acionador para um pacote do {{site.data.keyword.messagehub}} fora do {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger_outside)
* [Atendendo mensagens](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#message_hub_listen)
* [Exemplos](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#examples)

## Produzindo mensagens para o {{site.data.keyword.messagehub}}
{: #producing_messages}

A ação `/messaging/messageHubProduce` foi descontinuada e será removida em uma data futura. Ela já foi removida na região de Tóquio. Para manter o desempenho ideal, migre seu uso da ação `/messaging/messageHubProduce` para usar uma conexão persistente quando os dados são produzidos para o {{site.data.keyword.messagehub}}/Kafka.
{: tip}

Para saber mais sobre a produção de mensagens, confira a [documentação do Event Streams](/docs/services/EventStreams?topic=eventstreams-producing_messages#producing_messages).

## Referência
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)
- [Apache Kafka](https://kafka.apache.org)
