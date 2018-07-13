---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant package
{: #cloudant_actions}

The `/whisk.system/cloudant` package enables you to work with a [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) database, and includes the following actions and Feeds:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password | Work with a Cloudant database. |
| `/whisk.system/cloudant/read` | action | dbname, id | Read a document from a database. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Write a document to a database. |
| `/whisk.system/cloudant/changes` | Feed | dbname, filter, query_params, maxTriggers | Fire trigger events on changes to a database. |
{: shortdesc}

The following sections step you through setting up a {{site.data.keyword.cloudant_short_notm}} database, and how to read and write to it.
For more information on how to use Feeds with the `/whisk.system/cloudant` package, see [{{site.data.keyword.cloudant_short_notm}} events source](./openwhisk_cloudant.html).

## Setting up a {{site.data.keyword.cloudant_short_notm}} database in the {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

If you're using {{site.data.keyword.openwhisk}} from the {{site.data.keyword.Bluemix_notm}}, {{site.data.keyword.openwhisk_short}} automatically creates package bindings for your {{site.data.keyword.cloudant_short_notm}} service instances. If you aren't using {{site.data.keyword.openwhisk_short}} and {{site.data.keyword.cloudant_short_notm}} from {{site.data.keyword.Bluemix_notm}}, skip to the next section.

1. Create a {{site.data.keyword.cloudant_short_notm}} service instance in your [{{site.data.keyword.Bluemix_notm}} dashboard](http://console.bluemix.net).

  Be sure to create a credential key for each new service instance.

2. Refresh the packages in your namespace. The refresh automatically creates a package binding for each {{site.data.keyword.cloudant_short_notm}} service instance with a credential key defined.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Example output:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  Your package binding now contains the credentials that are associated with your {{site.data.keyword.cloudant_short_notm}} service instance.

3. Check to see that the package binding that was created previously is configured with your {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}} service instance host and credentials.

  ```
  ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  Example output:
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

## Setting up a {{site.data.keyword.cloudant_short_notm}} database outside the {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_nocloud}

If you're not using {{site.data.keyword.openwhisk_short}} in the {{site.data.keyword.Bluemix_notm}} or if you want to set up your {{site.data.keyword.cloudant_short_notm}} database outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your {{site.data.keyword.cloudant_short_notm}} account. You need the {{site.data.keyword.cloudant_short_notm}} account host name, user name, and password.

1. Create a package binding that is configured for your {{site.data.keyword.cloudant_short_notm}} account.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. Verify that the package binding exists.
  ```
  wsk package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## Reading from a {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_read}

You can use an action to fetch a document from a {{site.data.keyword.cloudant_short_notm}} database called **testdb**. Make sure that this database exists in your {{site.data.keyword.cloudant_short_notm}} account.

- Fetch a document by using the **read** action in the package binding that you created previously. Be sure to replace `/_/myCloudant` with your package name.
  ```
  ibmcloud wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
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

## Writing to a {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_write}

You can use an action to store a document in a {{site.data.keyword.cloudant_short_notm}} database called **testdb**. Make sure that this database exists in your {{site.data.keyword.cloudant_short_notm}} account.

1. Store a document by using the **write** action in the package binding you created previously. Be sure to replace `/_/myCloudant` with your package name.
  ```
  ibmcloud wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
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
