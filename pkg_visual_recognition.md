---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: visual recognition, watson, functions, cognitive,

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


# {{site.data.keyword.visualrecognitionshort}}
{: #pkg_visual_recognition}

The installable {{site.data.keyword.visualrecognitionfull}} service uses deep learning algorithms to identify scenes, objects, and faces  in images you upload to the service. You can create and train a custom classifier to identify subjects that suit your needs.
{:shortdesc}

The {{site.data.keyword.visualrecognitionshort}} package contains the following entities. For more information, see the {{site.data.keyword.visualrecognitionshort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html){: external} | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Work with the {{site.data.keyword.visualrecognitionshort}} service. |
| [`classify`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `accept_language`, `url`, `threshold`, `owners`, `classifier_ids`, `images_file_content_type`  | Classify images. |
| [`detect-faces`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `url`, `images_file_content_type`  | Detect faces in images. |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `name`, `classname_positive_examples`, `negative_examples` | Create a classifier. |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`  | Delete a classifier. |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`  | Retrieve classifier details. |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `verbose`  | Retrieve a list of classifiers. |
| [`update-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`, `classname_positive_examples`, `negative_examples`  | Update a classifier. |
| [`get-core-ml-model`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `classifier_id`  | Retrieve a Core ML model of a classifier. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data){: external} | Action |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `customer_id`  | Delete labeled data. |

## Creating a {{site.data.keyword.visualrecognitionshort}} service instance
{: #service_instance_recognition}

Before you install the package, you must create a {{site.data.keyword.visualrecognitionshort}} service instance and service credentials.
{: shortdesc}

1. [Create a {{site.data.keyword.visualrecognitionshort}} service instance ](https://cloud.ibm.com/catalog/services/watson_vision_combined){: external}.
2. When the service instance is created, auto-generated service credentials are also created for you.

## Installing the {{site.data.keyword.visualrecognitionshort}} package
{: #install_recognition}

After you have a {{site.data.keyword.visualrecognitionshort}} service instance, use the {{site.data.keyword.openwhisk}} CLI to install the {{site.data.keyword.visualrecognitionshort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #visualrecognition_cli}

**Before you begin**
[Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).

To install the {{site.data.keyword.visualrecognitionshort}} package:, run the following commands.

1. Clone the {{site.data.keyword.visualrecognitionshort}} package repo.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
    ```
    {: pre}

3. Verify that the package is added to your package list.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Output**
    ```
    packages
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.visualrecognitionshort}} instance you created to the package.
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    **Example output**
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.visualrecognitionshort}} service instance credentials.
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    **Example output**
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} UI
{: #visualrecognition_ui}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page ](https://cloud.ibm.com/openwhisk/create){: external}.

2. By using the **Cloud Foundry Org** and **Cloud Foundry Space** lists, select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Visual Recognition** Package.

5. Click **Install**.

6. Once the package is installed you are redirected to the actions page and can search for your new package, which is named **visual-recognition-v3**.

7. To use the actions in the **visual-recognition-v3** Package, you must bind service credentials to the actions.
  * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#visualrecognition_cli).
  * To bind service credentials to individual actions, complete the following steps in the UI.
   
  You must complete the following steps for each action that you want to use.
  {: note}

    1. Click an action from the **visual-recognition-v3** Package that you want to use. The details page for that action opens.
    2. In the left-hand navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.visualrecognitionshort}} package
{: #usage_recognition}

To use the actions in this package, run commands in the following format:

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

All actions require a version parameter in the format YYYY-MM-DD. When the API is changed in a backwards-incompatible way, a new version date is released. See more details in the [API reference](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning){: external}.

This package's functions use the current version of Visual Recognition, `2018-03-19`. Try out the `list-classifiers` action.
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}


