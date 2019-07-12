---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Incorporating packages
{: #pkg_ov}

Packages are bundled sets of related actions and feeds. Each package is designed for specific interaction with services and event providers. Some packages are installed already with {{site.data.keyword.openwhisk}} for you to use, but you can also install others.
{: shortdesc}

## Overview
{: #pkg_overview}

[Pre-installed packages](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse) are automatically registered within {{site.data.keyword.openwhisk_short}} in the `/whisk.system` namespace. You can use them without completing any installation steps.

Installable packages are packages that are available for you to install, edit, and use based on your needs. Installable packages do not reside within the {{site.data.keyword.openwhisk_short}} system. Instead, installable packages are externally housed in individual GitHub repositories.

You can install these packages or your own directly into your namespace, and can give a package a custom name. Because the package is installed into your own namespace, you can modify the actions and feeds in the package as needed.



## Browsing pre-installed packages
{: #pkg_browse}

Several packages are registered with {{site.data.keyword.openwhisk_short}} already for you. You can get a list of packages in a namespace, list the entities in a package, and get a description of the individual entities in a package.
{: shortdesc}

1. Get a list of packages in the `/whisk.system` namespace.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Package list output:
  ```
  packages
  /whisk.system/cloudant               shared
  /whisk.system/alarms                 shared
  /whisk.system/watson                 shared
  /whisk.system/websocket              shared
  /whisk.system/weather                shared
  /whisk.system/system                 shared
  /whisk.system/utils                  shared
  /whisk.system/slack                  shared
  /whisk.system/samples                shared
  /whisk.system/github                 shared
  /whisk.system/pushnotifications      shared
  ```
  {: screen}

2. Get a list of entities in a package.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Example**
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **Example output**
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  This output shows that the {{site.data.keyword.cloudant_short_notm}} package includes actions and a feed. For example, two actions, `read` and `write`, and one trigger feed called `changes`. The `changes` feed causes triggers to be fired when documents are added to the specified {{site.data.keyword.cloudant_short_notm}} database.

  The {{site.data.keyword.cloudant_short_notm}} package also defines the parameters `username`, `password`, `host`, and `port`. These parameters must be specified for the actions and feeds to be meaningful. The parameters allow the actions to operate on a specific {{site.data.keyword.cloudant_short_notm}} account, for example.

3. Get a description of an action or feed to see the parameters that are required.

  **Example**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **Example output**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
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
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **Example output**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Get a description of the package binding.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  **Example output**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Notice that all the actions in the `/whisk.system/samples` package are available in the `valhallaSamples` package binding.

3. Invoke an action in the package binding.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **Example output**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Notice from the result that the action inherits the `place` parameter that you set when you created the `valhallaSamples` package binding.

4. Invoke an action and overwrite the default parameter value.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **Example output**
  ```
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

**Before you begin**
- [Install the {{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.cloud_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cli_install).
- Create a `manifest.yaml` or `manifest.yml` file for your app and store it in the root directory. The `manifest.yaml` file specifies the overall structure of the package, including any metadata that must be included with the `ibmcloud fn deploy` command. To learn more about `manifest.yaml` files, see the [<ph class="ignoreSpelling">wskdeploy</ph> documentation ](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}.

To add a package:

1. Clone the package repo.
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Navigate to the directory that contains the `manifest.yaml` file.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Deploy the package.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Some packages require certain environment variables to enable the package to function properly. If so, include the environment variables with the `deploy` command. For example, you can choose a name for the package and specify it with the PACKAGE_NAME variable.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### {{site.data.keyword.cos_full_notm}} package example
{: #pkg_ex}

To see an example of how to install a package, check out the [{{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=cloud-functions-pkg_obstorage). {{site.data.keyword.cos_full}} is a service that allows users to store all types of files, such as images, videos, music, and text. To interact with the files, a Cloud-based datastore of key-value pairs is stored in a bucket. So, to use the [{{site.data.keyword.cos_full_notm}} package](/docs/openwhisk?topic=cloud-functions-pkg_obstorage), you must first create an {{site.data.keyword.cos_full_notm}} service instance, and then create a bucket. The bucket is used as an environment variable that is required to install this package.

After you create the service instance and bucket, you can install the package by using the following commands:

1. Clone the package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to the package directory that contains the `manifest.yaml`. In this example, the Node.js runtime version of the {{site.data.keyword.cos_full_notm}} package is used.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package, using your bucket as an environment variable.  You can give the package a custom name by using the `PACKAGE_NAME` environment variable.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



