---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: cloudant, event, action, trigger, sequence

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Cloudant
{: #pkg_cloudant}

Le package préinstallé `/whisk.system/cloudant` vous permet d'utiliser une base de données [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). Aucune liaison de service n'est requise pour utiliser ce package.


## Actions et flux disponibles
{: #cloudant_available}

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password | Utiliser une base de données Cloudant. |
| `/whisk.system/cloudant/read` | action | dbname, id | Lire un document dans une base de données. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Ecrire un document dans une base de données. |
| `/whisk.system/cloudant/changes` | flux | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Exécuter des événements déclencheurs en cas de modifications effectuées dans une base de données. |
{: shortdesc}

### Configuration d'une base de données {{site.data.keyword.cloudant_short_notm}} dans {{site.data.keyword.Bluemix_notm}}
{: #cloudant_db}

Si vous utilisez {{site.data.keyword.openwhisk}} à partir d'{{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le plug-in de l'interface de ligne de commande {{site.data.keyword.openwhisk}} pour lier un service à une action ou un package.

Vous devez d'abord créer manuellement une liaison de package pour votre compte {{site.data.keyword.cloudant_short_notm}}.

1. Créez une liaison de package configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Vérifiez que la liaison de package existe.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. Obtenez le nom de l'instance de service que vous souhaitez lier à une action ou un package.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    Exemple de sortie :
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

    Exemple de sortie :
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Liez le service au package créé à l'étape 1.
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Vérifiez que la liaison des données d'identification a abouti.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
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

    Dans cet exemple, les données d'identification du service Cloudant appartiennent à un paramètre nommé `__bx_creds`.


### Lecture à partir d'une base de données {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Vous pouvez utiliser une action pour extraire un document d'une base de données {{site.data.keyword.cloudant_short_notm}} appelée **testdb**. Vérifiez que cette base de données existe sur votre compte {{site.data.keyword.cloudant_short_notm}}.

- Procédez à l'extraction d'un document en utilisant l'action **read** dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/_/myCloudant` par le nom de votre package.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

### Ecriture dans une base de données {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

Vous pouvez utiliser une action pour stocker un document dans une base de données {{site.data.keyword.cloudant_short_notm}} appelée **testdb**. Vérifiez que cette base de données existe sur votre compte {{site.data.keyword.cloudant_short_notm}}.

1. Stockez un document en utilisant l'action **write** dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/_/myCloudant` par le nom de votre package.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Vérifiez que le document existe en le recherchant dans votre tableau de bord {{site.data.keyword.cloudant_short_notm}}.

  L'adresse URL du tableau de bord pour la base de données **bdtest** est similaire à la suivante : `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/bdtest/_all_docs?limit=100`.


### Création d'un déclencheur à l'aide de la fonction de filtrage
{: #cloudant_trigger}

Vous pouvez utiliser le flux `changes` pour configurer un service afin d'exécuter un déclencheur toutes les fois qu'une modification est apportée à votre base de données {{site.data.keyword.cloudant_short_notm}}.

Les paramètres utilisés dans cet exemple sont les suivants :

**dbname** : nom de la base de données {{site.data.keyword.cloudant_short_notm}} _(obligatoire)_.

**iamApiKey** : clé d'API IAM pour la base de données Cloudant.  Si elle est indiquée, elle sera utilisée comme données d'identification au lieu du nom d'utilisateur et du mot de passe _(facultatif)_.

**iamUrl** : URL du service de jeton IAM qui est utilisée quand `iamApiKey` est spécifié.  Par défaut, il s'agit de `https://iam.bluemix.net/identity/token` _(facultatif)_.

**maxTriggers** : l'exécution de déclencheurs s'arrête une fois cette limite atteinte _(facultatif)_. Par défaut, cette valeur est infinie.

**filter** : fonction de filtrage définie sur un document de conception _(facultatif)_

**query_params** : paramètres de requête supplémentaires pour la fonction de filtrage _(facultatif)_.

1. Créez un déclencheur nommé **myCloudantTrigger** avec le flux `changes` dans la liaison de package que vous avez créée précédemment. Incluez les éléments `filter` et `query_params` pour exécuter le déclencheur en cas d'ajout (ou de modification) d'un document lorsque son statut indique `new`.

  Prenez soin de remplacer `/_/myCloudant` par le nom de votre package.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. Commencez les interrogations pour rechercher les activations afin d'avoir une visibilité optimale de ce qui se passe actuellement.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. Créez une action qui nous permettra d'observer les effets d'un flux de changements. Par exemple, une action appelée **showCloudantChange** contenant le code JavaScript suivant :
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. Créez une règle pour connecter l'action **showCloudantChange** au déclencheur créé précédemment :
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Créez des actions et une règle pour associer ces actions au déclencheur **myCloudantTrigger**.

6. Dans votre tableau de bord {{site.data.keyword.cloudant_short_notm}}, modifiez un document existant ou créez-en un. Le document doit comporter une zone _status_, définie avec la valeur **new**.

7. Observez que, compte tenu de la fonction de filtrage et des paramètres de requête, les nouvelles activations du déclencheur **myCloudantTrigger** pour chaque modification de document interviennent uniquement si le statut du document indique **new**.

Testez les étapes de lecture et d'écriture pour vérifier que vos données d'identification {{site.data.keyword.cloudant_short_notm}} sont correctes.

### Structure de données d'un événement déclencheur
{: #cloudant_struct}

Le contenu des événements générés possède les paramètres suivants :

  - `id` : ID de document.
  - `seq` : identificateur de séquence généré par {{site.data.keyword.cloudant_short_notm}}.
  - `changes` : tableau d'objets possédant chacun une zone `rev` qui contient l'ID de révision du document.

La représentation JSON de l'événement déclencheur est la suivante :
```json
{
    "dbname": "testdb",
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

### Filtrage des événements de changement dans la base de données
{: #cloudant_filter}

Vous pouvez éventuellement définir une fonction de filtrage pour éviter que des événements de changement indésirables ne lancent votre déclencheur.

Pour créer un nouvelle fonction de filtrage, vous pouvez utiliser une action.

Créez un fichier de document JSON nommé `design_doc.json` avec la fonction de filtrage suivante :
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

Créez un document de conception sur la base de données avec la fonction de filtrage suivante :
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

Les informations relatives au nouveau document de conception sont affichées à l'écran :
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### Utilisation d'une séquence d'actions et d'un déclencheur de changement pour traiter un document provenant d'une base de données {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_seq}

Vous pouvez utiliser une séquence d'actions dans une règle pour extraire et traiter le document associé à un événement de changement {{site.data.keyword.cloudant_short_notm}}.

Voici un exemple de code d'une action qui traite un document :
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

Créez l'action permettant de traiter le document provenant de {{site.data.keyword.cloudant_short_notm}} :
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

Pour lire un document à partir de la base de données, vous pouvez utiliser l'action `read` dans le package {{site.data.keyword.cloudant_short_notm}}.
L'action `read` peut être associée à `myAction` pour créer une séquence d'actions.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

L'action `sequenceAction` peut être utilisée dans une règle qui active l'action en cas de nouveaux événements déclencheurs {{site.data.keyword.cloudant_short_notm}}.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Remarque :** Le déclencheur {{site.data.keyword.cloudant_short_notm}} `changes` était compatible avec le paramètre `includeDoc` qui n'est plus pris en charge.

Vous pouvez recréer des déclencheurs précédemment créés avec `includeDoc`. Pour récréer le déclencheur, procédez comme suit :
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

Vous pouvez utiliser cet exemple pour créer une séquence d'actions permettant de lire le document modifié et d'appeler vos actions existantes. N'oubliez pas de désactiver les règles qui ne sont plus valides et d'en créer de nouvelles avec le modèle de séquence d'actions.

