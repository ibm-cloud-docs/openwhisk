---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: planning

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

# Planning serverless apps
{: #plan}

Before you start creating your function, learn about the decisions you must make along the way.
{: shortdesc}

## Review the runtime support
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} provides standard [runtimes](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes) to execute code. But, because {{site.data.keyword.openwhisk_short}} is serverless, you're not limited in the runtimes that you can use. You can create custom runtimes by creating your own [custom Docker image](/docs/openwhisk?topic=cloud-functions-actions#actions-docker) to package your code.
{: shortdesc}



## Architect your code
{: #plan_architect}

You might need to make some tweaks to your existing code for it to run in {{site.data.keyword.openwhisk}}. If you have not written your code yet, keep these things in mind as you write it.
{: shortdesc}

1. Componentize your code.

    If you already have an app that you want to make serverless, consider how you might need to break your app apart into smaller pieces. Each function contains a set of triggers for code that you want to run. For example, if an issue is created on Github, run this JavaScript code. If your app contains several of these events or actions, consider separating them into different functions.

2. Use {{site.data.keyword.cloud_notm}} services instead of frameworks.

    Instead of using frameworks to make capabilities available in your function at runtime, you can {{site.data.keyword.cloud}} services. Many common tasks that frameworks help you to complete are available as services on {{site.data.keyword.cloud}}.
    {: shortdesc}

    For example, instead of using a framework for authentication, try {{site.data.keyword.appid_full}}. If you need external file storage, try {{site.data.keyword.cos_full}}.

    If the capability that you want to incorporate is not available as a service on {{site.data.keyword.cloud}}, you can always integrate that capability with feeds and APIs too, without the need for frameworks.

3. [Verify that your code meets the system limits.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    Your code must fit within the system limits to run. For example, if your packaged code is larger than 48 MB, you might need to continue breaking it down into smaller pieces or packaging it as a Docker image.

    Applications that contain many third-party modules, native libraries, or external tools might hit this limit. If you create a .zip or .jar package action that is larger than 48 MB, you must extend the runtime image with dependencies, and then use a single source file or smaller archive than 48 MB. For example, by building a custom Docker runtime that includes necessary shared libraries, dependencies are not required to be present in the archive file. Private source files can still be bundled in the archive and injected at runtime.

4. Determine the parameters that must be injected into your code.

    In serverless actions, data is supplied by adding parameters to the actions. Parameters are declared as an argument to the main serverless function. Most commonly, these parameters are credentials for a service, but they might be anything that makes your code reusable with different triggers.

5. [Verify that the structural requirements for your code to be used in a function.](/docs/openwhisk?topic=cloud-functions-prep)

    Whether you have an app already or plan to develop a script to use, your code probably needs a few tweaks to make it consumable by {{site.data.keyword.openwhisk}}. The code itself must meet some structural requirements, such as the input parameters and the output results, and it might need to be packaged into a single file that includes all of its dependencies.







## Determine your event source
{: #plan_source}

Start thinking about your function in terms of the event you want to happen to trigger code to run. Maybe you want code to run on every commit to a Github repository. Or maybe you want your code to run every that an update is made in a Cloudant database.
{: shortdesc}

After you choose an event to trigger your function, review the packages that are already available to use. You might be able to use one to simplify your function development. If not, you can create your own package for your event source.

You might need to go back to your code and make some revisions based on your event selections.


## Organize deployments across environments
{: #plan_environments}

Decide how you want to deploy your functions across environments, such as development, staging, and production environments.
{: shortdesc}

Because {{site.data.keyword.openwhisk_short}} is a Cloud Foundry-based service, you can manage the deployment of your functions in the Cloud Foundry organizations and spaces that are provided for you in {{site.data.keyword.cloud_notm}}. To organize your functions across these environments, you might choose to create one organization per function. Then, create a space for each environment you need. Instead, you could also have one organization for each environment and create a space per function. No matter how you arrange your organizations and spaces, choose a structure that allows you to effectively manage your function entities.

You can also use [namespaces](/docs/openwhisk?topic=cloud-functions-namespaces) to isolate resources. Each {{site.data.keyword.cloud_notm}} space contains an Open Whisk namespace by default. You can group entities, such as actions or triggers in a namespace, and then create Identity and Access (IAM) policies to manage user permissions for that group.



