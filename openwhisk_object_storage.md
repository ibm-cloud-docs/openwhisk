---

copyright:
  years: 2019, 2019
lastupdated: "2019-03-29"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (Experimental) Object Storage events source
{: #cloud_object_storage}

The `/whisk.system/cos-experimental` package might be unstable, change frequently in ways that aren't compatible with earlier versions, and might be discontinued with a short notice. This package isn't recommended for use in production environments. This experimental package is currently available only in the US-South region.
{: important}

In this example, you will learn how to: 
* [Listen for changes](#listening_to_cos_bucket_changes) to a {{site.data.keyword.cos_full}} instance.
* [Create a trigger](#creating_a_trigger_cos) to respond to respond to those changes.
* [Create actions](#creating_action_to_process_object) to retrieve and process the changes.
* [Create a rule](#associating_action_with_change_trigger) to associate your action to the change trigger.

**Sample use case:** With the `/whisk.system/cos-experimental` package, you can listen for changes to GPS street data stored in a {{site.data.keyword.cos_full_notm}} bucket. Then, when changes occur, you can trigger the automatic regeneration of a GPS map, so that users can have access to the latest street data for their GPS application.

## About IBM Cloud Object Storage
{: #cloud_object_storage_info}

**Before you begin:** To learn about {{site.data.keyword.cos_full_notm}}, see [About Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage). For more information about setting up the {{site.data.keyword.cos_full_notm}} instance, see [Provision an instance {{site.data.keyword.cos_full_notm}} actions](/docs/services/cloud-object-storage/basics/developers.html#provision-an-instance-of-ibm-cloud-object-storage).

## Listening for changes to a IBM Cloud Object Storage bucket
{: #listening_to_cos_bucket_changes}

You can use {{site.data.keyword.openwhisk}} to listen for changes to an [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) bucket and use an action to process one or more objects from the bucket. 

The `/whisk.system/cos-experimental` package enables you to configure events from a {{site.data.keyword.cos_full_notm}} instance, and includes the following feed:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | package | apikey, auth_endpoint, bucket, endpoint, interval | Package containing the `changes` feed action. |
| `/whisk.system/cos-experimental/changes` | feed | apikey, auth_endpoint, bucket, endpoint, interval | Fire trigger events on changes to a {{site.data.keyword.cos_full_notm}} bucket. |
{: shortdesc}

You can use the `changes` feed to configure the {{site.data.keyword.cos_full_notm}} events source service to fire a trigger on every change to a bucket in your {{site.data.keyword.cos_full_notm}} instance.

Parameters that are used in this example:

**apikey**: _(Required, unless bound to the package)_ .The `apikey` parameter is IAM API key for the {{site.data.keyword.cos_full_notm}} instance.  Normally, this value is bound to the package. However, if the `apikey` value is specified when using the `changes` feed action, the specified value is used for the credentials instead of the bound credentials' apikey.

**auth_endpoint**: _(Optional)_. The `auth_endpoint` parameter is the authorization endpoint used by {{site.data.keyword.cos_full_notm}} to generate a token from the `apikey`.  The default endpoint is the {{site.data.keyword.cos_full_notm}} endpoint.

**bucket**: _(Required)_. The `bucket` parameter is the name of the {{site.data.keyword.cos_full_notm}} bucket.

**endpoint**:  _(Required)_. The `endpoint` parameter is the {{site.data.keyword.cos_full_notm}} endpoint used to connect to your {{site.data.keyword.cos_full_notm}} instance. You can locate your endpoint in the [{{site.data.keyword.cos_full_notm}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints).

**interval**: _(Optional)_. The `interval` parameter is the bucket polling interval, in whole minutes. The `interval` value must be at least 1 minute and is set to 1 minute by default.

## Creating a trigger to respond to the changes feed
{: #creating_a_trigger_cos}

When creating the trigger, you can avoid passing your {{site.data.keyword.cos_full_notm}} credentials to the `changes` feed action by binding your credentials directly to the `cos-experimental` package.
 {: shortdesc}
 
 1. First, create a package binding that can be modified to contain your credentials. The following creates a package binding, `myCosPkg`, in your namespace.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
  
 2. Bind your {{site.data.keyword.cos_full_notm}} credentials to the package. 
 Binding your {{site.data.keyword.cos_full_notm}} credentials to the package will bind the `apikey` value to the package so you won't need to specify the `apikey` value when the `changes` feed action is invoked. {: note}
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
  
 3. Create a trigger named `myCosTrigger` with the `changes` feed in the package binding that you created. Use your bucket name and {{site.data.keyword.cos_full_notm}} endpoint parameter values.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  Example output:
  ```
  ok: created trigger feed myCosTrigger
  ```
  {: screen}

4. Start polling for activations to give clear visibility of what is happening.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

5. Create an action to observe the change feed. For example, an action called `showCosChange` containing the following JavaScript code:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}

6. Create a rule to connect the `showCosChange` action to the `myCosTrigger` trigger:
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}

7. In your {{site.data.keyword.cos_full_notm}} dashboard, either modify an existing bucket object or create one. To learn how to add an object to your bucket, see [Add some objects to your bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects).

8. For each bucket object change, observe new activations for the `myCosTrigger` trigger and `showCosChange` action. These activations appear within the configured bucket polling interval.

If you are unable to observe new activations, verify that the `apikey`, `endpoint` and `bucket` parameter values are correct.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Data structure of a trigger event
{: #data_structure_trigger_event}

The content of the generated events has the following parameters:

  - `file`: The file or object metadata.
  - `status`: The detected change.  This value is either `added`, `modified` or `deleted`.
  - `bucket`: The name of the {{site.data.keyword.cos_full_notm}} bucket.
  - `endpoint`:  The {{site.data.keyword.cos_full_notm}} endpoint used to connect to the {{site.data.keyword.cos_full_notm}} instance.
  - `key`: The identifier of the changed bucket object. This value is the same as `file.Key`, but available at the top of the trigger event JSON.

Example JSON representation of the bucket change trigger event:
```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Creating an action to process the changed object
{: #creating_action_to_process_object}

You can create a single action that retrieves and processes the object. Or, you can create a sequence that uses one action to retrieve the object and another action to process the object.

### Creating an action to retrieve and process the object
{: #creating_action_to_retrieve_object}

This sample action code retrieves and processes the bucket change notification document. You can pass the `apikey` and `serviceInstanceId` parameters directly to the action during manual action invocation, but when this action invoked by a trigger these values must obtained from your {{site.data.keyword.cos_full_notm}} which must be bound to the action with the `ibmcloud fn service bind` command.

Example code:

```javascript
const COS = require('ibm-cos-sdk')

function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

  const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

  return new Promise(function(resolve, reject) {
    client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
      if (err != null) {
        console.log(err)
        reject({ err: err })
      } else {
        console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
    })
  });
}
```
{: codeblock}

Because this action uses the `ibm-cos-sdk` npm package, the action must be packaged as either a [Node.js module](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action) or a [single bundle](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action).

After packaging this action into a .zip file, create the action to retrieve and process the object from {{site.data.keyword.cos_full_notm}}:

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

[Bind the {{site.data.keyword.cos_full_notm}} credentials]((/docs/openwhisk/openwhisk_object_storage.html#binding_credentials_to_your_action) to this action. Then, [create a rule](#associating_action_with_the_change_trigger) to invoke this action when the trigger fires.

### Creating an action sequence to retrieve and process the object

Instead of including the object retrieval code in your action, you can use the `object-read` action from the {{site.data.keyword.cos_full_notm}} package, which must be [manually installed](/docs/openwhisk/cloud_object_storage_actions.html#cloud_object_storage_installation).  Your action code would only need to process the results returned from `object-read`.

Example code of an action that only processes the bucket object:
```javascript
function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. Create the action to process only the object from {{site.data.keyword.cos_full_notm}}:
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}

2. Bind your {{site.data.keyword.cos_full_notm}} credentials to your `cos-experimental` package binding.
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}

3. The `object-read` action can be composed with `myCosProcessObjectAction` to create an action sequence.
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

In addition to the `object-read` action, you can use other actions included in the installable {{site.data.keyword.cos_full_notm}} package.

[Bind the {{site.data.keyword.cos_full_notm}} credentials](/docs/openwhisk?topic=cloud-object-storage#cos_binding_credentials_to_action) to this action. Then, [create a rule](/docs/openwhisk?topic=cloud-object-storage#associating_action_with_change_trigger) to invoke this action when the trigger fires.

 ## Binding credentials to your action
 {: #cos_binding_credentials_to_action}
 
 You can avoid passing sensitive credentials during invocation by binding the {{site.data.keyword.cos_full_notm}} credentials to the action with the following command:
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## Creating a rule to associate the action with the change trigger
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

You can use an action or action sequence in a [rule](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use) to fetch and process the object that is associated with a {{site.data.keyword.cos_full_notm}} change event.

Create a rule that activates `MyCosAction` action on new {{site.data.keyword.cos_full_notm}} trigger events.
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



