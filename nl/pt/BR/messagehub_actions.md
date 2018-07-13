---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}}
{: #openwhisk_catalog_message_hub}

Um pacote que permite a comunicação com instâncias do [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) para publicar e consumir mensagens usando a API nativa do Kafka de alto desempenho.
{: shortdesc}

## Configurando um pacote do {{site.data.keyword.messagehub}} usando o {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Crie uma instância do serviço {{site.data.keyword.messagehub}} em sua organização e espaço atuais que estejam sendo usados para o {{site.data.keyword.openwhisk}}.

2. Verifique se o tópico que você deseja receber está disponível no {{site.data.keyword.messagehub}} ou crie um novo tópico, por exemplo, chamado **mytopic**.

3. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço {{site.data.keyword.messagehub}} criada.
  ```
  ibmcloud wsk package refresh
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
  ibmcloud wsk package list
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
ibmcloud wsk package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Recebendo mensagens usando eventos

Para obter informações detalhadas sobre como usar acionadores no {{site.data.keyword.messagehub}} para receber mensagens, veja o tópico
[Origem de eventos do {{site.data.keyword.messagehub}}](./openwhisk_messagehub.html) no qual as tarefas a seguir são cobertas:
* [Criando um acionador que receba em uma instância do {{site.data.keyword.messagehub}}](./openwhisk_messagehub.html#create_message_hub_trigger)
* [Criando um acionador para um pacote do {{site.data.keyword.messagehub}} fora do {{site.data.keyword.Bluemix_notm}}](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [Atendendo mensagens](./openwhisk_messagehub.html#message_hub_listen)
* [Exemplos](./openwhisk_messagehub.html#examples)

## Produzindo mensagens para o {{site.data.keyword.messagehub}}
{: #producing_messages}

A ação `/messaging/messageHubProduce` foi descontinuada e será removida em uma data futura. Para manter o desempenho ideal, migre seu uso da ação `/messaging/messageHubProduce` para usar uma conexão persistente quando os dados são produzidos para o {{site.data.keyword.messagehub}}/Kafka.
{: tip}

Se você deseja usar uma ação do {{site.data.keyword.openwhisk_short}} para produzir de forma conveniente uma mensagem para o {{site.data.keyword.messagehub}}, é possível usar a ação `/messaging/messageHubProduce`. Essa ação usa os parâmetros a seguir:

|Nome|Digite|Descrição|
|---|---|---|
|kafka_brokers_sasl|Matriz JSON de sequência de caracteres|Esse parâmetro é uma matriz de sequências `<host>:<port>` que compõem os brokers em sua instância do {{site.data.keyword.messagehub}}.|
|usuário|Sequência|Seu nome do usuário do {{site.data.keyword.messagehub}}.|
|Senha|Sequência|Sua senha do {{site.data.keyword.messagehub}}.|
|tópico|Sequência|O tópico que você gostaria que o acionador atendesse.|
|valor|Sequência|O valor para a mensagem que você gostaria de produzir.|
|Chave|Sequência (opcional)|A chave para a mensagem que você gostaria de produzir.|

Embora os três primeiros parâmetros possam ser ligados automaticamente usando `ibmcloud wsk package refresh`, veja o exemplo a seguir que chama a ação com todos os parâmetros necessários:
```
ibmcloud wsk action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## Referência
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
