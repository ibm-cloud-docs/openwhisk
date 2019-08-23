---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, functions, packages

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


# Text to Speech
{: #pkg_text_to_speech}

## Options du package

| Package | Disponibilité | Description |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | Préinstallé (non disponible à Tokyo) | Package pour la conversion de texte en paroles |
| [`text-to-speech-v1`](#text_to_speech_ins) | Installable | Gestion du service {{site.data.keyword.texttospeechshort}}. |

## Watson Text to Speech
{: #text_to_speech}

Ce package préinstallé n'est pas disponible dans la région Tokyo. Consultez le package installable [Text to Speech](#text_to_speech_ins) en utilisant l'authentification IAM.
{: tip}

Le package `/whisk.system/watson-textToSpeech` permet d'appeler diverses API Watson pour convertir le texte en parole.
{: shortdesc}

Le package inclut les actions ci-dessous.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Package | `username`, `password` | Package pour la conversion de texte en paroles. |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Action | `payload`, `voice`, `accept`, `encoding`, `username`, `password` | Convertir du texte en contenu audio. |

Le package `/whisk.system/watson` est obsolète, y compris l'action `/whisk.system/watson/textToSpeech`. Consultez plutôt le [package installable {{site.data.keyword.texttospeechshort}}](#text_to_speech_ins).
{: deprecated}

### Configuration du package {{site.data.keyword.texttospeechshort}} dans {{site.data.keyword.cloud_notm}}

Si vous utilisez {{site.data.keyword.openwhisk}} depuis {{site.data.keyword.cloud_notm}}, les liaisons de package sont automatiquement créées pour vos instances de service {{site.data.keyword.cloud_notm}} Watson.

1. Créez une instance de service {{site.data.keyword.texttospeechshort}} dans votre [tableau de bord](https://cloud.ibm.com){: external} {{site.data.keyword.cloud_notm}}. Mémorisez le nom de l'instance de service ainsi que l'organisation et l'espace {{site.data.keyword.cloud_notm}} dans lesquels vous vous trouvez.

2. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour l'instance de service Watson que vous avez créée.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Exemple de sortie**
  ```
  created bindings:
  Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Affichez la liste des packages pour vérifier que la liaison de package a bien été créée.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Exemple de sortie**
  ```
  packages
  /myOrg_mySpace/Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### Configuration d'un package {{site.data.keyword.texttospeechshort}} en dehors d'{{site.data.keyword.cloud_notm}}

Si vous n'utilisez pas {{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.cloud_notm}} ou si vous voulez configurer {{site.data.keyword.texttospeechshort}} en dehors d'{{site.data.keyword.cloud_notm}}, vous devez créer une liaison de package manuellement pour votre service {{site.data.keyword.texttospeechshort}}. Vous avez besoin du nom d'utilisateur du service {{site.data.keyword.texttospeechshort}}, ainsi que du mot de passe. 

Créez une liaison de package configurée pour votre service {{site.data.keyword.texttospeechshort}}.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username <username> -p password <password>
```
{: pre}

### Conversion de texte en paroles

L'action `/whisk.system/watson-textToSpeech/textToSpeech` convertit un texte en séquence audio. Les paramètres ci-après sont pris en charge.

| `username` | Nom d'utilisateur de l'API Watson. |
| `password` | Mot de passe de l'API Watson. |
| `payload` | Texte à convertir en paroles. |
| `voice` | Voix du conférencier. |
| `accept` | Format du fichier voix. |
| `encoding` | Codage du fichier binaire vocal. |

Testez l'action `textToSpeech` dans votre liaison de package pour convertir le texte.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

**Exemple de sortie**
```
{
  "payload": "<base64 encoding of a .wav file>"
}
```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

Le service installable {{site.data.keyword.texttospeechfull}} fournit une API avec les fonctions de reconnaissance vocale d'IBM pour synthétiser du texte sous forme de voix naturelle dans diverses langues, dialectes et voix.
{:shortdesc}

Le service prend en charge au moins une voix masculine ou féminine, parfois les deux, pour chaque langue. Le contenu audio est diffusé jusqu'au client dans un délai minimal. Pour plus d'informations sur le service, consultez la [documentation IBM Cloud](/docs/services/text-to-speech?topic=text-to-speech-about).

Le package {{site.data.keyword.texttospeechshort}} contient les entités suivantes. Vous trouverez des informations supplémentaires dans la référence d'API {{site.data.keyword.texttospeechshort}} en cliquant sur le nom d'entité.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Gestion du service {{site.data.keyword.texttospeechshort}}. |
| [`get-voice`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `voice`, `customization_id` | Obtention d'une voix. |
| [`list-voices`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Affichage de la liste des voix. |
| [`synthesize`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `accept`, `voice`, `customization_id` | Synthétisation audio. |
| [`get-pronunciation`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `voice`, `format`, `customization_id` | Obtention de la prononciation. |
| [`create-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `language`, `description` | Création d'un modèle personnalisé. |
| [`delete-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Suppression d'un modèle personnalisé. |
| [`get-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Obtention d'un modèle personnalisé. |
| [`list-voice-models`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models){: external} | Action | `username`,`password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Affichage de la liste des modèles personnalisés. |
| [`update-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `name`, `description`, `words` | Mise à jour d'un modèle personnalisé. |
| [`add-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word`, `translation`, `part_of_speech` | Ajout d'un mot personnalisé. |
| [`add-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words`  | Ajout de mots personnalisés. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Suppression d'un mot personnalisé. |
| [`get-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Obtention d'un mot personnalisé. |
| [`list-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Affichage de la liste de mots personnalisés. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data){: external} | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Suppression de données de libellé. |

### Création d'une instance de service {{site.data.keyword.texttospeechshort}}
{: #service_instance_texttospeech}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.texttospeechshort}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.texttospeechshort}}](https://cloud.ibm.com/catalog/services/text_to_speech){: external}.
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

### Installation du package {{site.data.keyword.texttospeechshort}}
{: #install_texttospeech}

Dès que vous disposez d'une instance de service {{site.data.keyword.texttospeechshort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.texttospeechshort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

**Avant de commencer**
[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.texttospeechshort}}, exécutez les commandes suivantes. 

1. Clonez le référentiel de package {{site.data.keyword.texttospeechshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
    ```
    {: pre}

3. Vérifiez que le package est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Sortie**
    ```
    packages
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.texttospeechshort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    Selon la région où vous avez créé l'instance de service, celle-ci peut être nommée différemment car il s'agit d'un service IAM. Si la commande ci-dessus échoue, utilisez le nom de service suivant pour la commande bind :
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}

    **Exemple de sortie**
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    **Exemple de sortie**
    ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}}
{: #texttospeech_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer](https://cloud.ibm.com/openwhisk/create){: external}.

2. Sélectionnez l'espace de nom dans lequel installer le package en utilisant le menu déroulant espace de nom. 

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Text To Speech**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé `text-to-speech-v1`.

7. Pour utiliser les actions du package `text-to-speech-v1`, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes [4 et 6 dans les instructions de l'interface CLI](#texttospeech_cli). 
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur.

  Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
  {: note}

    1. Cliquez sur une action du package `text-to-speech-v1` que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau paramètre. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.texttospeechshort}}
{: #usage_texttospeech}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant :

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Testez l'action `list-voices`.
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}


