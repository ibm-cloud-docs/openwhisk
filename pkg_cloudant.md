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

With the pre-installed `/whisk.system/cloudant` package, you can work with a [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) database. No service binding is required to use this package.
{: shortdesc}


## Available entities
{: #cloudant_available}
The following table shows a selection of the entities available in the `whisk.system/cloudant` package. You can use the `whisk.system/cloudant` package to read, write, update, or delete documents. You can also use the `changes` feed to listen for changes to a {{site.data.keyword.cloudant_short_notm}} database.
{: shortdesc}

For a full list of the entities that are available in the `/whisk.system/cloudant` package, run `ibmcloud fn package get /whisk.system/cloudant`.
{: note}

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Package | `dbname`, `host`, `username`, `password` | Work with a Cloudant database. |
| `/whisk.system/cloudant/read` | Action | `dbname`, `id` | Read a document from a database. |
| `/whisk.system/cloudant/write` | Action | `dbname`, `overwrite`, `doc` | Write a document to a database. |
| `/whisk.system/cloudant/update-document` | Action | `dbname`, `doc` | Update a document in a database. |
| `/whisk.system/cloudant/changes` | Feed | `dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | Fire trigger events on changes to a database. |

The `includeDoc` parameter is no longer supported for use with `/whisk.system/cloudant/changes` feed. If you created triggers that use this parameter, you must re-create them without the `includeDoc` parameter.
{: deprecated}

## Binding the `/whisk.system/cloudant` package to your {{site.data.keyword.cloudant_short_notm}} database.
If you're using {{site.data.keyword.openwhisk}} from the {{site.data.keyword.cloud_notm}}, you can use the {{site.data.keyword.openwhisk}} CLI plug-in to bind a service to an action or package.
{: #cloudant_db}

**Before you begin**
You must have an instance of {{site.data.keyword.cloudant_short_notm}}. To create an instance, see [Getting started with {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started).

1. Create a `/whisk.system/cloudant` package binding that is configured for your {{site.data.keyword.cloudant_short_notm}} account. In this example, the package name is `myCloudant`.

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Verify that the package binding exists.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Example output**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. Get the name of the service instance that you want to bind to an action or package.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Example output**
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

    **Example output**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Bind the service to the package you created in step one.

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verify that the credentials are successfully bound.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **Example output**

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

In this example, the credentials for the {{site.data.keyword.cloudant_short_notm}} service belong to a parameter named `__bx_creds`.

## Working with documents in a {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_read}

You can use an action to read, write, update, delete a document from an {{site.data.keyword.cloudant_short_notm}} database.
{: shortdesc}

### Reading a document
You can use the `/whisk.system/cloudant/read` action to read a document from your {{site.data.keyword.cloudant_short_notm}} database.

**Before you begin**
If you do not have a document in your {{site.data.keyword.cloudant_short_notm}} database, you can create one by using the [{{site.data.keyword.cloudant_short_notm}} dashboard. The URL for the dashoard is `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`.

Fetch a document by using the `read` action. Replace `/_/myCloudant` with your package name, `<database_name>` with your database name, and `<document_id>` with the file ID. Invoke the action to test fetching a document.

**Command syntax**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**Example read action from a `test` database**
Invoke the action to test reading a file. This example reads a file with the `id` of `9f86f4955e7a38ab0169462e6ac0f476` which is an empty file.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**Example output**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### Writing a document to an {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_write}

You can use an action to create or update documents in an {{site.data.keyword.cloudant_short_notm}} database.
{: shortdesc}

**Before you begin**
  Create a `/whisk.system/cloudant` [package binding](#cloudant_db) that is configured for your {{site.data.keyword.cloudant_short_notm}} account.

1. Store a document by using the `write` action in the package binding you created. Replace `/_/myCloudant` with your package name, replace `<database_name>` with the name of your database, `<document_id>` with your document ID, and `<test_name>` with a name.

  **Command syntax**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **Example write action to a `test` database**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **Example output**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verify that the document exists in the {{site.data.keyword.cloudant_short_notm}} dashboard. The dashboard URL for the `test` database is in the following format: `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`.

  **Example document in {{site.data.keyword.cloudant_short_notm}} dashboard**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### Updating a document
You can use the `/update-document` action to update a document in your {{site.data.keyword.cloudant_short_notm}} database.
{: short desc}

**Before you begin**
Create a `/whisk.system/cloudant` [package binding](#cloudant_db) that is configured for your {{site.data.keyword.cloudant_short_notm}} account.

The following example updates the document that was created in the [Writing a document to an {{site.data.keyword.cloudant_short_notm}} database](#cloudant_write) section.
{: note}

You can update a document in your database by replacing `<test>` with your database name and replacing the `--param doc` flag with the `id` and contents of the document in your database that you want to update.


1. You can update a document in the `test` database by running the following command. This example adds the `shade` value to the `color` document. 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **Output**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. To see the update, fetch the document again.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **Example document**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## Create a trigger by using the filter function
{: #cloudant_trigger}

You can use the `changes` feed to configure a service to fire a trigger on every change to your {{site.data.keyword.cloudant_short_notm}} database.

**Before you begin**
  Create a `/whisk.system/cloudant` [package binding](#cloudant_db) that is configured for your {{site.data.keyword.cloudant_short_notm}} account.

Parameters that are used in this example.

| Parameter | Description |
| --- | --- |
| `dbname` | (Required) The name of the {{site.data.keyword.cloudant_short_notm}} database. |
| `iamApiKey` | (Optional) The IAM API key for the Cloudant database.  If specified, this value is used as the credentials instead of username and password. |
| `iamUrl` | (Optional) The IAM token service URL that is used when `iamApiKey` is specified.  The default value is `https://iam.cloud.ibm.com/identity/token`. | 
| `maxTriggers` | (Optional) Stop firing triggers when this limit is reached. Defaults to infinite. |
| `filter` | (Optional) The filter function that is defined in a design document. |
| `query_params` | (Optional) Any additional query parameters that might be needed for the filter function. |
| `includeDoc` | (Deprecated) The `includeDoc` parameter is no longer supported for use with `/whisk.system/cloudant/changes` feed. |

</br>

1. Create a trigger named `cloudantTrigger` with the `changes` feed in the package binding that you created previously. Including the `filter` and `query_params` functions to fire the trigger when a document is added (or modified) when the status is `new`.

  Replace `/_/myCloudant` with the name of your package. This example uses a database named `test`.
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **Example output**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. Save the following JavaScript code as `cloudantChange.js`.

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. Create an action called `cloudantChange` that you can use to observe the changes feed. Replace `<file_path>` with the file path to your `cloudantChange.js` file on your computer.

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. Create a rule named `cloudantRule` to connect the `cloudantChange` action to the `cloudantTrigger` that you created earlier.

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. In another terminal window, start polling so that you can see when activations occur.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. In your {{site.data.keyword.cloudant_short_notm}} dashboard, either modify an existing document or create one.

7. Observe activations for the `cloudantTrigger` trigger for each document change.

**Example activation of the `cloudantTrigger`**

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

### Data structure of a trigger activation
{: #cloudant_struct}

The content of the generated event has the following parameters.

| Parameter | Description |
| --- | --- |
| `id` | The document ID. |
| `seq` | The sequence identifier that is generated by {{site.data.keyword.cloudant_short_notm}}. |
| `changes` | An array of objects, each of which has a `rev` field that contains the revision ID of the document. |

**JSON representation of the trigger activation**

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

## Filter database change events
{: #cloudant_filter}

You might define a filter function to avoid having unnecessary change events that fire your trigger.

**Before you begin**
Create a `/whisk.system/cloudant` [package binding](#cloudant_db) that is configured for your {{site.data.keyword.cloudant_short_notm}} account.

To create a filter function, you can use an action.

1. Save the following JSON following filter in a file called `design_doc.json`.

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

2. Create a design document in the database with the following filter function. Replace `<database_name>` with the name of your database and `<file_path>` with the file path of your `design_doc.json`. Invoke the `write` action to test creating a design document.

**Command syntax**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**Example command to write a `design_doc.json` file to a `test` database**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**Example output**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


For more information about {{site.data.keyword.cloudant_short_notm}} design documents, see [Design documents](/docs/services/Cloudant?topic=cloudant-design-documents)

## Processing an individual document by using an action sequence
{: #cloudant_seq}

You can use an action sequence in a rule to fetch and process the document that is associated with an {{site.data.keyword.cloudant_short_notm}} change event.

**Before you begin**
Create a `/whisk.system/cloudant` [package binding](#cloudant_db) that is configured for your {{site.data.keyword.cloudant_short_notm}} account. 

This example updates the document that was created in the [Writing a document to an {{site.data.keyword.cloudant_short_notm}} database](#cloudant_write) section.
{: note}

### Creating an action to process an individual document

To create an action that handles changes to an individual document, run the following commands.
{: shortdesc}

1. Save the following code as `docChange.js`

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. Create an action called `docChange` to process the document with the name `blue` that you created earlier. Replace `<file_path>` with the file path of your `docChange.js`

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **Output**
  ```
  ok: created action docChange
  ```
  {: screen}

### Create a sequence with the `read` action 

The `read` action can be composed with your `docChange` action to create an action sequence.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **Output**
  ```
  ok: created action docSequence
  ```
  {: screen}

### Create a trigger with `changes` feed

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### Create a rule to associate the trigger with the sequence

The action `docSequence` can be used in a rule that activates the action on new {{site.data.keyword.cloudant_short_notm}} trigger events.

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **Output**
  ```
  ok: created rule docRule
  ```

  **Example activation**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### Test the sequence

1. Test the `docSequence` by making a change to the `blue` file that you created earlier. In this example, the `shade` value is changed to `indigo`.

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **Example activation**

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

2. Verify the file was updated to include the `shade` value by invoking the `read` action. Replace `<database>` name with the name of you database.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **Output**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### Next steps
Now that you are listening for changes to a document in your {{site.data.keyword.cloudant_short_notm}} database, you can trigger Slack notifications for the changes by using the [`/whisk.system/slack` package](/docs/openwhisk?topic=cloud-functions-pkg_slack).


