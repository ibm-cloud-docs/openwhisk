---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} offers a powerful plug-in for the {{site.data.keyword.Bluemix_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system.
{: shortdesc}

## Setting up the {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

Download and install the {{site.data.keyword.Bluemix_notm}} CLI here:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html

## Setting up the {{site.data.keyword.openwhisk_short}} plug-in
{: #cloudfunctions_plugin_setup}

To install the {{site.data.keyword.openwhisk_short}} plug-in, run the following command:
```
bx plugin install cloud-functions
```
{: pre}


To validate successful {{site.data.keyword.openwhisk_short}} plug-in installation, run the following command:
```
bx plugin list cloud-functions
```
{: pre}


The output displays the installed {{site.data.keyword.openwhisk_short}} version information:
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

You can upgrade the {{site.data.keyword.openwhisk_short}} plug-in by running the following command:
```
bx plugin update Cloud-Functions
```
{: pre}


For more information about the plug-in command, use `bx plugin --help` or check out the following documentation:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}} CLI authentication
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} is available in two {{site.data.keyword.Bluemix_notm}} regions.

When you log into the {{site.data.keyword.Bluemix_notm}} CLI, you can specify the {{site.data.keyword.Bluemix_notm}} API endpoint for the target region alongside the organization and space.

To log in to the US South region:
```
bx login -a api.ng.bluemix.net
```
{: pre}


To log in to the United Kingdom region:
```
bx login -a api.eu-gb.bluemix.net
```
{: pre}


The `-a` flag specifies the {{site.data.keyword.Bluemix_notm}} API endpoint to use. If the API endpoint is  specified, the `-a` option is not necessary. You can use the `bx api` command to explicitly set the {{site.data.keyword.Bluemix_notm}} API endpoint. To display the current API endpoint setting, use the `bx target` command."

The `login` command prompts you for information such as organization, space, and password if not specified. You can specify the organization and space in the command line to skip the prompts for them.
```
bx login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


You can also use a {{site.data.keyword.Bluemix_notm}} API key to log in. This method is useful when your account is configured with a Federated Login requiring you to log in with the flag `--sso`. Using an API key is also beneficial if you want to set a Continue Integration (CI), and want to configure an unattended pipeline.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

To create a new API key by using the {{site.data.keyword.Bluemix_notm}} CLI, run the following command:
```
bx iam api-key-create MyKey
```
{: pre}


Then, use the API key generated value to log in, like in the following example:
```
bx login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


For more information about the login command, use `bx login --help` or review the following documentation:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Using the {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cloudfunctions_plugin_usage}

Verify your setup. Perform a blocking (synchronous) invocation of echo, passing it `hello` as an argument like in the following example:
```
bx wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


See the following example output:
```
{
    "message":"hello"
}
```

After you configure your environment, you can use the {{site.data.keyword.openwhisk_short}} CLI to perform the following tasks:

* Run your code snippets, or Actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking Actions](./openwhisk_actions.html).
* Use Triggers and Rules to enable your Actions to respond to events. See [Creating Triggers and Rules](./openwhisk_triggers_rules.html).
* Learn how packages bundle Actions and configure external events sources. See [Create and use packages](./openwhisk_packages.html).
* Explore the catalog of packages and enhance your applications with external services, such as a [Cloudant event source](./openwhisk_cloudant.html). See [Using {{site.data.keyword.openwhisk_short}} enabled services](./openwhisk_catalog.html).

To get a list of commands for the {{site.data.keyword.openwhisk_short}} plug-in run `bx wsk` with no arguments.

## How to bind a service to an Action
{: #cli_bind}

Bind a service to an Action by using the `bx wsk service bind` command.

Usage syntax:
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

From here, you can see that the credentials for this conversation service (along with any other credentials for other service types) belong to a parameter named `__bx_creds`, that can now be used from within the Action code as any other bound parameter can be used. The Action picks the first available conversation service which includes the first set of credentials defined in that service. 

For further information about passing parameters to an Action, and how credentials are affected when performing an `action update` operation, see the following document [Create and invoke Actions](openwhisk_actions.html#openwhisk_pass_params).

The `wsk service` command supports the following two flags:

<dl>
    <dt>--instance</dt>
    <dd>The name of the specific service of the type you wish to use.</dd>
    <dt>--keyname</dt>
    <dd>The name of the specific credentials within the service that you wish to use.</dd>
</dl>

To undertand how to use these flags, see the following example. By using the previous `bx wsk service bind` command, assume there were actually two conversation services, and the Action default ended up binding the incorrect service/credentials. You could rerun the command with the `--instance` and `--keyname` flags to ensure that you bind the correct service to the correct Action. First, look at what services are available, and what credentials are bound to them. If we were to list our services we should see output like the following:

```
bx service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

From this output we see that **Conversation-qp** is the first of two services listed, and it is the one that the initial `bx wsk service bind conversation hello` command ended up binding to. Perhaps you want to bind to the **Conversation-uc** service instead. So to be absolutely sure, you can check what credentials **Conversation-uc** contains, to ensure that you bind by using the right set of credentials.

