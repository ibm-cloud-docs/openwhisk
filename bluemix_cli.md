---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Setting up the {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} offers a powerful plug-in for the {{site.data.keyword.Bluemix_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system.
{: shortdesc}

## Setting up the {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

Download and install the {{site.data.keyword.Bluemix_notm}} CLI, and log in.
{: shortdesc}

1. Download and install the [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

2. Log in to the {{site.data.keyword.Bluemix_notm}} CLI. {{site.data.keyword.openwhisk_short}} is available in the US South and United Kingdom {{site.data.keyword.Bluemix_notm}} regions. If the {{site.data.keyword.Bluemix_notm}} API endpoint is not specified, specify one with the `-a` flag.

    * To log in to the US South region:
      ```
      bx login -a api.ng.bluemix.net
      ```
      {: pre}

    * To log in to the United Kingdom region:
      ```
      bx login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  You can use the `bx api` command to explicitly set the {{site.data.keyword.Bluemix_notm}} API endpoint. To display the current API endpoint setting, use the `bx target` command.
  {: tip}

3. The `bx login` command prompts you for information such as organization, space, and password if not specified.

  You can specify the organization and space when you log in to skip the prompts for them. Use the following flags: `bx login -o <ORG> -s <SPACE>`.
  {: tip}

You can also use an {{site.data.keyword.Bluemix_notm}} API key to log in. This method is useful when your account is configured with a Federated Login requiring you to log in with the flag `--sso`. [Using an API key](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key) is also beneficial if you want to set up Continuous Integration (CI), and want to configure an unattended pipeline.

1. Create a new API key.
    ```
    bx iam api-key-create MyKey
    ```
    {: pre}

2. Use the generated value of the API key to log in.
    ```
    bx login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
For more information about the `bx login` command, use `bx login --help` or review the [IBM Cloud (bx) commands](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login) topic.

## Setting up the {{site.data.keyword.openwhisk_short}} plug-in
{: #cloudfunctions_plugin_setup}

Download and install the {{site.data.keyword.openwhisk_short}} plug-in.
{: shortdesc}

1. Install the {{site.data.keyword.openwhisk_short}} plug-in.
    ```
    bx plugin install cloud-functions
    ```
    {: pre}

2. Verify that the plug-in is installed.
    ```
    bx plugin list cloud-functions
    ```
    {: pre}

    Output:
    ```
    Plugin Name          Version
    Cloud-Functions      1.0.16
    ```
    {: screen}

3. Perform a blocking (synchronous) invocation of `echo`. Pass `hello` as an argument.
    ```
    bx wsk action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. Verify that the `hello` message is returned in the output.
    Example output:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. You can upgrade the {{site.data.keyword.openwhisk_short}} plug-in by running the following command:
    ```
    bx plugin update Cloud-Functions
    ```
    {: pre}

You can use the {{site.data.keyword.openwhisk_short}} CLI to perform the following tasks:

* Run your code snippets, or Actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking Actions](./openwhisk_actions.html).
* Use Triggers and Rules to enable your Actions to respond to events. See [Creating Triggers and Rules](./openwhisk_triggers_rules.html).
* Learn how packages bundle Actions and configure external events sources. See [Create and use packages](./openwhisk_packages.html).
* Explore the catalog of packages and enhance your applications with external services, such as a [Cloudant event source](./openwhisk_cloudant.html).

To list of commands for the {{site.data.keyword.openwhisk_short}} plug-in, run `bx wsk` with no arguments.
{: tip}

## Using services from Actions
{: #binding_services}

{{site.data.keyword.openwhisk_short}} provides the `service bind` command to make your {{site.data.keyword.Bluemix_notm}} service credentials available to your code at run time. The `service bind` command can then be leveraged to allow you to bind any {{site.data.keyword.Bluemix_notm}} service to any Action that is defined in {{site.data.keyword.openwhisk_short}}.

For detailed steps on how to use services from Actions, see [Binding services to Actions](./binding_services.html).

## Configuring the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_https_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be set  up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format:
`{PROXY IP}:{PROXY PORT}`.

## Switching to different regions, organizations, and spaces
{: #region_info}

If you are already logged in, you can run the `bx target` command in the {{site.data.keyword.Bluemix_notm}} CLI to switch regions, organization, and spaces.

{{site.data.keyword.openwhisk_short}} is available in the US South and United Kingdom {{site.data.keyword.Bluemix_notm}} regions. To change regions, use the `bx target` command. For example, to switch to the United Kingdom region, and to the space `staging` in that region:
```
bx target -r eu-gb -s staging
```
{: pre}

You can create spaces to handle your pre-production (staging) and production deployments by creating spaces for each. Creating spaces allows {{site.data.keyword.openwhisk_short}} to have two different namespaces that are defined for you. Run [`bx iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) to create more spaces under your organization such "staging" and "production":

```
bx iam space-create "staging"
bx iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} has restrictions on namespace names. For more information, refer to the [System details and Limits](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities) documentation.
{: tip}

## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migration}

With the introduction of the {{site.data.keyword.openwhisk_short}} CLI plug-in, the OpenWhisk stand-alone CLI is no longer needed.

### Command Syntax
{: #command_syntax}

All `wsk` commands, except the `wsk bluemix login` command that is no longer needed, work the same way by using the command `bx wsk`.  All command options and arguments are the same.

### API Authentication and Host
{: #api_authentication}

The OpenWhisk CLI required you to configure the authentication API key and the API host.
With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you do not need to explicitly configure the API key and API host. Instead, you can log in with `bx login` and target your region and namespace by using the `bx target` command. After logging in, all subsequent commands begin with `bx wsk`.

If you need to use the authentication API key for {{site.data.keyword.openwhisk_short}} in an external HTTP client such as cURL or Postman, you can retrieve it with the following commands:

To get the current API key:
```
bx wsk property get --auth
```
{: pre}

To get the current API host:
```
bx wsk property get --apihost
```
{: pre}

The API key is specific per region, organization, and space targeted by the {{site.data.keyword.openwhisk_short}} CLI plug-in.
{: tip}

### API Gateway authentication
{: #apigw_authentication}

The OpenWhisk CLI required you to run the `wsk bluemix login` to be able to configure the API Gateway authorization for management of your APIs by using the `wsk api` command. With the {{site.data.keyword.openwhisk_short}} CLI plug-in, there is no need to run `wsk bluemix login`. Instead, when you use the `bx login` command to log in to {{site.data.keyword.Bluemix_notm}}, the {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information. Now you can manage your APIs by using the `bx wsk api` command.

### Migrating deployment scripts
{: #migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the `wsk` binary, all commands work the same way by using the command `bx wsk`. You can modify your scripts to use the {{site.data.keyword.Bluemix_notm}} CLI plug-in, or create an alias or wrapper so that current executions of `wsk` get translated to `bx wsk`. The `bx login` and `bx target` commands in the {{site.data.keyword.Bluemix_notm}} CLI work in unattended mode. With unattended mode, you can configure your environment before executing `bx wsk` commands to deploy, and manage your {{site.data.keyword.openwhisk_short}} entities.

## Version history
{: #version_history}

A historical record of versions that show highlights and bug fixes.

1.0.16 (2018-05-24)
* Minor bug fixes and improvements.

1.0.15 (2018-05-21)
* Minor bug fixes and improvements.

1.0.14 (2018-05-17)
* Enable support for the `&` character in org and space names.

1.0.13 (2018-05-07)
* Minor bug fixes and error handling improvements.

1.0.12 (2018-04-30)
* {{site.data.keyword.Bluemix_notm}} SDK updates to maintain `bx` CLI compatibility.

1.0.11 (2018-04-23)
* Minor bug fixes and improvements.

1.0.10 (2018-04-09)
* Add new `--web-secure` option to the `bx wsk action create|update` commands to secure web action endpoints.
* Fix back-to-back path parameter [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237).

1.0.9 (2018-03-16)
* Enable support for service bind at the package level.

1.0.8 (2018-02-22)
* Enable support for IAM service bind.

1.0.7 (2018-02-02)
* `bx wsk api` now accepts path parameters such as `/api/{id}`. For info, see [API Gateway](./openwhisk_apigateway.html).
* Restore proxy support.
* Remove `swift:3`.

1.0.6 (2018-01-30)
* Bug fix for command `bx wsk service bind` for actions inside a package.
