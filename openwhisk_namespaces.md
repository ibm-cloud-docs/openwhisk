---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Creating namespaces
{: #openwhisk_namespaces}

In the Tokyo region, {{site.data.keyword.openwhisk_short}} uses Identity and Access (IAM) managed namespaces to group entities, such as actions or triggers, together. Then, you can create access policies for the namespace.
{: shortdesc}

When you create an {{site.data.keyword.openwhisk_short}} namespace it is identified as as an IAM service instance. IAM managed service instances must be created within a
[resource group](/docs/resources/resourcegroups.html). You can either create your own resource group or target the default. To see the IAM service-instances that you have in your account, you can run `ibmcloud resource service-instances`.

The following artifacts are created in conjunction with your namespace. Do not delete them.

* An ID is created that can be used as a functional id when you make outbound calls. All of the actions that are created in this namespace use this ID. To see all of your IDs, run `ibmcloud fn namespace list`.

* An API key is created and can be used to generate IAM tokens. You can then use the tokens to authenticate the namespace with other IBM Cloud services. You can download the API key from the Cloud Functions dashboard.



## Limitations
{: #limitations}

[Creating APIs with API Gateway](openwhisk_apigateway.html) and using the [mobile SDK](openwhisk_mobile_sdk.html) are not supported for IAM managed namespaces at this time.

</br>



## Creating a namespace with the CLI
{: #create_iam_cli}

You can create an IAM managed namespace as part of a resource group and manage access policies for your resources by targeting the resource group when a namespace is created. If you have other users that require access to your namespace, or if you want to access other resources from your namespace's actions, be sure that you set IAM policies after your namespace is created.
{: shortdesc}

1. Target the resource group where you want to create the namespace. If you haven't created a [resource group](/docs/cli/reference/ibmcloud/cli_resource_group.html#ibmcloud_resource_group_create) yet, you can target the `default` group.

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
          <td>The display name for the new IAM-based namespace.</td>
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
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
    ```
    {: pre}

    Example output:

    ```
    Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'us-south'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
    ```
    {: screen}

    You can also list all namespaces, including IAM-based and Cloud Foundry-based namespaces:
    ```
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. Before creating entities in the new namespace, set your CLI context to the namespace by targeting it.
    ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}




## Creating a namespace with the API
{: #create_iam_cli}

You can create an IAM managed namespace as part of a resource group and manage access policies for your resources by targeting the resource group when a namespace is created. If you have other users that require access to your namespace, or if you want to access other resources from your namespace's actions, be sure that you set IAM policies after your namespace is created.
{: shortdesc}



2. Create an IAM-enabled namespace.

    ```
    curl --request POST \
    --url 'https://openwhisk.ng.bluemix.net/api/servicebroker/api/v1/namespaces \
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
          <td>Your IBM Cloud Identity and Access Management (IAM) token. To retrieve your IAM token, run <code>ibmcloud iam oauth-tokens</code>.</td>
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
      "location": "us-south",
      "crn": "crn:v1:functions:us-south:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
    ```
    {: screen}

3. Verify that your new namespace is created.

    ```
    curl --request GET \
      --url 'https://openwhisk.ng.bluemix.net/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
    ```
    {: pre}




For more information about working with HTTP REST, check out the [Cloud Functions API docs](https://console.bluemix.net/apidocs/functions).

</br>
</br>


## Next steps
{: #next}

Now that you've created a namespace, you can create IAM access policies to help protect it. To get started, check out [Managing access](iam.html). For more information about how you can manage IAM-based namespaces, see the [{{site.data.keyword.openwhisk_short}} Namespaces REST API reference](/apidocs/functions/functions-namespace).
