---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: push notifications, functions, webhooks

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.mobilepushshort}}-Paket

Das {{site.data.keyword.mobilepushshort}}-Paket bietet eine Gruppe von Aktionen für die Interaktion mit {{site.data.keyword.mobilepushfull}}-Serviceinstanzen. Diese Aktionen ermöglichen Ihnen, eine Nachricht zu senden oder einen Webhook zu erstellen, zu aktualisieren oder zu löschen.
{: shortdesc}

Das {{site.data.keyword.mobilepushshort}}-Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/push-notifications` | Paket | apikey, appGuid | Arbeiten mit einer {{site.data.keyword.mobilepushshort}}-Instanz. |
| `/push-notifications/send-message` | Aktion | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Senden einer Push-Benachrichtigung an angegebene Geräte. |
| `/push-notifications/webhook` | Aktion | events | Aktivieren von Auslöserereignissen für Geräteaktivitäten (Registrierung des Geräts, Rücknahme der Registrierung, Abonnement für Gerät, Beendigung des Abonnements) für den Push-Service. |

## {{site.data.keyword.mobilepushshort}}-Serviceinstanz erstellen
{: #service_instance_push}

Vor dem Installieren des Pakets müssen Sie eine {{site.data.keyword.mobilepushshort}}-Instanz erstellen.


1. [Erstellen Sie eine {{site.data.keyword.mobilepushshort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Erstellen Sie eine Gruppe von Serviceberechtigungsnachweisen ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) für die Pushbenachrichtigungen-Serviceinstanz. 

3. [Konfigurieren Sie die {{site.data.keyword.mobilepushshort}}-Serviceinstanz ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2). 

## {{site.data.keyword.mobilepushshort}}-Paket installieren
{: #install_push}

Nachdem Sie über eine Instanz des {{site.data.keyword.mobilepushshort}}-Service verfügen, verwenden Sie wahlweise die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}} oder ihre Benutzerschnittstelle (UI), um das {{site.data.keyword.mobilepushshort}}-Paket in Ihrem Namensbereich zu installieren.


### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #install_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Gehen Sie zum Installieren des {{site.data.keyword.mobilepushshort}}-Pakets wie folgt vor:

1. Klonen Sie das Repository für das {{site.data.keyword.mobilepushshort}}-Paket.
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. Navigieren Sie zum Verzeichnis `runtimes/nodejs`.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Stellen Sie sicher, dass das Paket `push-notifications` zu Ihrer Paketliste hinzugefügt wird.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Ausgabe:
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Binden Sie die Berechtigungsnachweise aus der erstellten {{site.data.keyword.mobilepushshort}}-Serviceinstanz an das Paket.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    Beispielausgabe:
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.mobilepushshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/push-notifications parameters
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/push-notifications, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "imfpush": {
            "admin_url": "https://mobile.ng.bluemix.net/imfpushdashboard/?appGuid=12345a-a123-1234-ab12-1ba1234567",
            "apikey": "abcd1234abcd1234abcd1234",
            "appGuid": "12341-12345-1234-a1234-1abcd12345",
            "clientSecret": "1b1234ab-1234-1234-123a-ab12345abcd",
            "credentials": "Service credentials-1",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die Benutzerschnittstelle von {{site.data.keyword.openwhisk_short}} durchführen
{: #install_ui}

1. Öffnen Sie die [Seite 'Erstellen' ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/create) in der {{site.data.keyword.openwhisk_short}}-Konsole. 

2. Wählen Sie unter Verwendung des Namensbereichumschalters in der oberen rechten Ecke den Namensbereich aus, in dem Sie das {{site.data.keyword.cos_short}}-Paket installieren möchten. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die **IBM {{site.data.keyword.mobilepushshort}}**-Paketgruppe und klicken Sie dann auf das **IBM {{site.data.keyword.mobilepushshort}}**-Paket. 

5. Wählen Sie im Abschnitt für verfügbare Laufzeiten in der Dropdown-Liste 'NodeJS' aus und klicken Sie dann auf **Installieren**. 

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **push-notifications**. 

7. Um die Aktionen im Paket **push-notifications** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden. 
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus.
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **push-notifications**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet. 
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**. 
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

## {{site.data.keyword.mobilepushshort}}-Paket verwenden
{: #usage_push}

### Push-Benachrichtigung senden
{: #push_notif}

So senden Sie eine Nachricht über den Push-Benachrichtigungsservice mithilfe der Aktion `send-message`: 
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

Beispielausgabe:
```
{
  "response": {
      "result": {
          "message": {
              "alert": "let's code something",
              "url": "http://developer.ibm.com"
          },
          "messageId": "fLyql2tx"
      },
      "status": "success",
      "success": true
  },
}
```
{: screen}

## Webhook erstellen
{: #webhook}

So erstellen Sie einen Webhook für den {{site.data.keyword.mobilepushshort}}-Service für 'onDeviceRegister'-Ereignisse: 

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

Beispielausgabe:
```
{
  "response": {
    "result": {
      "error": {}
    },
  "status": "application error",
  "success": false
  },
}
```
{: screen}
