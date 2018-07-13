---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Push mobili sugli eventi del dispositivo
{: #openwhisk_pushnotifications}

Scopri come configurare il servizio Push per attivare un trigger quando si verifica un'attività del dispositivo, come la registrazione/l'annullamento della registrazione o la sottoscrizione/l'annullamento della sottoscrizione del dispositivo, in un'applicazione specificata.
{: shortdesc}

Per informazioni sul pacchetto `/whisk.system/pushnotifications`, vedi l'argomento [Push mobili](./mobile_push_actions.html) che riguarda la creazione di un bind di pacchetto Push e l'invio di una notifica di push.

## Parametri di push
{: #push_parameters}

I parametri di `/whisk.system/pushnotifications/webhook` sono i seguenti:
- **appId:** il GUID dell'applicazione {{site.data.keyword.Bluemix_notm}}.
- **appSecret:** l'appSecret del servizio di notifica di push {{site.data.keyword.Bluemix_notm}}.
- **events:** _onDeviceRegister_, _onDeviceUnregister_, _onDeviceUpdate_, _onSubscribe_, _onUnsubscribe_

  Puoi utilizzare il carattere jolly "`*`" per ricevere una notifica per tutti gli eventi.

## Attivazione di un evento trigger in caso di attività del servizio Push Notification
{: #trigger_push_notify}

Per creare un trigger che viene attivato ogni volta che un nuovo dispositivo viene registrato con l'applicazione del servizio Push Notification, vedi il seguente esempio:

1. Crea un bind di pacchetto configurato per il tuo servizio Push Notification utilizzando il tuo **appId** e **appSecret**.
  ```
  ibmcloud wsk package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Crea un trigger per il tipo di evento `onDeviceRegister` del servizio Push Notification utilizzando il tuo feed `myPush/webhook`.
  ```
  ibmcloud wsk trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Puoi creare una regola che invia un messaggio ogni volta che viene registrato un nuovo dispositivo. Crea una regola utilizzando l'azione e il trigger precedenti.
  ```
  ibmcloud wsk rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Controlla i risultati utilizzando il comando `ibmcloud wsk activation poll`.
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

5. Registra un dispositivo nella tua applicazione {{site.data.keyword.Bluemix_notm}}. Puoi vedere che `rule`, `trigger` e `action` vengono eseguiti nel [dashboard](https://console.bluemix.net/openwhisk/dashboard) {{site.data.keyword.openwhisk}}.

  L'azione invia una notifica di push.
