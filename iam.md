---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-29"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# Managing access
{: #iam}

{{site.data.keyword.openwhisk}} supports Identity and Access Management (IAM). You can now define IAM policies for your resources such as namespaces.
{: shortdesc}

IAM policies are available in the Tokyo region only for {{site.data.keyword.openwhisk_short}}. If you operate in the Tokyo region, you must use IAM policies to control access.
{: tip}

</br>

## Mapping IAM roles to {{site.data.keyword.openwhisk_short}}
{: #user-roles}

In {{site.data.keyword.openwhisk_short}}, namespace's are considered an IBM Cloud resource which allows you to work with IAM roles and policies for access management. All of the policies that you set for a namespace also apply to the {{site.data.keyword.openwhisk_short}} entities, such as actions or triggers, that the namespace contains.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} uses both the Platform and Service management roles. You can set policies about who can create namespaces at the platform level, while using the service roles to manage interaction with the namespaces themselves.

Want to learn more about IAM key concepts? Check out [the IAM docs](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Platform management roles

The following table details the actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level. For example, assign user access for the service, create or delete service IDs, create instances, and bind instances to applications.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Platform role</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Administrator</td>
      <td>Users are able to create namespaces.</td>
    </tr>
  </tbody>
</table>

You need to have the administrator role for platform management to work with the service. For more information about the roles, check out [Platform management roles](/docs/iam?topic=iam-userroles).

</br>

### Service specific roles

Service specific roles determine the scope of an access policy within a specific service. For {{site.data.keyword.openwhisk_short}}, the roles can apply to a users ability to use the service, such as accessing the UI or performing API calls.
{: shortdesc}


It is important to note that the permissions build on each other. For example, any operation that the `writer` role is able to perform, the `manager` role also can. However, the `manager` role would have more permissions added. To see the general permissions for each role, check out [Service access roles](/docs/iam?topic=iam-userroles).

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
    <td>View the available entities, such as rules, within a namespaces.</td>
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

For information about assigning user roles in the UI, see [Managing IAM access](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

</br>


## Setting IAM access policies
{: #set-iam}

When a service invokes an action, the action has a response. Because the response is sent from the namespace or action to a service, it is considered outbound information. If want to limit the amount of influence that your namespace has on other services, you might want to create an access policy.
{: shortdesc}

For information about how to assign, edit, review, or delete resource access policies, see [Managing IAM access](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Accessing other resources from a namespace
{: #namespace-access}

You can access other resources from an IAM managed namespace by using an IAM token. A token represents authentication and verifies the identity of the resource. The IAM token is needed to authenticate when accessing IAM managed services or resources.
{: shortdesc}

Similar to how a user ID identifies a user, a service ID represents a specific resource. This means that IAM policies can be applied to those resources that manage access permissions. Just like a user, a resource must authenticate to verify its identity. Within Functions, this can be leveraged by the actions' implementation when accessing other services or resources.

When you create a new IAM managed namespace, Functions automatically creates a corresponding service ID that identifies the namespace and an API key. At runtime, Cloud Functions passes the API key to the action code as the value of the environment variable `__OW_IAM_NAMESPACE_API_KEY`. The action code can use this API key to generate an IAM token. Most of the supported SDKs such as Cloudant, Watson, and COS authenticate with the IAM key itself. Other IAM managed services or resources that use a REST API, authenticate with the token that is derived from the IAM key.

Not quite sure how API keys and tokens fit together? Learn more in [the IAM docs](/docs/iam?topic=iam-iamapikeysforservices).
