---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-29"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Setting up the {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}} offers a powerful plug-in for the {{site.data.keyword.Bluemix_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system. You can use the {{site.data.keyword.openwhisk_short}} CLI plug-in to manage your code snippets in actions, create triggers and rules to enable your actions to respond to events, and bundle actions into packages.
{:shortdesc}

You can now use the alias `fn` in your {{site.data.keyword.openwhisk_short}} plug-in commands: `ibmcloud fn <command>`
{: tip}

## Setting up the {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

Download and install the {{site.data.keyword.Bluemix_notm}} CLI, and log in.
{: shortdesc}

1. Download and install the [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/bluemix_cli/download_cli.html).

2. Log in to the {{site.data.keyword.Bluemix_notm}} CLI. {{site.data.keyword.openwhisk_short}} is available in the US South, US East, Germany, United Kingdom, and Tokyo {{site.data.keyword.Bluemix_notm}} regions. If the {{site.data.keyword.Bluemix_notm}} API endpoint is not specified, specify one with the `-a` flag.

  ```
  ibmcloud login -a <endpoint>
  ```
  {: pre}

  <table>
    <tr>
      <th>Region</th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>US South</td>
      <td><code>api.us-south.functions.cloud.ibm.com</code></td>
    </tr>
     <tr>
      <td>US South - Stage 1</td>
      <td><code>api.us-south.functions.test.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>US East</td>
      <td><code>api.us-east.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>UK South</td>
      <td><code>api.eu-gb.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>EU Central</td>
      <td><code>api.eu-de.functions.cloud.ibm.com</code></td>
    </tr>
    <tr>
      <td>Tokyo</td>
      <td><code>api.jp-tok.functions.cloud.ibm.com</code></td>
    </tr>
  </table>

  You can specify the organization and space when you log in to skip the prompts for them by using the following flags: `ibmcloud login -a <endpoint> -o <ORG> -s <SPACE>`
  {: tip}

3. If you did not specify an org and space, complete the prompts that follow the login command.


## Setting up the {{site.data.keyword.openwhisk_short}} plug-in
{: #cloudfunctions_plugin_setup}

To work with {{site.data.keyword.openwhisk_short}}, download and install the CLI plug-in.
{: shortdesc}

You can use the plug-in to:

* Run your code snippets, or actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking actions](/docs/openwhisk/openwhisk_actions.html).
* Use triggers and rules to enable your actions to respond to events. See [Creating triggers and rules](/docs/openwhisk/openwhisk_triggers_rules.html).
* Learn how packages bundle actions and configure external events sources. See [Create and use packages](/docs/openwhisk/openwhisk_packages.html).
* Explore the catalog of packages and enhance your applications with external services, such as a [{{site.data.keyword.cloudant}} event source](/docs/openwhisk/openwhisk_cloudant.html).

To see everything that you can do with the {{site.data.keyword.openwhisk_short}} plug-in, run `ibmcloud fn` with no arguments.
{: tip}

1. Install the {{site.data.keyword.openwhisk_short}} plug-in.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Verify that the plug-in is installed.

  ```
  ibmcloud plugin list cloud-functions
  ```
  {: pre}

  Output:
  ```
  Plugin Name          Version
  Cloud-Functions      1.0.16
  ```
  {: screen}

Already have the plug-in but need to update? Run `ibmcloud plugin update cloud-functions`.
{:tip}



