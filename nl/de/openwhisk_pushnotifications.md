---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: push notifications, events, parameters, triggers, service activity

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Mobile Push bei Geräteereignissen
{: #openwhisk_pushnotifications}

Dieses vorinstallierte Paket ist nicht in der Region 'Tokio' verfügbar.
{: tip}

Hier erfahren Sie, wie Sie den Push-Service so konfigurieren, dass ein Auslöser aktiviert wird, wenn eine Geräteaktivität wie eine Geräteregistrierung bzw. die Rücknahme einer Geräteregistrierung oder ein Abonnement bzw. das Beenden eines Abonnements für eine angegebene Anwendung erfolgt.
{: shortdesc}

Weitere Informationen zum Paket `/whisk.system/pushnotifications` selbst finden Sie im Abschnitt [Mobile Push](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a#push_step_1a), in dem das Erstellen einer Push-Paketbindung und das Senden einer Push-Benachrichtigung beschrieben wird.

## Push-Parameter
{: #push_notif_parameters}

Für `/whisk.system/pushnotifications/webhook` gelten folgende Parameter:
- `appId`: Die {{site.data.keyword.Bluemix_notm}}-App-GUID. 
- `appSecret`: Der geheime App-Schlüssel des {{site.data.keyword.Bluemix_notm}} Push-Benachrichtigungsservice. 
- `events`: `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`

  Sie können das Platzhalterzeichen "`*`" verwenden, um sich über alle Ereignisse benachrichtigen zu lassen.

## Auslöserereignis für Aktivität des Push Notifications-Service aktivieren
{: #trigger_push_notify}

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

5. Registrieren Sie ein Gerät in Ihrer {{site.data.keyword.Bluemix_notm}}-Anwendung. Im [Dashboard](https://cloud.ibm.com/openwhisk/dashboard) von {{site.data.keyword.openwhisk}} ist zu sehen, wie die `Regel`, der `Auslöser` und die `Aktion` ausgeführt werden. 

  Die Aktion sendet eine Push-Benachrichtigung.
