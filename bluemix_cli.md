---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} offers a powerful plug-in for the {{site.data.keyword.Bluemix_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system.

## Setting up the {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

Download and install the {{site.data.keyword.Bluemix_notm}} CLI here: https://console.bluemix.net/docs/cli/reference/bluemix_cli/index.html

## Setting up the {{site.data.keyword.openwhisk_short}} plug-in
{: #cloudfunctions_plugin_setup}

To install the {{site.data.keyword.openwhisk_short}} plug-in, run the following command:
```
bx plugin install cloud-functions -r Bluemix
```
{: pre}


To validate the {{site.data.keyword.openwhisk_short}} plug-in installed successfully, run the following command:
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


For more information about the plug-in command use `bx plugin --help` or check out the documentation
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}} CLI authentication
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} is currently available in two {{site.data.keyword.Bluemix_notm}} regions.

When you log into the {{site.data.keyword.Bluemix_notm}} CLI you can specify the {{site.data.keyword.Bluemix_notm}} API endpoint for the target region alongside the organization and space.

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


The `-a` flag specifies the {{site.data.keyword.Bluemix_notm}} API endpoint to use. If the API endpoint has already been specified, the `-a` option is not necessary. You can use the `bx api` command to explicitly set the {{site.data.keyword.Bluemix_notm}} API endpoint. To display the current API endpoint setting, use the `bx target` command."

The `login` command will prompt you for information such as organization, space, and password if not specified. You can specify the organization and space in the command line to skip the prompts for them.
```
bx login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


You can also use a {{site.data.keyword.Bluemix_notm}} API key to login. This is useful when your account is configured with a Federated Login requiring you to log in with the flag `--sso`. Using an API key is also useful when setting a Continue Integration (CI) and want to configure an unattended pipeline.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

You can create a new API key using the {{site.data.keyword.Bluemix_notm}} CLI like this: 
```
bx iam api-key-create MyKey
```
{: pre}


Then use the API key generated value when logging in like so:
```
bx login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


For more information about the login command use `bx login --help` or review the following documentation.
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Using the {{site.data.keyword.openwhisk_short}} plug-in
{: #cloudfunctions_plugin_usage}

Verify your setup. Here, we perform a blocking (synchronous) invocation of echo, passing it "hello" as an argument.
```
bx wsk action invoke /whisk.system/utils/echo -p message hello --result
```
{: pre}


See the following example output:
```
{
    "message":"hello"
}
```

After you have configured your environment, you can begin using the {{site.data.keyword.openwhisk_short}} CLI to do the following:

* Run your code snippets, or actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking actions](./openwhisk_actions.html).
* Use triggers and rules to enable your actions to respond to events. See [Creating triggers and rules](./openwhisk_triggers_rules.html).
* Learn how packages bundle actions and configure external events sources. See [Using and creating packages](./openwhisk_packages.html).
* Explore the catalog of packages and enhance your applications with external services, such as a [Cloudant event source](./openwhisk_cloudant.html). See [Using OpenWhisk-enabled services](./openwhisk_catalog.html).

To get a list of commands for the {{site.data.keyword.openwhisk_short}} plug-in run `bx wsk` with no arguments.

## Configure the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_https_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be setup to use an HTTPS proxy. To setup an HTTPS proxy, an environment variable called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port using the following format:
`{PROXY IP}:{PROXY PORT}`.


## Working with regions, organizations and spaces
{: #region_info}

You can create spaces to handle your pre-production (staging) and production deployments by creating spaces for each. This will allows {{site.data.keyword.openwhisk_short}} to have two different namespaces defined for you.

You can use the `bx iam space-create` to create more spaces under your organization such "staging" and "production". See the following example:
```
bx iam space-create "staging"
bx iam space-create "production"
```

For more information about using the `bx iam` command refer to the following documentation: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

Take into account that {{site.data.keyword.openwhisk_short}} has restrictions on the namespace names. For more information about these restrictions refer to the following documentation: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

If you are already logged in, using the `bx target` command in the {{site.data.keyword.Bluemix_notm}} CLI allows you to switch regions, organization, and spaces.

Use the following command to display all of the {{site.data.keyword.Bluemix_notm}} regions.
**Note:** {{site.data.keyword.openwhisk_short}} is currently only supported in the `us-south` and `eu-gb` regions.
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


For more information about the `target` command use `bx target --help` or review the following documentation:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migration}

With the introduction of the {{site.data.keyword.openwhisk_short}} CLI plug-in, the OpenWhisk CLI is deprecated, and you should start by using the {{site.data.keyword.Bluemix_notm}} CLI with the {{site.data.keyword.openwhisk_short}} CLI plug-in.

### Command Syntax
{: #command_syntax}

All `wsk` commands, with the exception of the `wsk bluemix login` command which is no longer needed, will work the same way using the command `bx wsk`.  All command options and arguments are the same.

### API Authentication and Host
{: #api_authentication}

The OpenWhisk CLI currently requires you to configure the authentication API key and the API host. 
With the {{site.data.keyword.openwhisk_short}} CLI plug-in, there is no need to explicitly configure the API key and Host. You need to log in with `bx login` and target your region and namespace using the `bx target` command. After this, all subsequent commands begin with `bx wsk`. This also avoids accessing the {{site.data.keyword.Bluemix_notm}} web page to get the specific information to configure your CLI environment.

If you need to get the authentication API key for {{site.data.keyword.openwhisk_short}} to be used from an external http client such as cURL or Postman, you can retrieve them with the following commands:

To get the current API key run the following command:
```
bx wsk property get --auth
```
{: pre}


To get the current API host run the following command:
```
bx wsk property get --apihost
```
{: pre}


**Note:** Take into account that the API key is specific per region, organization, and space being targeted in the {{site.data.keyword.openwhisk_short}} CLI plug-in.

### API Gateway authentication
{: #apigw_authentication}

The OpenWhisk CLI currently requires you to run the `wsk bluemix login` to be able to configure the API Gateway authorization for management of your APIs using the `wsk api` command.

With the {{site.data.keyword.openwhisk}} CLI plug-in, there is no longer a need to run `wsk bluemix login`, instead use the `bx login` to log in to {{site.data.keyword.Bluemix_notm}} and the {{site.data.keyword.openwhisk}} plug-in will be able to use your current login and target information to manage your APIs using the `bx wsk api` command.

### Migrating deployment scripts
{: #migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the `wsk` binary, all commands will work the same way using the command `bx wsk`. You can modify your scripts to use the {{site.data.keyword.Bluemix_notm}} CLI plug-in, or create an alias or wrapper that current executions of `wsk` get translated to `bx wsk`. The `bx login` and `bx target` commands in {{site.data.keyword.Bluemix_notm}} CLI work in unattended mode, and this will allow you to configure your environment before executing `bx wsk` commands to deploy and manage your {{site.data.keyword.openwhisk_short}} entities.
