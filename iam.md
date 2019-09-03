---
copyright:
  years: 2017, 2019
lastupdated: "2019-09-03"

keywords: access policies, iam, roles, functions

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




# Setting access policies
{: #iam}

When setting the permissions on an IAM namespace, all entities such as actions, triggers, and packages inherit the permissions.

As the creator of a namespace, you do not need to set any IAM policies to view or work with your {{site.data.keyword.openwhisk_short}} entities.

## Mapping IAM roles to {{site.data.keyword.openwhisk_short}}
{: #user-roles}

In {{site.data.keyword.openwhisk_short}}, namespaces are {{site.data.keyword.cloud_notm}} resources that you can use to work with IAM roles and policies for access management. All of the policies that you set for a namespace also apply to the {{site.data.keyword.openwhisk_short}} entities, such as actions or triggers, that the namespace contains.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} uses both the Platform and Service management roles. You can set policies about who can create namespaces at the platform level, and use the service roles to manage interaction with the namespaces themselves.

### How do I set IAM policies so that others can work with my namespace?
{: #iam_namespace_policies}
In order for others to work with entities in your namespace, you must to set the appropriate [IAM policies in the UI](https://cloud.ibm.com/iam/overview) or the CLI. 

The minimum Platform level access is `Viewer`. The minimum Service level access is `Reader`. For more information about Platform and Service level access roles, see the [Platform management roles](#iam_platform_roles) and [Service-specific roles](#service_specific_roles) sections below.

For more information, see [Setting policies through the CLI](#cli-set).

Want to learn more about IAM key concepts? Check out [the IAM docs](/docs/iam?topic=iam-iamoverview#iamconcepts){: external}.
{: tip}

</br>

### Platform management roles
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
      <td>Users can create namespaces. The Administrator role is required to perform the `service id` and `apikey lock` operations during creation of the service.</td>
    </tr>
  </tbody>
</table>

Because the service does not need to be provisioned, the editor role is the only platform role that you need to work with the service. For more information about the other roles, check out [Platform management roles](/docs/iam?topic=iam-userroles){: external}.

</br>

### Service-specific roles
{: #service_specific_roles}

Service-specific roles determine the scope of an access policy within a specific service. For {{site.data.keyword.openwhisk_short}}, the roles can apply to a users ability to use the service, such as accessing the UI or performing API calls.
{: shortdesc}

Permissions build on each other. For example, any operation that the `Writer` role is able to perform, the `Manager` role also can. However, the `Manager` role would have more permissions added. To see the general permissions for each role, check out [Service access roles](/docs/iam?topic=iam-userroles){: external}.

To see which roles are required to perform each operation, check out the following table:

<table><caption>Which roles can perform which operations?</caption>
  <tr>
    <th style="width:150px">Action</th>
    <th style="width:2500px">Description</th>
    <th style="width:50px">Reader</th>
    <th style="width:50px">Writer</th>
    <th style="width:50px">Manager</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>Update a namespace.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Delete a namespace.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>View the available namespaces.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Create an entity, such as an action, within a Functions namespace.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Update an entity, such as a package, within a Functions namespace.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Delete an entity, such as a trigger, from a Functions namespace.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>View the available entities, such as rules, within a namespace.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Activate an entity, such as an action, within a namespace.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
</table>

</br>

### Setting policies through the CLI
{: #cli-set}

To give a resource, such as an action, in an IAM-based namespace access to an IAM-based service, you can create an IAM access policy for the namespace that resource is in.

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

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
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>The name of the other {{site.data.keyword.cloud_notm}} service type.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>The GUID of the other service instance that you want the action to have access to. To get the service instance GUID, run <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>.</td>
    </tr>
  </tbody>
</table>

</br>

**Next steps**
For more information about managing service credentials, see the [Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} blog.









