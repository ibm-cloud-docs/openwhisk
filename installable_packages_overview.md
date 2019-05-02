---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-02"

keywords: packages, installable packages

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Incorporating packages in your serverless app
{: #pkgs_ov}

Packages are bundled sets of related actions and feeds. Each package is designed for specific interaction with services and event providers. Some packages are installed already with {{site.data.keyword.openwhisk}} for you to use, but you can also install others.
{: shortdesc}

## Overview
{: #pkgs_overview}

[Pre-installed packages](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#browse-packages) are automatically registered within {{site.data.keyword.openwhisk_short}} in the `/whisk.system` namespace. You can use them without completing any installation steps.

Installable packages are packages that are available for you to install, edit, and use based on your needs. Installable packages do not reside within the {{site.data.keyword.openwhisk_short}} system. Instead, installable packages are externally housed in individual Github repositories. You can install these packages directly into your own namespace by using the `ibmcloud fn deploy` command, and can give a package a custom name. Because the package is installed into your own namespace, you can modify the actions and feeds in the package as needed.



## Browsing pre-installed packages
{: #pkgs_browse}

Several packages are registered with {{site.data.keyword.openwhisk_short}}. You can get a list of packages in a namespace, list the entities in a package, and get a description of the individual entities in a package.

1. Get a list of packages in the `/whisk.system` namespace.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Package list output:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Get a list of entities in the `/whisk.system/cloudant` package.
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  Example output:
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  This output shows that the {{site.data.keyword.cloudant_short_notm}} package provides two actions, `read` and `write`, and one trigger feed called `changes`. The `changes` feed causes triggers to be fired when documents are added to the specified {{site.data.keyword.cloudant_short_notm}} database.

  The {{site.data.keyword.cloudant_short_notm}} package also defines the parameters `username`, `password`, `host`, and `port`. These parameters must be specified for the actions and feeds to be meaningful. The parameters allow the actions to operate on a specific {{site.data.keyword.cloudant_short_notm}} account, for example.

3. Get a description of the `/whisk.system/cloudant/read` action.
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  Example output:
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  This output shows that the {{site.data.keyword.cloudant_short_notm}} `read` action requires three parameters, including the database and document ID to retrieve.



## Binding parameters to pre-installed packages
{: #pkgs_bind}

Although you can use the entities in a package directly, you might find yourself passing the same parameters to the action every time. You can simplify the process by binding to a package and specifying default parameters, which are inherited by the actions in the package.
{: shortdesc}

For example, in the `/whisk.system/cloudant` package, you can set default `username`, `password`, and `dbname` values in a package binding and these values are automatically passed to any actions in the package.

In the following simple example, you bind to the `/whisk.system/samples` package.

1. Bind to the `/whisk.system/samples` package and set a default `place` parameter value.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  Example output:
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Get a description of the package binding.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  Example output:
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

  Example output:
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

  Example output:
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Notice that the `place` parameter value that is specified with the action invocation overwrites the default value set in the `valhallaSamples` package binding.




  ## Using feeds as triggers
  {: #pkgs_feeds}

  Feeds offer a convenient way to configure an external event source to fire these events to a {{site.data.keyword.openwhisk_short}} trigger. This example shows how to use a feed in the Alarms package to fire a trigger once a minute, and how to use a rule to invoke an action once a minute.

  1. Get a description of the feed in the `/whisk.system/alarms` package.
    ```
    ibmcloud fn package get --summary /whisk.system/alarms
    ```
    {: pre}

    Example output:
    ```
    package /whisk.system/alarms
     feed   /whisk.system/alarms/alarm
    ```
    {: screen}

    ```
    ibmcloud fn action get --summary /whisk.system/alarms/alarm
    ```
    {: pre}

    Example output:
    ```
    action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
       (params: cron trigger_payload)
    ```
    {: screen}

    The `/whisk.system/alarms/alarm` feed takes two parameters:
    - `cron`: A crontab specification of when to fire the trigger.
    - `trigger_payload`: The payload parameter value to set in each trigger event.

  2. Create a trigger that fires every one minute.
    ```
    ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
    ```
    {: pre}

    Example output:
    ```
    ok: created trigger feed everyOneMinute
    ```
    {: screen}

  3. Create a file named `hello.js` with the following action code:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.name + ' from ' + params.place};
    }
    ```
    {: codeblock}

  4. Make sure that the action exists.
    ```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

  5. Create a rule that invokes the `hello` action every time the `everyOneMinute` trigger fires.
    ```
    ibmcloud fn rule create myRule everyOneMinute hello
    ```
    {: pre}

    Example output:
    ```
    ok: created rule myRule
    ```
    {: screen}

  6. Check that the action is being invoked by polling for activation logs.
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

    You can see that the activations are observed every one minute for the trigger, the rule, and the action. The action receives the parameters `{"name":"Mork", "place":"Ork"}` on every invocation.


## Adding your own packages
{: #pkgs_add}

You can create a package of local code or a clone of any Github repository.
{: shortdesc}

Before you begin:

  1. [Install the {{site.data.keyword.openwhisk_short}} plugin for the {{site.data.keyword.Bluemix_notm}} CLI](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

To install a package:

1. Clone the package repo. Package repos can be found on the individual pages for each package in this documentation set.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. Navigate to the package directory that contains a `manifest.yaml` or `manifest.yml` file. The `manifest.yaml` file specifies the overall structure of the package, including the package and actions to be installed into your namespace and any metadata that must be included with the `ibmcloud fn deploy` command. To learn more about `manifest.yaml` files, see the [wskdeploy documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Deploy the package. Some packages require certain environment variables to enable the package to function properly.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Example using the {{site.data.keyword.cos_full_notm}} package
{: #pkgs_ex}

To see an example of how to install a package, check out the [{{site.data.keyword.cos_short}} package](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions). {{site.data.keyword.cos_full}} is a service that allows users to store all types of files, such as images, videos, music, and text. To interact with the files, a Cloud-based datastore of key/value pairs are stored in a bucket. So, to use the [{{site.data.keyword.cos_short}} package](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions), you must first create an {{site.data.keyword.cos_short}} service instance, and then create a bucket. The bucket is used as an environment variable that is required to install this package.

After creating the service instance and bucket, installing the package requires the following commands:

1. Clone the package repo.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigate to the package directory that contains the `manifest.yaml`. In this example, the Node.js runtime version of the {{site.data.keyword.cos_short}} package is used.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Deploy the package, using your bucket as an environment variable. The dependency on the `PACKAGE_NAME` environment variable allows you to give this package a custom name.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}
