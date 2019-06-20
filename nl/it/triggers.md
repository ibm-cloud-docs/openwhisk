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


# Creazione di trigger per eventi
{: #triggers}

Un trigger è una dichiarazione da te scelta per reagire a un determinato tipo di evento, sia che provenga da un utente che da un'origine eventi.
{: shortdesc}

I seguenti sono esempi di trigger.
- Eventi di aggiornamento dell'ubicazione
- Caricamenti di documenti in un sito Web
- E-mail in entrata



## Creazione di trigger dalla CLI
{: #triggers_create}


1. Crea il trigger. I trigger devono essere creati direttamente all'interno di uno spazio dei nomi e non possono essere creati all'interno di pacchetti.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    Output di esempio:
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Verifica che il trigger venga creato.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Output di esempio:
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



Successivamente puoi [testare il trigger](/docs/openwhisk?topic=cloud-functions-test#test_triggers) o [creare una regola](/docs/openwhisk?topic=cloud-functions-rules) per associare il trigger a un'azione.



## Differenza tra feed e trigger
{: #triggers_difference}

I feed e i trigger sono strettamente correlati, ma restano due concetti tecnicamente distinti.

- {{site.data.keyword.openwhisk_short}} elabora gli **eventi** che fluiscono nel sistema.

- Un **trigger** è un nome per una classe di eventi. Ogni evento appartiene a un unico trigger; per analogia, un trigger assomiglia a un argomento nei sistemi di pubblicazione/sottoscrizione basati su argomenti. Una **regola** indica che ogni volta che arriva un evento da un trigger, deve essere richiamata l'azione con il payload del trigger.

- Un **feed** rappresenta un modo utile per configurare un'origine eventi esterna
per l'attivazione di eventi trigger che possono essere utilizzati da {{site.data.keyword.openwhisk_short}}. Un feed è un flusso di eventi che appartengono tutti allo stesso trigger. I feed possono essere contenuti in pacchetti preinstallati, pacchetti installabili e nei tuoi pacchetti personalizzati.  Un feed è controllato da un'**azione feed**, che gestisce la creazione, l'eliminazione, la pausa e il ripristino del flusso di eventi che formano un feed. L'azione di feed in genere interagisce con i servizi esterni che generano gli eventi, utilizzando un'API REST che gestisce le notifiche.

Esempi di feed:
- Un feed di modifica dati {{site.data.keyword.cloudant}} che attiva un evento trigger ogni volta che si aggiunge o modifica un documento in un database
- Un feed Git che attiva un evento trigger per ogni commit a un repository Git



## Creazione di un trigger per un feed
{: #triggers_feeds}

Questo esempio mostra come utilizzare un feed nel pacchetto Allarmi per attivare un trigger e come utilizzare una regola per chiamare un'azione una volta al minuto.

1. Ottieni un elenco di descrizioni delle entità del pacchetto `/whisk.system/alarms`.

    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    Output di esempio:
    ```
    package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. Ottieni una descrizione del feed del pacchetto `/whisk.system/alarms` per visualizzare i parametriche puoi usare.

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Output di esempio:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  Il feed `/whisk.system/alarms/alarm` utilizza due parametri:
  - `cron`: una specifica crontab che indica quando attivare il trigger.
  - `trigger_payload`: il valore del parametro payload da impostare in ogni evento trigger.

2. Crea un trigger che si attiva ogni minuto.
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. Crea un'applicazione. Esempio `hello.js`:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Crea un'azione.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. Crea una regola che richiama l'azione `hello` ogni volta che viene attivato il trigger `everyOneMinute`.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  Output di esempio:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Controlla che l'azione venga richiamata tramite il polling dei log di attivazione.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Puoi vedere che le attivazioni si verificano ogni minuto per il trigger, la regola e l'azione. L'azione riceve i parametri `{"name":"Mork", "place":"Ork"}` ad ogni chiamata.


