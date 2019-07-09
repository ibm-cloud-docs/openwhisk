---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: functions cli, serverless, cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Installing the CLI and plug-in
{: #cli_install}

{{site.data.keyword.openwhisk}} offers a powerful plug-in for the {{site.data.keyword.cloud_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system. You can use the {{site.data.keyword.openwhisk_short}} CLI plug-in to manage your code snippets in actions, create triggers and rules to enable your actions to respond to events, and bundle actions into packages.
{:shortdesc}


## Setting up the {{site.data.keyword.cloud_notm}} CLI
{: #cli_setup}

Before you begin, you must create an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/){: external}.
{: note}

Download and install the {{site.data.keyword.cloud_notm}} CLI, and log in.
{: shortdesc}

1. Download and install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. To specify an {{site.data.keyword.cloud_notm}} region, [include the API endpoint](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

3. Follow the prompts to select your {{site.data.keyword.cloud_notm}} account.

## Setting up the {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_plugin_setup}

To work with {{site.data.keyword.openwhisk_short}}, download and install the CLI plug-in.
{: shortdesc}

You can use the {{site.data.keyword.openwhisk_short}} CLI plug-in to perform the following tasks.

* Run your code snippets, or actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking actions](/docs/openwhisk?topic=cloud-functions-actions).
* Create triggers and rules to enable your actions to respond to events. See [Creating triggers and rules](/docs/openwhisk?topic=cloud-functions-triggers).
* Bundle actions and configure external events sources. See [Create and use packages](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Explore the catalog of packages and enhance your applications with external services. See [Adding {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services).

Already have the plug-in but need to update? Run `ibmcloud plugin update cloud-functions`.
{:tip}

Complete the following steps to install the {{site.data.keyword.openwhisk_short}} CLI plug-in

1. Install the {{site.data.keyword.openwhisk_short}} plug-in.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Verify that the plug-in is installed.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **Output**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

After logging in, all {{site.data.keyword.openwhisk_short}} commands begin with `ibmcloud fn`. To see everything that you can do with the {{site.data.keyword.openwhisk_short}} plug-in, run `ibmcloud fn` with no arguments.
{: note}

## Selecting regions, organizations, and spaces
{: #cli_regions}
By default, {{site.data.keyword.openwhisk_short}} uses [IAM-enabled namespaces](/docs/iam?topic=iam-iamoverview){: external}. You can no longer create Cloud Foundry-based namespaces.
{: important}

If you are already logged in, you can run the `ibmcloud fn property set` or `ibmcloud target` command in the {{site.data.keyword.cloud_notm}} CLI plug-in to switch regions, organization, and spaces.

Create or target a namespace.

  * Create an IAM-enabled namespace.
    ```
    ibmcloud fn namespace create <namespace_name> [--description <"description">]
    ```
    {: pre}


    **Response**
    ```
    ok: created namespace <namespace_name>
    ```
    {: screen}

  * Target an IAM-enabled namespace. 
    ```
    ibmcloud fn property set --namespace <namespace_name>
    ``` 
    {: pre}


    **Response**
    ```
    ok: whisk namespace set to <namespace_name>
    ```
    {: screen}
  
  * Target a Cloud Foundry-based namespace. You can use the `-o` and `-s` flags to target a specifc `org` and `space`, or you can follow the prompts.

    Include the `org` and `space` names in the `target` command.
    ```
    ibmcloud target --cf  -o <org> -s <space>
    ```
    {: pre}

    Target Cloud Foundry and follow the prompts to select a `org` and `space`.
    ```
    ibmcloud target --cf
    ```
    {: pre}


    **Response**
    ```
    Targeted Cloud Foundry (https://api.ng.bluemix.net)

    Targeted org <org_name>

    Targeted space <space_name>
                          
    API endpoint:      https://cloud.ibm.com   
    Region:            us-south   
    User:              <email>   
    Account:           (<account_id>) <-> <account>   
    Resource group:    default   
    CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
    Org:               <org_name>   
    Space:             <space_name>  
    ```
    {: screen} 

**Example** Creating namespaces for staging and production deployments.

You can create IAM-enabled namespaces to handle your pre-production (staging) and production {{site.data.keyword.openwhisk_short}} deployments by creating namespaces for each. Run [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) to create more namespaces under your organization such as "staging" and "production":

Create a staging namespace.
```
ibmcloud fn namespace create staging
```
{: pre}

Create a production namespace.
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} has restrictions on namespace names. For more information, see the [System details and Limits](/docs/openwhisk?topic=cloud-functions-limits#limits_entities) documentation.
{: tip}


## Configuring the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be set  up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format: `{PROXY IP}:{PROXY PORT}`.

Changing the name of the org or space creates a new namespace based on the changed name. The entities in the old namespace are not visible in the new namespace and might be deleted.
{: important}


## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migrate}

You can now use the {{site.data.keyword.openwhisk_short}} CLI plug-in to interact with {{site.data.keyword.openwhisk_short}} entities. Although you can continue to use the stand-alone OpenWhisk CLI, it does not have the latest features that are supported by {{site.data.keyword.openwhisk_short}}, such as IAM-based namespaces and `service bind`.
{: shortdesc}


### Command Syntax
{: #cli_syntax}
For {{site.data.keyword.openwhisk_short}} command syntax, see the [{{site.data.keyword.openwhisk_short}} CLI reference](/docs/openwhisk/topic=cloud-functions-cli-plugin-functions-cli).


### API Authentication and Host
{: #cli_api_auth}

With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you don't need to explicitly configure the API key and API host. Instead, you can log in with `ibmcloud login`. You can target an IAM-enabled namespace by running `ibmcloud fn property set --namespace <namespace_name>` or a Cloud Foundry-based namespace by running `ibmcloud target --cf`. After logging in, all commands begin with `ibmcloud fn`.


If you need to use the authentication API key for {{site.data.keyword.openwhisk_short}} in an external HTTP client such as cURL or Postman, you can retrieve it with the following commands.

Get the current API key by running the following command.
```
ibmcloud fn property get --auth
```
{: pre}

Get the current API host by running the following command.
```
ibmcloud fn property get --apihost
```
{: pre}

The API key is specific per region, organization, and space targeted by the {{site.data.keyword.openwhisk_short}} CLI plug-in.
{: tip}


### API Gateway authentication
{: #cli_apigw_authentication}

The OpenWhisk CLI required you to run the `wsk ibmcloud login` to be able to configure the API Gateway authorization for management of your APIs by using the `wsk api` command. With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you don't need to run `wsk ibmcloud login`. Instead, when you use the `ibmcloud login` command to log in to {{site.data.keyword.cloud_notm}}, the {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information. Now you can manage your APIs by using the `ibmcloud fn api` command.


### Migrating deployment scripts
{: #cli_migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the `wsk` commands, all commands work the same way by using the command `ibmcloud fn`. You can modify your scripts to use the {{site.data.keyword.cloud_notm}} CLI plug-in, or create an alias or wrapper so that current commands using `wsk` are translated to `ibmcloud fn`. The `ibmcloud login` and `ibmcloud target` commands in the {{site.data.keyword.cloud_notm}} CLI work in unattended mode. With unattended mode, you can configure your environment before you run `ibmcloud fn` commands to deploy and manage your {{site.data.keyword.openwhisk_short}} entities.


## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migrate_wsk}

The {{site.data.keyword.openwhisk_short}} CLI plug-in replaces the OpenWhisk stand-alone CLI (`wsk`) for interacting with {{site.data.keyword.openwhisk_short}} entities in IBM Cloud. The `wsk` stand-alone CLI does not have the latest features that are supported by {{site.data.keyword.openwhisk_short}}, such as IAM-based namespace support and `service bind` support.
{: shortdesc}

**What does the command syntax look like?**

All of the command options and arguments for commands in the Cloud Functions CLI plug-in are the same as the options for the [OpenWhisk stand-alone CLI ](https://github.com/apache/incubator-openwhisk-cli){: external}. But, note the following differences.

* The {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information.
* `wsk` commands are now run as `ibmcloud fn`.
* The `wsk ibmcloud login` command is no longer needed. You can sign in by using `ibmcloud login`.
* You can manage your APIs by using the `ibmcloud fn api`.

For more information, see the [{{site.data.keyword.openwhisk_short}} CLI reference](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli).

**Do I still need to configure an API key?**

With the {{site.data.keyword.openwhisk_short}} CLI plug-in, you don't need to explicitly configure an API key and host like you previously needed to. Instead, you can log in and target your region with `ibmcloud login -a <region_api_endpoint>` and then target your namespace to start working with your entities.

If you're using the {{site.data.keyword.openwhisk_short}} authentication API key in an external HTTP client such as cURL, you can retrieve it with the following commands.

Get the current API key.
```
ibmcloud fn property get --auth
```
{: pre}

Get the current API host.
```
ibmcloud fn property get --apihost
```
{: pre}

The API key is specific per region, organization, and space targeted by the {{site.data.keyword.openwhisk_short}} CLI plug-in.
{: tip}


## CLI version history
{: #cli_versions}

A historical record of versions that show highlights and bug fixes.

v1.0.30 (03 April 2019)
* Improved the `service bind` handling of IAM and org and space-based services.
* Added a fix for handling API endpoint https://cloud.ibm.com.

v1.0.29 (06 February 2019)
* Added commands `deploy` and `undeploy` to deploy or undeploy a collection of Functions entities via a manifest file. For more information, see the [Deployment](/docs/openwhisk?topic=cloud-functions-deploy#deploy) documentation.

v1.0.28 (21 January 2019)
* Added an error message when `update|delete|get namespace name` exists multiple times.

v1.0.27 (11 December 2018)
* Added `namespace get` fixes.
* Added a fix for `--save-as` when an action is a black box action.

v1.0.26 (30 November 2018)
* Enabled `fn property get --auth` to correctly return the auth key in a new environment.

v1.0.25 (23 November 2018)
* Improved error message result display.
* Added a `fn namespace get` fix to correctly display namespace properties.

1.0.23 (15 October 2018)
* Added support for ruby (`.rb`) action code recognition.

1.0.22 (20 August 2018)
* Added us-east region support.

1.0.21 (01 August 2018)
* Aliases `fn` and `functions` can now be used for {{site.data.keyword.openwhisk_short}} commands: `ibmcloud fn <command>` and `ibmcloud fn <command>`. You can also still use `ibmcloud wsk <command>`.

1.0.19 (02 July 2018)
* Minor bug fixes and improvements.

1.0.18 (20 June 2018)
* Added a fix for unbinding user-provided service instances.
* Performance improvements.

1.0.17 (12 June 2018)
* Added support for binding (`ibmcloud wsk service bind`) and unbinding (`ibmcloud wsk service unbind`) user-provided service instances that are created by using the `ibmcloud cf create-user-provided-service` command.

1.0.16 (24 May 2018)
* Minor bug fixes and improvements.

1.0.15 (21 May 2018)
* Minor bug fixes and improvements.

1.0.14 (17 May 2018)
* Enabled support for the `&` character in org and space names.

1.0.13 (07 May 2018)
* Minor bug fixes and error handling improvements.

1.0.12 (30 April 2018)
* {{site.data.keyword.cloud_notm}} SDK updates to maintain `bx` CLI compatibility.

1.0.11 (23 April 2018)
* Minor bug fixes and improvements.

1.0.10 (09 April 2018)
* Added new `--web-secure` option to the `ibmcloud wsk action create|update` commands to secure web action endpoints.
* Fixed back-to-back path parameter [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external}.

1.0.9 (16 March 2018)
* Enabled support for service bind at the package level.

1.0.8 (22 February 2018)
* Enabled support for IAM service bind.

1.0.7 (02 February 2018)
* Updated `ibmcloud wsk api` to accept path parameters such as `/api/{id}`. For more information, see [API Gateway](/docs/openwhisk?topic=cloud-functions-apigateway).
* Restored proxy support.
* Removed `swift:3`.

1.0.6 (30 January 2018)
* Fixed a bug with the command `ibmcloud wsk service bind` for actions inside a package.

