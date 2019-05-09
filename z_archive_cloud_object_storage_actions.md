---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: object storage, bucket, package

subcollection: cloud-functions

---




{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloud Object Storage package
{: #cloud_object_storage_actions}

The {{site.data.keyword.cos_full}} package provides a set of actions for interacting with {{site.data.keyword.cos_full_notm}} instances. These actions allow you to read, write, and delete from the buckets that are present on an {{site.data.keyword.cos_short}} instance.
{: shortdesc}

The {{site.data.keyword.cos_short}} package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | package | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Work with an {{site.data.keyword.cos_short}} instance. |
| `/cloud-object-storage/object-write` | action | bucket, key, body, endpoint, ibmAuthEndpoint | Write an object to a bucket. |
| `/cloud-object-storage/object-read` | action | bucket, key, endpoint, ibmAuthEndpoint | Read an object from a bucket. |
| `/cloud-object-storage/object-delete` | action | bucket, key, endpoint, ibmAuthEndpoint | Delete an object from a bucket. |
| `/cloud-object-storage/bucket-cors-put` | action | bucket, corsConfig, endpoint, ibmAuthEndpoint | Assign a CORS configuration to a bucket. |
| `/cloud-object-storage/bucket-cors-get` | action | bucket, endpoint, ibmAuthEndpoint | Read the CORS configuration of a bucket. |
| `/cloud-object-storage/bucket-cors-delete` | action | bucket, endpoint, ibmAuthEndpoint | Delete the CORS configuration of a bucket. |
| `/cloud-object-storage/client-get-signed-url` | action | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Obtain a signed URL to restrict the Write, Read, and Delete of an object from a bucket. |

## Package and action parameters

### Package parameters

The following parameters are expected to be bound to the package; this will make them automatically available for all actions. It is also possible to specify these parameters when invoking one of the actions.

**apikey**: The `apikey ` parameter is IAM API key for the {{site.data.keyword.cos_short}} instance.

**resource_instance_id**: The `resource_instance_id` parameter is the {{site.data.keyword.cos_short}} instance indentifier.

**cos_hmac_keys**: The `cos_hmac_keys` parameter is the {{site.data.keyword.cos_short}} instance HMAC credentials, which includes the `access_key_id` and `secret_access_key` values.  These credentials are used exclusively by the `client-get-signed-url` action.  Refer to [Using HMAC Credentials](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) for instructions on how to generate HMAC credentials for your {{site.data.keyword.cos_short}} instance.

### Action parameters

The following parameters are specified when invoking the individual actions.  Not all of these parameters are suppoted by every action; refer to the above table to see which parameters are supporte by which action.

**bucket**: The `bucket` parameter is the name of the {{site.data.keyword.cloud_object_storage_short}} bucket.

**endpoint**: The `endpoint` parameter is the {{site.data.keyword.cos_short}} endpoint used to connect to your {{site.data.keyword.cos_short}} instance. You can locate your endpoint in the [{{site.data.keyword.cos_short}} documentation](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-endpoints).

**expires**: The `expires` parameter is the number of seconds to expire the pre-signed URL operation.  The default `expires` value is 15 minutes.

**ibmAuthEndpoint**: The `ibmAuthEndpoint ` parameter is the IBM Cloud authorization endpoint used by {site.data.keyword.cos_short}} to generate a token from the `apikey`. The default authorization endpoint should work for all IBM Cloud Regions.

**key**: The `key` parameter is the bucket object key.

**operation**: The `operation` parameter is the pre-signed URL's operation to call.

**corsConfig**: The `corsConfig` parameter is a bucket's CORS configuration.


## Creating an IBM Cloud Object Storage service instance
{: #cloud_object_storage_service_instance}

Before you install the package, you must request an instance of {{site.data.keyword.cos_short}} and create at least one bucket.

1. [Create an {{site.data.keyword.cos_short}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-provision).

2. [Create a set of HMAC service credentials ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) for the {{site.data.keyword.cos_short}} service instance. In the **Add Inline Configuration Parameters (Optional)** field, add `{"HMAC":true}`.

3. [Create at least one bucket ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Installing the {{site.data.keyword.cos_short}} package
{: #cloud_object_storage_installation}

After you have an {{site.data.keyword.cos_short}} service instance, you can use either the {{site.data.keyword.openwhisk}} CLI or UI to install the {{site.data.keyword.cos_short}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #cloud_object_storage_cli}

Before you begin:

[Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

To install the {{site.data.keyword.cos_short}} package:

1. Clone the {{site.data.keyword.cos_short}} package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to either the `runtimes/nodejs` or `runtimes/python` directory. Actions in the {{site.data.keyword.cos_short}} package are deployed in the runtime that you choose.
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

5. Bind the credentials from the {{site.data.keyword.cos_short}} instance you created to the package.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Verify that the package is configured with your {{site.data.keyword.cos_short}} service instance credentials.
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
{: #cloud_object_storage_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create).

2. By using the **Cloud Foundry Org** and **Cloud Foundry Space** lists, select the namespace in which you want to install the {{site.data.keyword.cos_short}} package. Namespaces are formed from the combined `org` and `space` names.

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

## Writing to an {{site.data.keyword.cos_short}} bucket
{: #cloud_object_storage_write}

You can use the `object-write` action to write an object to an {{site.data.keyword.cos_short}} bucket.
{: shortdesc}

**Note**: In the following steps, the name `testbucket` is used as an example. Buckets in {{site.data.keyword.cos_short}} must be globally unique, so you must replace `testbucket` with a unique bucket name.

### Write to a bucket from the CLI
{: #write_bucket_cli}

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
{: #write_bucket_ui}

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

## Reading from an {{site.data.keyword.cos_short}} bucket
{: #cloud_object_storage_read}

You can use the `object-read` action to read from an object in an {{site.data.keyword.cos_short}} bucket.
{: shortdesc}

**Note**: In the following steps, the name `testbucket` is used as an example. Buckets in {{site.data.keyword.cos_short}} must be globally unique, so you must replace `testbucket` with a unique bucket name.

### Read from a bucket from the CLI
{: #read_bucket_cli}

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

### Read from a bucket from the UI
{: #read_bucket_ui}

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

