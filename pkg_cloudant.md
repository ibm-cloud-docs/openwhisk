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

The pre-installed `/whisk.system/cloudant` package enables you to work with a [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) database. No service binding is required to use this package.



## Available actions and feed
{: #cloudant_available}

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | package | dbname, host, username, password | Work with a Cloudant database. |
| `/whisk.system/cloudant/read` | action | dbname, id | Read a document from a database. |
| `/whisk.system/cloudant/write` | action | dbname, overwrite, doc | Write a document to a database. |
| `/whisk.system/cloudant/changes` | feed | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Fire trigger events on changes to a database. |
{: shortdesc}

For more information on how to use feeds with the `/whisk.system/cloudant` package, see [{{site.data.keyword.cloudant_short_notm}} events source](#cloudant_events).



## Setting up an {{site.data.keyword.cloudant_short_notm}} database in the {{site.data.keyword.Bluemix_notm}}
{: #cloudant_db}

If you're using {{site.data.keyword.openwhisk}} from the {{site.data.keyword.Bluemix_notm}} you can use the {{site.data.keyword.openwhisk}} CLI plug-in to bind a service to an action or package.

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


### Create a trigger by using the filter function
{: #cloudant_trigger}

You can use the `changes` feed to configure a service to fire a trigger on every change to your {{site.data.keyword.cloudant_short_notm}} database.

The parameters used in this example are as follows:

**dbname**: The name of the {{site.data.keyword.cloudant_short_notm}} database _(required)_.

**iamApiKey**: The IAM API key for the Cloudant database.  If specified will be used as the credentials instead of username and password _(optional)_.

**iamUrl**: The IAM token service url that is used when `iamApiKey` is specified.  Defaults to `https://iam.bluemix.net/identity/token` _(optional)_.

**maxTriggers**: Stop firing triggers when this limit is reached _(optional)_. Defaults to infinite.

**filter**: Filter function that is defined on a design document _(optional)_

**query_params**: Extra query parameters for the filter function _(optional)_.

1. Create a trigger named **myCloudantTrigger** with the `changes` feed in the package binding that you created previously. Including the `filter` and `query_params` functions to fire the trigger when a document is added (or modified) when the status is `new`.

  Be sure to replace `/_/myCloudant` with your package name.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  Example output:
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. Start polling for activations to give clear visibility of what is happening.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. Create an action that we can use to observe the effect of the change feed. For example, an action called **showCloudantChange** containing the following JavaScript code:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. Create a rule to connect the **showCloudantChange** action to the trigger created earlier:
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Create actions and a rule to associate them to the **myCloudantTrigger** trigger.

6. In your {{site.data.keyword.cloudant_short_notm}} dashboard, either modify an existing document or create a new one. The document should have a _status_ field, which is set to **new**.

7. Observe new activations for the **myCloudantTrigger** trigger for each document change only if the document status is **new** based on the filter function and query parameter.

Test the reading and writing steps to help to verify that your {{site.data.keyword.cloudant_short_notm}} credentials are correct.

### Data structure of a trigger event
{: #cloudant_struct}

The content of the generated events has the following parameters:

  - `id`: The document ID.
  - `seq`: The sequence identifier that is generated by {{site.data.keyword.cloudant_short_notm}}.
  - `changes`: An array of objects, each of which has a `rev` field that contains the revision ID of the document.

The JSON representation of the trigger event is as follows:
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

### Filter database change events
{: #cloudant_filter}

You may optionally define a filter function to avoid having unnecessary change events that fire your trigger.

To create a new filter function, you can use an action.

Create a json document file `design_doc.json` with the following filter function:
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

Create a design document on the database with the following filter function:
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

The information for the new design document is printed on the screen:
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### Using an action sequence and a change trigger to process a document from an {{site.data.keyword.cloudant_short_notm}} database
{: #cloudant_seq}

You can use an action sequence in a rule to fetch and process the document that is associated with an {{site.data.keyword.cloudant_short_notm}} change event.

Sample code of an action that handles a document:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

Create the action to process the document from {{site.data.keyword.cloudant_short_notm}}:
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

To read a document from the database, you can use the `read` action from the {{site.data.keyword.cloudant_short_notm}} package.
The `read` action can be composed with `myAction` to create an action sequence.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

The action `sequenceAction` can be used in a rule that activates the action on new {{site.data.keyword.cloudant_short_notm}} trigger events.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Note:** The {{site.data.keyword.cloudant_short_notm}} `changes` trigger used to support the parameter `includeDoc` which is no longer supported.

You can re-create triggers previously created with `includeDoc`. Follow these steps to re-create the trigger:
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

The example can be used to create an action sequence to read the changed document and call your existing actions. Remember to disable any rules that are no longer valid, and create new ones by using the action sequence pattern.

