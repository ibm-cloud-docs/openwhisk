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

# Object Storage
{: #cloud_object_storage_actions}

The {{site.data.keyword.cos_full_notm}} package provides a set of actions for interacting with {{site.data.keyword.cos_full}} instances. These actions allow you to read, write, and delete from the buckets that are present on a {{site.data.keyword.cos_short}} instance.
{: shortdesc}

The {{site.data.keyword.cos_short}} package includes the following actions:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/cloud-object-storage` | package | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Work with an {{site.data.keyword.cos_full_notm}} instance. |
| `/cloud-object-storage/object-write` | action | bucket, key, body | Write an object to a bucket. |
| `/cloud-object-storage/object-read` | action | bucket, key | Read an object from a bucket. |
| `/cloud-object-storage/object-delete` | action | bucket, key | Delete an object from a bucket. |
| `/cloud-object-storage/bucket-cors-put` | action | bucket, corsConfig | Assign a CORS configuration to a bucket. |
| `/cloud-object-storage/bucket-cors-get` | action | bucket | Read the CORS configuration of a bucket. |
| `/cloud-object-storage/bucket-cors-delete` | action | bucket | Delete the CORS configuration of a bucket. |
| `/cloud-object-storage/client-get-signed-url` | action | bucket, key, operation | Obtain a signed URL to restrict the Write, Read, and Delete of an object from a bucket. |

## Creating an {{site.data.keyword.cos_full_notm}} service instance
{: #cloud_object_storage_service_instance}

Before you install the package, you must request an instance of {{site.data.keyword.cos_short}} and create at least one bucket.

1. [Create an {{site.data.keyword.cos_full_notm}} service instance ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance).

2. [Create a set of HMAC service credentials ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials) for the {{site.data.keyword.cos_short}} service instance. In the **Add Inline Configuration Parameters (Optional)** field, add `{"HMAC":true}`.

3. [Create at least one bucket ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-object-storage/getting-started.html#create-buckets).

## Installing the {{site.data.keyword.cos_short}} package
{: #cloud_object_storage_installation}

After you have an {{site.data.keyword.cos_short}} service instance, you can use either the {{site.data.keyword.openwhisk}} CLI or UI to install the {{site.data.keyword.cos_short}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #cloud_object_storage_cli}

Before you begin:
  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Install the `wskdeploy` command. See the [Apache OpenWhisk documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project).

To install the {{site.data.keyword.cos_short}} package:

1. Clone the {{site.data.keyword.cos_short}} package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to either the `runtimes/nodejs` or `runtimes/python` directory. Actions in the {{site.data.keyword.cos_short}} package are deployed in the runtime that you choose.
    ```
    cd <filepath>/package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package. If you later decide to run the actions in this package in the other runtime, you can repeat the previous step and this step to redeploy the package.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. Verify that the `cloud-object-storage` package is added to your package list.
    ```
    ibmcloud wsk package list
    ```
    {: pre}

    Output:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. Bind the credentials from the {{site.data.keyword.cos_short}} instance you created to the package.
    ```
    ibmcloud wsk service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    Example output:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

3. Verify that the package is configured with your {{site.data.keyword.cos_short}} service instance `apikey`, `resource_instance_id`, and other credentials.
    ```
    ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg, displaying field parameters
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

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/openwhisk/create).

2. Using the **Cloud Foundry Org** and **Cloud Foundry Space** lists, select the namespace that you want to install the {{site.data.keyword.cos_short}} package into. Namespaces are formed from the combined org and space names.

3. Click **Quickstart templates**.

4. Click the **Upload Image** template.

5. Enter a name for your package or use the default name `upload-image`.

5. Click **Next**.

6. In the **{{site.data.keyword.cos_short}}** list, select the name of the {{site.data.keyword.cos_short}} service instance that you created earlier.

7. Click **Deploy**.

## Writing to an {{site.data.keyword.cos_short}} bucket
{: #cloud_object_storage_write}

You can use the `object-write` action to write an object to an {{site.data.keyword.cos_short}} bucket.
{: shortdesc}

**Note**: In the following steps, the name `testbucket` is used as an example. Buckets in {{site.data.keyword.cos_full_notm}} must be globally unique, so you must replace `testbucket` with a unique bucket name.

### Write to a bucket from the CLI
{: #write_bucket_cli}

Write an object to your bucket by using the `object-write` action.
```
ibmcloud wsk action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
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

1. Go to the [Actions page in the {{site.data.keyword.openwhisk_short}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/openwhisk/actions).

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

**Note**: In the following steps, the name `testbucket` is used as an example. Buckets in {{site.data.keyword.cos_full_notm}} must be globally unique, so you must replace `testbucket` with a unique bucket name.

### Read from a bucket from the CLI
{: #read_bucket_cli}

Read from an object in your bucket by using the `object-read` action.
```
ibmcloud wsk action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
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

1. Go to the [Actions page in the {{site.data.keyword.openwhisk_short}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/openwhisk/actions).

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
