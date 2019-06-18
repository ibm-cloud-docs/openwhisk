---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: triggers, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Criando acionadores para eventos
{: #triggers}

Um acionador é uma declaração de que você deseja reagir a um determinado tipo de evento, seja de um usuário ou por uma origem de eventos.
{: shortdesc}

A seguir estão exemplos de acionadores.
- Eventos de atualização de local
- Uploads de documentos para um website
- E-mails recebidos



## Criando acionadores por meio da CLI
{: #triggers_create}


1. Crie o acionador. Os acionadores devem ser criados diretamente dentro de um namespace e não podem
ser criados dentro de pacotes.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Verifique se o acionador foi criado.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Exemplo de Saída:
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



Em seguida, é possível [testar o acionador](/docs/openwhisk?topic=cloud-functions-test#test_triggers) ou [criar uma regra](/docs/openwhisk?topic=cloud-functions-rules) para associar o acionador a uma ação.



## Diferença entre feed e acionador
{: #triggers_difference}

Feeds e acionadores estão intimamente relacionados, mas tecnicamente têm conceitos distintos.

- O {{site.data.keyword.openwhisk_short}} processa **eventos** que fluem para o sistema.

- Um **acionador** é um nome para uma classe de eventos. Cada evento pertence a exatamente um acionador; por analogia, um acionador é semelhante a um tópico em sistemas pub-sub baseados em tópico. Uma **regra** significa que sempre que um evento do acionador chegar, chame a ação com a carga útil do acionador.

- Um **feed** é uma maneira conveniente de configurar uma origem de eventos externos para disparar eventos acionadores que podem ser consumidos pelo {{site.data.keyword.openwhisk_short}}. Um feed é um fluxo de eventos que todos pertencem a algum acionador. Os pacotes pré-instalados, os pacotes instaláveis e os seus próprios pacotes customizados podem conter feeds. Um feed é controlado por uma **ação de feed**, que manipula a criação, a exclusão, a pausa e a retomada do fluxo de eventos que compõem um feed. A ação de feed geralmente interage com serviços externos que produzem os eventos usando uma API de REST que gerencia notificações.

Exemplos de feeds:
- Um feed de mudança de dados do {{site.data.keyword.cloudant}} que dispara um evento
acionador toda vez que um documento é incluído ou modificado em um banco de dados
- Um feed do Git que dispara um evento acionador para cada confirmação em um repositório Git



## Criando um acionador para um feed
{: #triggers_feeds}

Esse exemplo mostra como usar um feed no pacote de Alarmes para disparar um acionador uma vez por minuto e como usar uma regra para chamar uma ação uma vez por minuto.

1. Obtenha uma lista de descrição das entidades no pacote `/whisk.system/alarms`.

    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    Exemplo de Saída:
    ```
    pacote /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. Obtenha uma descrição do feed no pacote `/whisk.system/alarms` para ver os parâmetros que podem ser usados.

  ```
  ibmcloud fn action get -- summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Exemplo de Saída:
  ```
  action /whisk.system/alarms/alarm: disparar acionador quando o alarme ocorrer
     (parâmetros: cron trigger_payload)
  ```
  {: screen}

  O feed  ` /whisk.system/alarms/alarm `  usa dois parâmetros:
  - `cron`: uma especificação de crontab de quando disparar o acionador.
  - `trigger_payload`: o valor de parâmetro de payload para configurar em cada evento acionador.

2. Crie um acionador que dispare a cada minuto.
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. Crie um app. Exemplo `hello.js`:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Crie uma ação.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. Crie uma regra que chame a ação `hello` toda vez que o acionador `everyOneMinute` for disparado.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: regra myRule criada
  ```
  {: screen}

6. Verifique se a ação está sendo chamada pesquisando os logs de ativação.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  É possível ver que as ativações ocorrem a cada minuto para o acionador, a regra e a ação. A ação recebe os parâmetros `{"name":"Mork", "place":"Ork"}` em cada chamada.


