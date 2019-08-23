---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-16"

keywords: object storage, bucket, package, functions

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
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Object Storage
{: #pkg_obstorage}

You can extend the functionality of your {{site.data.keyword.openwhisk}} app by integrating with an {{site.data.keyword.cos_full}} instance.

**Before you begin** 
To learn about {{site.data.keyword.cos_full_notm}}, see [About Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). For more information about setting up the {{site.data.keyword.cos_full_notm}} instance, see [Provision an instance {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Packages
{: #obstorage_packages}
| Package | Availability | Description |
| --- | --- | --- |
| [{{site.data.keyword.cos_full_notm}} package](#pkg_obstorage_install)| Installable | Read, write, and delete from an {{site.data.keyword.cos_full_notm}} instance. |
| [(Experimental) {{site.data.keyword.cos_full_notm}} events source](#pkg_obstorage_ev) | Pre-installed (US-South only) | Listen for changes to an {{site.data.keyword.cos_full_notm}} instance. |

## Creating an IBM Cloud Object Storage service instance
{: #pkg_obstorage_service}

Before you can use either package, you must request an instance of {{site.data.keyword.cos_full_notm}} and create at least one bucket.

1. [Create an {{site.data.keyword.cos_full_notm}} service instance ](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Create a set of HMAC service credentials ](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) for the {{site.data.keyword.cos_full_notm}} service instance. In the **Add Inline Configuration Parameters (Optional)** field, add `{"HMAC":true}`.

3. [Create at least one bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).





## Installing the {{site.data.keyword.cos_full_notm}} package from the {{site.data.keyword.openwhisk_short}} CLI
{: #pkg_obstorage_install}

After you have an {{site.data.keyword.cos_full_notm}} service instance, you can use either the {{site.data.keyword.openwhisk}} CLI or UI to install the {{site.data.keyword.cos_full_notm}} package into your namespace.
{: shortdesc}
{: #pkg_obstorage_cli}

The installable {{site.data.keyword.cos_full_notm}} package deploys a set of actions that you can use to work with your {{site.data.keyword.cos_full_notm}} instance. These are executed in either Node.js or Python. After you install the package, you can select a runtime. For a list of the actions in the `cloud-object-storage` package, see [Available entities](#pkg_obstorage_actions).
{: note}

**Before you begin**

[Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

To install the {{site.data.keyword.cos_full_notm}} package:

1. Clone the {{site.data.keyword.cos_full_notm}} package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to either the `runtimes/nodejs` or `runtimes/python` directory to select a runtime for the actions in the package.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. Deploy the `cloud-object-storage` package.
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **Response**
    ```
    Success: Deployment completed successfully.
    ```
    {: scree}

5. Verify that the `cloud-object-storage` package is added to your package list.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Output**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. Bind the credentials from the {{site.data.keyword.cos_full_notm}} instance you created to the package. You can include the `--keyname` flag to bind specific service credentials. For more information about binding services, see [Service commands](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service).

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **Example output**
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. Verify that the package is configured with your {{site.data.keyword.cos_full_notm}} service instance credentials.
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **Example output**
    ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## Binding parameters

You can use the [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind) command to bind the endpoint of a bucket to a specific action or to the `cloud-object-storage` package. Replace `<bucket_endpoint>` with the endpoint of your bucket.

When you update parameters for a package, action, or trigger you must specify all previously created parameters. Otherwise, the previously created paramters are removed. Any services that were bound to the package are also removed, so after you update other parameters you must [bind services](/docs/openwhisk?topic=cloud-functions-services) to your package again.
{: important}

**Bind parameters to all actions in a package**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**Bind parameters to a specific action**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

You can also bind parameters to actions by using the **Parameters** tab in the UI. To add parameters in the UI, navigate to the [**Actions** page](https://cloud.ibm.com/openwhisk/actions){: external} and click one of your actions. Then, click **Parameters** > **Add Parameter**. You must add parameters in `<key>` and `<value>` pairs.
{: tip}


## Installing the {{site.data.keyword.cos_full_notm}} package from the {{site.data.keyword.openwhisk_short}} UI
{: #pkg_obstorage_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page](https://cloud.ibm.com/openwhisk/create){: external}.

2. Select the namespace in which you want to install the {{site.data.keyword.cos_full_notm}} package by using the namespace drop-down menu

3. Click **Install Packages**.

4. Click the **IBM Cloud Object Storage** Package group, then click the **IBM Cloud Object Storage** Package.

5. In the **Available Runtimes** section, select either `Node.JS` or `Python` from the drop-down list. Then, click **Install**.

6. Once the package is installed you are redirected to the **Actions** page and can search for your new package, which is named `cloud-object-storage`.

7. To use the actions in the `cloud-object-storage` package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow step 5 in the [CLI instructions](#pkg_obstorage_cli).
  * To bind service credentials to individual actions, complete the following steps in the UI. 
  
If you bind service your service credentials to individual actions, you must complete the following steps for each action that you want to use.
{: note}

1. Click an action from the `cloud-object-storage` package that you want to use. The details page for that action opens.
2. In the left-hand navigation, click **Parameters**.
3. Enter a new parameter. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.


For more information about binding parameters, see [Binding parameters to packages](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params).


## Available entities
{: #pkg_obstorage_actions}

The {{site.data.keyword.cos_full_notm}} package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Package | `apikey`, `resource_instance_id`, `cos_hmac_keys.access_key_id`, `cos_hmac_keys.secret_access_key` | Work with an {{site.data.keyword.cos_full_notm}} instance. |
| `/cloud-object-storage/object-write` | Action | `bucket`, `key`, `body`, `endpoint`, `ibmAuthEndpoint` | Write an object to a bucket. |
| `/cloud-object-storage/object-read` | Action | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Read an object from a bucket. |
| `/cloud-object-storage/object-delete` | Action | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Delete an object from a bucket. |
| `/cloud-object-storage/bucket-cors-put` | Action | `bucket`, `corsConfig`, `endpoint`, `ibmAuthEndpoint` | Assign a CORS configuration to a bucket. |
| `/cloud-object-storage/bucket-cors-get` | Action | `bucket`, `endpoint`, `ibmAuthEndpoint` | Read the CORS configuration of a bucket. |
| `/cloud-object-storage/bucket-cors-delete` | Action | `bucket`, `endpoint`, `ibmAuthEndpoint` | Delete the CORS configuration of a bucket. |
| `/cloud-object-storage/client-get-signed-url` | Action | `bucket`, `key`, `operation`, `expires`, `endpoint`, `ibmAuthEndpoint` | Obtain a signed URL to restrict the Write, Read, and Delete of an object from a bucket. |

To get a full list of the available entities, run `ibmcloud fn package get cloud-object-storage`.
{: note}

### Package parameters
{: #pkg_obstorage_pkgparams}

The following package parameters are expected to be bound to the package, and are automatically available for all actions. It is also possible to specify these parameters when you invoke one of the actions.

| Package parameter | Description |
| --- | --- |
| `apikey` | The `apikey ` parameter is IAM API key for the {{site.data.keyword.cos_full_notm}} instance. |
| `resource_instance_id` | The `resource_instance_id` parameter is the {{site.data.keyword.cos_full_notm}} instance identifier. |
| `cos_hmac_keys` | The `cos_hmac_keys` parameter is the {{site.data.keyword.cos_full_notm}} instance HMAC credentials, which include the `access_key_id` and `secret_access_key` values.  These credentials are used exclusively by the `client-get-signed-url` action.  Refer to [Using HMAC Credentials](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) for instructions on how to generate HMAC credentials for your {{site.data.keyword.cos_full_notm}} instance. |
 
### Action parameters
{: #pkg_obstorage_actparams}

The following action parameters are specified when you invoke the individual actions.  Not all of these parameters are supported by every action. Refer to the [Available entities](#pkg_obstorage_actions) table to see which parameters are supported by which action.

| Action parameter | Description |
| --- | --- |
| `bucket` | The `bucket` parameter is the name of the {{site.data.keyword.cos_full_notm}} bucket. |
| `endpoint` | The `endpoint` parameter is the {{site.data.keyword.cos_full_notm}} endpoint that is used to connect to your {{site.data.keyword.cos_full_notm}} instance. You can locate your endpoint in the [{{site.data.keyword.cos_full_notm}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `expires` | The `expires` parameter is the number of seconds to expire the pre-signed URL operation.  The default `expires` value is 15 minutes. |
| `ibmAuthEndpoint` | The `ibmAuthEndpoint ` parameter is the IBM Cloud authorization endpoint that is used by {site.data.keyword.cos_short}} to generate a token from the `apikey`. The default authorization endpoint works for all IBM Cloud Regions. |
| `key` | The `key` parameter is the bucket object key. |
| `operation` | The `operation` parameter is the pre-signed URL's operation to call. |
| `corsConfig` | The `corsConfig` parameter is a bucket's CORS configuration. |


## Writing objects to a bucket
{: #pkg_obstorage_write}

You can use the `object-write` action to write an object to an {{site.data.keyword.cos_full_notm}} bucket.
{: shortdesc}

In the following steps, the name `test-bucket` is used as an example. Buckets in {{site.data.keyword.cos_full_notm}} must be globally unique, so you must replace `test-bucket` with a unique bucket name.
{: note}

### Writing an object to a bucket by using the CLI
{: #pkg_obstorage_write_cli}
Write an object to your bucket by using the `object-write` action.
{: shortdesc}


Invoke the `object-write` action to write an object to your bucket. If you bound your bucket endpoint to your package or to the `object-write` action you do not need to include the endpoint as a parameter. Replace `<org_space>` with name of your Cloud Foundry Org and Space, `<test-bucket>` with the name of your bucket, and `<test.txt>` with the name of the object you want to write.

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**Example output**

```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### Writing an object to a bucket by using the UI
{: #pkg_obstorage_write_ui}


1. Go to the [Actions page](https://cloud.ibm.com/openwhisk/actions){: external} in the {{site.data.keyword.openwhisk_short}} console.

2. Under the `cloud-object-storage` package, click the **object-write** action.

3. In the Code pane, click **Change Input**.

4. Enter a JSON object that contains your bucket, key, and body as object keys.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
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
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Reading objects from a bucket
{: #pkg_obstorage_read}

You can use the `object-read` action to read from an object in an {{site.data.keyword.cos_full_notm}} bucket.
{: shortdesc}

In the following steps, the name `test-bucket` is used as an example. Buckets in {{site.data.keyword.cos_full_notm}} must be globally unique, so you must replace `test-bucket` with a unique bucket name.
{: note}

### Reading an object from a bucket by using the CLI
{: #pkg_obstorage_read_cli}

Read from an object in your bucket by using the `object-read` action.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**Example output**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### Reading an object from a bucket by using the UI
{: #pkg_obstorage_read_ui}

1. Go to the [Actions page](https://cloud.ibm.com/openwhisk/actions){: external}.

2. Under the `cloud-object-storage` package, click the `object-read` action.

3. In the Code pane, click **Change Input**.

4. Enter a JSON object that contains your bucket and key as object keys.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
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
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Listening for changes to a bucket by using the (Experimental) Object Storage events source
{: #pkg_obstorage_ev}

The `/whisk.system/cos-experimental` package might be unstable, change frequently in ways that aren't compatible with earlier versions, and might be discontinued with a short notice. This package isn't recommended for use in production environments. This experimental package is available only in the US-South region.
{: important}

You can use {{site.data.keyword.openwhisk}} to listen for changes to an [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) bucket and use an action to process one or more objects from the bucket.

<br>

**Sample use case:** With the `/whisk.system/cos-experimental` package, you can listen for changes to GPS street data stored in an {{site.data.keyword.cos_full_notm}} bucket. Then, when changes occur, you can trigger the automatic regeneration of a GPS map so that users can have access to the latest street data for their GPS application.

### (Experimental) Object Storage events source parameters
{: #pkg_obstorage_ev_ch}

With the `/whisk.system/cos-experimental` package, you can configure events from an {{site.data.keyword.cos_full_notm}} instance, and includes the following feed:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Package | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Package containing the `changes` feed action. |
| `/whisk.system/cos-experimental/changes` | Feed | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Fire trigger events on changes to an {{site.data.keyword.cos_full_notm}} bucket. |
{: shortdesc}

You can use the `changes` feed to configure the {{site.data.keyword.cos_full_notm}} events source service to fire a trigger on every change to a bucket in your {{site.data.keyword.cos_full_notm}} instance.

Parameters that are used in this example:

| Parameter | Description |
| --- | --- |
| `apikey` | (Required, unless bound to the package). The `apikey` parameter is IAM API key for the {{site.data.keyword.cos_full_notm}} instance.  Normally, this value is bound to the package. However, if the `apikey` value is specified when using the `changes` feed action, the specified value is used for the credentials instead of the bound credentials' apikey. |
| ` auth_endpoint` | (Optional). The `auth_endpoint` parameter is the authorization endpoint that is used by {{site.data.keyword.cos_full_notm}} to generate a token from the `apikey`.  The default endpoint is the {{site.data.keyword.cloud}} endpoint. |
| `bucket` | (Required). The `bucket` parameter is the name of the {{site.data.keyword.cos_full_notm}} bucket. |
| `endpoint` | (Required). The `endpoint` parameter is the {{site.data.keyword.cos_full_notm}} endpoint used to connect to your {{site.data.keyword.cos_full_notm}} instance. You can locate your endpoint in the [{{site.data.keyword.cos_full_notm}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `interval` | (Optional). The `interval` parameter is the bucket polling interval, in whole minutes. The `interval` value must be at least 1 minute and is set to 1 minute by default. |

## Creating a trigger to respond to the changes feed
{: #pkg_obstorage_ev_trig}

When creating the trigger, you can avoid passing your {{site.data.keyword.cos_full_notm}} credentials to the `changes` feed action by binding your credentials directly to the `cos-experimental` package.
{: shortdesc}

 1. First, create a package binding that can be modified to contain your credentials. The following creates a package binding, `myCosPkg`, in your namespace.

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. Bind your {{site.data.keyword.cos_full_notm}} credentials to the package. Binding your {{site.data.keyword.cos_full_notm}} credentials to the package binds the `apikey` value to the package so you don't need to specify the `apikey` value when the `changes` feed action is invoked.

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. Create a trigger named `cosTrigger` with the `changes` feed in the package binding that you created. Use your bucket name and {{site.data.keyword.cos_full_notm}} endpoint parameter values.

  ```
  ibmcloud fn trigger create cosTrigger --feed myCosPkg/changes \
  --param bucket myBucket \
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **Example output**

    ```
    ok: created trigger feed cosTrigger
    ```
    {: pre}
 

Create an action to verify that the trigger, the change feed, and the rule are all configured and working correctly.
 
1. Save the following JavaScript code as `cosChange.js`. 

  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}

2. Create an action called `cosChange` by using the `cosChange.js` code.

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

3. Create a rule to connect the `cosChange` action to the `cosTrigger` trigger.

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

4. In a separate terminal window, start polling for activations to give clear visibility of what is happening. When the trigger fires and the action is run, this command lists the activation records for each of these operations as they occur.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
5. In your {{site.data.keyword.cos_full_notm}} dashboard, either modify an existing bucket object or create one. To learn how to add an object to your bucket, see [Add some objects to your bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).
  
6. For each bucket object change, observe new activations for the `cosTrigger` trigger and `cosChange` action.
  
7. If you are unable to observe new activations, verify that the `apikey`, `endpoint`, and `bucket` parameter values are correct.
  ```
  ibmcloud fn trigger get cosTrigger
  ```
  {: pre}


### Data structure of an Object Storage trigger activation
{: #pkg_obstorage_ev_data}

The content of the generated events has the following parameters:

| Parameter | Description |
| --- | --- |
| `file` | The file or object metadata. This structure is described in [List objects in a specific bucket](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets). |
| `status` | The detected change.  This value is either `added`, `modified` or `deleted`. |
| `bucket`| The name of the {{site.data.keyword.cos_full_notm}} bucket. |
| `endpoint` | The {{site.data.keyword.cos_full_notm}} endpoint used to connect to the {{site.data.keyword.cos_full_notm}} instance. |
| `key` | The identifier of the changed bucket object. This value is the same as `file.Key`, but available at the top of the trigger event JSON. |

**Example JSON representation of the bucket change trigger activation**

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

## Creating an action to process a changed object
{: #pkg_obstorage_ev_act}

You can create a single action that retrieves and processes an object. Or, you can create a sequence that uses one action to retrieve the object and another action to process the object.

### Creating an action to retrieve and process the object
{: #pkg_obstorage_ev_act_ret}

This sample action code retrieves and processes the bucket change notification document. You can pass the `apikey` and `serviceInstanceId` parameters directly to the action during manual action invocation, but when this action is invoked by a trigger these values must be obtained from your {{site.data.keyword.cos_full_notm}}, which must be bound to the action with the `ibmcloud fn service bind` command.

1. Save the following code into a .zip file called `myCosAction.zip`.

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

  Because this action uses the `ibm-cos-sdk` NPM package, the action must be packaged as either a [Node.js module](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) or a [single bundle](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

2. Create the action to retrieve and process the object from {{site.data.keyword.cos_full_notm}}:

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### Creating an action sequence to retrieve and process the object
{: #pkg_obstorage_ev_act_seq}

Instead of including the object retrieval code in your action, you can use the `object-read` action from the `cloud-object-storage` package, which must be [manually installed](#pkg_obstorage_install).  Your action code only needs to process the results returned from `object-read`.
{: shortdesc}

To create an action that only processes the bucket object:

1. Save the following code as `myCosAction.js`.

  ```javascript
  function main(data) {
    if (data) {
      // Process the object
    }
  }
  ```
  {: codeblock}

2. Create the action to process only the object from {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. Bind your {{site.data.keyword.cos_full_notm}} credentials to your manually installed `cloud-object-storage` package.

  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}

4. The `object-read` action can be composed with `myCosProcessObjectAction` to create an action sequence.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

In addition to the `object-read` action, you can use other actions that are included in the installable `cloud-object-storage` package.
{: tip}

To get a list of the available entities, run the following command.
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### Binding credentials to your action
{: #pkg_obstorage_ev_bind}

You can avoid passing sensitive credentials during invocation by binding your {{site.data.keyword.cos_full_notm}} credentials to the action by using the following command:
  ```
  ibmcloud fn service bind cloud-object-storage <action_name>
  ```
  {: pre}

### Creating a rule to associate the action with the change trigger
{: #pkg_obstorage_ev_rule}

You can use an action or action sequence in a [rule](/docs/openwhisk?topic=cloud-functions-rules) to fetch and process the object that is associated with an {{site.data.keyword.cos_full_notm}} change event.

Create a rule that activates `myCosAction` action on new {{site.data.keyword.cos_full_notm}} trigger events.
  ```
  ibmcloud fn rule create myRule cosTrigger myCosAction
  ```
  {: pre}



