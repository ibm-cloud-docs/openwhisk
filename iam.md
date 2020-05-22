---
copyright:
  years: 2017, 2020
lastupdated: "2020-05-22"

keywords: access policies, iam, roles, functions

subcollection: openwhisk

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

# Setting access policies
{: #iam}

Access to {{site.data.keyword.openwhisk_short}} service instances for users in your account is controlled by IBM Cloud Identity and Access Management (IAM). For {{site.data.keyword.openwhisk_short}}, your {{site.data.keyword.openwhisk_short}} namespace is considered to be your service instance. Every user that accesses {{site.data.keyword.openwhisk_short}} entities in your namespace must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the namespace that you select. The actions are then mapped to IAM user roles. If you created the namespace, then you do not need to set any IAM policies to view or work with your {{site.data.keyword.openwhisk_short}} entities.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} uses both the Platform and Service management roles. You can set policies about who can create namespaces at the platform level, and use the service roles to manage interaction with the namespaces themselves.

## How do I set IAM policies so that others can work with my namespace?
{: #iam_namespace_policies}
In order for others to work with entities in your namespace, you must to set the appropriate [IAM policies in the console](https://cloud.ibm.com/iam/overview) or [with the CLI](#cli-pol-set). 

The minimum Platform level access is Viewer. The minimum Service level access is Reader. For more information about Platform and Service level access roles, see [Platform management roles](#iam_platform_roles) and [Service-specific roles](#service_specific_roles).

Want to learn more about IAM key concepts? Check out [the IAM overview](/docs/iam?topic=iam-iamoverview){: external} or the [Best practices for assigning access](/docs/iam?topic=iam-account_setup){: external}.
{: tip}

## How do I set IAM policies so that others can create namespaces in my account?
{: #iam_namespace_create}
In order to allow other users to manage {{site.data.keyword.openwhisk_short}} namespaces, including creating new namespaces, you must set the following access policies for those users.

  * The user's **Platform role** must be set to Administrator. This policy applies to all resources of {{site.data.keyword.openwhisk_short}}.
  * The user's **Service role**  must be set to Manager. This policy applies to all resources of {{site.data.keyword.openwhisk_short}}.

## How do I know which access policies have set for me?
{: #iam_set_policies_me}
You can see which access policies have been set for you in the [{{site.data.keyword.Bluemix}} catalog](https://cloud.ibm.com/catalog){: external} console.

1. From the console, click **Manage** > **Access (IAM)** > **Users**. Or, navigate to `https://cloud.ibm.com/iam/users`.
2. Click your name in the user table.
3. Click the **Access policies** tab to see your access policies.

</br>

## Platform management roles
{: #iam_platform_roles}

The following table details the actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level. For example, assign user access for the service, create or delete service IDs, create instances, and bind instances to applications.
{: shortdesc}

For more information about how to assign, edit, review, or delete resource access policies, see [Managing IAM access](/docs/iam?topic=iam-iammanidaccser#iammanidaccser){: external}.
{: tip}

<table>
  <thead>
    <tr>
      <th>Platform role</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Viewer</td>
      <td>The Viewer role is a required role that allows users to work with namespaces of other accounts. You can also add the appropriate service-specific role for a namespace as described below.</td>
    </tr>
    <tr>
      <td>Administrator</td>
      <td>Administrators can create namespaces. The Administrator role is required to perform the service ID and apikey lock operations during creation of the service.</td>
    </tr>
  </tbody>
</table>

</br>

## Service-specific roles
{: #service_specific_roles}

Service-specific roles determine the scope of an access policy within a specific service. For {{site.data.keyword.openwhisk_short}}, the roles can apply to a users ability to use the service, such as accessing the console or performing API calls.
{: shortdesc}

Permissions build on each other. For example, any operation that the Writer role is able to perform, the Manager role also can. However, the Manager role has additional permissions. To see the general permissions for each role, check out [Service access roles](/docs/iam?topic=iam-userroles#service_access_roles){: external}.

To see which roles are required to perform each operation, check out the following table:

  <table><caption>Which roles can perform which operations?</caption>
  <tr>
    <th style="width:150px">Action</th>
    <th style="width:50px">Reader</th>
    <th style="width:50px">Writer</th>
    <th style="width:50px">Manager</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code><br>Update a namespace.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code><br>Delete a namespace.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code><br>View the available namespaces.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code><br>Create an entity within a Functions namespace, such as a package, action, trigger, or rule.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code><br>Update an entity within a Functions namespace, such as a package, action, trigger, or rule.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code><br>Delete an entity from a Functions namespace, such as a package, action, trigger, or rule.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code><br>View the available entities within a namespace, such as package, action, trigger, or rule.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code><br>Activate an entity within a namespace. Activate entities, for example, by invoking an action, firing a trigger, or enabling or disabling a rule.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
</table>

</br>

## Setting access policies for a user with the CLI
You can set access policies for a specific users with the `iam user-policy-create` command. In this example `name@example.com` is assigned the Viewer role for {{site.data.keyword.openwhisk_short}}. After this role is applied, users with the Viewer role can access all of your {{site.data.keyword.openwhisk_short}} namespaces.
{: #cli-pol-set}

```
ibmcloud iam user-policy-create name@example.com --roles Viewer --service-name functions
```
{: pre}

</br>

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>ibmcloud iam service-policy-create</code> command components</th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>The service ID for the namespace. To see all service IDs, run <code>ibmcloud iam service-ids</code>.</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>The type of IAM service access role that the action must have to use the target service. To see the supported roles for the other service, run <code>ibmcloud iam roles --service SERVICE_NAME</code>. For more information, see [IAM access roles](/docs/iam?topic=iam-userroles#service-access-roles).</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_name&gt;</code></td>
      <td>The name of the other {{site.data.keyword.cloud_notm}} service name.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>The GUID of the other service instance that you want the action to have access to. To get the service instance GUID, run <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>.</td>
    </tr>
  </tbody>
</table>

For more information about IAM commands, see the [IAM CLI reference docs](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create){: external}.

</br>

**Next steps**
For more information about managing service credentials, see the [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} blog.
