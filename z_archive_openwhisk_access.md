---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-10"

keywords: access, assign, roles, policies

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Assigning user access
{: #openwhisk_access}

Create and manage IAM user access policies to determine how users can interact with {{site.data.keyword.openwhisk}}.
{: shortdesc}

IAM-based namespaces are configured to use {{site.data.keyword.Bluemix_notm}} IAM policies, so you must define access policies for every user that works with an IAM-based namespace in {{site.data.keyword.openwhisk_short}}. The scope of an access policy is based on a user's defined role or roles that determine the actions that they are allowed to perform. Some policies are pre-defined, but others can be customized. The same policy is enforced whether a user makes a request through the {{site.data.keyword.openwhisk_short}} GUI, CLI, or API.

Have a Cloud Foundry-based namespace? Check out [general information about Cloud Foundry access and roles](/docs/iam?topic=iam-cfaccess#cfaccess) or [steps for managing Cloud Foundry access](/docs/iam?topic=iam-mngcf#mngcf).
{: tip}

## Types of user access roles
{: #types}

IAM user access policies grant roles to determine how individuals can interact with {{site.data.keyword.openwhisk_short}}. These roles are classified according to platform and service:

<dl>
<dt>Platform management roles</dt>
<dd>Platform management roles determine the user's ability to create IAM-based namespaces and manage namespace service IDs. More broadly, platform management roles also determine the user's ability to manage users and permissions and create resource groups. The four possible roles are `Viewer`, `Editor`, `Operator`, and `Administrator`. The role permissions build on each other, which means that the `Editor` role has all of the same permissions as the `Viewer` role, plus the permissions that are granted to an editor. To see the general permissions for each role, check out [Platform management roles](/docs/iam?topic=iam-userroles).</dd>
<dt>Service access roles</dt>
<dd>Service access roles determine the user’s ability to use {{site.data.keyword.openwhisk_short}}, such as accessing the UI or performing API calls. The three possible roles are `Reader`, `Writer`, and `Manager`. The role permissions build on each other, which means that the `Writer` role has all of the same permissions as the `Reader` role, plus the permissions that are granted to a writer. To see the general permissions for each role, check out [Service access roles](/docs/iam?topic=iam-userroles).</dd>
</dl>

Within {{site.data.keyword.openwhisk_short}}, for example, IAM-based namespace processes require the following access roles:

| IAM-based namespace process | Required access role |
| --------------------------- | -------------------- |
| List all namespaces or view individual namespace info | Reader, Writer, or Manager service access role |
| Create an IAM-based namespace  | Editor platform management role |
| Update or delete an IAM-based namespace | Manager service access role |

## Setting user access policies
{: #set}

When you assign a policy, you first choose from setting the policy for access to a resource group or an individual resource. Then, depending on your initial selection, you can select a service within a resource group or a single instance for the selected resource. Finally, you select roles to assign to a specific user, a group of users, or to the default resource group:
* *Users*: You might have a specific user that needs more or less permissions than the rest of your team. You can customize permissions on an individual basis so that each person has the right amount of permissions that they need to complete their task.
* *Access groups*: You can create groups of users and then assign permissions to a specific group. For instance, you could group all of your team leads and give that group administrator access.
* *Resource groups*: You can create access policies for a group of resources and grant users access to this group. These resources can be part of one {{site.data.keyword.Bluemix_notm}} service or you can also group resources across service instances.

For steps on how to assign, edit, review, or delete user access policies, see [Managing IAM access](/docs/iam?topic=iam-iammanidaccser) in the IAM documentation.


