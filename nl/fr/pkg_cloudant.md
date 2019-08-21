---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

Avec le package préinstallé `/whisk.system/cloudant`, vous pouvez utiliser une base de données [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). Aucune liaison de service n'est requise pour utiliser ce package.
{: shortdesc}


## Entités disponibles
{: #cloudant_available}
Le tableau suivant présente une sélection des entités disponibles dans le package `whisk.system/cloudant`. Vous pouvez utiliser le package `whisk.system/cloudant` pour lire, écrire, mettre à jour ou supprimer des documents. Vous pouvez également utiliser le flux `changes` pour guetter les modifications apportées à une base de données {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

Pour obtenir la liste complète des entités disponibles dans le package `/whisk.system/cloudant`, exécutez la commande `ibmcloud fn package get /whisk.system/cloudant`.
{: note}

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Package | `dbname`, `host`, `username`, `password` | Utiliser une base de données Cloudant. |
| `/whisk.system/cloudant/read` | Action | `dbname`, `id` | Lire un document dans une base de données. |
| `/whisk.system/cloudant/write` | Action | `dbname`, `overwrite`, `doc` | Ecrire un document dans une base de données. |
| `/whisk.system/cloudant/update-document` | Action | `dbname`, `doc` | Mettre à jour un document dans une base de données. |
| `/whisk.system/cloudant/changes` | Flux | `dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | Exécuter des événements déclencheurs en cas de modifications effectuées dans une base de données. |

Le paramètre `includeDoc` n'est plus pris en charge pour une utilisation avec le flux `/whisk.system/cloudant/changes`. Si vous avez créé des déclencheurs qui utilisent ce paramètre, vous devez les recréer sans le paramètre `includeDoc`.
{: deprecated}

## Lier le package `/whisk.system/cloudant` à votre base de données {{site.data.keyword.cloudant_short_notm}}
Si vous utilisez {{site.data.keyword.openwhisk}} depuis l'interface CLI {{site.data.keyword.cloud_notm}}, vous pouvez utiliser le plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour lier un service à une action ou un package.
{: #cloudant_db}

**Avant de commencer**
Vous devez disposer d'une instance {{site.data.keyword.cloudant_short_notm}}. Pour créer une instance, voir [Initiation à {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started).

1. Créez une liaison de package `/whisk.system/cloudant` configurée pour compte {{site.data.keyword.cloudant_short_notm}}. Dans cet exemple, le nom de package est `myCloudant`.

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Vérifiez que la liaison de package existe.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Exemple de sortie**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. Obtenez le nom de l'instance de service que vous souhaitez lier à une action ou un package.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Exemple de sortie**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. Obtenez le nom des données d'identification définies pour l'instance de service que vous avez extraite à l'étape précédente.

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **Exemple de sortie**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Liez le service au package que vous avez créé à l'étape 1.

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Vérifiez que la liaison des données d'identification a abouti.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **Exemple de sortie**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "serviceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

Dans cet exemple, les données d'identification du service {{site.data.keyword.cloudant_short_notm}} appartiennent à un paramètre nommé `__bx_creds`.

## Utilisation de documents dans une base de données {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Vous pouvez utiliser une action pour lire, écrire, mettre à jour, supprimer un document d'une base de données {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

### Lecture d'un document
Vous pouvez utiliser l'action `/whisk.system/cloudant/read` pour lire un document depuis votre base de données {{site.data.keyword.cloudant_short_notm}}.

**Avant de commencer**
Si vous ne disposez pas d'un document dans votre base de données {{site.data.keyword.cloudant_short_notm}}, vous pouvez en créer un via le tableau de bord {{site.data.keyword.cloudant_short_notm}}. L'URL du tableau de bord est `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`.

Procédez à l'extraction d'un document en utilisant l'action `read`. Remplacez `/_/myCloudant` par votre nom de package, `<database_name>` par votre nom de base de données et `<document_id>` par l'ID fichier. Appelez l'action pour tester l'extraction d'un document.

**Syntaxe de commande**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**Exemple d'action de lecture depuis une base de données `test`**
Appelez l'action pour tester la lecture d'un fichier. Cet exemple lit un fichier portant l'`id` `9f86f4955e7a38ab0169462e6ac0f476` et qui est un fichier vide. 

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**Exemple de sortie**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### Ecriture d'un document dans une base de données {{site.data.keyword.cloudant_short_notm}} 
{: #cloudant_write}

Vous pouvez utiliser une action pour créer ou mettre à jour des documents dans une base de données {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

**Avant de commencer**
  Créez une [liaison de package](#cloudant_db) `/whisk.system/cloudant` configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}.

1. En utilisant l'action `write`, stockez un document dans la liaison de package que vous avez créée. Remplacez `/_/myCloudant` par votre nom de package, `<database_name>` par le nom de votre base de données, `<document_id>` par votre ID document et `<test_name>` par un nom.

  **Syntaxe de commande**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **Exemple d'action d'écriture dans une base de données `test`**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **Exemple de sortie**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Vérifiez que le document existe dans le tableau de bord {{site.data.keyword.cloudant_short_notm}}. L'URL du tableau de bord pour la base de données `test` est au format suivant : `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`.

  **Exemple de document dans le tableau de bord {{site.data.keyword.cloudant_short_notm}}**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### Mise à jour d'un document
Vous pouvez utiliser l'action `/update-document` pour mettre à jour un document dans votre base de données {{site.data.keyword.cloudant_short_notm}}.
{: short desc}

**Avant de commencer**
Créez une [liaison de package](#cloudant_db) `/whisk.system/cloudant` configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}.

L'exemple suivant met à jour le document créé à la section [Ecriture d'un document dans une base de données {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

Vous pouvez mettre à jour un document dans votre base de données en remplaçant `<test>` par le nom de votre base de données et en remplaçant l'indicateur `--param doc` par l' `id` et le contenu du document dans votre base de données que vous souhaitez mettre à jour.


1. Vous pouvez mettre à jour un document dans la base de données `test` en exécutant la commande suivante. Cet exemple ajoute la valeur `shade` au document `color`. 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **Sortie**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. Pour voir la mise à jour, extrayez à nouveau le document.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **Exemple de document**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## Création d'un déclencheur à l'aide de la fonction de filtrage
{: #cloudant_trigger}

Vous pouvez utiliser le flux `changes` pour configurer un service afin d'exécuter un déclencheur toutes les fois qu'une modification est apportée à votre base de données {{site.data.keyword.cloudant_short_notm}}.

**Avant de commencer**
  Créez une [liaison de package](#cloudant_db) `/whisk.system/cloudant` configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}.

Paramètres utilisés dans cet exemple. 

| Paramètre | Description |
| --- | --- |
| `dbname` | (Obligatoire) Nom de la base de données {{site.data.keyword.cloudant_short_notm}}. |
| `iamApiKey` | (Facultatif) Clé d'API IAM pour la base de données Cloudant. Si elle est spécifiée, cette valeur est utilisée comme données d'identification au lieu du nom d'utilisateur et du mot de passe. |
| `iamUrl` | (Facultatif) URL du service de jeton IAM qui est utilisée lorsque `iamApiKey` est spécifié. Valeur par défaut : `https://iam.cloud.ibm.com/identity/token`. | 
| `maxTriggers` | (Facultatif) L'exécution des déclencheurs est arrêtée lorsque cette limite est atteinte. Par défaut, cette valeur est infinie. |
| `filter` | (Facultatif) Fonction de filtrage définie dans un document de conception. |
| `query_params` | (Facultatif) Tous les paramètres de requête supplémentaires pouvant être requis pour la fonction de filtrage. |
| `includeDoc` | (Obsolète) Le paramètre `includeDoc` n'est plus pris en charge pour une utilisation avec le flux `/whisk.system/cloudant/changes`. |

</br>

1. Créez un déclencheur nommé `cloudantTrigger` avec le flux `changes` dans la liaison de package que vous avez créée précédemment. Incluez les éléments `filter` et `query_params` pour exécuter le déclencheur en cas d'ajout (ou de modification) d'un document lorsque son statut indique `new`.

  Remplacez `/_/myCloudant` par le nom de votre package. Cet exemple utilise une base de données nommée `test`.
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **Exemple de sortie**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. Sauvegardez le code JavaScript suivant en tant que `cloudantChange.js`.

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. Créez une action appelée `cloudantChange` que vous pourrez utiliser pour observer le flux de modifications. Remplacez `<file_path>` par le chemin d'accès au fichier `cloudantChange.js` sur votre ordinateur.

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. Créez une règle nommée `cloudantRule` pour connecter l'action `cloudantChange` au déclencheur `cloudantTrigger` que vous avez créé précédemment.

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. Dans une autre fenêtre de terminal, démarrez l'interrogation afin de voir à quel moment les activations se produisent.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. Dans votre tableau de bord {{site.data.keyword.cloudant_short_notm}}, modifiez un document existant ou créez-en un. 

7. Observez les actions pour le déclencheur `cloudantTrigger` pour chaque modification du document. 

**Exemple d'activation du déclencheur `cloudantTrigger`**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### Structure de données d'une activation de déclencheur
{: #cloudant_struct}

Le contenu de l'événement généré possède les paramètres suivants. 

| Paramètre | Description |
| --- | --- |
| `id` | ID document. |
| `seq` | Identificateur de séquence généré par {{site.data.keyword.cloudant_short_notm}}. |
| `changes` | Tableau d'objets, dont chacun comporte une zone `rev` qui contient l'ID révision du document. |

**Représentation JSON de l'activation du déclencheur**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

## Filtrage des événements de changement dans la base de données
{: #cloudant_filter}

Vous pouvez définir une fonction de filtrage pour éviter que des événements de modification inutiles exécutent votre déclencheur.

**Avant de commencer**
Créez une [liaison de package](#cloudant_db) `/whisk.system/cloudant` configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}.

Pour créer une fonction de filtrage, vous pouvez utiliser une action.

1. Sauvegardez le filter suivant dans un fichier JSON nommé `design_doc.json`. 

  ```json
  {
    "doc": {
      "_id": "_design/mailbox",
    "filters": {
        "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
    }
  }
  ```
  {: codeblock}

2. Créez un document de conception dans la base de données avec la fonction de filtrage suivante. Remplacez `<database_name>` par le nom de votre base de données et `<file_path>` par le chemin de votre fichier `design_doc.json`. Appelez l'action `write` pour tester la création d'un document de conception.

**Syntaxe de commande**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**Exemple de commande permettant d'écrire un fichier `design_doc.json` dans une base de données `test`**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**Exemple de sortie**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


Pour plus d'informations sur les documents de conception {{site.data.keyword.cloudant_short_notm}}, voir [Documents de conception](/docs/services/Cloudant?topic=cloudant-design-documents)

## Traitement d'un document individuel via une séquence d'actions
{: #cloudant_seq}

Vous pouvez utiliser une séquence d'actions dans une règle pour extraire et traiter le document associé à un événement de changement {{site.data.keyword.cloudant_short_notm}}.

**Avant de commencer**
Créez une [liaison de package](#cloudant_db) `/whisk.system/cloudant` configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}. 

Cet exemple met à jour le document créé à la section [Ecriture d'un document dans une base de données {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

### Création d'une action pour traiter un document individuel

Pour créer une action qui gère les modifications apportées à un document individuel, exécutez les commandes suivantes.
{: shortdesc}

1. Sauvegardez le code suivant en tant que `docChange.js`

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. Créez une action appelée `docChange` pour traiter le document portant le nom `blue`, que vous avez créé précédemment. Remplacez `<file_path>` par le chemin d'accès au fichier `docChange.js`. 

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **Sortie**
  ```
  ok: created action docChange
  ```
  {: screen}

### Créer une séquence avec l'action `read` 

L'action `read` peut être associée à votre action `docChange` pour créer une séquence d'actions.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **Sortie**
  ```
  ok: created action docSequence
  ```
  {: screen}

### Créer un déclencheur avec le flux `changes`

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### Créer une règle afin d'associer le déclencheur à la séquence

L'action `docSequence` peut être utilisée dans une règle qui active l'action en cas de nouveaux événements déclencheurs {{site.data.keyword.cloudant_short_notm}}.

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **Sortie**
  ```
  ok: created rule docRule
  ```

  **Exemple d'activation**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### Tester la séquence

1. Testez la séquence `docSequence` en effectuant une modification dans le fichier `blue` créé précédemment. Dans cet exemple, la valeur `shade` est remplacée par `indigo`.

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **Exemple d'activation**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. Vérifiez que le fichier a été mis à jour pour inclure la valeur `shade` en appelant l'action `read`. Remplacez le nom `<database>` par le nom de votre base de données.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **Sortie**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### Etapes suivantes
A présent que vous êtes à l'écoute des modifications apportées à un document de votre base de données {{site.data.keyword.cloudant_short_notm}}, vous pouvez déclencher des notifications Slack pour les changements en utilisant le [package `/whisk.system/slack`](/docs/openwhisk?topic=cloud-functions-pkg_slack).


