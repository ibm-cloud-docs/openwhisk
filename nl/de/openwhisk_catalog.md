---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Vorinstallierte Pakete
{: #openwhisk_ecosystem}

In {{site.data.keyword.openwhisk}} stellt ein Katalog von Paketen eine einfache Methode bereit, Ihre App um nützliche Funktionen zu erweitern und auf externe Services im direkten Geschäftsumfeld ('Ökosystem') zuzugreifen. Zu den externen Services, die für {{site.data.keyword.openwhisk_short}} eingerichtet sind, gehören zum Beispiel Cloudant, Message Hub, Watson, The Weather Company, Slack und GitHub.
{: shortdesc}

Der Katalog ist in Form von Paketen in den Namensbereichen `/whisk.system` und `/watson-iot` verfügbar. Weitere Informationen finden Sie in [Pakete durchsuchen](openwhisk_packages.html#browse-packages).

## Katalogpakete
{: notoc}

| Paket | Beschreibung |
| --- | --- |
| [/whisk.system/alarms](./openwhisk_alarms.html) | Paket zum Erstellen regelmäßig aktivierter Auslöser |
| [/whisk.system/cloudant](./openwhisk_cloudant.html) | Paket zum Arbeiten mit dem Service [Cloudant noSQL DB](https://console.ng.bluemix.net/docs/services/Cloudant/index.html) |
| [/whisk.system/github](./openwhisk_github.html) | Paket zum Erstellen von Webhook-Auslösern für [GitHub](https://developer.github.com/) |
| [/whisk.system/messaging](./openwhisk_messagehub.html) | Paket zum Arbeiten mit dem [Message Hub](https://console.ng.bluemix.net/docs/services/MessageHub/index.html)-Service |
| [/whisk.system/pushnotifications](./openwhisk_pushnotifications.html) | Paket zum Arbeiten mit dem [Push Notifications](https://console.ng.bluemix.net/docs/services/mobilepush/index.html)-Service |
| [/whisk.system/slack](./openwhisk_slack.html) | Paket zum Senden an die [Slack-APIs](https://api.slack.com/) |
| [/whisk.system/watson-translator](./openwhisk_watson_translator.html) | Paket für [Textübersetzung und Spracherkennung](https://www.ibm.com/watson/developercloud/language-translator.html) |
| [/whisk.system/watson-speechToText](./openwhisk_watson_speechtotext.html) | Paket zum Umwandeln von [Sprache in Text](https://www.ibm.com/watson/developercloud/speech-to-text.html) |
| [/whisk.system/watson-textToSpeech](./openwhisk_watson_texttospeech.html) | Paket zum Umwandeln von [Text in Sprache](https://www.ibm.com/watson/developercloud/text-to-speech.html) |
| [/watson-iot/iot-gateway](https://console.stage1.bluemix.net/docs/services/IoT/gateways/iotgw.html) | Paket zum Arbeiten mit dem [Watson IoT Platform Gateway](https://console.stage1.bluemix.net/docs/services/IoT/index.html)-Service |
| [/whisk.system/weather](./openwhisk_weather.html) | Paket zum Arbeiten mit dem [Weather Company Data](https://console.ng.bluemix.net/docs/services/Weather/index.html)-Service |
| [/whisk.system/websocket](./openwhisk_websocket.html) | Paket zum Arbeiten mit einem [Web-Socket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)-Server |
