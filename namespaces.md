---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-25"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Managing namespaces
{: #namespaces}

{{site.data.keyword.openwhisk}} is deprecated. As of 28 December 2023, you can't create new function instances, and access to free instances will be removed. Existing premium plan function instances are supported until October 2024. Any function instances that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

With {{site.data.keyword.openwhisk}}, you can create Identity and Access (IAM) managed namespaces to group entities, such as actions or triggers, together. Then, you can create IAM access policies for the namespace. For an overview of IAM, see the [{{site.data.keyword.openwhisk_short}} IAM-enablement announcement blog](https://www.ibm.com/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}.
{: shortdesc}

## What is a namespace?
{: #what_is_namespace}

Namespaces contain {{site.data.keyword.openwhisk_short}} entities, such as actions and triggers, and belong to a resource group. You can let users access your entities by granting them access to the namespace.

The fully qualified name of an entity is `/<namespace_ID>/<package_name>/<entity_name>`.

### What happens when I create a namespace?
{: #what_happens_create_namespace}

Namespaces that are created within {{site.data.keyword.openwhisk_short}} are identified as an IAM service instance.
When you create a namespace, you can specify the [resource group](/docs/account?topic=account-rgs) in which to add the service instance.

When you create a namespace, the following components are created:

| Component | Description |
| --- | --- | 
| Service ID | You can use the service ID as a functional ID when you make outbound calls. All the actions that are created in this namespace can use this service ID for access to other resources. The functional user gets the Reader role by default. Reader access means it can read namespace entities and invoke actions. The Reader role is used by triggers to invoke actions. To control inbound traffic, you might want to grant access to other users such as assigning Reader role to invoke actions. |
| API key | An API Key for the service ID that can be used to generate IAM tokens. You can use the tokens to authenticate the namespace with other {{site.data.keyword.cloud_notm}} services. The API key is provided to actions as the environment variable `__OW_IAM_NAMESPACE_API_KEY`. |
{: caption="Table 1. Components of a namespace" caption-side="bottom"}

You can view a list of your service IDs by running the following command.

```bash
ibmcloud iam service-ids
```
{: pre}

You can view the API keys that are associated with a service ID by running the following command.

```bash
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

Do not delete service IDs or API keys.
{: tip}

### Are there any limitations for namespaces?
{: #namespace_limitations}

The [mobile SDK](/docs/openwhisk?topic=openwhisk-pkg_mobile_sdk) is not supported for IAM-managed namespaces. 

The names of all entities, including actions, triggers, rules, packages, and namespaces, are a sequence of characters that follow the following format:

* The first character must be an alphanumeric character, or an underscore.
* The subsequent characters can be alphanumeric, spaces, or any of the following values: `_`, `@`, `.`, `-`.
* The last character can't be a space.

### What do I do if I have a Cloud Foundry-based namespace?
{: #cf_namespace}

Your Cloud Foundry-based namespaces still work. However, to take advantage of new features, you must create an IAM-enabled namespace.

You can target a Cloud Foundry-based namespace or IAM-enabled namespace by running `ibmcloud fn property set --namespace <namespace_name_or_ID>` or `ibmlcoud fn namespace target <namespace_name_or_ID>`.

As a result of the [Cloud Foundry service deprecation](/docs/cloud-foundry-public?topic=cloud-foundry-public-deprecation), Cloud Foundry-based namespaces in {{site.data.keyword.openwhisk_short}} are deprecated and will stop working on Oct 31st, 2023. If you are still using Cloud-Foundry-based namespaces, [migrate your namespace to an IAM namespace](/docs/openwhisk?topic=openwhisk-namespaces#create_iam_namespace) to ensure that your Cloud Function workloads continue to run.
{: deprecated}

### How do I see a list of my {{site.data.keyword.openwhisk_short}} namespaces?
{: #how_list_namespace}

You can see a list of your {{site.data.keyword.openwhisk_short}} namespaces by running the [**`namespace list`**](/docs/openwhisk?topic=openwhisk-functions-cli#cli_namespace_list) command. The command output includes all the namespaces in the currently selected region and also lists whether the namespace is a Cloud Foundry-based or an IAM-based namespace. Note that the `namespace list` command is scoped to the targeted region and resource group that you set. To view your current region and resource group, run `ibmcloud target`. To set your region and resource group to a different choice, run `ibmcloud target -r REGION -g RESOURCE_GROUP`, where `REGION` and `RESOURCE_GROUP` are the new targets.

```bash
ibmcloud fn namespace list
```
{: pre}

The following example shows sample output from the **`ibmcloud fn namespace list`** command.

```sh
name          type            id                                    description
dev           CF-based        dev                           
cfsdocs_prod  CF-based        prod                          
playground    IAM-based       <id_string>                           IAM playground.
```
{: screen}


You can see a list of your IAM-based namespaces in the [{{site.data.keyword.Bluemix_notm}} Console Resource list](https://cloud.ibm.com/resources){: external} or by using the **`resource service-instances`** CLI command. This command lists {{site.data.keyword.openwhisk_short}} namespaces of all regions.

```bash
ibmcloud resource service-instances --service-name functions
```
{: pre}

The following example shows sample output from the **`ibmcloud resource service-instances --service-name functions`** command.

```sh
Name         Location   State    Type   
playground   us-south   active   service_instance
```
{: screen}

### How do I see the {{site.data.keyword.openwhisk_short}} entities in my namespace?
{: #how_find_iam_namespace}

You can see a list of your {{site.data.keyword.openwhisk_short}} entities by running the [**`namespace get`**](/docs/openwhisk?topic=openwhisk-functions-cli#cli_namespace_get) command. Replace `<namespace_name>` or `<namespace_id>` with the name or ID of your namespace.

```bash
ibmcloud fn namespace get <namespace_name> or <namespace_id>
```
{: pre}

## Creating an IAM-based namespace
{: #create_iam_namespace}

You can create an IAM-managed namespace as part of a resource group and manage access policies for your resources by targeting the resource group when a namespace is created. If you have other users that require access to your namespace, or if you want to access other resources from your namespace's actions, be sure that you set IAM policies after your namespace is created.
{: shortdesc}

If you are creating a namespace in a resource group of an account that is not your own, you must have the Administrator Platform access role for Cloud Functions. Account owners have the Administrator role by default. For more information, see [Platform management roles](/docs/openwhisk?topic=openwhisk-iam#iam_platform_roles).
{: note}

If you are an Administrator and want to allow other users to create namespaces, see [How do I set IAM policies so that others can create namespaces in my account?](/docs/openwhisk?topic=openwhisk-iam#iam_namespace_policies).

### Creating an IAM-based namespace from the console
{: #create_iam_ui}

Create your IAM-based namespace by using the console.
{: shortdesc}

1. From the [{{site.data.keyword.openwhisk_short}} console](https://cloud.ibm.com/functions){: external} namespace menu, select **Create namespace**.
2. Enter a display name for the namespace and a short description, such as the actions or packages that you plan to create in this namespace.
3. Choose the resource group where you want to create the namespace and a location to deploy the namespace resource.
4. Click **Create**.
5. To view the service instance for the namespace resource, go to your [{{site.data.keyword.cloud_notm}} dashboard](https://cloud.ibm.com/resources){: external} and find your namespace name in **Functions namespaces**.

You can update the name or description of the namespace from the **Namespace settings** page in the {{site.data.keyword.openwhisk_short}} console.

### Creating an IAM-based namespace with the CLI
{: #namespaces_create}

You can create an IAM-managed namespace with the CLI.
{: shortdesc}

1. Target the resource group where you want to create the namespace. If you haven't created a [resource group](/docs/account?topic=account-rgs) yet, you can target the `default` group.

    ```bash
    ibmcloud target -g default
    ```
    {: pre}

2. Create an IAM-enabled namespace. Optional: Include a description for your namespace by using the `-n` or `--description` flag. If your description is longer than one word, it must be in quotations.

    ```bash
    ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
    ```
    {: pre}

    | `<namespace_name>` | The display name for the IAM-based namespace. |
    | `-n <description>` | Optional: Add a description to the namespace, such as which kind of actions or packages you plan to create. If your description is longer than one word, it must be in quotations. |
    | `--description <description>` | Optional: Add a description to the namespace, such as which kind of actions or packages you plan to create. If your description is longer than one word, it must be in quotations. |
    {: caption="Table 2. Understanding this command's components" caption-side="bottom"}

    The following example shows sample output from the `namespace create` command.

    ```sh
    ok: created namespace myNamespace
    ```
    {: screen}

3. Verify that your new namespace is created.

    ```bash
    ibmcloud fn namespace get <namespace_name_or_id> --properties
    ```
    {: pre}

    The following example shows sample output from the `namespace get` command.

    ```sh
    Details of namespace: myNamespace
    Description: short description
    Resource Plan Id: functions-base-plan
    Location: jp-tok
    ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
    ```
    {: screen}

    You can also list all namespaces, including IAM-based and Cloud Foundry-based namespaces:

    ```bash
    ibmcloud fn namespace list
    ```
    {: pre}

4. Before you can create entities in the namespace, you must set your CLI context to the namespace by targeting it.

    ```bash
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

After you set a property, such as the `--namespace` property, it is retained until you manually unset it. If you want to switch between IAM namespaces or between Cloud Foundry and IAM namespaces, you must unset the namespace property and then reset it. For more information, see [**`ibmcloud fn property set`**](/docs/openwhisk?topic=openwhisk-functions-cli#cli_prop_set).
{: note}

### Creating a namespace with the API
{: #namespaces_create_api}

Create your IAM-managed namespace with the API.
{: shortdesc}

1. Create an IAM-enabled namespace.

    ```bash
    curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
    ```
    {: pre}
    
    | `<IAM_token>` | Your {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token. To retrieve your IAM token, run `ibmcloud iam oauth-tokens`. |
    |  `-n <name>` | The name of the namespace. |
    |  `-n <resource_group_id>` | The ID of the resource group that you want to create the namespace in. To see resource group IDs, run `ibmcloud resource groups`. |
    |  `-n <resource_plan_id>` | The ID of the resource plan, such as functions-base-plan |
    |  `-n <description>` | Optional: Add a description to the namespace, such as which kind of actions or packages it will contain. |
    {: caption="Table 3. Understanding this command components" caption-side="bottom"}

    The following example shows sample output from the previous command.

    ```sh
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

2. Verify that your new namespace is created.

    ```bash
    curl --request GET \
    --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
    ```
    {: pre}

    You can also list all namespaces, including IAM-based and Cloud Foundry-based namespaces:

    ```bash
    curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
    ```
    {: pre}

    The following example shows sample output from the previous command.

    ```sh
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

For more information about working with HTTP REST, check out the [{{site.data.keyword.openwhisk_short}} API Docs](/apidocs/functions).
{: tip}

### Optional workflow: Creating namespaces for staging and production deployments
{: #namespaces_create_stage_prod}

You can create IAM-enabled namespaces to handle your pre-production (staging) and production {{site.data.keyword.openwhisk_short}} deployments by creating namespaces for each. Run [`ibmcloud fn namespace create`](/docs/openwhisk?topic=openwhisk-functions-cli#cli_namespace_create) to create more namespaces under your account such as "staging" and "production":

* Create a staging namespace.

    ```bash
    ibmcloud fn namespace create staging
    ```
    {: pre}

* Create a production namespace.

    ```bash
    ibmcloud fn namespace create production
    ```
    {: pre}

{{site.data.keyword.openwhisk_short}} has restrictions on namespace names. For more information, see the [System details and Limits](/docs/openwhisk?topic=openwhisk-limits) documentation.
{: tip}

## Targeting namespaces
{: #targeting-namespaces}

Before you can work in {{site.data.keyword.openwhisk_short}}, you must target a namespace. You can target IAM namespaces or Cloud Foundry namespaces.
{: shortdesc}

To target a namespace, use the [**`ibmcloud fn namespace target`**](/docs/openwhisk?topic=openwhisk-functions-cli#cli_namespace_target) command or the `ibmcloud fn property set --namespace <namespace_name_or_ID>` command.

For example, to target an IAM namespace called `playground`,

```bash
ibmcloud fn namespace target playground
```
{: pre}


### Accessing other resources from a namespace
{: #namespace-access}

Actions typically call other {{site.data.keyword.cloud_notm}} resources and services that require appropriate authentication. If these services are IAM-enabled and accept IAM tokens, you can leverage the namespace functional ID for outbound communication.
{: shortdesc}

As described in [Managing IAM access](/docs/account?topic=account-userroles), for each namespace, a service ID is created that represents the namespace. You can grant access to other services and resources for this service ID by assigning the appropriate roles by using IAM policy management. For more information about creating service IDs to access other IAM-enabled services, see [Creating and working with service IDs](/docs/account?topic=account-serviceids).

At runtime, {{site.data.keyword.openwhisk_short}} passes an API key of the namespace service ID to the action code as the environment variable `__OW_IAM_NAMESPACE_API_KEY`. The action code can use this API key to generate an IAM token. Most of the supported {{site.data.keyword.openwhisk_short}} SDKs such as Cloudant, {{site.data.keyword.watson}}, and {{site.data.keyword.cos_full_notm}} authenticate with the IAM API key itself. For other IAM-managed services or resources that use a REST API, you can authenticate with the token that is derived from the IAM API key. For more information, see [Create an IAM access token for a user or service ID](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

## Next steps for namespaces
{: #namespaces_next}

Not sure how API keys and tokens fit together? Learn more in [the IAM Docs](/docs/account?topic=account-manapikey).

Now that you created a namespace, you can create IAM access policies to help protect it. To get started, check out [Managing access](/docs/openwhisk?topic=openwhisk-iam). 

For more information about how to manage IAM-based namespaces, see the [{{site.data.keyword.openwhisk_short}} REST API reference](/apidocs/functions).


