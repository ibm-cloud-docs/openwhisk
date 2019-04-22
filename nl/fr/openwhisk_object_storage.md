---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (Expérimental) Source d'événements Object Storage
{: #cloud_object_storage}

Le package `/whisk.system/cos-experimental` risque d'être instable, de changer fréquemment d'une façon non compatible avec les versions antérieures et peut être retiré dans un délai court. Ce package n'est pas recommandé pour une utilisation dans des environnements de production. Ce package expérimental est actuellement disponible uniquement dans la région Sud des Etats-Unis.
{: important}

Dans cet exemple, vous allez apprendre à : 
* [Ecouter les modifications](#listening_to_cos_bucket_changes) sur une instance {{site.data.keyword.cos_full}}. 
* [Créer un déclencheur](#creating_a_trigger_cos) pour répondre à ces modifications. 
* [Créer des actions](#creating_action_to_process_object) pour extraire et traiter les modifications. 
* [Créer une règle](#associating_action_with_change_trigger) pour associer votre action au déclencheur de changement. 
<br>

**Exemple de cas d'utilisation :** Le package `/whisk.system/cos-experimental` vous permet d'écouter les modifications apportées aux données de rue GPS stockées dans un compartiment {{site.data.keyword.cos_full_notm}}. Ensuite, lorsque des modifications ont lieu, vous pouvez déclencher la régénération automatique d'une carte GPS, afin que les utilisateurs puissent avoir accès aux données de rue les plus récentes pour leur application GPS. 

## A propos d'IBM Cloud Object Storage
{: #cloud_object_storage_info}

**Avant de commencer :** pour en savoir plus sur {{site.data.keyword.cos_full_notm}}, voir [A propos d'Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage). Pour plus d'informations sur la configuration de l'instance {{site.data.keyword.cos_full_notm}}, voir [Mise à disposition d'une instance {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage). 

## Ecoute des modifications dans un compartiment IBM Cloud Object Storage
{: #listening_to_cos_bucket_changes}

Vous pouvez utiliser {{site.data.keyword.openwhisk}} pour écouter les modifications apportées à un compartiment [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) et utiliser une action pour traiter un ou plusieurs objets du compartiment.  

Le package `/whisk.system/cos-experimental` vous permet de configurer des événements à partir d'une instance {{site.data.keyword.cos_full_notm}}, et inclut le flux suivant : 

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | package | apikey, auth_endpoint, bucket, endpoint, interval | Package contenant l'action de flux `changes`. |
| `/whisk.system/cos-experimental/changes` | flux | apikey, auth_endpoint, bucket, endpoint, interval | Exécute des événements déclencheurs en cas de modifications effectuées dans un compartiment {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

Vous pouvez utiliser le flux `changes` pour configurer le service de source d'événements {{site.data.keyword.cos_full_notm}} afin d'exécuter un déclencheur toutes les fois qu'une modification est apportée à un compartiment dans votre instance {{site.data.keyword.cos_full_notm}}. 

Paramètres utilisés dans cet exemple : 

**apikey** : _(obligatoire, sauf s'il est lié au package)_. Le paramètre `apikey` est la clé d'API IAM pour l'instance {{site.data.keyword.cos_full_notm}}. Normalement, cette valeur est liée au package. Toutefois, si la valeur `apikey` est spécifiée lors de l'utilisation de l'action de flux `changes`, la valeur indiquée est utilisée pour les données d'identification au lieu de la clé d'API des données d'identification liées. 

**auth_endpoint** : _(facultatif)_. Le paramètre `auth_endpoint` est le noeud final d'autorisation utilisé par {{site.data.keyword.cos_full_notm}} pour générer un jeton à partir du paramètre `apikey`. Le noeud final par défaut est le noeud
final {{site.data.keyword.Bluemix}}. 

**bucket** : _(obligatoire)_. Le paramètre `bucket` est le nom du compartiment {{site.data.keyword.cos_full_notm}}. 

**endpoint** : _(obligatoire)_. Le paramètre `endpoint` est le noeud final {{site.data.keyword.cos_full_notm}} utilisé pour la connexion à votre instance {{site.data.keyword.cos_full_notm}}. Vous pouvez localiser votre noeud final dans la documentation [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints). 

**interval**: _(facultatif)_. Le paramètre `interval` est l'intervalle d'interrogation du compartiment, en minutes entières. La valeur `interval` doit être d'au moins 1 minute, qui est la valeur par défaut. 

## Création d'un déclencheur pour répondre au flux changes
{: #creating_a_trigger_cos}

Lors de la création du déclencheur, vous pouvez éviter de transmettre vos données d'identification {{site.data.keyword.cos_full_notm}} à l'action de flux `changes` en liant vos données d'identification directement au package `cos-experimental`.
 {: shortdesc}
 
 1. Créez d'abord une liaison de package qui peut être modifiée pour contenir vos données d'identification. La commande suivante crée une liaison de package `myCosPkg` dans votre espace de nom. 
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Liez vos données d'identification {{site.data.keyword.cos_short}} au package.
 La liaison de vos données d'identification {{site.data.keyword.cos_short}} au package associe la valeur `apikey` au package, de sorte que vous n'avez pas besoin de spécifier la valeur `apikey` lors de l'appel de l'action de flux `changes`.  
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Créez un déclencheur nommé `myCosTrigger` avec le flux `changes` dans la liaison de package que vous avez créée. Utilisez les valeurs de paramètre de votre nom de compartiment et de votre noeud final {{site.data.keyword.cos_short}}. 
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
    Exemple de sortie :
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. Commencez les interrogations pour rechercher les activations afin d'avoir une visibilité optimale de ce qui se passe actuellement.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. Créez une action de flux pour observer le flux de modification. Par exemple, une action appelée `showCosChange` contenant le code JavaScript suivant :
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
 6. Créez une règle pour connecter l'action `showCosChange` au déclencheur `myCosTrigger` : 
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. Dans votre tableau de bord {{site.data.keyword.cos_short}}, modifiez un objet de compartiment existant ou créez-en un. Pour savoir comment ajouter un objet à votre compartiment, voir [Ajout d'objets à votre compartiment](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects). 
 
 8. Pour chaque modification d'objet de compartiment, observez les nouvelles activations pour le déclencheur `myCosTrigger` et l'action `showCosChange`. Ces activations apparaissent dans l'intervalle d'interrogation de compartiment configuré. 

Si vous ne parvenez pas à observer de nouvelles activations, vérifiez que les valeurs des paramètres `apikey`, `endpoint` et `bucket` sont correctes. 
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Structure de données d'un événement déclencheur
{: #data_structure_trigger_event}

Le contenu des événements générés possède les paramètres suivants :

  - `file` : Métadonnées du fichier ou de l'objet. 
  - `status` : Modification détectée. Cette valeur peut être : `added`, `modified` ou `deleted`. 
  - `bucket` : Nom du compartiment {{site.data.keyword.cos_short}}. 
  - `endpoint` : Noeud final {{site.data.keyword.cos_short}} utilisé pour la connexion à l'instance {{site.data.keyword.cos_short}}. 
  - `key` : Identificateur de l'objet de compartiment modifié. Cette valeur est identique à `file.Key`, mais elle est disponible en haut de l'événement déclencheur JSON. 

Exemple de représentation JSON de l'événement déclencheur de modification dans le compartiment : 
```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Création d'une action pour traiter l'objet modifié
{: #creating_action_to_process_object}

Vous pouvez créer une action unique pour extraire et traiter l'objet. Vous pouvez également créer une séquence qui utilise une action pour extraire l'objet et une autre action pour le traiter. 

### Création d'une action pour extraire et traiter l'objet
{: #creating_action_to_retrieve_object}

Cet exemple de code d'action extrait et traite le document de notification de changement du compartiment. Vous pouvez transmettre les paramètres `apikey` et `serviceInstanceId` directement à l'action lors de l'appel manuel d'une action, mais lorsque cette action est appelée par un déclencheur, ces valeurs doivent
être obtenues à partir de votre instance {{site.data.keyword.cos_short}}, qui doit être liée à l'action avec la commande `ibmcloud fn service bind`. 

Exemple de code : 

```javascript
const COS = require('ibm-cos-sdk')

function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

  const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

  return new Promise(function(resolve, reject) {
    client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
      if (err != null) {
        console.log(err)
        reject({ err: err })
      } else {
        console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
    })
  });
}
```
{: codeblock}

Cette action utilisant le package du gestionnaire de package de noeud `ibm-cos-sdk`, elle doit être conditionnée sous forme
de [module Node.js](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action) ou de [bundle unique](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action). 

Une fois que cette action est conditionnée dans un fichier .zip, créez l'action qui permet d'extraire et de traiter l'objet à partir d'{{site.data.keyword.cos_short}} : 

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

[Liez](#cos_binding_credentials_to_action) les données d'identification {{site.data.keyword.cos_short}} à cette action. Ensuite, [créez une règle](#associating_action_with_change_trigger) pour appeler cette action lors de l'exécution du déclencheur. 

### Création d'une séquence d'actions pour extraire et traiter l'objet

Au lieu d'inclure le code d'extraction de l'objet dans votre action, vous pouvez utiliser l'action `object-read` à partir du
package {{site.data.keyword.cos_short}}, qui doit être [installé manuellement](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation). Il suffit que votre code d'action traite les résultats renvoyés par `object-read`. 

Exemple de code d'une action qui traite uniquement l'objet de compartiment : 
```javascript
function main(data) {
  if (data) {
    // Traitement de l'objet
  }
}
```
{: codeblock}

1. Créez l'action permettant de traiter uniquement l'objet provenant de {{site.data.keyword.cos_short}} :
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. Liez vos données d'identification {{site.data.keyword.cos_short}} à votre liaison de package `cos-experimental`. 
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. L'action `object-read` peut être associée à `myCosProcessObjectAction` pour créer une séquence d'actions.
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

Outre l'action `object-read`, vous pouvez utiliser d'autres actions incluses dans le package installable {{site.data.keyword.cos_short}}. 

[Liez](#cos_binding_credentials_to_action) les données d'identification {{site.data.keyword.cos_short}} à cette action. Ensuite, [créez une règle](#associating_action_with_change_trigger) pour appeler cette action lors de l'exécution du déclencheur. 

 ## Liaison de données d'identification à votre action
 {: #cos_binding_credentials_to_action}
 
 Vous pouvez éviter de transmettre des données d'identification sensibles lors d'un appel en associant les données d'identification {{site.data.keyword.cos_short}} à l'action à l'aide de la commande suivante : 
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## Création d'une règle pour associer l'action au déclencheur de changement
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

Vous pouvez utiliser une action ou une séquence d'actions dans une [règle](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use) pour extraire et traiter l'objet qui est associé à un événement de changement {{site.data.keyword.cos_short}}. 

Créez une règle qui active l'action `MyCosAction` sur les nouveaux événements déclencheurs {{site.data.keyword.cos_short}}. 
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



