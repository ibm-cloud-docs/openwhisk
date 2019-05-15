---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-07"

keywords: cloudant, database, actions

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloudant package
{: #cloudant_actions}

The pre-installed `/whisk.system/cloudant` package enables you to work with a [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) database, and includes the following actions and feeds:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password | Work with a Cloudant database. |
| `/whisk.system/cloudant/read` | action | dbname, id | Read a document from a database. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Write a document to a database. |
| `/whisk.system/cloudant/changes` | feed | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Fire trigger events on changes to a database. |
{: shortdesc}

The following sections step you through setting up an {{site.data.keyword.cloudant_short_notm}} database, and how to read and write to it.
For more information on how to use feeds with the `/whisk.system/cloudant` package, see [{{site.data.keyword.cloudant_short_notm}} events source](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

## Setting up an {{site.data.keyword.cloudant_short_notm}} database in the {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

If you're using {{site.data.keyword.openwhisk}} from the {{site.data.keyword.Bluemix_notm}} you can use the [{{site.data.keyword.openwhisk}} CLI plug-in](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) to bind a service to an action or package.

You must first manually create a package binding for your {{site.data.keyword.cloudant_short_notm}} account.

1. Create a package binding that is configured for your {{site.data.keyword.cloudant_short_notm}} account.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Verify that the package binding exists.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. Get the name of the service instance that you want to bind to an action or package.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    Example output:
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. Get the name of the credentials that are defined for the service instance you got in the previous step.
    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    Example output:
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Bind the service to the package you created in step 1.
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verify that the credentials are successfully bound.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    Example output:
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

    In this example, the credentials for the Cloudant service belong to a parameter named `__bx_creds`.


## Reading from an {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_read}

You can use an action to fetch a document from an {{site.data.keyword.cloudant_short_notm}} database called **testdb**. Make sure that this database exists in your {{site.data.keyword.cloudant_short_notm}} account.

- Fetch a document by using the **read** action in the package binding that you created previously. Be sure to replace `/_/myCloudant` with your package name.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  Example output:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

## Writing to an {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_write}

You can use an action to store a document in an {{site.data.keyword.cloudant_short_notm}} database called **testdb**. Make sure that this database exists in your {{site.data.keyword.cloudant_short_notm}} account.

1. Store a document by using the **write** action in the package binding you created previously. Be sure to replace `/_/myCloudant` with your package name.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  Example output:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verify that the document exists by browsing for it in your {{site.data.keyword.cloudant_short_notm}} dashboard.

  The dashboard URL for the **testdb** database looks something like the following: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.

