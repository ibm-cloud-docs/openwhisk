---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: services, serverless

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


# Adding IBM Cloud services
{: #services}

You can use incorporate functionality from IBM Cloud services in your app.
{: shortdesc}

**How do I add IBM Cloud services to my app?**
1. You can hard code REST API calls into your app. This option might be the quickest way to communicate with an IBM Cloud service.
2. You can use a pre-installed or installable package to incorporate functionality. You can use the actions and feeds that are stored in the packages within your app code. This option might slim down your code a bit, which might be useful if your app is close to the system limits.

**How do I set up parameters that must be accessed by my app?**
These parameters might include values that make your app reusable with different data or they might include values required by the service, such as credentials. 
1. You can hard code parameters into your app. This option might not be the most secure way of storing confidential information like credentials.
2. You can bind the parameters to your app by binding them to an action or package.


## Binding a service to an action or package
{: #services_bind}

Bind any {{site.data.keyword.Bluemix_notm}} service to any action that is defined in {{site.data.keyword.openwhisk_short}}. Binding a service creates new parameter on your existing action that contains the service instance credentials.

**Note**: You can only bind one service of each type to an action or package. Binding services of the same type isn't supported.

Before you begin, [define credentials](/docs/resources?topic=resources-externalapp#externalapp) for the service that you want to bind.

1. Get the name of the service instance that you want to bind to an action or package.
    ```
    ibmcloud service list
    ```
    {: pre}

    Example output:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Get the name of the credentials that are defined for the service instance you got in the previous step.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    Example output:
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
    Credentials-1
    Credentials-2
    ```
    {: screen}

3. Bind the service to an action. The {{site.data.keyword.openwhisk_short}} `ibmcloud fn service bind` command makes your {{site.data.keyword.Bluemix_notm}} service credentials available to your {{site.data.keyword.openwhisk_short}} code at runtime.
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Understanding the <code>ibmcloud fn service bind</code> command components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>ibmcloud fn service bind</code> command components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>The type of service that you're binding.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>The name of the action or package that you want to bind the service to.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>Optional: Specify a service instance name. If you don't specify a service instance name, the first instance for the service is selected.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>Optional: Specify a credentials set name. If you don't specify a credentials set name, the first credentials set for the service instance is selected.</td>
    </tr>
    </tbody></table>

    For example, to bind a {{site.data.keyword.ibmwatson}} Conversation service to an action named `hello`:
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1
    ```
    {: pre}

    Output:
    ```
    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verify that the credentials are successfully bound. The action that the service is bound to doesn't support any custom flags, but does support the debug and verbose flags.
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    Example output:
    ```
    ok: got action Hello World
    {
        "parameters": [
            {
                "key": "var1",
                "value": "val1"
            },
            {
                "key": "dog",
                "value": "cat"
            },
            {
                "key": "__bx_creds",
                "value": {
                    "conversation": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Conversation-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    In this example, the credentials for the conversation service, along with any other credentials for other service types, belong to a parameter named `__bx_creds`. The action looks for the `__bx_creds` bound parameter, and removes the reference to the service type listed. If that service type is the only one listed, the action nulls out the `__bx_creds` parameter's value. If more than one service is bound to the action, the `__bx_creds` parameter remains with whatever services are still bound.

For more information about passing parameters to an action or package, see [Binding parameters to actions](/docs/openwhisk?topic=cloud-functions-actions_params).

If you change the name of the org or space that contains entities, then a namespace is created with the new name. The entities that your old namespace contained aren't visible in the new namespace and are scheduled to be deleted. If you made the change accidentally, you can revert it and you might be able to save your entities before they are deleted.
{: tip}


## Unbinding services from actions
{: #services_unbind}

Unbind a service from an action or package. Unbinding a service removes existing bindings created by the `service bind` command.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
