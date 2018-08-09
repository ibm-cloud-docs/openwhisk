---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Organizing actions in packages
{: #openwhisk_packages}

In {{site.data.keyword.openwhisk}}, you can use packages to bundle together a set of related actions, and share them with others.
{: shortdesc}

A package can include *actions* and *feeds*.
- An action is a piece of code that runs on {{site.data.keyword.openwhisk_short}}. For example, the {{site.data.keyword.cloudant}} package includes actions to read and write records to a {{site.data.keyword.cloudant_short_notm}} database.
- A feed is used to configure an external event source to fire trigger events. For example, the Alarm package includes a feed that can fire a trigger at a specified frequency.

Every {{site.data.keyword.openwhisk_short}} entity, including packages, belongs in a *namespace*, and the fully qualified name of an entity is `/namespaceName[/packageName]/entityName`. For more information, refer to the [naming guidelines](./openwhisk_reference.html#openwhisk_entities).

The following sections describe how to browse packages and use the triggers and feeds in them. In addition, if you are interested in contributing your own packages to the catalog, read the sections on creating and sharing packages.

## Browse packages
{: #browse-packages}

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

## Invoke actions in a package
{: #openwhisk_package_invoke}

You can invoke actions in a package, as with other actions. The next few steps show how to invoke the `greeting` action in the `/whisk.system/samples` package with different parameters.

1. Get a description of the `/whisk.system/samples/greeting` action.
  ```
  ibmcloud fn action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  Example output:
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  Notice that the `greeting` action takes two parameters: `name` and `place`.

2. Invoke the action without any parameters.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  Example output:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  The output is a generic message because no parameters were specified.

3. Invoke the action with parameters.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  Example output:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Notice that the output uses the `name` and `place` parameters that were passed to the action.

## Create and use package bindings
{: #openwhisk_package_bind}

Although you can use the entities in a package directly, you might find yourself passing the same parameters to the action every time. You can simplify the process by binding to a package and specifying default parameters, which are inherited by the actions in the package.

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

## Create and use trigger feeds
{: #openwhisk_package_trigger}

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

3. Create a file named **hello.js** with the following action code:
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

5. Create a rule that invokes the **hello** action every time the `everyOneMinute` trigger fires.
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

## Create a package
{: #openwhisk_packages_create}

A package is used to organize a set of related actions and feeds.
It also allows for parameters to be shared across all entities in the package.

To create a custom package with a simple action in it, try the following example:

1. Create a package called **custom**.
  ```
  ibmcloud fn package create custom
  ```
  {: pre}

  Example output:
  ```
  ok: created package custom
  ```
  {: screen}

2. Get a summary of the package.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Example output:
  ```
  package /myNamespace/custom
  ```
  {: screen}

  Notice that the package is empty.

3. Create a file that is called `identity.js` that contains the following action code. This action returns all input parameters.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Create an action called **identity** in the `custom` package.
  ```
  ibmcloud fn action create custom/identity identity.js
  ```
  {: pre}

  Example output:
  ```
  ok: created action custom/identity
  ```
  {: screen}

  Creating an action in a package requires that you prefix the action name with a package name. Package nesting is not allowed. A package can contain only actions and can't contain another package.

5. Get a summary of the package again.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Example output:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  You can see the **custom/identity** action in your namespace now.

6. Invoke the action in the package.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Example output:
  ```
  {}
  ```
  {: screen}

You can set default parameters for all the entities in a package by setting package-level parameters that are inherited by all actions in the package. To see how this inheritance works, try the following example:

1. Update the **custom** package with two parameters: `city` and `country`.
  ```
  ibmcloud fn package update custom --param city Austin --param country USA
  ```
  {: pre}

  Example output:
  ```
  ok: updated package custom
  ```
  {: screen}

2. Display the parameters in the **custom** package and **identidy** action, and see how the **identity** action in the package inherits parameters from the package.
  ```
  ibmcloud fn package get custom parameters
  ```
  {: pre}

  Example output:
  ```
  ok: got package custom, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

  ```
  ibmcloud fn action get custom/identity parameters
  ```
  {: pre}

  Example output:
  ```
  ok: got action custom/identity, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

3. Invoke the **identity** action without any parameters to verify that the action indeed inherits the parameters.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Example output:
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. Invoke the **identity** action with some parameters. Invocation parameters are merged with the package parameters; the invocation parameters override the package parameters.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  Example output:
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## Share a package
{: #openwhisk_packages_share}

After the actions and feeds that comprise a package are debugged and tested, the package can be shared with all {{site.data.keyword.openwhisk_short}} users. Sharing the package makes it possible for the users to bind the package, invoke actions in the package, and author {{site.data.keyword.openwhisk_short}} Rules and sequence actions.

1. Share the package with all users:
  ```
  ibmcloud fn package update custom --shared yes
  ```
  {: pre}

  Example output:
  ```
  ok: updated package custom
  ```
  {: screen}

2. Display the `publish` property of the package to verify that it is now true.
  ```
  ibmcloud fn package get custom publish
  ```
  {: pre}

  Example output:
  ```
  ok: got package custom, displaying field publish

  true
  ```
  {: screen}

Others can now use your **custom** package, including binding to the package or directly invoking an action in it. Other users must know the fully qualified names of the package to bind it or invoke actions in it. actions and feeds within a shared package are _public_. If the package is private, then all of its contents are also private.

1. Get a description of the package to show the fully qualified names of the package and action.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Example output:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  In the previous example, you're working with the **myNamespace** namespace, and this namespace appears in the fully qualified name.
