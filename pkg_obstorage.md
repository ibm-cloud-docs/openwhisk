---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: object storage, bucket, package

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

# Object Storage
{: #pkg_obstorage}

You can extend the functionality of your {{site.data.keyword.openwhisk}} app by integrating with an {{site.data.keyword.cos_full}} instance.

**Before you begin:** To learn about {{site.data.keyword.cos_full_notm}}, see [About Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). For more information about setting up the {{site.data.keyword.cos_full_notm}} instance, see [Provision an instance {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Packages
{: #obstorage_packages}
| Package | Availability | Description |
| --- | --- | --- |
| [{{site.data.keyword.cos_full_notm}} package](#pkg_obstorage)| Installable | Read, write, and delete from an {{site.data.keyword.cos_full_notm}} instance. |
| [(Experimental) {{site.data.keyword.cos_full_notm}} events source](#pkg_obstorage_ev) | Pre-installed (US-South only) | Listen for changes to an {{site.data.keyword.cos_full_notm}} instance. |

## Creating an IBM Cloud Object Storage service instance
{: #pkg_obstorage_service}

Before you can use either package, you must request an instance of {{site.data.keyword.cos_full_notm}} and create at least one bucket.

1. [Create an {{site.data.keyword.cos_full_notm}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Create a set of HMAC service credentials ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) for the {{site.data.keyword.cos_full_notm}} service instance. In the **Add Inline Configuration Parameters (Optional)** field, add `{"HMAC":true}`.

3. [Create at least one bucket ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Reading and writing to a bucket with the {{site.data.keyword.cos_full_notm}} package
{: #pkg_obstorage_install}

After you have an {{site.data.keyword.cos_full_notm}} service instance, you can use either the {{site.data.keyword.openwhisk}} CLI or UI to install the {{site.data.keyword.cos_full_notm}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #pkg_obstorage_cli}

Before you begin:

[Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

To install the {{site.data.keyword.cos_full_notm}} package:

1. Clone the {{site.data.keyword.cos_full_notm}} package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to either the `runtimes/nodejs` or `runtimes/python` directory. Actions in the {{site.data.keyword.cos_full_notm}} package are deployed in the runtime that you choose.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package. You can repeat the previous steps to redeploy the package in another runtime.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Verify that the `cloud-object-storage` package is added to your package list.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.cos_full_notm}} instance you created to the package.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Verify that the package is configured with your {{site.data.keyword.cos_full_notm}} service instance credentials.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #pkg_obstorage_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create).

2. By using the **Cloud Foundry Org** and **Cloud Foundry Space** lists, select the namespace in which you want to install the {{site.data.keyword.cos_full_notm}} package. Namespaces are formed from the combined `org` and `space` names.

3. Click **Install Packages**.

4. Click the **IBM Cloud Object Storage** Package group, then click the **IBM Cloud Object Storage** Package.

5. In the **Available Runtimes** section, select either `Node.JS` or `Python` from the drop-down list. Then, click **Install**.

6. Once the package has been installed you will be redirected to the Actions page and can search for your new Package, which is named **cloud-object-storage**.

7. To use the actions in the **cloud-object-storage** package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 5 and 6 in the CLI instructions listed above.
  * To bind service credentials to individual actions, complete the following steps in the UI. **Note**: You must complete the following steps for each action that you want to use.
    1. Click an action from the **cloud-object-storage** package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.


### Available actions
{: #pkg_obstorage_actions}

The {{site.data.keyword.cos_full_notm}} package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | package | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Work with an {{site.data.keyword.cos_full_notm}} instance. |
| `/cloud-object-storage/object-write` | action | bucket, key, body, endpoint, ibmAuthEndpoint | Write an object to a bucket. |
| `/cloud-object-storage/object-read` | action | bucket, key, endpoint, ibmAuthEndpoint | Read an object from a bucket. |
| `/cloud-object-storage/object-delete` | action | bucket, key, endpoint, ibmAuthEndpoint | Delete an object from a bucket. |
| `/cloud-object-storage/bucket-cors-put` | action | bucket, corsConfig, endpoint, ibmAuthEndpoint | Assign a CORS configuration to a bucket. |
| `/cloud-object-storage/bucket-cors-get` | action | bucket, endpoint, ibmAuthEndpoint | Read the CORS configuration of a bucket. |
| `/cloud-object-storage/bucket-cors-delete` | action | bucket, endpoint, ibmAuthEndpoint | Delete the CORS configuration of a bucket. |
| `/cloud-object-storage/client-get-signed-url` | action | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Obtain a signed URL to restrict the Write, Read, and Delete of an object from a bucket. |


### Package parameters
{: #pkg_obstorage_pkgparams}

The following parameters are expected to be bound to the package; this will make them automatically available for all actions. It is also possible to specify these parameters when invoking one of the actions.

**apikey**: The `apikey ` parameter is IAM API key for the {{site.data.keyword.cos_full_notm}} instance.

**resource_instance_id**: The `resource_instance_id` parameter is the {{site.data.keyword.cos_full_notm}} instance identifier.

**cos_hmac_keys**: The `cos_hmac_keys` parameter is the {{site.data.keyword.cos_full_notm}} instance HMAC credentials, which includes the `access_key_id` and `secret_access_key` values.  These credentials are used exclusively by the `client-get-signed-url` action.  Refer to [Using HMAC Credentials](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) for instructions on how to generate HMAC credentials for your {{site.data.keyword.cos_full_notm}} instance.

#### Action parameters
{: #pkg_obstorage_actparams}

The following parameters are specified when invoking the individual actions.  Not all of these parameters are supported by every action; refer to the above table to see which parameters are supported by which action.

**bucket**: The `bucket` parameter is the name of the {{site.data.keyword.cloud_object_storage_short_notm}} bucket.

**endpoint**: The `endpoint` parameter is the {{site.data.keyword.cos_full_notm}} endpoint used to connect to your {{site.data.keyword.cos_full_notm}} instance. You can locate your endpoint in the [{{site.data.keyword.cos_full_notm}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints).

**expires**: The `expires` parameter is the number of seconds to expire the pre-signed URL operation.  The default `expires` value is 15 minutes.

**ibmAuthEndpoint**: The `ibmAuthEndpoint ` parameter is the IBM Cloud authorization endpoint used by {site.data.keyword.cos_short}} to generate a token from the `apikey`. The default authorization endpoint should work for all IBM Cloud Regions.

**key**: The `key` parameter is the bucket object key.

**operation**: The `operation` parameter is the pre-signed URL's operation to call.

**corsConfig**: The `corsConfig` parameter is a bucket's CORS configuration.


### Writing to an {{site.data.keyword.cos_full_notm}} bucket
{: #pkg_obstorage_write}

You can use the `object-write` action to write an object to an {{site.data.keyword.cos_full_notm}} bucket.
{: shortdesc}

**Note**: In the following steps, the name `testbucket` is used as an example. Buckets in {{site.data.keyword.cos_full_notm}} must be globally unique, so you must replace `testbucket` with a unique bucket name.

#### Write to a bucket from the CLI
{: #pkg_obstorage_write_cli}

Write an object to your bucket by using the `object-write` action.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

Example output:
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### Write to a bucket from the UI
{: #pkg_obstorage_write_ui}

1. Go to the [Actions page in the {{site.data.keyword.openwhisk_short}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/actions).

2. Under the `cloud-object-storage` package, click the **object-write** action.

3. In the Code pane, click **Change Input**.

4. Enter a JSON object that contains your bucket, key, and body as object keys.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. Click **Save**.

6. Click **Invoke**.

7. Verify that the output looks similar to the following:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

### Reading from an {{site.data.keyword.cos_full_notm}} bucket
{: #pkg_obstorage_read}

You can use the `object-read` action to read from an object in an {{site.data.keyword.cos_full_notm}} bucket.
{: shortdesc}

**Note**: In the following steps, the name `testbucket` is used as an example. Buckets in {{site.data.keyword.cos_full_notm}} must be globally unique, so you must replace `testbucket` with a unique bucket name.

#### Read from a bucket from the CLI
{: #pkg_obstorage_read_cli}

Read from an object in your bucket by using the `object-read` action.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

Example output:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

#### Read from a bucket from the UI
{: #pkg_obstorage_read_ui}

1. Go to the [Actions page in the {{site.data.keyword.openwhisk_short}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/actions).

2. Under the `cloud-object-storage` package, click the **object-read** action.

3. In the Code pane, click **Change Input**.

4. Enter a JSON object that contains your bucket and key as object keys.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. Click **Save**.

6. Click **Invoke**.

7. Verify that the output looks similar to the following:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Listening for changes to a bucket with the (Experimental) Object Storage events source
{: #pkg_obstorage_ev}

The `/whisk.system/cos-experimental` package might be unstable, change frequently in ways that aren't compatible with earlier versions, and might be discontinued with a short notice. This package isn't recommended for use in production environments. This experimental package is currently available only in the US-South region.
{: important}

You can use {{site.data.keyword.openwhisk}} to listen for changes to an [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) bucket and use an action to process one or more objects from the bucket.

<br>

**Sample use case:** With the `/whisk.system/cos-experimental` package, you can listen for changes to GPS street data stored in a {{site.data.keyword.cos_full_notm}} bucket. Then, when changes occur, you can trigger the automatic regeneration of a GPS map, so that users can have access to the latest street data for their GPS application.

### (Experimental) Object Storage events source parameters
{: #pkg_obstorage_ev_ch}

The `/whisk.system/cos-experimental` package enables you to configure events from a {{site.data.keyword.cos_full_notm}} instance, and includes the following feed:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | package | apikey, auth_endpoint, bucket, endpoint, interval | Package containing the `changes` feed action. |
| `/whisk.system/cos-experimental/changes` | feed | apikey, auth_endpoint, bucket, endpoint, interval | Fire trigger events on changes to a {{site.data.keyword.cos_full_notm}} bucket. |
{: shortdesc}

You can use the `changes` feed to configure the {{site.data.keyword.cos_full_notm}} events source service to fire a trigger on every change to a bucket in your {{site.data.keyword.cos_full_notm}} instance.

Parameters that are used in this example:

**apikey**: _(Required, unless bound to the package)_. The `apikey` parameter is IAM API key for the {{site.data.keyword.cos_full_notm}} instance.  Normally, this value is bound to the package. However, if the `apikey` value is specified when using the `changes` feed action, the specified value is used for the credentials instead of the bound credentials' apikey.

**auth_endpoint**: _(Optional)_. The `auth_endpoint` parameter is the authorization endpoint used by {{site.data.keyword.cos_full_notm}} to generate a token from the `apikey`.  The default endpoint is the {{site.data.keyword.Bluemix}} endpoint.

**bucket**: _(Required)_. The `bucket` parameter is the name of the {{site.data.keyword.cos_full_notm}} bucket.

**endpoint**:  _(Required)_. The `endpoint` parameter is the {{site.data.keyword.cos_full_notm}} endpoint used to connect to your {{site.data.keyword.cos_full_notm}} instance. You can locate your endpoint in the [{{site.data.keyword.cos_full_notm}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints).

**interval**: _(Optional)_. The `interval` parameter is the bucket polling interval, in whole minutes. The `interval` value must be at least 1 minute and is set to 1 minute by default.

### Creating a trigger to respond to the changes feed
{: #pkg_obstorage_ev_trig}

When creating the trigger, you can avoid passing your {{site.data.keyword.cos_full_notm}} credentials to the `changes` feed action by binding your credentials directly to the `cos-experimental` package.
 {: shortdesc}

 1. First, create a package binding that can be modified to contain your credentials. The following creates a package binding, `myCosPkg`, in your namespace.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Bind your {{site.data.keyword.cos_full_notm}} credentials to the package.
 Binding your {{site.data.keyword.cos_full_notm}} credentials to the package will bind the `apikey` value to the package so you won't need to specify the `apikey` value when the `changes` feed action is invoked.
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Create a trigger named `myCosTrigger` with the `changes` feed in the package binding that you created. Use your bucket name and {{site.data.keyword.cos_full_notm}} endpoint parameter values.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

    Example output:
    ```
    ok: created trigger feed myCosTrigger
    ```
  {: pre}
 4. Create a simple action that only serves to verify the trigger, the change feed, and the rule are all configured and working correctly. For example, create an action called `showCosChange` containing the following `showCosChange.js` JavaScript code:
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
Sample code showing
 5. Create a rule to connect the `showCosChange` action to the `myCosTrigger` trigger:
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 6. In a separate window, start polling for activations to give clear visibility of what is happening. When the trigger fires and the action is run, this command will list the activation records for each of these operations as they occur.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 7. In your {{site.data.keyword.cos_full_notm}} dashboard, either modify an existing bucket object or create one. To learn how to add an object to your bucket, see [Add some objects to your bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).

 8. For each bucket object change, observe new activations for the `myCosTrigger` trigger and `showCosChange` action. These activations appear in your window running the `ibmcloud fn activation poll` command within the configured bucket polling interval.

If you are unable to observe new activations, verify that the `apikey`, `endpoint` and `bucket` parameter values are correct.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Data structure of an Object Storage trigger event
{: #pkg_obstorage_ev_data}

The content of the generated events has the following parameters:

  - `file`: The file or object metadata. This structure is described in [List objects in a specific bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets).
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

### Creating an action to process the changed object
{: #pkg_obstorage_ev_act}

You can create a single action that retrieves and processes the object. Or, you can create a sequence that uses one action to retrieve the object and another action to process the object.

### Creating an action to retrieve and process the object
{: #pkg_obstorage_ev_act_ret}

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
exports.main = main;
```
{: codeblock}

Because this action uses the `ibm-cos-sdk` npm package, the action must be packaged as either a [Node.js module](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) or a [single bundle](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

After packaging this action into a .zip file, `myCosAction.zip`, create the action to retrieve and process the object from {{site.data.keyword.cos_full_notm}}:

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

### Creating an action sequence to retrieve and process the object
{: #pkg_obstorage_ev_act_seq}

Instead of including the object retrieval code in your action, you can use the `object-read` action from the `cloud-object-storage` package, which must be [manually installed](#pkg_obstorage_install).  Your action code would only need to process the results returned from `object-read`.

Example `myCosAction.js` code of an action that only processes the bucket object:
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
2. Bind your {{site.data.keyword.cos_full_notm}} credentials to your manually installed `cloud-object-storage` package.
  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}
3. The `object-read` action can be composed with `myCosProcessObjectAction` to create an action sequence.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

In addition to the `object-read` action, you can use other actions included in the installable `cloud-object-storage` package.

### Binding credentials to your action
{: #pkg_obstorage_ev_bind}

You can avoid passing sensitive credentials during invocation by binding the {{site.data.keyword.cos_full_notm}} credentials to the action with the following command:
```
ibmcloud fn service bind cloud-object-storage myCosAction
```
{: pre}

### Creating a rule to associate the action with the change trigger
{: #pkg_obstorage_ev_rule}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

You can use an action or action sequence in a [rule](/docs/openwhisk?topic=cloud-functions-rules) to fetch and process the object that is associated with a {{site.data.keyword.cos_full_notm}} change event.

Create a rule that activates `MyCosAction` action on new {{site.data.keyword.cos_full_notm}} trigger events.
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}

