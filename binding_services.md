---

copyright:
  years: 2018
lastupdated: "2018-05-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Binding services to Actions
{: #binding_services}

You can leverage the [{{site.data.keyword.openwhisk}} CLI plug-in](./bluemix_cli.html) to bind a service to an Action or Package. The {{site.data.keyword.openwhisk_short}} `bx wsk service bind` command makes your {{site.data.keyword.Bluemix_notm}} service credentials available to your Cloud Functions code at run time.
{: shortdesc}

The `bx wsk service bind` command is not to be confused with the `cf bind-service` command that is available in Cloud Foundry.

## Binding a service to an Action or Package
{: #cli_bind}

Bind any {{site.data.keyword.Bluemix_notm}} service to any Action that is defined in {{site.data.keyword.openwhisk_short}}. Binding a service creates new parameter on your existing Action that contains the service instance credentials.

**Note**:
* The `bx wsk service bind` command does not support binding user-provided services that are created with the `bx cf create-user-provided-service` command.
* You can only bind one service of each type to an Action or Package. Binding multiple services of the same type is not supported.

Before you begin, [define credentials](/docs/apps/reqnsi.html#accser_external) for the service that you want to bind.

1. Get the name of the service instance that you want to bind to an Action or Package.
    ```
    bx service list
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
    bx service keys Conversation-qp
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

3. Bind the service to an Action.
    ```
    bx wsk service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Understanding the <code>bx wsk service bind</code> command components</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the <code>bx wsk service bind</code> command components</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>The type of service that you are binding.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>The name of the Action or Package that you want to bind the service to.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>Optional: Specify a service instance name. If you do not specify a service instance name, the first instance for the service is selected.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>Optional: Specify a credentials set name. If you do not specify a credentials set name, the first credentials set for the service instance is selected.</td>
    </tr>
    </tbody></table>

    For example, to bind a {{site.data.keyword.ibmwatson}} conversation service to an Action named `hello`:
    ```
    bx wsk service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verify that the credentials are successfully bound. The Action that the service is bound to does not support any custom flags, but does support the debug and verbose flags.
    ```
    bx wsk action get hello parameters
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

    In this example, the credentials for the conversation service, along with any other credentials for other service types, belong to a parameter named `__bx_creds`. The Action looks for the `__bx_creds` bound parameter, and removes the reference to the service type listed. If that service type is the only one listed, the Action nulls out the `__bx_creds` parameter's value. If more than one service is bound to the Action, the `__bx_creds` parameter remains with whatever services are still bound.

For more information about passing parameters to an Action or Package and how credentials are affected when performing `update` operations, see [Working with parameters](./parameters.html#pass-params-action).


## Unbinding a service from an Action or Package
{: #cli_unbind}

Unbind a service from an Action or Package. Unbinding a service removes existing bindings created by the `service bind` command.

```
bx wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
