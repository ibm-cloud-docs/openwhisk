---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} wsk CLI

The {{site.data.keyword.openwhisk_short}} **wsk** command line interface allows management of all {{site.data.keyword.openwhisk_short}} entities. 

**NOTE: ** IBM recommends you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI.](./bluemix_cli.html)
when managing {{site.data.keyword.openwhisk_short}} entities. 

## Setting up the {{site.data.keyword.openwhisk_short}} CLI 
{: #openwhisk_cli_setup}

You can use the {{site.data.keyword.openwhisk_short}} command line interface (CLI) to set up your namespace and authorization key.
Go to [Configure CLI](https://console.{DomainName}/openwhisk/cli) and follow the instructions to install it.

There are two required properties to configure in order to use the CLI:

1. **API host** (name or IP address) for the {{site.data.keyword.openwhisk_short}} deployment you want to use.
2. **Authorization key** (username and password) which grants you access to the {{site.data.keyword.openwhisk_short}} API.

There are two available {{site.data.keyword.Bluemix_notm}} regions which require their own unique API host and Authorization key.

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

**Note:** If you ever need to switch regions, you must reconfigure the CLI with both the apihost and authorization key as the authorization key is specific per region.

Artifacts (i.e. actions, rules, packages) are also region specific, so if you use the same artifact in multiple regions you must deploy it to each desired region.

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

After you have configured your environment, you can begin using the {{site.data.keyword.openwhisk_short}} CLI to do the following:

* Run your code snippets, or actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking actions](./openwhisk_actions.html).
* Use triggers and rules to enable your actions to respond to events. See [Creating triggers and rules](./openwhisk_triggers_rules.html).
* Learn how packages bundle actions and configure external events sources. See [Using and creating packages](./openwhisk_packages.html).
* Explore the catalog of packages and enhance your applications with external services, such as a [Cloudant event source](./openwhisk_cloudant.html). See [Using {{site.data.keyword.openwhisk_short}}-enabled services](./openwhisk_catalog.html).

## Configure the CLI to use an HTTPS proxy
{: #cli_https_proxy}

The CLI can be set up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and it's port using the following format:
`{PROXY IP}:{PROXY PORT}`.