```
bx service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

You want to bind to "Credentials-2" from this service. To make sure the Action performs the desired behavior, run the following command:
```
bx wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

Which produces the following output:
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

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

That is the jist of the `bx wsk service bind` command. The normal debug flags are supported, and print out response headers from calls.

## How to unbind a service from an Action
{: #cli_unbind}

Unbind a service from an Action by using the `bx wsk service unbind`. The `service unbind` command removes bindings created by the `service bind` command.

Usage syntax:
```
bx wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

The `service` Action does not support any custom flags, but does support the usual debug, and verbose flags. The Action looks for the `__bx_creds` bound parameter, and removes the reference to the service type listed. If that service type is the only one listed, the Action nulls out the `__bx_creds` parameter's value. If more than one service is bound to the Action, the `__bx_creds` parameter remains with whatever services are still bound.

You can only bind one service of each type to an Action. Binding multiple services of the same type within a single Action is not supported.
{: tip}


## Configure the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_https_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be set  up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format:
`{PROXY IP}:{PROXY PORT}`.


## Working with regions, organizations and spaces
{: #region_info}

You can create spaces to handle your pre-production (staging) and production deployments by creating spaces for each. Creating spaces allows {{site.data.keyword.openwhisk_short}} to have two different namespaces that are defined for you.

You can use the `bx iam space-create` to create more spaces under your organization such "staging" and "production". See the following example:
```
bx iam space-create "staging"
bx iam space-create "production"
```

For more information about the `bx iam` command, refer to the following documentation: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} has restrictions on the namespace names. For more information about these restrictions, refer to the following documentation: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

If you are already logged in, you can run the `bx target` command in the {{site.data.keyword.Bluemix_notm}} CLI to switch regions, organization, and spaces.

Use the following command to display all of the {{site.data.keyword.Bluemix_notm}} regions.

{{site.data.keyword.openwhisk_short}} is only supported in the `us-south` and `eu-gb` regions.
{: tip}

```
bx regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

Use the `bx target` command to change regions.  For example, if you want to switch to the United Kingdom region, and space `staging`:
```
bx target -r eu-gb -s staging
```
{: pre}


If you need to change spaces within the same region, like from staging to production, run the following command:
```
bx target -s production
```
{: pre}


For more information about the `target` command, use `bx target --help` or review the following documentation:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migration}

With the introduction of the {{site.data.keyword.openwhisk_short}} CLI plug-in, the OpenWhisk CLI is no longer needed.

### Command Syntax
{: #command_syntax}

All `wsk` commands, except the `wsk bluemix login` command that is no longer needed, work the same way by using the command `bx wsk`.  All command options and arguments are the same.

### API Authentication and Host
{: #api_authentication}

The OpenWhisk CLI requires you to configure the authentication API key and the API host. 
With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you do not need to explicitly configure the API key and API host. You need to log in with `bx login` and target your region and namespace by using the `bx target` command. Afterwards, all subsequent commands begin with `bx wsk`. Authenticating in this manner also avoids accessing the {{site.data.keyword.Bluemix_notm}} web page to get the specific information to configure your CLI environment.

If you need to get the authentication API key for {{site.data.keyword.openwhisk_short}} to be used from an external http client such as cURL or Postman, you can retrieve them with the following commands:

To get the current API key, run the following command:
```
bx wsk property get --auth
```
{: pre}


To get the current API host, run the following command:
```
bx wsk property get --apihost
```
{: pre}


The API key is specific per region, organization, and space targeted by the {{site.data.keyword.openwhisk_short}} CLI plug-in.
{: tip}

### API Gateway authentication
{: #apigw_authentication}

The OpenWhisk CLI currently requires you to run the `wsk bluemix login` to be able to configure the API Gateway authorization for management of your APIs by using the `wsk api` command.

With the {{site.data.keyword.openwhisk}} CLI plug-in, there is no longer a need to run `wsk bluemix login`. Instead, use the `bx login` command to log in to {{site.data.keyword.Bluemix_notm}}, and the {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information. Now you can manage your APIs by using the `bx wsk api` command.

### Migrating deployment scripts
{: #migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the `wsk` binary, all commands work the same way by using the command `bx wsk`. You can modify your scripts to use the {{site.data.keyword.Bluemix_notm}} CLI plug-in, or create an alias or wrapper so that current executions of `wsk` get translated to `bx wsk`. The `bx login` and `bx target` commands in the {{site.data.keyword.Bluemix_notm}} CLI work in unattended mode. With unattended mode, you can configure your environment before executing `bx wsk` commands to deploy, and manage your {{site.data.keyword.openwhisk_short}} entities.


## Version history
{: #version_history}

A historical record of versions that show highlights and bug fixes.

1.0.7 (2018-02-02)
* `bx wsk api` now accepts path parameters such as `/api/{id}`. For info, see [API Gateway](./openwhisk_apigateway.html)
* Restore proxy support.
* Remove swift:3.
* Add s390x architecture support.
* Add i18n process to build.gradle.

1.0.6 (2018-01-30)
* Bug fixed for command `bx wsk service bind` for actions inside a package.