## Using services from actions
{: #binding_services}

{{site.data.keyword.openwhisk_short}} provides the `service bind` command to make your {{site.data.keyword.Bluemix_notm}} service credentials available to your code at run time. You can then use the `service bind` command to bind any {{site.data.keyword.Bluemix_notm}} service to any action that is defined in {{site.data.keyword.openwhisk_short}}.

For detailed steps on how to use services from actions, see [Binding services to actions](./binding_services.html).


## Configuring the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_https_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be set  up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format: `{PROXY IP}:{PROXY PORT}`.



## Switching to different regions, organizations, and spaces
{: #region_info}

If you are already logged in, you can run the `ibmcloud target` command in the {{site.data.keyword.Bluemix_notm}} CLI to switch regions, organization, and spaces.


To create and manage entities, you must target a namespace. The default namespace, which can be denoted by an underscore (`_`) in some situations, corresponds to the Cloud Foundry-based namespace that is currently targeted.

You can create spaces to handle your pre-production (staging) and production deployments by creating spaces for each. Creating spaces allows {{site.data.keyword.openwhisk_short}} to have two different namespaces that are defined for you. Run [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) to create more spaces under your organization such "staging" and "production":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} has restrictions on namespace names. For more information, refer to the [System details and Limits](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities) documentation.
{: tip}

**Warning**: Changing the name of the org or space creates a new namespace based on the changed name. The entities in the old namespace are not visible in the new namespace and might be deleted.


## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migration}

You can now use the {{site.data.keyword.openwhisk_short}} CLI plug-in to interact with {{site.data.keyword.openwhisk_short}} entities. Although you can continue to use the stand-alone OpenWhisk CLI, it will not have the latest features that are supported by {{site.data.keyword.openwhisk_short}}, such as IAM-based namespaces and `service bind`.
{: shortdesc}

### Command Syntax
{: #command_syntax}

All `wsk` commands, except the `wsk bluemix login` command that is no longer needed, work the same way by using the command `ibmcloud fn`. All command options and arguments for commands in the Cloud Functions CLI plug-in are the same as the commands for the OpenWhisk stand-alone CLI. For more information, see the [Apache OpenWhisk CLI project ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-cli).

### API Authentication and Host
{: #api_authentication}

The OpenWhisk CLI required you to configure the authentication API key and the API host. With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you don't need to explicitly configure the API key and API host. Instead, you can log in with `ibmcloud login` and target your region and namespace by using the `ibmcloud target` command. After logging in, all commands begin with `ibmcloud fn`.


If you need to use the authentication API key for {{site.data.keyword.openwhisk_short}} in an external HTTP client such as cURL or Postman, you can retrieve it with the following commands:

To get the current API key:
```
ibmcloud fn property get --auth
```
{: pre}

To get the current API host:
```
ibmcloud fn property get --apihost
```
{: pre}

The API key is specific per region, organization, and space targeted by the {{site.data.keyword.openwhisk_short}} CLI plug-in.
{: tip}

### API Gateway authentication
{: #apigw_authentication}

The OpenWhisk CLI required you to run the `wsk bluemix login` to be able to configure the API Gateway authorization for management of your APIs by using the `wsk api` command. With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you don't need to run `wsk bluemix login`. Instead, when you use the `ibmcloud login` command to log in to {{site.data.keyword.Bluemix_notm}}, the {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information. Now you can manage your APIs by using the `ibmcloud fn api` command.

### Migrating deployment scripts
{: #migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the `wsk` binary, all commands work the same way by using the command `ibmcloud fn`. You can modify your scripts to use the {{site.data.keyword.Bluemix_notm}} CLI plug-in, or create an alias or wrapper so that current commands using `wsk` are translated to `ibmcloud fn`. The `ibmcloud login` and `ibmcloud target` commands in the {{site.data.keyword.Bluemix_notm}} CLI work in unattended mode. With unattended mode, you can configure your environment before you run `ibmcloud fn` commands to deploy and manage your {{site.data.keyword.openwhisk_short}} entities.





## CLI version history
{: #version_history}

A historical record of versions that show highlights and bug fixes.

v1.0.28 (2019-01-21)
* Return an error when `update|delete|get namespace name` exists multiple times.

v1.0.27 (2018-12-11)
* `namespace get` fixes
* `--save-as` fix when action is a black box action
* addition of `--concurrency` flag for action create and action update commands

v1.0.26 (2018-11-30)
* `fn property get --auth` correctly returns the auth key in a new environment

v1.0.25 (2018-11-23)
* improve error message result display
* `fn namespace get` fix to correctly display namespace properties

1.0.23 (2018-10-15)
* Add support for ruby (.rb) action code recognition

1.0.22 (2018-08-20)
* Add us-east region support

1.0.21 (2018-08-01)
* Aliases `fn` and `functions` can now be used for {{site.data.keyword.openwhisk_short}} commands: `ibmcloud fn <command>` and `ibmcloud fn <command>`. You can also still use `ibmcloud wsk <command>`.

1.0.19 (2018-07-02)
* Minor bug fixes and improvements.

1.0.18 (2018-06-20)
* Fix for unbinding user-provided service instances.
* Performance improvements.

1.0.17 (2018-06-12)
* Add support for binding (`ibmcloud wsk service bind`) and unbinding (`ibmcloud wsk service unbind`) user-provided service instances that are created by using the `ibmcloud cf create-user-provided-service` command.

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
* Add new `--web-secure` option to the `ibmcloud wsk action create|update` commands to secure web action endpoints.
* Fix back-to-back path parameter [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237).

1.0.9 (2018-03-16)
* Enable support for service bind at the package level.

1.0.8 (2018-02-22)
* Enable support for IAM service bind.

1.0.7 (2018-02-02)
* `ibmcloud wsk api` now accepts path parameters such as `/api/{id}`. For info, see [API Gateway](./openwhisk_apigateway.html).
* Restore proxy support.
* Remove `swift:3`.

1.0.6 (2018-01-30)
* Bug fix for command `ibmcloud wsk service bind` for actions inside a package.
