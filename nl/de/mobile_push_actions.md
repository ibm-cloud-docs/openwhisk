---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: mobile, push notifications, binding, notifications

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Mobile Push-Paket
{: #openwhisk_catalog_pushnotifications}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar. Die Aktion `sendMessage` mit IAM-Authentifizierung finden Sie im installierbaren Paket [Push Notification](/docs/openwhisk?topic=cloud-functions-push-notifications-package).
{: tip}

Hier erfahren Sie, wie Sie eine Push-Paketbindung erstellen und eine einfache Push-Benachrichtigung mit dem Paket `/whisk.system/pushnotifications` senden, das Ihnen die Möglichkeit bietet, mit einem Push-Service zu arbeiten.
{: shortdesc}

Das Paket enthält die folgende Aktionen und Feeds:

| Entität | Typ | Parameter | Beschreibung |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | Paket | appId, appSecret, admin_url | Arbeiten mit dem Push-Service. |
| `/whisk.system/pushnotifications/sendMessage` | Aktion | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Senden einer Push-Benachrichtigung an angegebene Geräte. |


Informationen zum Aktivieren von Auslöserereignissen bei einer Geräteaktivität finden Sie im Abschnitt [Mobile Push bei Geräteereignissen](/docs/openwhisk?topic=cloud-functions-openwhisk_pushnotifications).

## Push-Paketbindung erstellen
{: #create_push_binding}

Zum Erstellen einer Paketbindung für Push-Benachrichtigungen müssen Sie die folgenden Parameter angeben:

-  **appId**: Die {{site.data.keyword.Bluemix}} **App-GUID**.
-  **appSecret**: Der **geheime Schlüssel** des {{site.data.keyword.Bluemix_notm}} Push-Benachrichtigungsservice.

Führen Sie zum Erstellen einer Paketbindung die folgenden Schritte aus:

1. Erstellen Sie eine {{site.data.keyword.Bluemix_notm}}-Anwendung im [{{site.data.keyword.Bluemix_notm}}-Dashboard](http://cloud.ibm.com).

2. Initialisieren Sie den Push-Benachrichtigungsservice und binden Sie den Service an die {{site.data.keyword.Bluemix_notm}}-Anwendung.

3. Konfigurieren Sie die [Push Notifications-Anwendung](/docs/services/mobilepush?topic=mobile-pushnotification-gettingstartedtemplate).

  Notieren Sie sich die Werte für **App GUID** und **App Secret** der von Ihnen erstellten {{site.data.keyword.Bluemix_notm}}-App.

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

  Beispielausgabe:
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

## Parameter für Push-Benachrichtigungen
{: #push_parameters}

Von der Aktion `/whisk.system/pushnotifications/sendMessage` werden Push-Benachrichtigungen an registrierte Geräte gesendet. Die folgenden Parameter sind verfügbar:
- `text`: Die Benachrichtigung, die dem Benutzer angezeigt wird. Beispiel: `-p text "Hallo, OpenWhisk sendet eine Benachrichtigung"`.
- `url`: Eine URL, die zusammen mit einem Alert gesendet werden kann. Beispiel: `-p url "https:\\www.w3.ibm.com"`.
- `apiHost`: Eine optionale Zeichenfolge, die den API-Host angibt. Der Standardwert ist `mobile.ng.bluemix.net`.  Beispiel: `-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`: Die Liste der angegebenen Geräte. Beispiel: `-p deviceIds ["deviceID1"]`.
- `platforms`: Zum Senden einer Benachrichtigung an die Geräte der angegebenen Plattformen. 'A' für Apple- (iOS) Geräte und 'G' für Google- (Android) Geräte. Beispiel: `-p platforms ["A"]`.
- `userIds`: Zum Senden einer Benachrichtigung an die Geräte der angegebenen Benutzer. Beispiel: `-p userIds "[\"testUser\"]"`
- `tagNames`: Zum Senden einer Benachrichtigung an die Geräte, die einen dieser Tags subskribiert haben. Beispiel: `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey`: Dieser Parameter gibt eine Gruppe aus Nachrichten an.
- `gcmCategory`: Die Kategoriekennung, die für interaktive Push-Benachrichtigungen verwendet werden soll.
- `gcmIcon`: Gibt den Namen des Symbols an, der für die Benachrichtigung angezeigt werden soll. Stellen Sie sicher, dass das Symbol bereits in der Clientanwendung enthalten ist.
- `gcmDelayWhileIdle`: Wenn für diesen Parameter der Wert 'true' festgelegt wird, wird die Nachricht gesendet, bis das Gerät aktiv wird.
- `gcmSync`: Durch Nachrichtenübermittlung in der Gerätegruppe kann jede App-Instanz in einer Gruppe den aktuellen Nachrichtenstatus wiedergeben.
- `gcmVisibility`: private/public - Die Sichtbarkeit dieser Benachrichtigung, die beeinflusst, wie und wann Benachrichtigungen auf einer sicheren gesperrten Anzeige sichtbar werden.
- `gcmPayload`: Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden. Beispiel: `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`: Legt die Priorität der Nachricht fest.
- `gcmSound`: Die Audiodatei (auf einem Gerät), die abgespielt wird, wenn die Benachrichtigung vom Gerät empfangen wird.
- `gcmTimeToLive`: Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist.
- `gcmStyleType`: Gibt den Typ von erweiterbaren Benachrichtigungen an. Mögliche Werte: `bigtext_notification`, `picture_notification`, `inbox_notification`.
- `gcmStyleTitle`: Gibt den Titel der Benachrichtigung an. Der Titel wird angezeigt, wenn die Benachrichtigung erweitert wird. Der Titel muss für alle drei erweiterbaren Benachrichtigungen angegeben werden.
- `gcmStyleUrl`: Eine URL, über die die Abbildung für die Benachrichtigung abzurufen ist. Muss für `picture_notification` angegeben werden.
- `gcmStyleText`: Der umfangreiche Text, der nach dem Erweitern einer Benachrichtigung mit umfangreichem Text (`bigtext_notification`) angezeigt werden muss. Muss für `bigtext_notification` angegeben werden.
- `gcmStyleLines`: Ein Array mit Zeichenfolgen, das im Posteingangsstil für eine Posteingangsbenachrichtigung (`inbox_notification`) angezeigt werden soll. Muss für `inbox_notification` angegeben werden.
- `gcmLightsLedArgb`: Die Farbe der LED. Die Hardware versucht eine Annäherung an die optimale Einstellung.
- `gcmLightsLedOnMs`: Die Anzahl der Millisekunden, die die LED leuchtet, wenn sie blinkt. Die Hardware versucht eine Annäherung an die optimale Einstellung.
- `gcmLightsLedOffMs`: Die Anzahl der Millisekunden, die die LED nicht leuchtet, wenn sie blinkt. Die Hardware versucht eine Annäherung an die optimale Einstellung.
- `apnsBadge`: Die Nummer, die als Markierung des Anwendungssymbols angezeigt werden soll.
- `apnsCategory`: Die Kategoriekennung, die für interaktive Push-Benachrichtigungen verwendet werden soll.
- `apnsIosActionKey`: Der Titel für den Aktionsschlüssel.
- `apnsPayload`: Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden.
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound`: Der Name der Audiodatei im Anwendungsbundle. Die Tonsignale dieser Datei werden als Alert abgespielt.
- `apnsTitleLocKey`: Der Schlüssel zu einer Titelzeichenfolge in der Datei `Localizable.strings` für die aktuelle Lokalisierung. Die Schlüsselzeichenfolge kann mit den Kennungen %@ und %n$@ formatiert werden, um die im Array `titleLocArgs` angegebenen Variablen zu verwenden.
- `apnsLocKey`: Ein Schlüssel zu einer Zeichenfolge einer Alertnachricht in der Datei `Localizable.strings` für die aktuelle Lokalisierung (die durch die Sprachvorgabe des Benutzers festgelegt wird). Die Schlüsselzeichenfolge kann mit den Kennungen %@ und %n$@ formatiert werden, um die im Array `locArgs` angegebenen Variablen zu verwenden.
- `apnsLaunchImage`: Der Name einer Imagedatei im App-Bundle mit oder ohne Dateinamenerweiterung. Das Image wird als Startimage verwendet, wenn der Benutzer die Aktionsschaltfläche antippt oder den Aktionsschieberegler bewegt.
- `pnsTitleLocArgs`: Variablenzeichenfolgewerte, die anstelle der Formatkennungen in `title-loc-key` angezeigt werden.
- `apnsLocArgs`: Variablenzeichenfolgewerte, die anstelle der Formatkennungen in `locKey` angezeigt werden.
- `apnstitle`: Der Titel von Rich-Push-Benachrichtigungen (nur unterstützt ab iOS 10).
- `apnsSubtitle`: Der Untertitel der Rich-Benachrichtigungen. (Nur unterstützt ab iOS 10.)
- `apnsAttachmentUrl`: Der Link zu den iOS-Benachrichtigungsmedien (Video, Audio, GIF, Images - unterstützt nur ab iOS 10).
- `fireFoxTitle`: Gibt den Titel an, der für die WebPush Notification festgelegt werden soll.
- `fireFoxIconUrl`: Die URL des Symbols, das für die WebPush Notification festgelegt werden soll.
- `fireFoxTimeToLive`: Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist.
- `fireFoxPayload`: Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden.
- `chromeTitle`: Gibt den Titel an, der für die WebPush Notification festgelegt werden soll.
- `chromeIconUrl`: Die URL des Symbols, das für die WebPush Notification festgelegt werden soll.
- `chromeTimeToLive`: Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist.
- `chromePayload`: Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden.
- `safariTitle`: Gibt den Titel an, der für die Safari-Push-Benachrichtigungen festgelegt werden soll.
- `safariUrlArgs`: Die URL-Argumente, die mit dieser Benachrichtigung verwendet werden müssen. Diese Argumente müssen in Form eines JSON-Arrays bereitgestellt werden.
- `safariAction`: Die Bezeichnung der Aktionsschaltfläche.
- `chromeAppExtTitle`: Gibt den Titel an, der für die WebPush Notification festgelegt werden soll.
- `chromeAppExtCollapseKey`: Dieser Parameter gibt eine Gruppe aus Nachrichten an.
- `chromeAppExtDelayWhileIdle`: Wenn für diesen Parameter der Wert 'true' festgelegt wird, gibt dies an, dass die Nachricht erst gesendet werden darf, wenn das Gerät aktiv wird.
- `chromeAppExtIconUrl`: Die URL des Symbols, das für die WebPush Notification festgelegt werden soll.
- `chromeAppExtTimeToLive`: Dieser Parameter gibt an, wie lange (in Sekunden) die Nachricht im GCM-Speicher aufbewahrt wird, wenn das Gerät offline ist.
- `chromeAppExtPayload`: Angepasste JSON-Nutzdaten, die als Bestandteil der Benachrichtigung gesendet werden.

## Push-Benachrichtigungen senden
{: #send_push_notifications}

Das folgende Beispiel zeigt, wie eine Push-Benachrichtigung aus dem Paket 'Push Notification' gesendet wird.

Senden Sie eine Push-Benachrichtigung mithilfe der Aktion **sendMessage** in der Paketbindung ab, die Sie zuvor erstellt haben. Stellen Sie sicher, dass Sie `/myNamespace/myPush` durch Ihren Paketnamen ersetzen.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

Beispielausgabe:
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
