---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} offers a powerful plug-in for the {{site.data.keyword.Bluemix_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system.
{: shortdesc}

## Setting up the {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

Download and install the [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

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

## Using services from Actions
{: #binding_services}

{{site.data.keyword.openwhisk_short}} provides the `service bind` command to make your {{site.data.keyword.Bluemix_notm}} service credentials available to your code at run time. The `service bind` command can then be leveraged to allow you to bind any {{site.data.keyword.Bluemix_notm}} service to any Action that is defined in {{site.data.keyword.openwhisk_short}}.

For detailed steps on how to use services from Actions, see the [Using services from Actions](./binding_services.html) topic.

## Configure the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_https_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be set  up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format:
`{PROXY IP}:{PROXY PORT}`.

## Working with regions, organizations and spaces
{: #region_info}

If you are already logged in, you can run the `bx target` command in the {{site.data.keyword.Bluemix_notm}} CLI to switch regions, organization, and spaces.

Use the following command to display all of the {{site.data.keyword.Bluemix_notm}} regions.

{{site.data.keyword.openwhisk_short}} is supported in the following regions: `us-south`, `eu-gb`, and `eu-fra`.
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

You can create spaces to handle your pre-production (staging) and production deployments by creating spaces for each. Creating spaces allows {{site.data.keyword.openwhisk_short}} to have two different namespaces that are defined for you.

You can use the `bx iam space-create` to create more spaces under your organization such "staging" and "production". See the following example:
```
bx iam space-create "staging"
bx iam space-create "production"
```

For more information about the `bx iam` command, refer to the following documentation: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} has restrictions on the namespace names. For more information about these restrictions, refer to the following documentation: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

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

1.0.8 (2018-02-22)
* Added support for IAM service bind.

1.0.7 (2018-02-02)
* `bx wsk api` now accepts path parameters such as `/api/{id}`. For info, see [API Gateway](./openwhisk_apigateway.html).
* Restore proxy support.
* Remove `swift:3`.

1.0.6 (2018-01-30)
* Bug fixed for command `bx wsk service bind` for actions inside a package.