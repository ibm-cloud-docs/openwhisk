---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Package Watson : Text to Speech
{: #openwhisk_catalog_watson_texttospeech}

Le package `/whisk.system/watson-textToSpeech` permet d'appeler diverses API Watson pour convertir le texte en parole.
{: shortdesc}

Le package inclut les actions ci-dessous.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | package | username, password | Package pour la conversion de texte en paroles |
| `/whisk.system/watson-textToSpeech/textToSpeech` | action | payload, voice, accept, encoding, username, password | Convertir le texte en contenu audio |

**Remarque** : Le package `/whisk.system/watson` est obsolète, y compris l'action `/whisk.system/watson/textToSpeech`.

## Configuration du package Watson Text to Speech dans {{site.data.keyword.Bluemix_notm}}

Si vous utilisez {{site.data.keyword.openwhisk}} depuis {{site.data.keyword.Bluemix_notm}}, les liaisons de package sont automatiquement créées pour vos instances de service {{site.data.keyword.Bluemix_notm}} Watson.

1. Créez une instance de service Watson Text to Speech dans votre [tableau de bord](http://console.bluemix.net) {{site.data.keyword.Bluemix_notm}}.

  Mémorisez le nom de l'instance de service ainsi que l'organisation et l'espace {{site.data.keyword.Bluemix_notm}} dans lesquels vous vous trouvez.

2. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour l'instance de service Watson que vous avez créée.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Exemple de sortie :
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Affichez la liste des packages pour vérifier que la liaison de package a bien été créée :
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## Configuration d'un package Watson Text to Speech en dehors d'{{site.data.keywrod.Bluemix_notm}}

Si vous n'utilisez pas {{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}} ou si vous voulez configurer Watson Text to Speech en dehors d'{{site.data.keyword.Bluemix_notm}}, vous devez créer une liaison de package manuellement pour votre service Watson Text to Speech. Vous avez besoin du nom d'utilisateur du service et du mot de passe du service Watson Text to Speech.

Créez une liaison de package configurée pour votre service Watson Text to Speech.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Conversion de texte en paroles

L'action `/whisk.system/watson-textToSpeech/textToSpeech` convertit un texte en séquence audio. Les paramètres sont les suivants :

- `username` : nom d'utilisateur de l'API Watson.
- `password` : mot de passe de l'API Watson.
- `payload` : texte à convertir en paroles.
- `voice` : voix du conférencier.
- `accept` : format du fichier vocal.
- `encoding` : codage du fichier binaire vocal.

Appelez l'action **textToSpeech** dans votre liaison de package pour convertir le texte.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

Exemple de sortie :
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}
