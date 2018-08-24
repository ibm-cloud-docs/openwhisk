---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Source d'événements Cloudant
{: #openwhisk_cloudant}

Apprenez à être à l'écoute des changements intervenant dans une base de données {{site.data.keyword.cloudant}}, à filtrer les événements de ces changements et à utiliser une séquence d'actions pour traiter un document à partir d'une base de données {{site.data.keyword.cloudant_short_notm}}. Le package `/whisk.system/cloudant` vous permet d'utiliser une base de données {{site.data.keyword.cloudant_short_notm}}. Il comprend les actions et les flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password | Utiliser une base de données Cloudant. |
| `/whisk.system/cloudant/read` | action | dbname, id | Lire un document dans une base de données. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Ecrire un document dans une base de données. |
| `/whisk.system/cloudant/changes` | flux | dbname, filter, query_params, maxTriggers | Exécuter des événements déclencheurs en cas de modifications effectuées dans une base de données. |
{: shortdesc}

Les sections suivantes vous guident pour configurer un package associé et utiliser des actions et des flux dans le package `/whisk.system/cloudant`. Pour plus d'informations sur la configuration de la base de données {{site.data.keyword.cloudant_short_notm}} et savoir comment y effectuer des opérations de lecture/écriture, voir [Actions {{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html).

## Créez un déclencheur à l'aide de la fonction de filtrage

Vous pouvez utiliser le flux `changes` pour configurer un service afin d'exécuter un déclencheur toutes les fois qu'une modification est apportée à votre base de données {{site.data.keyword.cloudant_short_notm}}.

Les paramètres utilisés dans cet exemple sont les suivants :

**dbname** : nom de la base de données {{site.data.keyword.cloudant_short_notm}} _(obligatoire)_.

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

Si vous ne parvenez pas à observer de nouvelles activations, voir la rubrique [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) qui illustre comment lire et écrire dans une base de données {{site.data.keyword.cloudant_short_notm}}. Testez les étapes de lecture et d'écriture pour vérifier que vos données d'identification {{site.data.keyword.cloudant_short_notm}} sont correctes.
{: tip}

## Structure de données d'un événement déclencheur

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

## Filtrage des événements de changement dans la base de données

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

## Utilisation d'une séquence d'actions et d'un déclencheur de changement pour traiter un document provenant d'une base de données {{site.data.keyword.cloudant_short_notm}}
{: #openwhisk_catalog_cloudant_read_change notoc}

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

Vous pouvez recréer les déclencheurs précédemment créés avec `includeDoc`. Pour recréer le déclencheur, procédez comme suit :
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

Vous pouvez utiliser cet exemple pour créer une séquence d'actions permettant de lire le document modifié et d'appeler vos actions existantes. N'oubliez pas de désactiver les règles qui ne sont plus valides et d'en créer de nouvelles avec le modèle de séquence d'actions.
