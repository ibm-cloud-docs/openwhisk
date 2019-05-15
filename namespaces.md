---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: namespaces, iam, cloud foundry, classic namespaces

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



# Managing namespaces
{: #namespaces}

With {{site.data.keyword.openwhisk}}, you can create Identity and Access (IAM) managed namespaces to group entities, such as actions or triggers, together. Then you can create IAM access policies for the namespace.
{: shortdesc}


**What is a namespace?**

Namespaces contain {{site.data.keyword.openwhisk_short}} entities, such as actions and triggers, and belong to a resource group. You can let users access your entities by granting them access to the namespace.

The fully qualified name of an entity is `/namespaceName/[packageName]/entityName`.


**What happens when I create a namespace?**

Namespaces that are created within {{site.data.keyword.openwhisk_short}}, are identified as an IAM service instance.
During the creation of a namespace, you can specify the [resource group](/docs/resources?topic=resources-rgs) in which to add the service instance.

When you create your namespace, the following artifacts are created at the same time:

* A service ID that can be used as a functional id when you make outbound calls. All of the actions that are created in this namespace can use this service ID for access to other resources. To see all of your service IDs, run `ibmcloud iam service-ids`.

* An API key for the service ID which can be used to generate IAM tokens. You can then use the tokens to authenticate the namespace with other {{site.data.keyword.Bluemix_notm}} services. The API key is provided to the actions as environment variable.

    Do not delete API keys.
    {: tip}

**Are there any limitations for namespaces?**

[Creating APIs with API Gateway](/docs/openwhisk?topic=cloud-functions-apigateway) and using the [mobile SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) are not supported for IAM-managed namespaces.

{{site.data.keyword.openwhisk_short}} has restrictions on namespace names. For more information, refer to the [System details and Limits](/docs/openwhisk?topic=cloud-functions-limits#openwhisk_entities) documentation.
{: tip}



**What do I do if I have a Cloud Foundry-based namespace?**

Your Cloud Foundry-based namespaces will continue to work. However, in order to take advantage of new features, you must [migrate your namespaces to IAM](/docs/resources?topic=resources-migrate).

</br>


## Creating a namespace with the CLI
{: #namespaces_create}

You can create an IAM managed namespace as part of a resource group and manage access policies for your resources by targeting the resource group when a namespace is created. If you have other users that require access to your namespace, or if you want to access other resources from your namespace's actions, be sure that you set IAM policies after your namespace is created.
{: shortdesc}

1. Target the resource group where you want to create the namespace. If you haven't created a [resource group](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) yet, you can target the `default` group.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Create an IAM-enabled namespace.

  ```
  ibmcloud fn namespace create <namespace_name> [-n <description>]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>The display name for the IAM-based namespace.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Optional: Add a description to the namespace, such as which kind of actions or packages it will contain.</td>
      </tr>
    </tbody>
  </table>

  Example output:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Verify that your new namespace is created.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Example output:

  ```
  Details of namespace: 'myNamespace'
  Description: 'short description'
  Resource Plan Id: 'functions-base-plan'
  Location: 'jp-tok'
  ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  You can also list all namespaces, including IAM-based and Cloud Foundry-based namespaces:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Before creating entities in the namespace, set your CLI context to the namespace by targeting it.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

</br>

## Creating a namespace with the API
{: #namespaces_create_api}

You can create an IAM managed namespace as part of a resource group and manage access policies for your resources by targeting the resource group when a namespace is created. If you have other users that require access to your namespace, or if you want to access other resources from your namespace's actions, be sure that you set IAM policies after your namespace is created.
{: shortdesc}


1. Target the resource group where you want to create the namespace. If you haven't created a [resource group](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) yet, you can target the `default` group.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Create an IAM-enabled namespace.

  ```
  curl --request POST \
  --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
  --header 'accept: application/json' \
  --header 'authorization: <IAM_token>' \
  --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding this command's components</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>Your {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) token. To retrieve your IAM token, run <code>ibmcloud iam oauth-tokens</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>The name of the namespace.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>The ID of the resource group that you want to create the namespace in. To see resource group IDs, run <code>ibmcloud resource groups</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>The ID of the resource plan, such as functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Optional: Add a description to the namespace, such as which kind of actions or packages it will contain.</td>
      </tr>
    </tbody>
  </table>

  Example output:

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
    "id": "12345678-1234-abcd-1234-123456789abc",
    "location": "jp-tok",
    "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
    "name": "mynamespace",
    "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
    "resource_plan_id": "functions-base-plan"
  }
  ```
  {: screen}

3. Verify that your new namespace is created.

  ```
  curl --request GET \
    --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  You can also list all namespaces, including IAM-based and Cloud Foundry-based namespaces:
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Example output:
  ```
  {
    "limit": 10,
    "offset": 0,
    "total_Count": 2,
    "namespaces": [
      {
        "id": "12345678-1234-abcd-1234-123456789abc",
        "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
        "classic_type": 1,
        "location": "jp-tok"
      }
    ]
  }
  ```
  {: screen}


For more information about working with HTTP REST, check out the [{{site.data.keyword.openwhisk_short}} API docs](/apidocs/functions).
{: tip}



## Next steps
{: #namespaces_next}

Now that you've created a namespace, you can create IAM access policies to help protect it. To get started, check out [Managing access](/docs/openwhisk?topic=cloud-functions-iam). For more information about how you can manage IAM-based namespaces, see the [{{site.data.keyword.openwhisk_short}} REST API reference](/apidocs/functions/functions).


