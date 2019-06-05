---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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

With {{site.data.keyword.openwhisk}}, you can create Identity and Access (IAM) managed namespaces to group entities, such as actions or triggers, together. Then you can create IAM access policies for the namespace. For an overview of IAM, see the [{{site.data.keyword.openwhisk_short}} IAM-enablement announcement blog](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled).
{: shortdesc}

## What is a namespace?

Namespaces contain {{site.data.keyword.openwhisk_short}} entities, such as actions and triggers, and belong to a resource group. You can let users access your entities by granting them access to the namespace.

The fully qualified name of an entity is `/namespaceName/[packageName]/entityName`.

### What happens when I create a namespace?

Namespaces that are created within {{site.data.keyword.openwhisk_short}}, are identified as an IAM service instance.
During the creation of a namespace, you can specify the [resource group](/docs/resources?topic=resources-rgs) in which to add the service instance.

When you create a namespace, the following components are created:

| Component | Description |
| --- | --- | 
| A service ID | You can use the service ID as a functional ID when you make outbound calls. All of the actions that are created in this namespace can use this service ID for access to other resources. The functional user gets the Reader role by default. Reader access means it can read namespace entities and invoke actions. The Reader role is used by triggers to invoke actions. To control inbound traffic, you might want to grant access to other users such as assigning Reader role to invoke actions. |
| An API key | An API Key for the service ID that can be used to generate IAM tokens. You can use the tokens to authenticate the namespace with other {{site.data.keyword.cloud_notm}} services. The API key is provided to actions as the environment variable `__OW_IAM_NAMESPACE_API_KEY`. |

View all of your service IDs.
```
ibmcloud iam service-ids
```
{: pre}

View the API keys that are associated with a service ID. 
```
ibmcloud fn iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

Do not delete API keys.
{: tip}

### Are there any limitations for namespaces?

[Creating APIs with API Gateway](/docs/openwhisk?topic=cloud-functions-apigateway) and using the [mobile SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) are not supported for IAM-managed namespaces. 

The names of all entities, including actions, triggers, rules, packages, and namespaces, are a sequence of characters that follow the following format:
* The first character must be an alphanumeric character, or an underscore.
* The subsequent characters can be alphanumeric, spaces, or any of the following values: `_`, `@`, `.`, `-`.
* The last character can't be a space.

### What do I do if I have a Cloud Foundry-based namespace?

Your Cloud Foundry-based namespaces will continue to work. However, in order to take advantage of new features, you must create an IAM-enabled namespace.


## Creating an IAM-based namespace in the UI
{: #create_iam_ui}

1. In the [{{site.data.keyword.openwhisk_short}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk), click the namespace drop-down menu.

2. At the bottom of the namespace list, click **Create namespace**.

3. Enter a display name for the namespace and a short description, such as the kinds of actions or packages that you plan to create in this namespace.

4. Choose the resource group where you want to create the namespace and a location to deploy the namespace resource into.

5. Click **Create**.

6. To view the service instance for the namespace resource, go to your [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/dashboard/apps) and find your namespace name in the **Services** pane.

If you need to, you can update the name or description of the namespace on the **Namespace settings** page in the {{site.data.keyword.openwhisk_short}} console.

## Creating a namespace with the CLI
{: #namespaces_create}

You can create an IAM managed namespace as part of a resource group and manage access policies for your resources by targeting the resource group when a namespace is created. If you have other users that require access to your namespace, or if you want to access other resources from your namespace's actions, be sure that you set IAM policies after your namespace is created.
{: shortdesc}

1. Target the resource group where you want to create the namespace. If you haven't created a [resource group](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) yet, you can target the `default` group.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Create an IAM-enabled namespace. Optional: Include a description for your namespace by using the `-n` or `--description` flag. If your description is longer than one word it must be in quotations.

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
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
        <td>Optional: Add a description to the namespace, such as which kind of actions or packages it will contain. If your description is longer than one word it must be in quotations.</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>Optional: Add a description to the namespace, such as which kind of actions or packages it will contain. If your description is longer than one word it must be in quotations.</td>
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
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
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
        <td>Your {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token. To retrieve your IAM token, run <code>ibmcloud iam oauth-tokens</code>.</td>
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

### Accessing other resources from a namespace
{: #namespace-access}

Actions typically call other {{site.data.keyword.cloud_notm}} resources and services which require appropriate authentication. If these services are IAM enabled and accept IAM tokens, you can leverage the namespace's functional ID for outbound communication.
{: shortdesc}

As described in [Managing IAM access](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), for each namespace, a service ID is created that represents the namespace. You can grant access to other services and resources for this service ID by assigning the appropriate roles using IAM policy management. For more information about creating service IDs to access other IAM-enabled services, see [Creating and working with service IDs](/docs/iam?topic=iam-serviceids#serviceids).

At runtime, {{site.data.keyword.openwhisk_short}} passes an API key of the namespace service ID to the action code as the environment variable `__OW_IAM_NAMESPACE_API_KEY`. The action code can use this API key to generate an IAM token. Most of the supported {{site.data.keyword.openwhisk_short}} SDKs such as Cloudant, {{site.data.keyword.watson}}, and {{site.data.keyword.cos_full_notm}} authenticate with the IAM API key itself. For other IAM managed services or resources that use a REST API, you can authenticate with the token that is derived from the IAM API key. For more information, see [Create an IAM access token for a user or service ID](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Not quite sure how API keys and tokens fit together? Learn more in [the IAM docs](/docs/iam?topic=iam-iamapikeysforservices).

## Next steps
{: #namespaces_next}

Now that you've created a namespace, you can create IAM access policies to help protect it. To get started, check out [Managing access](/docs/openwhisk?topic=cloud-functions-iam). 

For more information about how to manage IAM-based namespaces, see the [{{site.data.keyword.openwhisk_short}} REST API reference](/apidocs/functions).




