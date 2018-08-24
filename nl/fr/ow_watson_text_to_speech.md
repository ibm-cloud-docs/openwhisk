---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Package {{site.data.keyword.texttospeechshort}}

Le service {{site.data.keyword.texttospeechfull}} fournit une API qui utilise les fonctions de reconnaissance vocale d'IBM pour synthétiser du texte sous forme de voix naturelle dans une variété de langues, dialectes et voix.
{:shortdesc}

Le service prend en charge au moins une voix masculine ou féminine, parfois les deux, pour chaque langue. Le contenu audio est diffusé jusqu'au client dans un délai minimal. Pour plus d'informations sur le service, consultez la [documentation IBM Cloud](https://console.bluemix.net/docs/services/text-to-speech/index.html).

Le package {{site.data.keyword.texttospeechshort}} contient les entités suivantes. Vous trouverez des détails supplémentaires dans la référence d'API {{site.data.keyword.texttospeechshort}} en cliquant sur le nom d'entité.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |Gestion du service {{site.data.keyword.texttospeechshort}}. |
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    voice,     customization_id,  | Obtention d'une voix. |
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Affichage de la liste des voix. |
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text,     accept,     voice,     customization_id,  | Synthétisation audio. |
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,     voice,     format,     customization_id,  | Obtention de la prononciation. |
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, language, description,  | Création d'un modèle personnalisé. |
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Suppression d'un modèle personnalisé. |
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Obtention d'un modèle personnalisé. |
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Affichage de la liste des modèles personnalisés. |
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    name, description, words,  | Mise à jour d'un modèle personnalisé. |
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,    translation, part_of_speech,  | Ajout d'un mot personnalisé. |
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  |Ajout de mots personnalisés. |
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | Suppression d'un mot personnalisé. |
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | Obtention d'un mot personnalisé. |
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Affichage de la liste de mots personnalisés. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Suppression de données de libellé. |

## Création d'une instance de service {{site.data.keyword.texttospeechshort}}
{: #service_instance}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.texttospeechshort}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.texttospeechshort}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog/services/text_to_speech).
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.texttospeechshort}}
{: #install}

Dès que vous disposez d'une instance de service {{site.data.keyword.texttospeechshort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.texttospeechshort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installez la commande [`wskdeploy`![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) et ajoutez le fichier binaire téléchargé à votre variable PATH.

Pour installer le package {{site.data.keyword.texttospeechshort}} :

1. Clonez le référentiel de package {{site.data.keyword.texttospeechshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    wskdeploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
    ```
    {: pre}

3. Vérifiez que le package est ajouté à votre liste de packages.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Sortie :
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
    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    Exemple de sortie :
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

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à [Create page ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/create).

2. A l'aide des listes **Cloud Foundry Org** et **Cloud Foundry Space**, sélectionnez l'espace de nom dans lequel vous voulez installer le package {{site.data.keyword.cos_short}}. Les espaces de nom sont formés à partir de la combinaison des espaces de nom org et space.

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Text To Speech**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **text-to-speech-v1**.

7. Pour utiliser les Actions du package **text-to-speech-v1**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus. 
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **text-to-speech-v1** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche. 
    2. Dans le volet de navigation gauche, cliquez sur la section **Parameters**. 
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.texttospeechshort}}
{: #usage}

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
