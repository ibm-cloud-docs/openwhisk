---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-01"

keywords: packages, installable packages, functions, binding, pre-installed

subcollection: openwhisk

---
{{site.data.keyword.attribute-definition-list}}

# Incorporating packages
{: #pkg_ov}

{{site.data.keyword.openwhisk}} is deprecated. Existing Functions entities such as actions, triggers, or sequences will continue to run, but as of 28 December 2023, you can’t create new Functions entities. Existing Functions entities are supported until October 2024. Any Functions entities that still exist on that date will be deleted. For more information, see [Deprecation overview](/docs/openwhisk?topic=openwhisk-dep-overview).
{: deprecated}

Packages are bundled sets of related actions and feeds. Each package is designed for specific interaction with services and event providers. Some packages are installed already with {{site.data.keyword.openwhisk}} for you to use, but you can also install others.
{: shortdesc}

## Overview for packages
{: #pkg_overview}

[Pre-installed packages](/docs/openwhisk?topic=openwhisk-pkg_ov#pkg_browse) are automatically registered within {{site.data.keyword.openwhisk_short}} in the `/whisk.system` namespace. You can use them without completing any installation steps.

Installable packages are packages that are available for you to install, edit, and use based on your needs. Installable packages do not reside within the {{site.data.keyword.openwhisk_short}} system. Instead, installable packages are externally housed in individual GitHub repositories.

You can install these packages or your own directly into your namespace, and can give a package a custom name. Because the package is installed into your own namespace, you can modify the actions and feeds in the package as needed.

## Browsing pre-installed packages
{: #pkg_browse}

Several packages are registered with {{site.data.keyword.openwhisk_short}} already for you. You can get a list of packages in a namespace, list the entities in a package, and get a description of the individual entities in a package.
{: shortdesc}

1. Get a list of packages in the `/whisk.system` namespace.

    ```sh
    ibmcloud fn package list /whisk.system
    ```
    {: pre}

    Package list output:

    ```sh
    packages
    /whisk.system/cloudant         shared
    /whisk.system/websocket        shared
    /whisk.system/utils            shared
    /whisk.system/samples          shared
    /whisk.system/cos              shared
    /whisk.system/messaging        shared
    /whisk.system/alarms           shared
    ```
    {: screen}

2. Get a list of entities in a package.

    ```sh
    ibmcloud fn package get --summary <package_name>
    ```
    {: pre}

    **Example**

    ```sh
    ibmcloud fn package get --summary /whisk.system/cloudant
    ```
    {: pre}

    **Example output**

    ```sh
    package /whisk.system/cloudant: Cloudant database service
        (parameters: *apihost, *bluemixServiceName, dbname, host, iamApiKey, iamUrl, overwrite, password, username)
    action /whisk.system/cloudant/delete-attachment: Delete document attachment from database
        (parameters: attachmentname, dbname, docid, docrev, params)
    action /whisk.system/cloudant/update-attachment: Update document attachment in database
        (parameters: attachment, attachmentname, contenttype, dbname, docid, docrev, params)
    action /whisk.system/cloudant/read-attachment: Read document attachment from database
        (parameters: attachmentname, dbname, docid, params)
    action /whisk.system/cloudant/create-attachment: Create document attachment in database
        (parameters: attachment, attachmentname, contenttype, dbname, docid, docrev, params)
    action /whisk.system/cloudant/read-changes-feed: Read Cloudant database changes feed (non-continuous)
        (parameters: dbname, params)
    ...
    ...
    feed   /whisk.system/cloudant/changes: Database change feed
        (parameters: dbname, filter, iamApiKey, iamUrl, query_params)
    ```
    {: screen}

    This output shows that the {{site.data.keyword.cloudant_short_notm}} package includes several actions and a feed.

    The {{site.data.keyword.cloudant_short_notm}} package also defines the parameters `username`, `password`, `host`, and `port`. These parameters must be specified for the actions and feeds to be meaningful. 

3. Get a description of an action or feed to see the parameters that are required.

    **Example**

    ```h
    ibmcloud fn action get --summary /whisk.system/cloudant/read
    ```
    {: pre}

    **Example output**

    ```sh
    action /whisk.system/cloudant/read: Read document from database
        (parameters: *apihost, *bluemixServiceName, dbname, *id, params)
    ```
    {: screen}

    This output shows that the {{site.data.keyword.cloudant_short_notm}} `read` action requires three parameters, including the database and document ID to retrieve.

## Binding parameters to pre-installed packages
{: #pkg_bind}

Although you can use the entities in a package directly, you might find yourself passing the same parameters to the action every time. You can simplify the process by binding to a package and specifying default parameters, which are inherited by the actions in the package.
{: shortdesc}

For example, in the `/whisk.system/cloudant` package, you can set default `username`, `password`, and `dbname` values in a package binding and these values are automatically passed to any actions in the package.

In the following example, you bind to the `/whisk.system/samples` package.

1. Bind to the `/whisk.system/samples` package and set a default `place` parameter value.

    ```sh
    ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
    ```
    {: pre}

    **Example output**

    ```sh
    ok: created binding valhallaSamples
    ```
    {: screen}

2. Get a description of the package binding.

    ```sh
    ibmcloud fn package get --summary valhallaSamples
    ```
    {: pre}

    **Example output**

    ```sh
    package /<namespace_ID>/valhallaSamples
    action /<namespace_ID>/valhallaSamples/greeting: Returns a friendly greeting
    action /<namespace_ID>/valhallaSamples/wordCount: Count words in a string
    action /<namespace_ID>/valhallaSamples/helloWorld: Demonstrates logging facilities
    action /<namespace_ID>/valhallaSamples/curl: Curl a host url
    ```
    {: screen}

    Notice that all the actions in the `/whisk.system/samples` package are available in the `valhallaSamples` package binding.

3. Invoke an action in the package binding.

    ```sh
    ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
    ```
    {: pre}

    **Example output**

    ```sh
    {
        "payload": "Hello, Odin from Valhalla!"
    }
    ```
    {: screen}

    Notice from the result that the action inherits the `place` parameter that you set when you created the `valhallaSamples` package binding.

4. Invoke an action and overwrite the default parameter value.

    ```sh
    ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
    ```
    {: pre}

    **Example output**

    ```sh
    {
        "payload": "Hello, Odin from Asgard!"
    }
    ```
    {: screen}

    Notice that the `place` parameter value that is specified with the action invocation overwrites the default value set in the `valhallaSamples` package binding.

## Adding your own packages
{: #pkg_add}

You can create a package of local code or a clone of any GitHub repository.
{: shortdesc}

Before you begin

- [Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=openwhisk-cli_install).
- Create a `manifest.yaml` or `manifest.yml` file for your app and store it in the root directory. The `manifest.yaml` file specifies the overall structure of the package, including any metadata that must be included with the `ibmcloud fn deploy` command. To learn more about `manifest.yaml` files, see the [`wskdeploy` documentation](https://github.com/apache/openwhisk-wskdeploy/blob/master/docs/programming_guide.md){: external}.

To add a package:

1. Clone the package repo.

    ```sh
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Navigate to the directory that contains the `manifest.yaml` file.

    ```sh
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Deploy the package.

    ```sh
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Some packages require certain environment variables to enable the package to function properly. If so, include the environment variables with the `deploy` command. For example, you can choose a name for the package and specify it with the PACKAGE_NAME variable.

    ```sh
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### {{site.data.keyword.cos_full_notm}} package example
{: #pkg_ex}

To see an example of how to install a package, check out the [{{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=openwhisk-pkg_obstorage). {{site.data.keyword.cos_full}} is a service that allows users to store all types of files, such as images, videos, music, and text. To interact with the files, a Cloud-based datastore of key-value pairs is stored in a bucket. So, to use the [{{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=openwhisk-pkg_obstorage), you must first create an {{site.data.keyword.cos_full_notm}} service instance, and then create a bucket. The bucket is used as an environment variable that is required to install this package.

After you create the service instance and bucket, you can install the package by using the following commands:

1. Clone the package repo.

    ```sh
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to the package directory that contains the `manifest.yaml`. In this example, the Node.js runtime version of the {{site.data.keyword.cos_full_notm}} package is used.

    ```sh
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package, using your bucket as an environment variable.  You can give the package a custom name by using the `PACKAGE_NAME` environment variable.

    ```sh
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}


