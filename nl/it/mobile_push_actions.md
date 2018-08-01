---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Push mobili
{: #openwhisk_catalog_pushnotifications}

Scopri come creare un bind di pacchetto Push e inviare una semplice notifica di push utilizzando il pacchetto `/whisk.system/pushnotifications`, che ti offre la possibilità di lavorare con un servizio push.
{: shortdesc}

Il pacchetto include le azioni e i feed che seguono:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/pushnotifications` | pacchetto | appId, appSecret, admin_url | Lavorare con il servizio push. |
| `/whisk.system/pushnotifications/sendMessage` | azione | text, url, deviceIds, platforms, userIds, tagNames, gcmCollapseKey, gcmCategory, gcmIcon, gcmDelayWhileIdle, gcmSync, gcmVisibility, gcmPayload, gcmPriority, gcmSound, gcmTimeToLive, gcmStyleType, gcmStyleTitle, gcmStyleUrl, gcmStyleText, gcmStyleLines, gcmLightsLedArgb, gcmLightsLedOnMs, gcmLightsLedOffMs, apnsBadge, apnsCategory, apnsIosActionKey, apnsPayload, apnsType, apnsSound, apnsTitleLocKey, apnsLocKey, apnsLaunchImage, apnsTitleLocArgs, apnsLocArgs, apnstitle, apnsSubtitle, apnsAttachmentUrl, fireFoxTitle, fireFoxIconUrl, fireFoxTimeToLive, fireFoxPayload, safariTitle, safariUrlArgs, safariAction, chromeTitle, chromeIconUrl, chromeTimeToLive, chromePayload, chromeAppExtTitle, chromeAppExtCollapseKey, chromeAppExtDelayWhileIdle, chromeAppExtIconUrl, chromeAppExtTimeToLive, chromeAppExtPayload | Inviare una notifica di push a uno o più dispositivi specificati. |
| `/whisk.system/pushnotifications/webhook` | feed | events | Attivare gli eventi trigger per le attività del dispositivo (registrazione, annullamento della registrazione, sottoscrizione o annullamento della sottoscrizione del dispositivo) sul servizio push. |

Per informazioni sull'attivazione degli eventi trigger quando si verifica un'attività del dispositivo, vedi l'argomento [Push mobili sugli eventi del dispositivo](./openwhisk_pushnotifications.html).

## Creazione di un bind di pacchetto Push
{: #create_push_binding}

Per creare un bind di pacchetto Push Notification, devi specificare i seguenti parametri:

-  **appId**: il **GUID applicazione** di {{site.data.keyword.Bluemix}}.
-  **appSecret**: il **Segreto applicazione** del servizio {{site.data.keyword.Bluemix_notm}} Push Notification.

Per creare un bind di pacchetto, vedi la seguente procedura:

1. Crea un'applicazione {{site.data.keyword.Bluemix_notm}} nel [Dashboard {{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

2. Inizializza il servizio Push Notification ed esegui il bind del servizio all'applicazione {{site.data.keyword.Bluemix_notm}}.

3. Configura l'[applicazione Notifica di push](https://console.bluemix.net/docs/services/mobilepush/index.html).

  Assicurati di ricordare il **GUID applicazione** e il **Segreto applicazione** dell'applicazione {{site.data.keyword.Bluemix_notm}} che hai creato.

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

  Output di esempio:
  ```
  packages
  /myNamespace/myPush private binding
  ```
  {: screen}

## Parametri delle notifiche di push
{: #push_parameters}

L'azione `/whisk.system/pushnotifications/sendMessage` invia notifiche di push ai dispositivi registrati. I parametri sono i seguenti:
- `text`: il messaggio di notifica da mostrare all'utente. Ad esempio, `-p text "Hi, OpenWhisk send a notification"`.
- `url`: un URL che può essere inviato con l'avviso. Ad esempio, `-p url "https:\\www.w3.ibm.com"`.
- `apiHost`: una stringa facoltativa che specifica l'host API. Il valore predefinito è `mobile.ng.bluemix.net`.  Ad esempio: `-p apiHost "mobile.eu-gb.bluemix.net"`
- `deviceIds`: l'elenco dei dispositivi specificati. Ad esempio, `-p deviceIds ["deviceID1"]`.
- `platforms`: invia notifiche ai dispositivi delle piattaforme specificate. 'A' per i dispositivi Apple (iOS) e 'G' per i dispositivi Google (Android). Ad esempio, `-p platforms ["A"]`.
- `userIds`: invia notifiche ai dispositivi degli utenti specificati. Ad esempio, `-p userIds "[\"testUser\"]"`
- `tagNames`: invia notifiche ai dispositivi sottoscritti a una di queste tag. Ad esempio, `-p tagNames "[\"tag1\"]"`.
- `gcmCollapseKey`: questo parametro identifica un gruppo di messaggi.
- `gcmCategory`: l'identificativo della categoria da utilizzare per le notifiche di push interattive.
- `gcmIcon`: specifica il nome dell'icona da visualizzare per la notifica. Assicurati che l'icona sia già nel pacchetto dell'applicazione client.
- `gcmDelayWhileIdle`: quando questo parametro è impostato su true, il messaggio non viene inviato finché il dispositivo non diventa attivo.
- `gcmSync`: la messaggistica dei gruppi di dispositivi consente a tutte le istanze dell'applicazione in un gruppo di riflettere lo stato di messaggistica più recente.
- `gcmVisibility`: privato/pubblico - Visibilità di questa notifica, che influisce su come e quando le notifiche vengono rivelate su una schermata di blocco protetta.
- `gcmPayload`: payload JSON personalizzato che viene inviato come parte del messaggio di notifica. Ad esempio, `-p gcmPayload "{\"hi\":\"hello\"}"`
- `gcmPriority`: imposta la priorità del messaggio.
- `gcmSound`: il file audio (sul dispositivo) riprodotto quando arriva la notifica sul dispositivo.
- `gcmTimeToLive`: questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline.
- `gcmStyleType`: specifica il tipo di notifiche espandibili. I valori possibili sono `bigtext_notification`, `picture_notification`, `inbox_notification`.
- `gcmStyleTitle`: specifica il titolo della notifica. Il titolo viene visualizzato quando si espande la notifica. Il titolo deve essere specificato per tutte e tre le notifiche espandibili.
- `gcmStyleUrl`: un URL da cui deve essere ottenuta l'immagine per la notifica. Deve essere specificato per `picture_notification`.
- `gcmStyleText`: il testo di grandi dimensioni che deve essere visualizzato quando si espande una `bigtext_notification`. Deve essere specificato per `bigtext_notification`.
- `gcmStyleLines`: un array di stringhe che deve essere visualizzato in stile casella di posta in arrivo per `inbox_notification`. Deve essere specificato per `inbox_notification`.
- `gcmLightsLedArgb`: il colore del led. L'hardware fornisce l'approssimazione migliore.
- `gcmLightsLedOnMs`: il numero di millisecondi per accendere il LED mentre lampeggia. L'hardware fornisce l'approssimazione migliore.
- `gcmLightsLedOffMs`: il numero di millisecondi per spegnere il LED mentre lampeggia. L'hardware fornisce l'approssimazione migliore.
- `apnsBadge`: il numero da visualizzare come badge dell'icona dell'applicazione.
- `apnsCategory`: l'identificativo della categoria da utilizzare per le notifiche di push interattive.
- `apnsIosActionKey`: il titolo per la chiave di azione.
- `apnsPayload`: payload JSON personalizzato che viene inviato come parte del messaggio di notifica.
- `apnsType`: ['DEFAULT', 'MIXED', 'SILENT'].
- `apnsSound`: il nome del file audio nel bundle dell'applicazione. L'audio di questo file viene riprodotto come un avviso.
- `apnsTitleLocKey`: la chiave per una stringa di titolo nel file `Localizable.strings` per la localizzazione corrente. La stringa della chiave può essere formattata con gli specificatori  %@ e %n$@ per recuperare le variabili specificate nell'array `titleLocArgs`.
- `apnsLocKey`: una chiave per una stringa di messaggio di avviso nel file `Localizable.strings` per la localizzazione corrente (impostata mediante la preferenza della lingua dell'utente). La stringa della chiave può essere formattata con gli specificatori  %@ e %n$@ per recuperare le variabili specificate nell'array locArgs.
- `apnsLaunchImage`: il nome di un file di immagine nel bundle dell'applicazione, con o senza estensione del nome file. L'immagine viene utilizzata come immagine di avvio quando gli utenti premono il pulsante di azione o spostano il cursore di azione.
- `pnsTitleLocArgs`: valori di stringa variabili da visualizzare al posto degli specificatori di formato in `title-loc-key`.
- `apnsLocArgs`: valori di stringa variabili da visualizzare al posto degli specificatori di formato in `locKey`.
- `apnstitle`: il titolo delle notifiche rich push (supportato solo su iOS 10 e superiore).
- `apnsSubtitle`: il sottotitolo delle notifiche rich. (Supportato solo su iOS 10 e superiore).
- `apnsAttachmentUrl`: il link ai supporti delle notifiche iOS (video, audio, GIF, immagini - Supportato solo su iOS 10 e superiore).
- `fireFoxTitle`: specifica il titolo da impostare per la notifica push web.
- `fireFoxIconUrl`: l'URL dell'icona da impostare per la notifica push web.
- `fireFoxTimeToLive`: questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline.
- `fireFoxPayload`: payload JSON personalizzato che viene inviato come parte del messaggio di notifica.
- `chromeTitle`: specifica il titolo da impostare per la notifica push web.
- `chromeIconUrl`: l'URL dell'icona da impostare per la notifica push web.
- `chromeTimeToLive`: questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline.
- `chromePayload`: payload JSON personalizzato che viene inviato come parte del messaggio di notifica.
- `safariTitle`: specifica il titolo da impostare per le notifiche di push Safari.
- `safariUrlArgs`: gli argomenti URL che devono essere utilizzati con questa notifica. Questi argomenti devono essere forniti sotto forma di un array JSON.
- `safariAction`: l'etichetta del pulsante di azione.
- `chromeAppExtTitle`: specifica il titolo da impostare per la notifica push web.
- `chromeAppExtCollapseKey`: questo parametro identifica un gruppo di messaggi.
- `chromeAppExtDelayWhileIdle`: quando questo parametro è impostato su true, indica che il messaggio non viene inviato finché il dispositivo non diventa attivo.
- `chromeAppExtIconUrl`: l'URL dell'icona da impostare per la notifica push web.
- `chromeAppExtTimeToLive`: questo parametro specifica per quanto tempo (in secondi) il messaggio viene conservato nell'archiviazione GCM se il dispositivo è offline.
- `chromeAppExtPayload`: payload JSON personalizzato che viene inviato come parte del messaggio di notifica.

## Invio di notifiche di push
{: #send_push_notifications}

Vedi il seguente esempio per inviare una notifica di push dal pacchetto Push notification.

Invia una notifica di push utilizzando l'azione **sendMessage** nel bind di pacchetto che hai creato in precedenza. Assicurati di sostituire `/myNamespace/myPush` con il tuo nome pacchetto.
```
ibmcloud fn action invoke /myNamespace/myPush/sendMessage --blocking --result -p url https://example.com -p text "this is my message" -p sound soundFileName -p deviceIds "[\"T1\",\"T2\"]"
```
{: pre}

Output di esempio:
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
