---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: triggers, serverless, functions

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



# Auslöser für Ereignisse erstellen
{: #triggers}

Bei einem Auslöser handelt es sich um eine Deklaration, die auf einen bestimmten Typ von Ereignis reagieren soll, entweder durch einen Benutzer oder eine Ereignisquelle.
{: shortdesc}

Es gibt zum Beispiel die folgenden Auslöser.
- Ereignisse der Standortaktualisierung
- Dokumentuploads auf eine Website
- Eingehende E-Mails



## Auslöser über die CLI erstellen
{: #triggers_create}


1. Erstellen Sie den Auslöser. Auslöser müssen direkt in einem Namensbereich erstellt werden. Ihre Erstellung in einem Paket ist nicht möglich.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Überprüfen Sie ob der Auslöser erstellt wurde.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    **Beispielausgabe**
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



Als Nächstes können Sie den [Auslöser testen](/docs/openwhisk?topic=cloud-functions-test#test_triggers) oder eine [Regel erstellen](/docs/openwhisk?topic=cloud-functions-rules), um dem Auslöser eine Aktion zuzuordnen.



## Unterschied zwischen Feed und Auslöser
{: #triggers_difference}

Feeds und Auslöser sind sich sehr ähnlich, aber technisch gesehen unterschiedliche Konzepte.

- {{site.data.keyword.openwhisk_short}} verarbeitet **Ereignisse**, die im System eintreffen.

- Ein **Auslöser** ist ein Name für eine Klasse von Ereignissen. Jedes Ereignis gehört zu genau einem Auslöser. Ein Auslöser ähnelt daher einem Thema ('topic') in themenbasierten Publish/Subscribe-Systemen. Eine **Regel** bedeutet: Wenn ein Ereignis des Auslösers eintrifft, soll die Aktion mit den Auslösernutzdaten aufgerufen werden.

- Ein **Feed** ist eine bequeme Methode zum Konfigurieren einer externen Ereignisquelle, damit diese Auslöserereignisse aktiviert, die von {{site.data.keyword.openwhisk_short}} verarbeitet werden können. Ein Feed ist ein Strom von Ereignissen, die alle zu einem Auslöser gehören. Vorinstallierte Pakete, installierbare Pakete und Ihre eigenen angepassten Pakete können Feeds enthalten.  Ein Feed wird durch eine **Feedaktion** gesteuert, die das Erstellen, Löschen, Anhalten und Fortsetzen des Ereignisstroms abwickelt, aus dem der Feed besteht. Die Feedaktion interagiert in der Regel mit externen Services, die die Ereignisse über eine REST-API, die Benachrichtigungen verwaltet, erstellen.

Beispiele für Feeds:
- Feed für {{site.data.keyword.cloudant}}-Datenänderungen, der immer dann ein Auslöserereignis aktiviert, wenn ein Dokument zu einer Datenbank hinzugefügt oder ein in der Datenbank vorhandenes Dokument geändert wird
- Git-Feed, der ein Auslöserereignis für jede Festschreibung (Commit) in einem Git-Repository aktiviert



## Auslöser für einen Feed erstellen
{: #triggers_feeds}

Das folgende Beispiel zeigt, wie ein Feed im Paket für Alarme verwendet wird, um einen Auslöser einmal pro Minute zu aktivieren, und eine Regel verwendet wird, um jede Minute eine Aktion aufzurufen.

1. Rufen Sie eine Beschreibungsliste der Entitäten im Paket `/whisk.system/alarms` ab.

    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    **Beispielausgabe**
    ```
    package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. Rufen Sie eine Beschreibung des Feeds im Paket `/whisk.system/alarms` ab, um die Parameter anzuzeigen, die Sie verwenden können.

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  **Beispielausgabe**
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  Der `/whisk.system/alarms/alarm` hat zwei Parameter:
  - `cron`: Eine crontab-Angabe für den Zeitpunkt, wann der Auslöser zu aktivieren ist.
  - `trigger_payload`: Der Payload-Parameterwert, der in jedem Auslöserereignis festgelegt werden soll.

2. Erstellen Sie einen Auslöser, der jede Minute aktiviert wird. 
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  **Beispielausgabe**
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. Erstellen Sie eine App. Beispiel für `hello.js`:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Erstellen Sie eine Aktion.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

5. Erstellen Sie eine Regel, die die Aktion `hello` jedes Mal aufruft, wenn der Auslöser `everyOneMinute` aktiviert wird.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  **Beispielausgabe**
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Prüfen Sie, ob die Aktion aufgerufen wird, indem Sie die Aktivierungsprotokolle durch Polling abfragen.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Es ist zu erkennen, dass die Aktivierungen einmal pro Minute für den Auslöser, die Regel und die Aktion erfolgen. Die Aktion empfängt die Parameter `{"name":"Mork", "place":"Ork"}` bei jedem Aufruf.



