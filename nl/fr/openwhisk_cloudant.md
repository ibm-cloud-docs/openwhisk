---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation du package Cloudant
{: #openwhisk_catalog_cloudant}
Le package `/whisk.system/cloudant` permet d'utiliser une base de données Cloudant. Il inclut les actions et les flux ci-après.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Package | dbname, host, username, password | Utiliser une base de données Cloudant |
| `/whisk.system/cloudant/read` | Action | dbname, id | Lire un document à partir d'une base de données |
| `/whisk.system/cloudant/write` | Action | dbname, overwrite, doc | Ecrire un document dans une base de données |
| `/whisk.system/cloudant/changes` | Flux | dbname, filter, query_params, maxTriggers | Exécuter des événements déclencheurs en cas de modification d'une base de données |
{: shortdesc}

Les rubriques ci-après expliquent comment configurer une base de données Cloudant, comment configurer un package associé et comment utiliser les actions et les flux du package `/whisk.system/cloudant`.

## Configuration d'une base de données Cloudant dans {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_in}

Si vous utilisez OpenWhisk depuis {{site.data.keyword.Bluemix_notm}}, OpenWhisk crée automatiquement des liaisons de package pour vos instances de service Cloudant. Si vous n'utilisez pas OpenWhisk et Cloudant depuis {{site.data.keyword.Bluemix_notm}}, passez à l'étape suivante.

1. Créez une instance de service Cloudant dans votre [tableau de bord](http://console.ng.Bluemix.net) {{site.data.keyword.Bluemix_notm}}.

  Prenez soin de créer une clé de données d'identification pour chaque nouvelle instance de service. 

2. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour chaque instance de service Cloudant ayant une clé de données d'identification définie. 

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```

  Votre liaison de package contient désormais les données d'identification associées à votre instance de service Cloudant. 

3. Vérifiez que la liaison de package qui a été créée précédemment est configurée avec l'hôte et les données d'identification de votre instance de service
{{site.data.keyword.Bluemix_notm}} Cloudant.

  ```
  wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters
  ```
  ```json
  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```

## Configuration d'une base de données Cloudant en dehors de {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_outside}

Si vous n'utilisez pas OpenWhisk dans {{site.data.keyword.Bluemix_notm}} ou si vous souhaitez configurer votre base de données Cloudant en dehors de {{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour votre compte Cloudant. Vous avez besoin du nom d'hôte, du nom d'utilisateur et du mot de passe du compte Cloudant.

1. Créez une liaison de package qui est configurée pour votre compte Cloudant.

  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}
  

2. Vérifiez que la liaison de package existe.

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /monEspaceNom/monCloudant private binding
  ```


## Ecoute des modifications apportées dans une base de données Cloudant
{: #openwhisk_catalog_cloudant_listen}

### Filtrage des événements de modification de la base de données

Vous pouvez définir une fonction de filtrage pour éviter que des événements de modification indésirables ne lancent votre déclencheur.

Pour créer une nouvelle fonction de filtrage, vous pouvez utiliser une action.

Créez un fichier de document json nommé `design_doc.json` et contenant la fonction de filtrage suivante
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

Créez un document de conception sur la base de données avec la fonction de filtrage
```
wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
Les informations pour le nouveau document de conception sont affichées à l'écran.
```json
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```

### Créez le déclencheur à l'aide de la fonction de filtrage

Vous pouvez utiliser le flux `changes` pour configurer un service afin d'exécuter un déclencheur à chaque fois qu'une modification est apportée dans votre base de données Cloudant. Les paramètres sont les suivants :

- `dbname` : nom de la base de données Cloudant.
- `maxTriggers` : l'exécution de déclencheurs s'arrête lorsque cette limite est atteinte. Par défaut, cette valeur est infinie.
- `filter` : fonction de filtrage qui est définie sur un document de conception.
- `query_params` : paramètres de requête supplémentaires pour la fonction de filtrage.


1. Créez un déclencheur avec le flux `changes` dans la liaison de package que vous avez créée précédemment. Incluez les éléments `filter` et `query_params` pour lancer le déclencheur lorsqu'un document est ajouté ou modifié alors que son statut indique `new`.
Prenez soin de remplacer `/_/myCloudant` par le nom de vote package.

  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}
  ```
  ok: created trigger feed myCloudantTrigger
  ```

2. Recherchez les activations.

  ```
  wsk activation poll
  ```
  {: pre}

3. Dans votre tableau de bord Cloudant, modifiez un document existant ou créez-en un.

4. Vous constaterez que compte tenu de la fonction de filtrage et des paramètres de requête, les nouvelles activations du déclencheur `myCloudantTrigger` pour chaque modification de document n'interviennent plus que si le statut du document indique `new`.
  
  **Remarque** : si vous ne parvenez pas à observer de nouvelles activations, reportez-vous aux sections suivantes relatives à la lecture et à l'écriture dans une base de données Cloudant. Les tests de lecture et d'écriture ci-après vous permettent de vérifier que vos données d'identification Cloudant sont correctes.
  
  A présent, vous pouvez créer des règles et les associer à des actions afin de réagir aux mises à jour de document.
  
  Le contenu des événements générés possède les paramètres suivants :
  
  - `id` : ID de document.
  - `seq` : identificateur de séquence généré par Cloudant.
  - `changes` : tableau d'objets possédant chacun une zone `rev` qui contient l'ID de révision du document.
  
  La représentation JSON de l'événement déclencheur est la suivante :
  
  ```json
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```
  
## Ecriture dans une base de données Cloudant
{: #openwhisk_catalog_cloudant_write}

Vous pouvez utiliser une action pour stocker un document dans une base de données Cloudant appelée `bdtest`. Assurez-vous que cette base de données existe sur votre compte Cloudant.

1. Stockez un document en utilisant l'action `write` dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/_/myCloudant` par le nom de vote package.

  ```
  wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287
  ```
  ```json
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```

2. Vérifiez que le document existe en le recherchant dans votre tableau de bord Cloudant.

  L'adresse URL du tableau de bord pour la base de données `bdtest` est similaire à la suivante :
`https://MON_COMPTE_CLOUDANT.cloudant.com/dashboard.html#database/bdtest/_all_docs?limit=100`.


## Lecture depuis une base de données Cloudant
{: #openwhisk_catalog_cloudant_read}

Vous pouvez utiliser une action pour extraire un document depuis une base de données Cloudant appelée `bdtest`. Assurez-vous que cette base de données existe sur votre compte Cloudant.

- Procédez à l'extraction d'un document en utilisant l'action `read` dans la liaison de package que vous avez créée précédemment. Prenez soin de remplacer `/_/myCloudant` par le nom de vote package.

  ```
  wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```json
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```

## Utilisation d'une séquence d'actions et d'un déclencheur de changement pour traiter un document provenant d'une base de données Cloudant
{: #openwhisk_catalog_cloudant_read_change notoc}

Vous pouvez utiliser une séquence d'actions dans une règle pour extraire et traiter le document associé à un événement de changement Cloudant.

Voici un exemple de code d'une action qui traite un document :
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```

Créez l'action permettant de traiter le document depuis Cloudant :
```
wsk action create myAction myAction.js
```
{: pre}

Pour lire un document à partir de la base de données, vous pouvez utiliser l'action `read` dans le package Cloudant.
L'action `read` peut être associée à `myAction` pour créer une séquence d'actions.
```
wsk action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

L'action `sequenceAction` peut être utilisée dans une règle qui active l'action sur de nouveaux événements déclencheurs Cloudant.
```
wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Remarque** : le déclencheur Cloudant `changes` prenait en charge le paramètre `includeDoc`, mais cela n'est plus le cas.
  Vous pouvez recréer les déclencheurs précédemment créés avec `includeDoc`. Pour recréer le déclencheur, procédez comme suit : 
  ```
  wsk trigger delete myCloudantTrigger
  ```
  {: pre}
  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
  ```
  {: pre}

  Vous pouvez utiliser l'exemple illustré ci-dessus pour créer une séquence d'actions permettant de lire le document modifié et d'appeler vos actions existantes.
  N'oubliez pas de désactiver les règles qui ne sont plus valides et d'en créer de nouvelles avec le modèle de séquence d'actions.

