---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: push notifications, functions, webhooks

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


# Push-Benachrichtigungen
{: #pkg_push_notifications}


## Pakete
{: #pkg_push_packages}

| Paket | Verfügbarkeit | Beschreibung |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | Vorinstalliert (nicht in Tokio verfügbar) | Senden einer Push-Benachrichtigung an angegebene Geräte. |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | Vorinstalliert (nicht in Tokio verfügbar) | Senden von Mobile-Push-Benachrichtigungen bei Geräteereignissen. |
| [`/push-notifications`](#pkg_push_mobile_send) | Installierbar | Interaktion mit {{site.data.keyword.mobilepushfull}}-Serviceinstanzen. Senden einer Nachricht oder Erstellen, Aktualisieren oder Löschen eines Webhooks. |

## Push-Benachrichtigungen senden
{: #pkg_push_send}

Das vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar. Die Aktion `sendMessage` mit IAM-Authentifizierung finden Sie im installierbaren Paket [Push Notification](#pkg_push_mobile_send).
{: tip}

Hier erfahren Sie, wie Sie eine Bindung für ein Paket 'Push Notification' erstellen und wie Sie eine einfache Push-Benachrichtigung mit dem Paket `/whisk.system/pushnotifications` senden.
{: shortdesc}

Das Paket enthält die folgende Aktionen und Feeds:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | Paket |2`appId`, `appSecret`, `admin_url` | Arbeiten mit dem Push-Service. |
| `/whisk.system/pushnotifications/sendMessage` | Aktion | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Senden einer Push-Benachrichtigung an angegebene Geräte. |


Informationen zum Aktivieren von Auslöserereignissen bei einer Geräteaktivität finden Sie im Abschnitt [Mobile Push bei Geräteereignissen](#pkg_push_mobile).

### Push-Paketbindung erstellen
{: #pkg_push_create}

Zum Erstellen einer Paketbindung für Push-Benachrichtigungen müssen Sie die folgenden Parameter angeben. 

| Parameter | Beschreibung |
| --- | --- |
| `appId` | Die **App-GUID** für {{site.data.keyword.cloud}}. |
| `appSecret` | Der **geheime App-Schlüssel** des {{site.data.keyword.cloud_notm}} Push Notification-Service. |

Führen Sie zum Erstellen einer Paketbindung die folgenden Schritte aus:

1. Erstellen Sie eine {{site.data.keyword.cloud_notm}}-Anwendung im [{{site.data.keyword.cloud_notm}}-Dashboard](https://cloud.ibm.com){: external}.

2. Initialisieren Sie den Push-Benachrichtigungsservice und binden Sie den Service an die {{site.data.keyword.cloud_notm}}-Anwendung.

3. Konfigurieren Sie die [Push Notifications-Anwendung](/docs/services/mobilepush?topic=mobile-pushnotification-getting-started).

  Notieren Sie sich die Werte für **App GUID** und **App Secret** der von Ihnen erstellten {{site.data.keyword.cloud_notm}}-App.

4. Erstellen Sie eine Paketbindung mit `/whisk.system/pushnotifications`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. Prüfen Sie, ob die Paketbindung vorhanden ist:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Beispielausgabe**
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### Parameter für Push-Benachrichtigungen
{: #pkg_push_params}

Von der Aktion `/whisk.system/pushnotifications/sendMessage` werden Push-Benachrichtigungen an registrierte Geräte gesendet. Die Parameter lauten wie folgt. 

| Parameter | Beschreibung |
| --- | --- |
| `text` | Die Benachrichtigung, die dem Benutzer angezeigt wird. Beispiel: `-p text "Hallo, OpenWhisk sendet eine Benachrichtigung"`. |
| `url` | Eine URL, die zusammen mit dem Alert gesendet werden kann. Beispiel: `-p url "https:\\www.w3.ibm.com"`. |
| `apiHost` | Eine optionale Zeichenfolge, die den API-Host angibt. Der Standardwert ist `mobile.ng.bluemix.net`.  Beispiel: `-p apiHost "mobile.eu-gb.bluemix.net"`. |
| `deviceIds` | Die Liste der angegebenen Geräte. Beispiel: `-p deviceIds ["deviceID1"]`. |
| `platforms` | Zum Senden einer Benachrichtigung an die Geräte der angegebenen Plattformen. 'A' für Apple- (iOS) Geräte und 'G' für Google- (Android) Geräte. Beispiel: `-p platforms ["A"]`. |
| `userIds` | Zum Senden einer Benachrichtigung an die Geräte der angegebenen Benutzer. Beispiel: `-p userIds "[\"testUser\"]"`. |
| `tagNames` | Zum Senden einer Benachrichtigung an die Geräte, die einen dieser Tags abonniert haben. Beispiel: `-p tagNames "[\"tag1\"]"`. |
| `gcmCollapseKey` | Dieser Parameter gibt eine Gruppe von Nachrichten an. |
| `gcmCategory` | Die Kategorie-ID, die für interaktive Push-Benachrichtigungen verwendet werden soll. |
| `gcmIcon` | Gibt den Namen des Symbols an, das für die Benachrichtigung angezeigt werden soll. Stellen Sie sicher, dass das Symbol bereits in der Clientanwendung enthalten ist. |
| `gcmDelayWhileIdle` | Wenn dieser Parameter auf 'true' gesetzt ist, wird die Nachricht gesendet, bis das Gerät aktiv wird. |
| `gcmSync` | Durch Nachrichtenübermittlung in der Gerätegruppe kann jede App-Instanz in einer Gruppe den aktuellen Nachrichtenstatus widerspiegeln. |
| `gcmVisibility` | private/public - Die Sichtbarkeit dieser Benachrichtigung, die beeinflusst, wie und wann die Benachrichtigungen auf einer sicheren gesperrten Anzeige sichtbar werden. |
| `gcmPayload` | Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden. Beispiel: `-p gcmPayload "{\"hi\":\"hello\"}"`. |
| `gcmPriority` | Legt die Priorität der Nachricht fest. |
| `gcmSound` | Die Audiodatei (auf einem Gerät), die abgespielt wird, wenn die Benachrichtigung vom Gerät empfangen wird. |
| `gcmTimeToLive` | Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist. |
| `gcmStyleType` | Gibt den Typ der erweiterbaren Benachrichtigungen an. Mögliche Werte: `bigtext_notification`, `picture_notification`, `inbox_notification`. |
| `gcmStyleTitle` | Gibt den Titel der Benachrichtigung an. Der Titel wird angezeigt, wenn die Benachrichtigung erweitert wird. Der Titel muss für alle drei erweiterbaren Benachrichtigungen angegeben werden. |
| `gcmStyleUrl` | Eine URL, über die die Abbildung für die Benachrichtigung abzurufen ist. Muss für `picture_notification` angegeben werden. |
| `gcmStyleText` | Der umfangreiche Text, der nach dem Erweitern einer Benachrichtigung mit umfangreichem Text (`bigtext_notification`) angezeigt werden muss. Muss für `bigtext_notification` angegeben werden. |
| `gcmStyleLines` | Ein Array mit Zeichenfolgen, das im Posteingangsstil für eine Posteingangsbenachrichtigung (`inbox_notification`) angezeigt werden soll. Muss für `inbox_notification` angegeben werden. |
| `gcmLightsLedArgb` | Die Farbe der LED. Die Hardware versucht eine Annäherung an die optimale Einstellung. |
| `gcmLightsLedOnMs` | Die Anzahl der Millisekunden, die die LED leuchtet, wenn sie blinkt. Die Hardware versucht eine Annäherung an die optimale Einstellung. |
| `gcmLightsLedOffMs` | Die Anzahl der Millisekunden, die die LED nicht leuchtet, wenn sie blinkt. Die Hardware versucht eine Annäherung an die optimale Einstellung. |
| `apnsBadge` | Die Nummer, die als Markierung des Anwendungssymbols angezeigt werden soll. |
| `apnsCategory` | Die Kategorie-ID, die für interaktive Push-Benachrichtigungen verwendet werden soll. |
| `apnsIosActionKey` | Der Titel für den Aktionsschlüssel. |
| `apnsPayload` | Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden. |
| `apnsType` | ['DEFAULT', 'MIXED', 'SILENT']. |
| `apnsSound` | Der Name der Audiodatei im Anwendungsbundle. Die Tonsignale dieser Datei werden als Alert abgespielt. |
| `apnsTitleLocKey` | Der Schlüssel zu einer Titelzeichenfolge in der Datei `Localizable.strings` für die aktuelle Ländereinstellung. Die Schlüsselzeichenfolge kann mit den Kennungen %@ und %n$@ formatiert werden, um die im Array `titleLocArgs` angegebenen Variablen zu verwenden. |
| `apnsLocKey` | Ein Schlüssel zu einer Zeichenfolge einer Alertnachricht in der Datei `Localizable.strings` für die aktuelle Ländereinstellung (die durch die Sprachvorgabe des Benutzers festgelegt wird). Die Schlüsselzeichenfolge kann mit den Kennungen %@ und %n$@ formatiert werden, um die im Array `locArgs` angegebenen Variablen zu verwenden. |
| `apnsLaunchImage` | Der Dateiname einer Imagedatei im App-Bundle mit oder ohne Dateinamenerweiterung. Das Image wird als Startimage verwendet, wenn der Benutzer die Aktionsschaltfläche antippt oder den Aktionsschieberegler bewegt. |
| `pnsTitleLocArgs` | Variablenzeichenfolgewerte, die anstelle der Formatkennungen in `title-loc-key` angezeigt werden. |
| `apnsLocArgs` | Variablenzeichenfolgewerte, die anstelle der Formatkennungen in `locKey` angezeigt werden. |
| `apnstitle` | Der Titel von Rich-Push-Benachrichtigungen (nur unterstützt ab iOS 10). |
| `apnsSubtitle` | Der Untertitel der Rich-Benachrichtigungen. (Nur unterstützt ab iOS 10.)
| `apnsAttachmentUrl` | Der Link zu den iOS-Benachrichtigungsmedien (Video, Audio, GIF, Bilder - unterstützt nur ab iOS 10). |
| `fireFoxTitle` | Gibt den Titel an, der für die Web-Push-Benachrichtigung festgelegt werden soll. |
| `fireFoxIconUrl` | Die URL des Symbols, das für die Web-Push-Benachrichtigung festgelegt werden soll. |
| `fireFoxTimeToLive` | Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist. |
| `fireFoxPayload` | Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden. |
| `chromeTitle` | Gibt den Titel an, der für die Web-Push-Benachrichtigung festgelegt werden soll. |
| `chromeIconUrl` | Die URL des Symbols, das für die Web-Push-Benachrichtigung festgelegt werden soll. |
| `chromeTimeToLive` | Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist. |
| `chromePayload` | Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden. |
| `safariTitle` | Gibt den Titel an, der für die Safari-Push-Benachrichtigungen festgelegt werden soll. |
| `safariUrlArgs` | Die URL-Argumente, die mit dieser Benachrichtigung verwendet werden müssen. Diese Argumente müssen in Form eines JSON-Arrays bereitgestellt werden. |
| `safariAction` | Die Bezeichnung der Aktionsschaltfläche. |
| `chromeAppExtTitle` | Gibt den Titel an, der für die Web-Push-Benachrichtigung festgelegt werden soll. |
| `chromeAppExtCollapseKey` | Dieser Parameter gibt eine Gruppe von Nachrichten an. |
| `chromeAppExtDelayWhileIdle` | Wenn dieser Parameter auf den Wert 'true' gesetzt ist, gibt dies an, dass die Nachricht erst gesendet wird, wenn das Gerät aktiv wird. |
| `chromeAppExtIconUrl` | Die URL des Symbols, das für die Web-Push-Benachrichtigung festgelegt werden soll. |
| `chromeAppExtTimeToLive` | Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist. |
| `chromeAppExtPayload` | Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden. |

### Beispiel für Push-Benachrichtigung
{: #pkg_push_ex}

Das folgende Beispiel zeigt, wie eine Push-Benachrichtigung aus dem Paket 'Push Notification' gesendet wird.

Senden Sie eine Push-Benachrichtigung mithilfe der Aktion **sendMessage** in der Paketbindung ab, die Sie zuvor erstellt haben. Stellen Sie sicher, dass Sie `/myNamespace/myPush` durch Ihren Paketnamen ersetzen.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

**Beispielausgabe**
```
{
  "result": {
  "pushResponse":
    {
      "messageId":"11111H",
      "message":{
        "alert":"this is my message",
        "url":""
      },
      "settings":{
        "apns":{
          "sound":"default"
        },
        "gcm":{
          "sound":"default"
          },
        "target":{
          "deviceIds":["T1","T2"]
        }
      }
    }
  },
  "status": "success",
  "success": true
}
```
{: screen}

## Mobile-Push-Benachrichtigungen bei Geräteereignissen senden
{: #pkg_push_mobile}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar.
{: tip}

Hier erfahren Sie, wie Sie den Push-Benachrichtigungsservice so konfigurieren, dass ein Auslöser aktiviert wird, wenn eine Geräteaktivität wie eine Geräteregistrierung bzw. die Rücknahme einer Geräteregistrierung oder ein Abonnement bzw. das Beenden eines Abonnements für eine angegebene Anwendung erfolgt.
{: shortdesc}

### Parameter
{: #pkg_push_mobile_params}

Für `/whisk.system/pushnotifications/webhook` gelten folgende Parameter:

| Parameter | Beschreibung |
|--- | --- |
| `appId` | Die App-GUID für {{site.data.keyword.cloud_notm}}. |
| `appSecret` | Der `geheime Schlüssel` des {{site.data.keyword.cloud_notm}} Push Notification-Service. |
| `events` | `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`. Sie können das Platzhalterzeichen "`*`" verwenden, um sich über alle Ereignisse benachrichtigen zu lassen. |

### Auslöserereignis für Aktivität des Push Notifications-Service aktivieren
{: #pkg_push_mobile_trigger}

Das folgende Beispiel zeigt, wie ein Auslöser erstellt wird, der jedes Mal aktiviert wird, wenn ein neues Gerät in der Anwendung des Push Notifications-Service registriert wird:

1. Erstellen Sie eine Paketbindung, die für den Push Notifications-Service konfiguriert ist, und verwenden Sie dabei Ihre Werte `appId` und `appSecret`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Erstellen Sie mithilfe des Feeds `myPush/webhook` einen Auslöser für den Ereignistyp `onDeviceRegister` des Push Notifications-Service.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Sie können eine Regel erstellen, die jedes Mal eine Nachricht sendet, wenn ein neues Gerät registriert wird. Erstellen Sie eine Regel unter Verwendung der vorherigen Aktion und des vorherigen Auslösers.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Überprüfen Sie die Ergebnisse unter Verwendung des Befehls `ibmcloud fn activation poll`.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Registrieren Sie ein Gerät in Ihrer {{site.data.keyword.cloud_notm}}-Anwendung. Im {{site.data.keyword.openwhisk}}-[Dashboard](https://cloud.ibm.com/openwhisk/dashboard){: external} ist zu sehen, wie die Regel (`rule`), der Auslöser (`trigger`) und die Aktion (`action`) ausgeführt werden. 

  **Ausgabe**
  
  Die Aktion sendet eine Push-Benachrichtigung.


## Push-Benachrichtigungen senden oder Webhooks erstellen, aktualisieren und löschen
{: #pkg_push_mobile_send}

Das installierbare {{site.data.keyword.mobilepushshort}}-Paket bietet eine Gruppe von Aktionen für die Interaktion mit {{site.data.keyword.mobilepushfull}}-Serviceinstanzen. Diese Aktionen ermöglichen Ihnen, eine Nachricht zu senden oder einen Webhook zu erstellen, zu aktualisieren oder zu löschen.
{: shortdesc}

Das {{site.data.keyword.mobilepushshort}}-Paket enthält die folgenden Aktionen:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/push-notifications` | Paket | `apikey`, `appGuid` | Arbeiten mit einer {{site.data.keyword.mobilepushshort}}-Instanz. |
| `/push-notifications/send-message` | Aktion | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Senden einer Push-Benachrichtigung an angegebene Geräte. |
| `/push-notifications/webhook` | Aktion | `events` | Aktivieren von Auslöserereignissen für Geräteaktivitäten (Registrierung des Geräts, Rücknahme der Registrierung, Abonnement für Gerät, Beendigung des Abonnements) für den Push-Service. |

### {{site.data.keyword.mobilepushshort}}-Serviceinstanz erstellen
{: #service_instance_push}

Vor dem Installieren des Pakets müssen Sie eine {{site.data.keyword.mobilepushshort}}-Instanz erstellen.

1. [Erstellen Sie eine {{site.data.keyword.mobilepushshort}}-Serviceinstanz](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a). 

2. [Erstellen Sie eine Gruppe von Serviceberechtigungsnachweisen](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) für die Push Notifications-Serviceinstanz. 

3. [Konfigurieren Sie die {{site.data.keyword.mobilepushshort}}-Serviceinstanz](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2). 

### {{site.data.keyword.mobilepushshort}}-Paket installieren
{: #pkg_push_mobile_install}

Nachdem Sie über eine Instanz des {{site.data.keyword.mobilepushshort}}-Service verfügen, verwenden Sie wahlweise die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.openwhisk}} oder ihre Benutzerschnittstelle (UI), um das {{site.data.keyword.mobilepushshort}}-Paket in Ihrem Namensbereich zu installieren.

### Installation über die {{site.data.keyword.openwhisk_short}}-CLI durchführen
{: #pkg_push_mobile_cli}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 

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

    **Ausgabe**
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

    **Beispielausgabe**
    ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Stellen Sie sicher, dass das Paket mit Ihren Berechtigungsnachweisen für die {{site.data.keyword.mobilepushshort}}-Serviceinstanz konfiguriert ist.
    ```
    ibmcloud fn package get /myOrg_mySpace/push-notifications parameters
    ```
    {: pre}

    **Beispielausgabe**
    ```
    ok: got package /myOrg_mySpace/push-notifications, displaying field parameters
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
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:imfpush:us-south:a/abcd1234abcd1234:abcd1234-abcd-1234-abcd1234::",
            "iam_apikey_name": "auto-generated-apikey-abcd1234abcd1234abcd1234",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/1234abcd1234abcd::serviceid:ServiceId-12345678-1234-12ab-abc1-1234abcd1234abcd",
            "instance": "Push Notifications-ab",
            "plan": "LITE",
            "url": "https://imfpush.ng.bluemix.net/imfpush/v1/apps/1234abcd-1234-abcd-1234"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation über die {{site.data.keyword.openwhisk_short}}-UI durchführen
{: #pkg_push_mobile_ui}

1. In der {{site.data.keyword.openwhisk_short}}-Konsole rufen Sie die [Seite 'Erstellen'](https://cloud.ibm.com/openwhisk/create){: external} auf. 

2. Wählen Sie mithilfe des Dropdown-Menüs für den Namensbereich in der rechten oberen Ecke den Namensbereich aus, in dem das {{site.data.keyword.cos_full_notm}}-Paket installiert werden soll. 

3. Klicken Sie auf **Pakete installieren**.

4. Klicken Sie auf die **IBM {{site.data.keyword.mobilepushshort}}**-Paketgruppe und klicken Sie dann auf das **IBM {{site.data.keyword.mobilepushshort}}**-Paket.

5. Wählen Sie im Abschnitt für verfügbare Laufzeiten in der Dropdown-Liste 'nodeJS' aus und klicken Sie dann auf **Installieren**. 

6. Nachdem das Paket installiert worden ist, werden Sie zur Seite 'Aktionen' weitergeleitet, auf der Sie nach Ihrem neuen Paket suchen können. Dieses trägt die Bezeichnung **push-notifications**.

7. Um die Aktionen im Paket **push-notifications** verwenden zu können, müssen Sie Serviceberechtigungsnachweise an die Aktionen binden.
  * Wenn Sie Serviceberechtigungsnachweise an alle Aktionen im Paket binden wollen, führen Sie die Schritte 5 und 6 in den oben aufgeführten CLI-Anweisungen aus.
  * Wenn Sie Serviceberechtigungsnachweise an einzelne Aktionen binden wollen, führen Sie die nachfolgend aufgeführten Schritte in der UI (Benutzerschnittstelle) aus. **Hinweis**: Für jede Aktion, die Sie verwenden wollen, müssen Sie die nachfolgend aufgeführten Schritte ausführen.
    1. Klicken Sie auf eine Aktion aus dem Paket **push-notifications**, die Sie verwenden wollen. Die Detailseite für diese Aktion wird geöffnet.
    2. Klicken Sie im Navigationsbereich links auf den Abschnitt **Parameter**.
    3. Geben Sie einen neuen **Parameter** ein. Geben Sie als Schlüssel die Zeichenfolge `__bx_creds` ein. Fügen Sie als Wert das JSON-Serviceberechtigungsnachweisobjekt aus der Serviceinstanz ein, die Sie zuvor erstellt haben.

### Push-Benachrichtigung senden
{: #pkg_push_mobile_sendmsg}

So senden Sie eine Nachricht über den Push-Benachrichtigungsservice mithilfe der Aktion `send-message`:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

**Beispielausgabe**
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

### Webhook erstellen
{: #pkg_push_mobile_hook}

So erstellen Sie einen Webhook für den {{site.data.keyword.mobilepushshort}}-Service für `onDeviceRegister`-Ereignisse: 

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

**Beispielausgabe**
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



