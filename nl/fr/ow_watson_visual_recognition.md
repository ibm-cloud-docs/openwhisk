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

# Package {{site.data.keyword.visualrecognitionshort}}

Le service {{site.data.keyword.visualrecognitionfull}} utilise des algorithmes d'apprentissage en profondeur pour identifier des scènes, des objets et des visages dans les images que vous téléchargez dans le service. Vous pouvez créer et entraîner un discriminant personnalisé pour identifier les sujets qui répondent à vos besoins.{:shortdesc}

Le package {{site.data.keyword.visualrecognitionshort}} contient les entités suivantes. Vous trouverez des détails supplémentaires dans la référence d'API {{site.data.keyword.visualrecognitionshort}} en cliquant sur le nom d'entité.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html) | package | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |Gestion du service {{site.data.keyword.visualrecognitionshort}}. |
| [classify](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    images_file,     accept_language,     url,     threshold,     owners,     classifier_ids,     images_file_content_type,  | Classement des images. |
| [detect-faces](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    images_file,     url,     images_file_content_type,  | Détection des visages dans les images. |
| [create-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,     classname_positive_examples,     negative_examples,  | Création d'un discriminant. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Suppression d'un discriminant. |
| [get-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Extraction des détails d'un discriminant. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    verbose,  | Extraction d'une liste de discriminants. |
| [update-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,     classname_positive_examples,     negative_examples,  | Mise à jour d'un discriminant. |
| [get-core-ml-model](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  | Extraction d'un modèle Core ML d'un discriminant. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data) | action |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Suppression de données de libellé. |

## Création d'une instance de service {{site.data.keyword.}} 
{: #service_instance}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.}}![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/catalog/services/watson_vision_combined).
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.}}
{: #install}

Dès que vous disposez d'une instance de service {{site.data.keyword.}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #visualrecognition_cli}

Avant de commencer :
  1. [Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface de ligne de commande {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Installez la commande [`wskdeploy`![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) et ajoutez le fichier binaire téléchargé à votre variable PATH.

Pour installer le package {{site.data.keyword.}} :

1. Clonez le référentiel de package {{site.data.keyword.}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    wskdeploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
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
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.}} que vous avez créée au package.
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    Exemple de sortie :
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.}}.
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Installation depuis l'interface utilisateur {{site.data.keyword.openwhisk_short}} 
{: #visualrecognition_ui}

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à [Create page ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/create).

2. A l'aide des listes **Cloud Foundry Org** et **Cloud Foundry Space**, sélectionnez l'espace de nom dans lequel vous voulez installer le package {{site.data.keyword.cos_short}}. Les espaces de nom sont formés à partir de la combinaison des espaces de nom org et space.

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Visual Recognition**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page Actions et vous pouvez rechercher votre nouveau package, nommé **visual-recognition-v3**.

7. Pour utiliser les Actions du package **visual-recognition-v3**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 5 et 6 dans les instructions de l'interface CLI ci-dessus. 
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur. **Remarque **: Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
    1. Cliquez sur une action du package **visual-recognition-v3** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche. 
    2. Dans le volet de navigation gauche, cliquez sur la section **Parameters**. 
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.visualrecognitionshort}}
{: #usage}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant :

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Toutes les actions exigent un paramètre de version au format AAAA-MM-JJ. Lorsque l'API est modifiée avec rétrocompatibilité impossible, une nouvelle date de version est publiée. Pour plus de détails, consultez la [référence d'API](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning).

Les fonctions de ce package utilisent la version actuelle de Visual Recognition, 2018-03-19. Testez l'action `list-classifiers`.
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}
