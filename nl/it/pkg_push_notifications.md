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


# Notifiche push
{: #pkg_push_notifications}


## Pacchetti
{: #pkg_push_packages}

| Pacchetto | Disponibilità | Descrizione |
| --- | --- | --- |
| [`/whisk.system/pushnotifications`](#pkg_push_send) | Preinstallato (non disponibile a Tokyo) | Inviare una notifica di push a uno o più dispositivi specificati. |
| [`/whisk.system/pushnotifications/webhook`](#pkg_push_mobile) | Preinstallato (non disponibile a Tokyo) | Invia notifiche di push mobili sugli eventi del dispositivo. |
| [`/push-notifications`](#pkg_push_mobile_send) | Installabile | Interagisci con le istanze di servizio {{site.data.keyword.mobilepushfull}}. Invia un messaggio oppure crea, aggiorna o elimina un webhook. |

## Invio di notifiche di push
{: #pkg_push_send}

Il pacchetto preinstallato non è disponibile nella regione di Tokyo. Vedi il pacchetto [Push Notification](#pkg_push_mobile_send) installabile per l'azione `sendMessage` attraverso l'autenticazione IAM
{: tip}

Scopri come creare un bind di pacchetto Push Notification e inviare una semplice notifica di push utilizzando il pacchetto `/whisk.system/pushnotifications`.
{: shortdesc}

Il pacchetto include le azioni e i feed che seguono:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | Pacchetto | `appId`, `appSecret`, `admin_url` | Lavorare con il servizio push. |
| `/whisk.system/pushnotifications/sendMessage` | Azione | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Inviare una notifica di push a uno o più dispositivi specificati. |


Per informazioni sull'attivazione degli eventi trigger quando si verifica un'attività del dispositivo, vedi l'argomento [Push mobili sugli eventi del dispositivo](#pkg_push_mobile).

### Creazione di un bind di pacchetto Push
{: #pkg_push_create}

Per creare un bind di pacchetto Push Notification, devi specificare i seguenti parametri.

| Parametro | Descrizione |
| --- | --- |
| `appId` | Il **GUID applicazione** di {{site.data.keyword.cloud}}. |
| `appSecret` | Il **Segreto applicazione** del servizio {{site.data.keyword.cloud_notm}} Push Notification. |

Per creare un bind di pacchetto, vedi la seguente procedura:

1. Crea un'applicazione {{site.data.keyword.cloud_notm}} nel [dashboard {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external}.

2. Inizializza il servizio Push Notification ed esegui il bind del servizio all'applicazione {{site.data.keyword.cloud_notm}}.

3. Configura l'[applicazione Notifica di push](/docs/services/mobilepush?topic=mobile-pushnotification-getting-started).

  Assicurati di ricordare il **GUID applicazione** e il **Segreto applicazione** dell'applicazione {{site.data.keyword.cloud_notm}} che hai creato.

4. Crea un bind di pacchetto con `/whisk.system/pushnotifications`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myPush -p appId myAppID -p appSecret myAppSecret
  ```
  {: pre}

5. Verifica che il bind di pacchetto esista:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Output di esempio**
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

### Parametri delle notifiche di push
{: #pkg_push_params}

L'azione `/whisk.system/pushnotifications/sendMessage` invia notifiche push ai dispositivi registrati. I parametri sono i seguenti.

| Parametro | Descrizione |
| --- | --- |
| `text` | Il messaggio di notifica da mostrare all'utente. Ad esempio, `-p text "Hi, OpenWhisk send a notification"`. |
| `url` | Un URL che può essere inviato insieme all'avviso. Ad esempio, `-p url "https:\\www.w3.ibm.com"`. |
| `apiHost` | Una stringa facoltativa che specifica l'host API. Il valore predefinito è `mobile.ng.bluemix.net`.  Ad esempio, `-p apiHost "mobile.eu-gb.bluemix.net"`. |
| `deviceIds` | L'elenco dei dispositivi specificati. Ad esempio, `-p deviceIds ["deviceID1"]`. |
| `platforms` | Invia notifiche ai dispositivi delle piattaforme specificate. 'A' per i dispositivi Apple (iOS) e 'G' per i dispositivi Google (Android). Ad esempio, `-p platforms ["A"]`. |
| `userIds` | Invia notifiche ai dispositivi degli utenti specificati. Ad esempio, `-p userIds "[\"testUser\"]"`. |
| `tagNames` | Invia notifiche ai dispositivi sottoscritti a una di queste tag. Ad esempio, `-p tagNames "[\"tag1\"]"`. |
| `gcmCollapseKey` | Questo parametro identifica un gruppo di messaggi. |
| `gcmCategory` | L'identificativo di categoria da utilizzare per le notifiche di push interattive. |
| `gcmIcon` | Specifica il nome dell'icona da visualizzare per la notifica. Assicurati che l'icona sia già nel pacchetto dell'applicazione client. |
| `gcmDelayWhileIdle` | Se questo parametro è impostato su true, il messaggio viene inviato fino a quando il dispositivo diventa attivo. |
| `gcmSync` | La messaggistica del gruppo di dispositivi consente a ogni istanza dell'applicazione in un gruppo di riflettere lo stato di messaggistica più recente. |
| `gcmVisibility` | privato/pubblico - Visibilità di questa notifica, che influisce su come e quando le notifiche vengono rivelate su una schermata di blocco protetta. |
| `gcmPayload` | Payload JSON personalizzato che viene inviato come parte del messaggio di notifica. Ad esempio, `-p gcmPayload "{\"hi\":\"hello\"}"`. |
| `gcmPriority` | Imposta la priorità del messaggio. |
| `gcmSound` | Il file audio (sul dispositivo) che viene riprodotto quando arriva la notifica sul dispositivo. |
| `gcmTimeToLive` | Questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline. |
| `gcmStyleType` | Specifica il tipo di notifiche espandibili. I valori possibili sono `bigtext_notification`, `picture_notification`, `inbox_notification`. |
| `gcmStyleTitle` | Specifica il titolo della notifica. Il titolo viene visualizzato quando si espande la notifica. Il titolo deve essere specificato per tutte e tre le notifiche espandibili. |
| `gcmStyleUrl` | Un URL da cui deve essere ottenuta l'immagine per la notifica. Deve essere specificato per `picture_notification`. |
| `gcmStyleText` | Il testo di grandi dimensioni che deve essere visualizzato quando si espande una `bigtext_notification`. Deve essere specificato per `bigtext_notification`. |
| `gcmStyleLines` | Un array di stringhe che deve essere visualizzato in stile casella di posta in arrivo per `inbox_notification`. Deve essere specificato per `inbox_notification`. |
| `gcmLightsLedArgb` | Il colore del led. L'hardware fornisce l'approssimazione migliore. |
| `gcmLightsLedOnMs` | Il numero di millisecondi in cui il LED deve essere acceso mentre lampeggia. L'hardware fornisce l'approssimazione migliore. |
| `gcmLightsLedOffMs` | Il numero di millisecondi in cui il LED deve essere spento mentre lampeggia. L'hardware fornisce l'approssimazione migliore. |
| `apnsBadge` | Il numero da visualizzare come badge dell'icona dell'applicazione.|
| `apnsCategory` | L'identificativo di categoria da utilizzare per le notifiche di push interattive. |
| `apnsIosActionKey` | Il titolo per la chiave di azione. |
| `apnsPayload` | Payload JSON personalizzato che viene inviato come parte del messaggio di notifica. |
| `apnsType` | ['DEFAULT', 'MIXED', 'SILENT']. |
| `apnsSound` | Il nome del file audio nel bundle dell'applicazione. L'audio di questo file viene riprodotto come un avviso. |
| `apnsTitleLocKey` | La chiave per una stringa di titolo nel file `Localizable.strings` per la locale corrente. La stringa della chiave può essere formattata con gli specificatori %@ e %n$@ per recuperare le variabili specificate nell'array `titleLocArgs`. |
| `apnsLocKey` | Una chiave per una stringa di messaggio di avviso in un file `Localizable.strings` per la locale corrente (impostata mediante la preferenza della lingua dell'utente). La stringa della chiave può essere formattata con gli specificatori %@ e %n$@ per recuperare le variabili specificate nell'array `locArgs`. |
| `apnsLaunchImage` | Il nome file di un file di immagine nel bundle dell'applicazione, con o senza l'estensione del nome file. L'immagine viene utilizzata come immagine di avvio quando gli utenti premono il pulsante di azione o spostano il cursore di azione. |
| `pnsTitleLocArgs` | Valori di stringa variabili da visualizzare al posto degli specificatori di formato in `title-loc-key`. |
| `apnsLocArgs` | Valori di stringa variabili da visualizzare al posto degli specificatori di formato in `locKey`. |
| `apnstitle` | Il titolo delle notifiche rich push (supportato solo su iOS 10 e superiore). |
| `apnsSubtitle` | Il sottotitolo delle notifiche rich. (Supportato solo su iOS 10 e superiore).
| `apnsAttachmentUrl` | Il link ai supporti delle notifiche iOS (video, audio, GIF, immagini - Supportato solo su iOS 10 e superiore). |
| `fireFoxTitle` | Specifica il titolo da impostare per la notifica push web. |
| `fireFoxIconUrl` | L'URL dell'icona da impostare per la notifica push web. |
| `fireFoxTimeToLive` | Questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline. |
| `fireFoxPayload` | Payload JSON personalizzato che viene inviato come parte del messaggio di notifica. |
| `chromeTitle` | Specifica il titolo da impostare per la notifica push web. |
| `chromeIconUrl` | L'URL dell'icona da impostare per la notifica push web. |
| `chromeTimeToLive` | Questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline. |
| `chromePayload` | Payload JSON personalizzato che viene inviato come parte del messaggio di notifica. |
| `safariTitle` | Specifica il titolo da impostare per le notifiche di push Safari |
| `safariUrlArgs` | Gli argomenti URL che devono essere utilizzati con questa notifica. Questi argomenti devono essere forniti sotto forma di un array JSON. |
| `safariAction` | L'etichetta del pulsante di azione. |
| `chromeAppExtTitle` | Specifica il titolo da impostare per la notifica push web. |
| `chromeAppExtCollapseKey` | Questo parametro identifica un gruppo di messaggi. |
| `chromeAppExtDelayWhileIdle` | Se questo parametro è impostato su true, indica che il messaggio non viene inviato finché il dispositivo non diventa attivo. |
| `chromeAppExtIconUrl` | L'URL dell'icona da impostare per la notifica push web. |
| `chromeAppExtTimeToLive` | Questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline. |
| `chromeAppExtPayload` | Payload JSON personalizzato che viene inviato come parte del messaggio di notifica. |

### Notifica di push di esempio
{: #pkg_push_ex}

Vedi il seguente esempio per inviare una notifica di push dal pacchetto Push notification.

Invia una notifica di push attraverso l'azione **sendMessage**nel bind di pacchetto che hai creato in precedenza. Assicurati di sostituire `/myNamespace/myPush` con il tuo nome pacchetto.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

**Output di esempio**
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

## Invio di notifiche di push sugli eventi dei dispositivi mobili
{: #pkg_push_mobile}

Questo pacchetto pre-installato non è disponibile nella regione Tokyo.
{: tip}

Scopri come configurare il servizio Push Notification per attivare un trigger quando si verifica un'attività del dispositivo, come la registrazione/l'annullamento della registrazione o la sottoscrizione/l'annullamento della sottoscrizione del dispositivo, in un'applicazione specificata.
{: shortdesc}

### Parametri
{: #pkg_push_mobile_params}

I parametri di `/whisk.system/pushnotifications/webhook` sono i seguenti:

| Parametro | Descrizione |
|--- | --- |
| `appId` | Il GUID applicazione di {{site.data.keyword.cloud_notm}}. |
| `appSecret` | L'`appSecret` del servizio {{site.data.keyword.cloud_notm}} Push Notification. |
| `events` | `onDeviceRegister`, `onDeviceUnregister`, `onDeviceUpdate`, `onSubscribe`, `onUnsubscribe`. Puoi utilizzare il carattere jolly "`*`" per ricevere una notifica per tutti gli eventi. |

### Attivazione di un evento trigger in caso di attività del servizio Push Notification
{: #pkg_push_mobile_trigger}

Per creare un trigger che viene attivato ogni volta che un nuovo dispositivo viene registrato con l'applicazione del servizio Push Notification, vedi il seguente esempio:

1. Crea un bind di pacchetto configurato per il tuo servizio Push Notification utilizzando il tuo `appId` e `appSecret`.
  ```
  ibmcloud fn package bind /whisk.system/pushnotifications myNewDeviceFeed --param appID myapp --param appSecret myAppSecret --param events onDeviceRegister
  ```
  {: pre}

2. Crea un trigger per il tipo di evento `onDeviceRegister` del servizio Push Notification utilizzando il tuo feed `myPush/webhook`.
  ```
  ibmcloud fn trigger create myPushTrigger --feed myPush/webhook --param events onDeviceRegister
  ```
  {: pre}

3. Puoi creare una regola che invia un messaggio ogni volta che viene registrato un nuovo dispositivo. Crea una regola utilizzando l'azione e il trigger precedenti.
  ```
  ibmcloud fn rule create --enable myRule myPushTrigger sendMessage
  ```
  {: pre}

4. Controlla i risultati utilizzando il comando `ibmcloud fn activation poll`.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Registra un dispositivo nella tua applicazione {{site.data.keyword.cloud_notm}}. Puoi vedere che `rule`, `trigger` e `action` vengono eseguiti nel [dashboard](https://cloud.ibm.com/openwhisk/dashboard){: external} {{site.data.keyword.openwhisk}}.

  **Output**
  
  L'azione invia una notifica di push.


## Invio di notifiche di push o creazione, aggiornamento ed eliminazione di webhook
{: #pkg_push_mobile_send}

Il pacchetto {{site.data.keyword.mobilepushshort}} installabile fornisce una serie di azioni per interagire con le istanze del servizio {{site.data.keyword.mobilepushfull}}. Queste azioni ti consentono di inviare un messaggio oppure creare, aggiornare o eliminare un webhook.
{: shortdesc}

Il pacchetto {{site.data.keyword.mobilepushshort}} include le seguenti azioni:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/push-notifications` | Pacchetto | `apikey`, `appGuid` | Lavorare con un'istanza {{site.data.keyword.mobilepushshort}}. |
| `/push-notifications/send-message` | Azione | `text`, `url`, `deviceIds`, `platforms`, `userIds`, `tagNames`, `gcmCollapseKey`, `gcmCategory`, `gcmIcon`, `gcmDelayWhileIdle`, `gcmSync`, `gcmVisibility`, `gcmPayload`, `gcmPriority`, `gcmSound`, `gcmTimeToLive`, `gcmStyleType`, `gcmStyleTitle`, `gcmStyleUrl`, `gcmStyleText`, `gcmStyleLines`, `gcmLightsLedArgb`, `gcmLightsLedOnMs`, `gcmLightsLedOffMs`, `apnsBadge`, `apnsCategory`, `apnsIosActionKey`, `apnsPayload`, `apnsType`, `apnsSound`, `apnsTitleLocKey`, `apnsLocKey`, `apnsLaunchImage`, `apnsTitleLocArgs`, `apnsLocArgs`, `apnstitle`, `apnsSubtitle`, `apnsAttachmentUrl`, `fireFoxTitle`, `fireFoxIconUrl`, `fireFoxTimeToLive`, `fireFoxPayload`, `safariTitle`, `safariUrlArgs`, `safariAction`, `chromeTitle`, `chromeIconUrl`, `chromeTimeToLive`, `chromePayload`, `chromeAppExtTitle`, `chromeAppExtCollapseKey`, `chromeAppExtDelayWhileIdle`, `chromeAppExtIconUrl`, `chromeAppExtTimeToLive`, `chromeAppExtPayload` | Inviare una notifica di push a uno o più dispositivi specificati. |
| `/push-notifications/webhook` | Azione | `events` | Attivare gli eventi trigger per le attività del dispositivo (registrazione, annullamento della registrazione, sottoscrizione o annullamento della sottoscrizione del dispositivo) sul servizio push. |

### Creazione di un'istanza del servizio {{site.data.keyword.mobilepushshort}}
{: #service_instance_push}

Prima di installare il pacchetto, devi creare un'istanza {{site.data.keyword.mobilepushshort}}.

1. [Crea un'istanza del servizio {{site.data.keyword.mobilepushshort}}](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1a).

2. [Crea una serie di credenziali del servizio](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_1#push_step_1) per l'istanza del servizio Push Notifications.

3. [Configura l'istanza del servizio {{site.data.keyword.mobilepushshort}}](/docs/services/mobilepush?topic=mobile-pushnotification-push_step_2#push_step_2).

### Installazione del pacchetto {{site.data.keyword.mobilepushshort}}
{: #pkg_push_mobile_install}

Una volta che disponi di un'istanza del servizio {{site.data.keyword.mobilepushshort}}, utilizza l'IU o la CLI {{site.data.keyword.openwhisk}} per installare il pacchetto {{site.data.keyword.mobilepushshort}} nel tuo spazio dei nomi.

### Installazione dalla CLI {{site.data.keyword.openwhisk_short}}
{: #pkg_push_mobile_cli}

Prima di iniziare:
  1. [Installa il plug-in {{site.data.keyword.openwhisk_short}} per la CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Per installare il pacchetto {{site.data.keyword.mobilepushshort}}:

1. Clona il repository del pacchetto {{site.data.keyword.mobilepushshort}}.
    ```
    git clone https://github.com/ibm-functions/package-push-notifications.git
    ```
    {: pre}

2. Vai alla directory `runtimes/nodejs`.
    ```
    cd package-push-notifications/runtimes/nodejs
    ```
    {: pre}

3. Distribuisci il pacchetto.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Verifica che il pacchetto `push-notifications` venga aggiunto al tuo elenco pacchetti.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Output**
    ```
    packages
    /myOrg_mySpace/push-notifications private
    ```
    {: screen}

5. Esegui il bind delle credenziali dall'istanza del servizio {{site.data.keyword.mobilepushshort}} che hai creato al pacchetto.
    ```
    ibmcloud fn service bind imfpush push-notifications
    ```
    {: pre}

    **Output di esempio**
      ```
    Credentials 'Credentials-1' from 'imfpush' service instance 'Push-Notifications-r1' bound to 'push-notifications'.
    ```
    {: screen}

6. Verifica che il pacchetto sia configurato con le tue credenziali dell'istanza del servizio {{site.data.keyword.mobilepushshort}}.
    ```
    ibmcloud fn package get /myOrg_mySpace/push-notifications parameters
    ```
    {: pre}

    **Output di esempio**
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

### Installazione dall'IU {{site.data.keyword.openwhisk_short}}
{: #pkg_push_mobile_ui}

1. Nella console {{site.data.keyword.openwhisk_short}}, vai alla [pagina di creazione](https://cloud.ibm.com/openwhisk/create){: external}.

2. Utilizzando il menu a discesa dello spazio dei nomi nell'angolo superiore destro, seleziona lo spazio dei nomi in cui vuoi installare il pacchetto {{site.data.keyword.cos_full_notm}}.

3. Fai clic su **Installa pacchetti**.

4. Fai clic sul gruppo di pacchetti **IBM {{site.data.keyword.mobilepushshort}}** e poi sul pacchetto **IBM {{site.data.keyword.mobilepushshort}}**.

5. Nella sezione Runtime disponibili, seleziona nodeJS dall'elenco a discesa e fai clic su **Installa**.

6. Dopo che il pacchetto è stato installato, verrai reindirizzato alla pagina Azioni e puoi cercare il tuo nuovo pacchetto, che è denominato **push-notifications**.

7. Per utilizzare le azioni nel pacchetto **push-notifications**, devi eseguire il bind delle credenziali del servizio alle azioni.
  * Per eseguire il bind delle credenziali del servizio a tutte le azioni nel pacchetto, attieniti ai passi 5 e 6 nelle istruzioni della CLI sopra elencate.
  * Per eseguire il bind delle credenziali del servizio a singole azioni, completa la seguente procedura nell'IU. **Nota**: devi completare la seguente procedura per ogni azione che desideri utilizzare.
    1. Fai clic su un'azione dal pacchetto **push-notifications** che vuoi utilizzare. Viene aperta la pagina dei dettagli per tale azione.
    2. Nella navigazione sulla sinistra, fai clic sulla sezione **Parametri**.
    3. Immetti un nuovo **parametro**. Per la chiave, immetti `__bx_creds`. Per il valore, incolla l'oggetto JSON delle credenziali del servizio dall'istanza del servizio che hai creato in precedenza.

### Invia una notifica push
{: #pkg_push_mobile_sendmsg}

Per inviare un messaggio tramite il servizio push notification utilizzando l'azione `send-message`:
```
ibmcloud fn action invoke push-notifications/send-message --blocking --result --param messageText "Let's code something" --param messageUrl "http://developer.ibm.com"
```
{: pre}

**Output di esempio**
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

### Crea un webhook
{: #pkg_push_mobile_hook}

Per creare un webhook per il servizio {{site.data.keyword.mobilepushshort}} per gli eventi `onDeviceRegister`:

```
 ibmcloud fn action invoke push-notifications/webhook --blocking --param triggerName "/myPackage/myTrigger" --param events onDeviceRegister
```
{: pre}

**Output di esempio**
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



