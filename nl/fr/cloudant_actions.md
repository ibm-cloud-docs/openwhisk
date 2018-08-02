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

# Cloudant
{: #cloudant_actions}

Le package `/whisk.system/cloudant` vous permet d'utiliser une base de données [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant). Il comprend les actions et les flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password |Utiliser une base de données Cloudant. |
| `/whisk.system/cloudant/read` | action | dbname, id | Lire un document dans une base de données. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Ecrire un document dans une base de données. |
| `/whisk.system/cloudant/changes` | Flux | dbname, filter, query_params, maxTriggers | Exécuter des événements déclencheurs en cas de modifications effectuées dans une base de données. |
{: shortdesc}

Les sections suivantes vous guident pour configurer une base de données {{site.data.keyword.cloudant_short_notm}} et savoir comment effectuer des opérations de lecture/écriture dans cette base de données.
Pour plus d'informations sur l'utilisation des flux avec le package `/whisk.system/cloudant`, voir [Source d'événements {{site.data.keyword.cloudant_short_notm}}](./openwhisk_cloudant.html).

## Configuration d'une base de données {{site.data.keyword.cloudant_short_notm}} dans {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Si vous utilisez {{site.data.keyword.openwhisk}} à partir d'{{site.data.keyword.Bluemix_notm}}, {{site.data.keyword.openwhisk_short}} crée automatiquement les liaisons de package pour vos instances de service {{site.data.keyword.cloudant_short_notm}}. Si vous n'utilisez pas {{site.data.keyword.openwhisk_short}} et {{site.data.keyword.cloudant_short_notm}} à partir d'{{site.data.keyword.Bluemix_notm}}, passez à la section suivante.

1. Créez une instance de service {{site.data.keyword.cloudant_short_notm}} dans votre [tableau de bord {{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

  Prenez soin de créer une clé de données d'identification pour chaque nouvelle instance de service. 

2. Actualisez les packages dans votre espace de nom. Cette opération crée automatiquement une liaison de package pour chaque instance de service {{site.data.keyword.cloudant_short_notm}} ayant une clé de données d'identification définie.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Exemple de sortie :
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  Votre liaison de package contient désormais les données d'identification associées à votre instance de service {{site.data.keyword.cloudant_short_notm}}.

3. Vérifiez que la liaison de package qui a été créée précédemment est configurée avec l'hôte et les données d'identification de votre instance de service {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.cloudant_short_notm}}.

  ```
  ibmcloud fn package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

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
  {: screen}

## Configuration d'une base de données {{site.data.keyword.cloudant_short_notm}} en dehors d'{{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_nocloud}

Si vous n'utilisez pas {{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}} ou si vous souhaitez configurer votre base de données {{site.data.keyword.cloudant_short_notm}} en dehors d'{{site.data.keyword.Bluemix_notm}}, vous devez créer manuellement une liaison de package pour votre compte {{site.data.keyword.cloudant_short_notm}}. Vous avez besoin du nom d'hôte, du nom d'utilisateur et du mot de passe du compte {{site.data.keyword.cloudant_short_notm}}.

1. Créez une liaison de package configurée pour votre compte {{site.data.keyword.cloudant_short_notm}}.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. Vérifiez que la liaison de package existe.
  ```
  wsk package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /monEspaceNom/monCloudant private binding
  ```
  {: screen}

## Lecture dans une base de données {{site.data.keyword.cloudant_short_notm}}
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

## Ecriture dans une base de données {{site.data.keyword.cloudant_short_notm}}
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
