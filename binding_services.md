---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Using services from Actions
{: #binding_services}

You can leverage the [{{site.data.keyword.openwhisk}} CLI plug-in](./bluemix_cli.html) to bind a service to an Action. {{site.data.keyword.openwhisk_short}} provides the `service bind` command to make your {{site.data.keyword.Bluemix}} service credentials available to your Cloud Functions code at run time. The `service bind` command is not to be confused with the `cf bind-service` command that is available in Cloud Foundry. It is simply an automated way to create a new parameter on your existing Action that contains service credentials. The {{site.data.keyword.openwhisk_short}} `service bind` command is more flexible and allows you to bind any {{site.data.keyword.Bluemix_notm}} service to any Action that is defined in {{site.data.keyword.openwhisk_short}}. The only caveat is that you must have credentials defined for the service that you want to bind.
{: shortdesc}

## How to bind a service to an Action
{: #cli_bind}

Bind a service to an Action by using the `bx wsk service bind` command which is provided by the [{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) CLI plug-in. Additional information can be found in the [Limitations](./binding_services.html#limitations) section.

Usage syntax using `bind`:
```
bx wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

The `service bind` command requires a service type and an Action name to bind to. For example, if you want to bind a Watson conversation service to an Action named `hello`, then your invocation would look similar to the following command:
```
bx wsk service bind conversation hello
```
{: pre}

Which produces the following output:
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```
{: screen}

This command searches your current space for existing Watson conversation services, takes the first conversation service it finds, and then retrieves all of the credentials that belong to this service. Using the first set of credentials that belong to this service, it binds those credentials as a parameter to the `hello` Action specified. The output shows you exactly which service the Action is bound to, and which set of credentials from that service were used to bind with.

To verify that credentials are successfully bound, issue the following command:
```
bx wsk action get hello parameters
```
{: pre}

Sample output:
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

From here, you can see that the credentials for this conversation service (along with any other credentials for other service types) belong to a parameter named `__bx_creds`, that can now be used from within the Action code as any other bound parameter can be used. The Action picks the first available conversation service which includes the first set of credentials defined in that service. 

For further information about passing parameters to an Action, and how credentials are affected when performing an `action update` operation, see the following document [Create and invoke Actions](./openwhisk_actions.html#openwhisk_pass_params).

The `bx wsk service` command supports the following two flags:

<dl>
    <dt>--instance</dt>
    <dd>The name of the specific service of the type you wish to use.</dd>
    <dt>--keyname</dt>
    <dd>The name of the specific credentials within the service that you wish to use.</dd>
</dl>

To undertand how to use these flags, see the following example. By using the previous `bx wsk service bind` command, assume there were actually two conversation services, and the Action default ended up binding the incorrect service/credentials. You could rerun the command with the `--instance` and `--keyname` flags to ensure that you bind the correct service to the correct Action. First, look at what services are available, and what credentials are bound to them by running `bx service list` to see output like the following:
```
bx service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```
{: screen}

From this output we see that **Conversation-qp** is the first of two services listed, and it is the one that the initial `bx wsk service bind conversation hello` command ended up binding to. Perhaps you want to bind to the **Conversation-uc** service instead. So to be absolutely sure, you can check what credentials **Conversation-uc** contains, to ensure that you bind by using the right set of credentials by running `bx service keys Conversation-uc` as shown in the following example:
```
bx service keys Conversation-uc
```
{: pre}

**Output:**
```
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```
{: screen}

You want to bind to "Credentials-2" from this service. To make sure the Action performs the desired behavior, run the following command:
```
bx wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

Which produces the following output:
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```
{: screen}

From the output, you can see that the correct set of credentials are bound to the Action. Again, to verfiy, you can look at the following `bx wsk action get` command.
```
bx wsk action get hello parameters
```
{: pre}

Which produces the following results:
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```
{: screen}

The normal debug flags are supported, and print out response headers from calls.

## How to unbind a service from an Action
{: #cli_unbind}

Unbind a service from an Action by using the `bx wsk service unbind`. The `service unbind` command removes existing bindings created by the `service bind` command.

Usage syntax using `unbind`:
```
bx wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## Limitations
{: #limitations}

The `service` Action does not support any custom flags, but does support the usual debug, and verbose flags. The Action looks for the `__bx_creds` bound parameter, and removes the reference to the service type listed. If that service type is the only one listed, the Action nulls out the `__bx_creds` parameter's value. If more than one service is bound to the Action, the `__bx_creds` parameter remains with whatever services are still bound.

You can only bind one service of each type to an Action. Binding multiple services of the same type within a single Action is not supported.
{: tip}

