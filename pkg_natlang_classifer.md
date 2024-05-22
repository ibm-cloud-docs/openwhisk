---

copyright:
  years: 2017, 2024
lastupdated: "2024-05-22"

keywords: machine learning, functions, national language classifier, watson, classifier

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.nlclassifiershort}}
{: #pkg_natlang_classifier}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

The {{site.data.keyword.nlclassifierfull}} uses machine learning algorithms to return the matching predefined classes for short text input. You create and train a classifier to connect predefined classes to example texts so that the service can apply those classes to new inputs. For more information about this service, see {{site.data.keyword.nlclassifiershort}}.
{: shortdesc}

The Watson packages are deprecated. Please use the [Watson SDKs](/docs/watson?topic=watson-using-sdks){: external} to perform Watson related functionality. For more information about which Watson SDK is included in the available runtimes, see the [Functions runtime](/docs/openwhisk?topic=openwhisk-runtimes) documentation.
{: deprecated}

The {{site.data.keyword.nlclassifiershort}} package contains the following entities. You can find additional details in the {{site.data.keyword.nlclassifiershort}} API reference by clicking the entity name.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `natural-language-classifier-v1` | Package | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Work with the {{site.data.keyword.nlclassifiershort}} service. |
| `classify` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `text`,  | Classify a phrase. |
| `classify-collection` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `collection` | Classify multiple phrases. |
| `create-classifier` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `metadata`, `training_data` | Create a classifier. |
| `delete-classifier` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id` | Delete a classifier. |
| `get-classifier` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id` | Get information about a classifier. |
| `list-classifiers` | Action | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | List classifiers. |
{: caption="Table 1. {{site.data.keyword.nlclassifiershort}} package entities" caption-side="bottom"}

## Creating a {{site.data.keyword.nlclassifiershort}} service instance
{: #service_instance_classifier}

Before you install the package, you must create a {{site.data.keyword.nlclassifiershort}} service instance and service credentials.
{: shortdesc}

1. Create a {{site.data.keyword.nlclassifiershort}} service instance.
2. When the service instance is created, service credentials are automatically created for you.

## Installing the {{site.data.keyword.nlclassifiershort}} package
{: #install_classifier}

After you have a {{site.data.keyword.nlclassifiershort}} service instance, install the {{site.data.keyword.nlclassifiershort}} package into your namespace.
{: shortdesc}

### Installing from the {{site.data.keyword.openwhisk_short}} CLI
{: #nlclassifier_cli}

Install the {{site.data.keyword.nlclassifiershort}} package from the CLI. Be sure to [install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install) first.
{: shortdesc}

To install the {{site.data.keyword.nlclassifiershort}} package, run the following command.

1. Clone the {{site.data.keyword.nlclassifiershort}} package repo.

    ```sh
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Deploy the package.

    ```sh
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
    ```
    {: pre}

3. Verify that the package is added to your package list.

    ```sh
    ibmcloud fn package list
    ```
    {: pre}

    **Example output**

    ```sh
    packages
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. Bind the credentials from the {{site.data.keyword.nlclassifiershort}} instance you created to the package.

    ```sh
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    Depending on the region where you created the service instance, the service instance might be named differently because it is an IAM service. If the command fails, use the following service name for the bind command.

    ```sh
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    **Example output**

    ```sh
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. Verify that the package is configured with your {{site.data.keyword.nlclassifiershort}} service instance credentials.

    ```sh
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    **Example output**

    ```sh
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
        {
        "key": "__bx_creds",
        "value": {
          "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
        }
    ]
    ```
    {: screen}

### Installing from the {{site.data.keyword.openwhisk_short}} console
{: #nlclassifier_ui}

You can install your package from the {{site.data.keyword.openwhisk_short}} console.
{: shortdesc}

1. In the {{site.data.keyword.openwhisk_short}} console, go to the [Create page](https://cloud.ibm.com/functions/create){: external}.

2. Select the namespace that you want to install the package into.

3. Click **Install Packages**.

4. Click the **Watson** Package group.

5. Click the **Natural Language Classifier** Package.

6. Click **Install**.

7. After the package is installed, you are redirected to the actions page and can search for your new package, which is named **natural-language-classifier-v1**.

8. To use the actions in the **natural-language-classifier-v1** Package, you must bind service credentials to the actions.
    * To bind service credentials to all actions in the package, follow steps 4 and 5 in the [CLI instructions](#nlclassifier_cli).
    * To bind service credentials to individual actions, complete the following steps in the console.

    You must complete the following steps for each action that you want to use.
    {: note}

    1. Click an action from the **natural-language-classifier-v1** Package that you want to use. The details page for that action opens.
    2. In the left navigation, click the **Parameters** section.
    3. Enter a new **parameter**. For the key, enter `__bx_creds`. For the value, paste in the service credentials JSON object from the service instance that you created earlier.

## Using the {{site.data.keyword.nlclassifiershort}} package
{: #usage_classifier}

To use the actions in this package, run commands in the following format.

```sh
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Try out the `list-classifiers` action.

```sh
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}


