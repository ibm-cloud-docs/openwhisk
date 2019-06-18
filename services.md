---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

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

These parameters might include values that make your app reusable with different data or they might include values that are required by the service, such as credentials. 
1. You can hard code parameters into your app. This option might not be the most secure way of storing confidential information like credentials.
2. You can bind the parameters to your app by binding them to an action or package.


## Binding a service to an action or package
{: #services_bind}

Bind any {{site.data.keyword.cloud_notm}} service to any action. When a service is bound, a new parameter is created on your existing action that contains the service instance credentials.

You cannot bind multiple instances of the same service to an action or package. You can bind only one instance of a service.
{: note}

Before you begin, [create an action](/docs/openwhisk?topic=cloud-functions-actions) and [define credentials](/docs/resources?topic=resources-externalapp#externalapp) for the service that you want to bind to the action.

1. Get the name of the service and the service instance that you want to bind to an action or package. In the example output, `composer` is the service and `Composer-qp` is the service instance name.
    ```
    ibmcloud service list
    ```
    {: pre}

    **Example output**
    ```
    name              service        plan   bound apps   last operation
    Composer-qp       composer       free                create succeeded
    Composer-uc       composer       free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Get the name of the credentials that are defined for a service instance.

    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    **Example**
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    **Example output**
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
    Credentials-1
    Credentials-2
    ```
    {: screen}

3. Bind the service to an action. The {{site.data.keyword.openwhisk_short}} `ibmcloud fn service bind` command makes your {{site.data.keyword.cloud_notm}} service credentials available to your {{site.data.keyword.openwhisk_short}} code at runtime. 

    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    Understanding the `ibmcloud fn service bind` command components.

        * `SERVICE` - The service name that you're binding.
        * `ACTION_NAME` - The name of the action or package that you want to bind the service to.
        * `--instance INSTANCE_NAME` - (Optional) Specify a service instance name. If you don't specify a service instance name, the first instance for the service is selected.
        * `--keyname CREDENTIALS_NAME` - (Optional) Specify the credentials name. If you don't specify the credentials name, the first credentials for the service instance are selected.

    For example, to bind an {{site.data.keyword.ibmwatson}} Composer service to an action named `hello`, run the following command.

    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    **Output**

    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verify that the credentials are successfully bound. The action that the service is bound to doesn't support any custom flags, but does support the debug and verbose flags.

    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    **Example output**
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
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    In this example, the credentials for the Composer service, along with any other credentials for other service types, belong to a parameter named `__bx_creds`. The action looks for the `__bx_creds` bound parameter, and removes the reference to the service type listed. If that service type is the only one listed, the action nulls out the `__bx_creds` parameter's value. If more than one service is bound to the action, the `__bx_creds` parameter remains with whatever services are still bound.

For more information about passing parameters to an action or package, see [Binding parameters to actions](/docs/openwhisk?topic=cloud-functions-actions#actions_params).

## Unbinding services from actions
{: #services_unbind}

Unbinding a service from an action or package removes existing service bindings.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
