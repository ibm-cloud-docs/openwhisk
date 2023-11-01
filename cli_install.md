---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: functions cli, serverless, cli, install, functions plug-in, API, migrating, syntax

subcollection: openwhisk

---

{{site.data.keyword.attribute-definition-list}}


# Installing the CLI and plug-in
{: #cli_install}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

{{site.data.keyword.openwhisk}} offers a powerful plug-in for the {{site.data.keyword.cloud_notm}} CLI that allows complete management of the {{site.data.keyword.openwhisk_short}} system. You can use the {{site.data.keyword.openwhisk_short}} CLI plug-in to manage your code snippets in actions, create triggers and rules to enable your actions to respond to events, and bundle actions into packages.
{: shortdesc}

## Setting up the {{site.data.keyword.cloud_notm}} CLI
{: #cli_setup}

Install the latest version of the IBM Cloud CLI.
{: shortdesc}

Before you begin

You must create an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/){: external}.

1. Download and install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. Log in to the {{site.data.keyword.cloud_notm}} CLI.

    ```sh
    ibmcloud login
    ```
    {: pre}

3. If you have more than one account, you are prompted to select which account to use. Follow the prompts or use the `target` command to select your {{site.data.keyword.cloud_notm}} account.

    ```sh
    ibmcloud target -c <account_id>
    ```
    {: pre}

4. You must also specify a region. You can use the `target` command to target or change regions.

    ```sh
    ibmcloud target -r <region>
    ```
    {: pre}

5. You must specify a resource group. To get a list of your resource groups, run the following command.

    ```sh
    ibmcloud resource groups
    ```
    {: pre}

    **Example output**

    ```sh
    Retrieving all resource groups under account <account_name> as email@ibm.com...
    OK
    Name      ID                                 Default Group   State   
    default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
    test      a8a12abbbd63b437cca6d58fb8b462ca7   false           ACTIVE
    ```
    {: screen}

6. Target a resource group by running the following command.

    ```sh
    ibmcloud target -g <resource_group>
    ```
    {: pre}

    **Example output**

    ```sh
    Targeted resource group default
    ```
    {: screen}

## Setting up the {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_plugin_setup}

To work with {{site.data.keyword.openwhisk_short}}, download and install the CLI plug-in.
{: shortdesc}

You can use the {{site.data.keyword.openwhisk_short}} CLI plug-in to perform the following tasks.

* Run your code snippets, or actions, on {{site.data.keyword.openwhisk_short}}. See [Creating and invoking actions](/docs/openwhisk?topic=openwhisk-actions).
* Create triggers and rules to enable your actions to respond to events. See [Creating triggers and rules](/docs/openwhisk?topic=openwhisk-triggers).
* Bundle actions and configure external events sources. See [Create and use packages](/docs/openwhisk?topic=openwhisk-pkg_ov).
* Explore the catalog of packages and enhance your applications with external services. See [Adding {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=openwhisk-services).

Complete the following steps to install the {{site.data.keyword.openwhisk_short}} CLI plug-in

1. Install the {{site.data.keyword.openwhisk_short}} plug-in.

    ```sh
    ibmcloud plugin install cloud-functions
    ```
    {: pre}

2. Verify that the plug-in is installed.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

    **Example Output**

    ```sh
    Plugin Name          Version
    cloud-functions/wsk/functions/fn      1.0.32
    ```
    {: screen}

3. All {{site.data.keyword.openwhisk_short}} commands begin with **`ibmcloud fn`**. To see everything that you can do with the {{site.data.keyword.openwhisk_short}} plug-in, run **`ibmcloud fn`** with no arguments.

    ```sh
    ibmcloud fn
    ```
    {: pre}

For more information about {{site.data.keyword.openwhisk_short}} commands, see the [{{site.data.keyword.openwhisk_short}} CLI reference](/docs/openwhisk?topic=openwhisk-functions-cli).

## Next steps for {{site.data.keyword.openwhisk}} CLI
{: #install_next}

To work with {{site.data.keyword.openwhisk_short}} entities, you must first create or target a namespace. For more information, see [Managing namespaces](/docs/openwhisk?topic=openwhisk-namespaces).

## Updating the {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_update}

You might want to update the CLI periodically to use new features.
{: shortdesc}

1. View your current plug-in list by running `ibmcloud plugin list` command.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}


    **Example Output**

    ```sh
    Plugin Name                                 Version   Status        
    cloud-functions/wsk/functions/fn            1.0.35    Update Available   
    cloud-object-storage                        1.1.0        
    container-registry                          0.1.437      
    container-service/kubernetes-service        0.4.51       
    ```
    {: screen}

2. If an update is available, run the **`ibmcloud plugin update`** command.

    ```sh
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

## Configuring the {{site.data.keyword.openwhisk_short}} CLI to use an HTTPS proxy
{: #cli_proxy}

The {{site.data.keyword.openwhisk_short}} CLI can be set  up to use an HTTPS proxy. To set up an HTTPS proxy, an environment variable that is called `HTTPS_PROXY` must be created. The variable must be set to the address of the HTTPS proxy, and its port by using the following format: `{PROXY IP}:{PROXY PORT}`.
{: shortdesc}

Changing the name of the `org` or `space` creates a new namespace based on the changed name. The entities in the old namespace are not visible in the new namespace and might be deleted.
{: important}

## Migrating from OpenWhisk CLI to {{site.data.keyword.openwhisk_short}} CLI plug-in
{: #cli_migrate}

You can now use the {{site.data.keyword.openwhisk_short}} CLI plug-in to interact with {{site.data.keyword.openwhisk_short}} entities. Although you can continue to use the stand-alone OpenWhisk CLI, it does not have the latest features that are supported by {{site.data.keyword.openwhisk_short}}, such as IAM-based namespaces and `service bind`.
{: shortdesc}

### Command syntax
{: #cli_syntax}

All the command options and arguments for commands in the Cloud Functions CLI plug-in are the same as the options for the [OpenWhisk stand-alone CLI](https://github.com/apache/openwhisk-cli){: external} with the following differences.
{: shortdesc}

* The {{site.data.keyword.openwhisk}} plug-in automatically utilizes your current login and target information.
* `wsk` commands are now run as `ibmcloud fn`.
* The **`wsk ibmcloud login`** command is no longer needed. You can sign in by using **`ibmcloud login`** command.
* You can manage your APIs by using the **`ibmcloud fn api`** commands.

For more information, see the [{{site.data.keyword.openwhisk_short}} CLI reference](/docs/openwhisk?topic=openwhisk-functions-cli).

### API Authentication and Host
{: #cli_api_auth}


With the Cloud Functions CLI plug-in, you don't need to explicitly configure the API key and API host. Instead, you can log in with the `ibmcloud login` command. You can target an IAM-enabled namespace or Cloud Foundry-based namespace by running `ibmcloud fn property set --namespace <namespace_name_or_ID>`. After you log in, all commands begin with `ibmcloud fn`.
  




### Migrating deployment scripts
{: #cli_migrating_deploy_scripts}

If you have scripts that use the OpenWhisk CLI with the **`wsk`** commands, all commands work the same way by using the command **`ibmcloud fn`**. You can modify your scripts to use the {{site.data.keyword.cloud_notm}} CLI plug-in, or create an alias or wrapper so that current commands that use `wsk` are translated to **`ibmcloud fn`**. The **`ibmcloud login`** and **`ibmcloud target`** commands in the {{site.data.keyword.cloud_notm}} CLI work in unattended mode. With unattended mode, you can configure your environment before you run **`ibmcloud fn`** commands to deploy and manage your {{site.data.keyword.openwhisk_short}} entities.
{: shortdesc}


