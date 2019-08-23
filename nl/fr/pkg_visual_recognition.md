---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: functions, cognitive,

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


# {{site.data.keyword.visualrecognitionshort}}
{: #pkg_visual_recognition}

Le service installable {{site.data.keyword.visualrecognitionfull}} utilise des algorithmes d'apprentissage en profondeur pour identifier des scènes, des objets et des visages dans les images que vous téléchargez dans le service. Vous pouvez créer et entraîner un discriminant personnalisé pour identifier les sujets qui répondent à vos besoins.
{:shortdesc}

Le package {{site.data.keyword.visualrecognitionshort}} contient les entités suivantes. Pour plus d'informations, voir la référence d'API {{site.data.keyword.visualrecognitionshort}} en cliquant sur le nom de l'entité. 

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Gestion du service {{site.data.keyword.visualrecognitionshort}}. |
| [`classify`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `accept_language`, `url`, `threshold`, `owners`, `classifier_ids`, `images_file_content_type`  | Classement des images. |
| [`detect-faces`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `url`, `images_file_content_type`  | Détection des visages dans les images. |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `name`, `classname_positive_examples`, `negative_examples` | Création d'un discriminant. |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`  | Suppression d'un discriminant. |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`  | Extraction des détails d'un discriminant. |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `verbose`  | Extraction d'une liste de discriminants. |
| [`update-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`, `classname_positive_examples`, `negative_examples`  | Mise à jour d'un discriminant. |
| [`get-core-ml-model`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`  | Extraction d'un modèle Core ML d'un discriminant. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `customer_id`  | Suppression de données de libellé. |

## Création d'une instance de service {{site.data.keyword.visualrecognitionshort}}
{: #service_instance_recognition}

Avant d'installer le package, vous devez créer une instance de service {{site.data.keyword.visualrecognitionshort}} et des données d'identification du service.
{: shortdesc}

1. [Créez une instance de service {{site.data.keyword.visualrecognitionshort}}](https://cloud.ibm.com/catalog/services/watson_vision_combined){: external}.
2. Une fois l'instance de service créée, des données d'identification du service à génération automatique sont également créées pour vous.

## Installation du package {{site.data.keyword.visualrecognitionshort}}
{: #install_recognition}

Dès que vous disposez d'une instance de service {{site.data.keyword.visualrecognitionshort}}, utilisez l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour installer le package {{site.data.keyword.visualrecognitionshort}} dans votre espace de nom.
{: shortdesc}

### Installation depuis l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}
{: #visualrecognition_cli}

**Avant de commencer**
[Installez le plug-in {{site.data.keyword.openwhisk_short}} pour l'interface CLI {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Pour installer le package {{site.data.keyword.visualrecognitionshort}}, exécutez les commandes suivantes. 

1. Clonez le référentiel de package {{site.data.keyword.visualrecognitionshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Déployez le package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
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
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. Liez les données d'identification de l'instance {{site.data.keyword.visualrecognitionshort}} que vous avez créée au package.
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    **Exemple de sortie**
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. Vérifiez que le package est configuré avec les données d'identification de votre instance de service {{site.data.keyword.visualrecognitionshort}}.
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    **Exemple de sortie**
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
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

1. Sur la console {{site.data.keyword.openwhisk_short}}, accédez à la [page Créer](https://cloud.ibm.com/openwhisk/create){: external}.

2. A l'aide des listes **Organisation Cloud Foundry** et **Espace Cloud Foundry**, sélectionnez l'espace de nom où vous voulez installer le package.

3. Cliquez sur **Installer les packages**.

4. Cliquez sur le groupe de packages **Watson**.

5. Cliquez sur le package **Visual Recognition**.

5. Cliquez sur **Installer**.

6. Une fois le package installé, vous êtes redirigé vers la page des actions et vous pouvez rechercher votre nouveau package, nommé **visual-recognition-v3**.

7. Pour utiliser les actions du package **visual-recognition-v3**, vous devez lier les données d'identification du service aux actions.
  * Pour lier les données d'identification du service à toutes les actions du package, suivez les étapes 4 et 5 des [instructions de l'interface de ligne de commande](#visualrecognition_cli).
  * Pour lier les données d'identification du service à des actions individuelles, suivez les étapes ci-après dans l'interface utilisateur.
   
  Vous devez suivre les étapes ci-après pour chaque action que vous voulez utiliser.
  {: note}

    1. Cliquez sur une action du package **visual-recognition-v3** que vous voulez utiliser. La page des informations détaillées de cette action s'affiche.
    2. Dans le volet de navigation gauche, cliquez sur la section **Paramètres**.
    3. Entrez un nouveau **paramètre**. Pour le paramètre key, entrez `__bx_creds`. Comme valeur, collez l'objet JSON des données d'identification du service de l'instance de service que vous avez créée plus haut.

## Utilisation du package {{site.data.keyword.visualrecognitionshort}}
{: #usage_recognition}

Pour utiliser les actions de ce package, exécutez les commandes au format suivant :

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Toutes les actions nécessitent un paramètre de version au format AAAA-MM-JJ. Lorsque l'API est modifiée avec rétrocompatibilité impossible, une nouvelle date de version est publiée. Pour plus de détails, consultez la [référence d'API](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning){: external}.

Les fonctions de ce package utilisent la version actuelle de Visual Recognition, `2018-03-19`. Testez l'action `list-classifiers`.
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}


