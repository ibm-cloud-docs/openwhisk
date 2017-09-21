---

copyright:
  years: 2016, 2017
lastupdated: "2017-09-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} stand-alone CLI

The {{site.data.keyword.openwhisk_short}} distributed **wsk** command line interface allows management of all {{site.data.keyword.openwhisk_short}} entities. 
{: shortdesc}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}

* Configuration of the {{site.data.keyword.openwhisk_short}} authentication key
  * No need to access the {{site.data.keyword.openwhisk_short}} console to obtain the authentication key
  * Automatic authentication key generation occurs when switching between regions, orgs, and spaces
* Refreshing an expired {{site.data.keyword.openwhisk_short}} authentication key
* Updating the CLI to a later version


The following management tasks require you to use the plugin.

* API management
  * Configuration of the API GW access token
  * Refreshing an expired API GW access token

## Setting up the {{site.data.keyword.openwhisk_short}} CLI 
{: #openwhisk_cli_setup}

You can use the {{site.data.keyword.openwhisk_short}} command line interface (CLI) to set up your namespace and authorization key.
Go to [Configure CLI](https://console.{DomainName}/openwhisk/cli) and follow the instructions to install it.

First, configure the following two properties:

1. **API host** (name or IP address) for the {{site.data.keyword.openwhisk_short}} deployment you want to use.
2. **Authorization key** (username and password) which grants you access to the {{site.data.keyword.openwhisk_short}} API.

Two {{site.data.keyword.Bluemix_notm}} regions are available that require their own unique API host and Authorization key.

* US South
  * API host: `openwhisk.ng.bluemix.net`

* United Kingdom
  * API host: `openwhisk.eu-gb.bluemix.net`

Run the following command to set the API host for your desired {{site.data.keyword.Bluemix_notm}} region:

US South:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

United Kingdom:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

If you ever need to switch regions, you must reconfigure the CLI with both the API host and authorization key as the authorization key is specific per region.
{: tip}

Artifacts such as actions, rules, and packages are region-specific. So if you use the same artifact in multiple regions you must deploy it to each desired region.

If you know your authorization key, you can configure the CLI to use it. 

Run the following command to set the Authorization key:

```
wsk property set --auth <authorization_key>
```
{: pre}

**Tip:** The {{site.data.keyword.openwhisk_short}} CLI stores the properties set in `~/.wskprops` by default. The location of this file can be altered by setting the `WSK_CONFIG_FILE` environment variable. 

To verify your CLI setup, try [creating and running an action](./index.html#openwhisk_start_hello_world).

## Using the {{site.data.keyword.openwhisk_short}} CLI
{: #using_openwhisk_cli}

Once the environment is configured, you can use the {{site.data.keyword.openwhisk_short}} CLI to perform the following tasks:

* Run your code snippets, or actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking actions](./openwhisk_actions.html).
* Use triggers and rules to enable your actions to respond to events. See [Creating triggers and rules](./openwhisk_triggers_rules.html).
* Learn how packages bundle actions and configure external events sources. See [Using and creating packages](./openwhisk_packages.html).
* Explore the catalog of packages and enhance your applications with external services, such as a [Cloudant event source](./openwhisk_cloudant.html). See [Pre-installed {{site.data.keyword.openwhisk_short}} packages](./openwhisk_catalog.html).

## Configure the CLI to use an HTTPS proxy
{: #cli_https_proxy}

The CLI can be set up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format: `{PROXY IP}:{PROXY PORT}`.
