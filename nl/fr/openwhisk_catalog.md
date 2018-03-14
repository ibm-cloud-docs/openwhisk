---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Packages préinstallés
{: #openwhisk_ecosystem}

Dans {{site.data.keyword.openwhisk}}, un catalogue de packages permet d'améliorer facilement votre application en ajoutant des fonctions utiles. Il permet également d'accéder à des services externes dans l'écosystème. Cloudant, Message Hub, Watson, The Weather Company, Slack, Github, etc. sont des exemples de service externe compatibles avec {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Le catalogue est disponible sous forme de packages dans les espaces de nom `/whisk.system` et `/watson-iot`. Pour plus d'informations, voir [Exploration des packages](openwhisk_packages.html#browse-packages).

## Packages du catalogue
{: notoc}

| Package | Description |
| --- | --- |
| [/whisk.system/alarms](./openwhisk_alarms.html) | Package permettant de créer des déclencheurs périodiques |
| [/whisk.system/cloudant](./openwhisk_cloudant.html) | Package permettant d'utiliser un service [Cloudant noSQL DB](https://console.ng.bluemix.net/docs/services/Cloudant/index.html) |
| [/whisk.system/github](./openwhisk_github.html) | Package permettant de créer des déclencheurs webhook pour [GitHub](https://developer.github.com/) |
| [/whisk.system/messaging](./openwhisk_messagehub.html) | Package permettant d'utiliser le service [Message Hub](https://console.ng.bluemix.net/docs/services/MessageHub/index.html) |
| [/whisk.system/pushnotifications](./openwhisk_pushnotifications.html) | Package permettant d'utiliser le service [Push Notification](https://console.ng.bluemix.net/docs/services/mobilepush/index.html) |
| [/whisk.system/slack](./openwhisk_slack.html) | Package permettant d'effectuer des publications sur les [API Slack](https://api.slack.com/) |
| [/whisk.system/watson-translator](./openwhisk_watson_translator.html) | Package pour traduction de texte [ et identification de la langue](https://www.ibm.com/watson/developercloud/language-translator.html) |
| [/whisk.system/watson-speechToText](./openwhisk_watson_speechtotext.html) | Package pour conversion de paroles [ en texte](https://www.ibm.com/watson/developercloud/speech-to-text.html) |
| [/whisk.system/watson-textToSpeech](./openwhisk_watson_texttospeech.html) | Package pour conversion de texte [ en paroles](https://www.ibm.com/watson/developercloud/text-to-speech.html) |
| [/watson-iot/iot-gateway](https://console.stage1.bluemix.net/docs/services/IoT/gateways/iotgw.html) | Package permettant d'utiliser le service [Watson IoT Platform Gateway](https://console.stage1.bluemix.net/docs/services/IoT/index.html) |
| [/whisk.system/weather](./openwhisk_weather.html) | Package permettant d'utiliser le service [Weather Company Data](https://console.ng.bluemix.net/docs/services/Weather/index.html) |
| [/whisk.system/websocket](./openwhisk_websocket.html) | Package permettant d'utiliser le serveur [Web Socket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) |
