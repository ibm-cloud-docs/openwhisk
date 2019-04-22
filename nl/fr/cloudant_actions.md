---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Package Cloudant
{: #cloudant_actions}

Le package `/whisk.system/cloudant` vous permet d'utiliser une base de données [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). Il comprend les actions et les flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password | Utiliser une base de données Cloudant. |
| `/whisk.system/cloudant/read` | action | dbname, id | Lire un document dans une base de données. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Ecrire un document dans une base de données. |
| `/whisk.system/cloudant/changes` | flux | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Exécuter des événements déclencheurs en cas de modifications effectuées dans une base de données. |
{: shortdesc}

Les sections suivantes vous guident pour configurer une base de données {{site.data.keyword.cloudant_short_notm}} et savoir comment effectuer des opérations de lecture/écriture dans cette base de données. Pour plus d'informations sur l'utilisation des flux avec le package `/whisk.system/cloudant`, voir [Source d'événements {{site.data.keyword.cloudant_short_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

## Configuration d'une base de données {{site.data.keyword.cloudant_short_notm}} dans {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Si vous utilisez {{site.data.keyword.openwhisk}} à partir d'{{site.data.keyword.Bluemix_notm}}, vous pouvez utiliser le [plug-in d'interface de ligne de commande {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) pour lier un service à une action ou un package. 

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
  

## Lecture à partir d'une base de données {{site.data.keyword.cloudant_short_notm}}
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
