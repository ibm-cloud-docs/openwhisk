---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

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

</br>

## Mapping IAM roles to {{site.data.keyword.openwhisk_short}}
{: #user-roles}

In {{site.data.keyword.openwhisk_short}}, namespace's are considered an {{site.data.keyword.cloud_notm}} resource which allows you to work with IAM roles and policies for access management. All of the policies that you set for a namespace also apply to the {{site.data.keyword.openwhisk_short}} entities, such as actions or triggers, that the namespace contains.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} uses both the platform and service management roles. You can set policies about who can create namespaces at the platform level, while using the service roles to manage interaction with the namespaces themselves.

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
{: #service_specific_roles}

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


## Setting IAM access policies for a namespace
{: #set-iam}

When managing a namespace or the entities inside, you can grant access to other users by using the [service specific roles](#service_specific_roles) listed above. During creation of the namespace, a service ID is created which represents the namespace along with a functional user ID. By default, the functional user ID is assigned the Reader role. Readers can read namespace entities and invoke actions. The Reader role is used by triggers to invoke actions. To control inbound traffic, you might want to grant access to other users such as assigning Reader role to invoke actions.
{: shortdesc}

For information about how to assign, edit, review, or delete resource access policies, see [Managing IAM access](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Accessing other resources from a namespace
{: #namespace-access-resources}

Actions typically call other {{site.data.keyword.cloud_notm}} resources and services which require the appropriate authentication.
If these services are IAM enabled and accept IAM tokens, you can leverage the namespace's functional ID for outbound communication.
As described in [Managing IAM access](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), for each namespace, a service ID is created that represents the namespace. You can grant access to other services and resources for this service ID by assigning the appropriate roles using IAM policy management.

At runtime, {{site.data.keyword.openwhisk_short}} passes an API key of the namespace service ID to the action code as the value of the environment variable `__OW_IAM_NAMESPACE_API_KEY`. The action code can use this API key to generate an IAM token. Most of the supported {{site.data.keyword.openwhisk_short}} SDKs such as Cloudant, {{site.data.keyword.watson}}, and {{site.data.keyword.cos_full_notm}} authenticate with the IAM API key itself. For other IAM managed services or resources that use a REST API, you can authenticate with the token that is derived from the IAM API key. For more information, see [Create an IAM access token for a user or service ID](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Not quite sure how API keys and tokens fit together? Learn more in [the IAM docs](/docs/iam?topic=iam-iamapikeysforservices).

