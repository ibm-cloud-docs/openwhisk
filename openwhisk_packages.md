---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Organizing Actions in packages

In {{site.data.keyword.openwhisk}}, you can use packages to bundle together a set of related Actions, and share them with others.
{: shortdesc}

A package can include *Actions* and *Feeds*.
- An Action is a piece of code that runs on {{site.data.keyword.openwhisk_short}}. For example, the Cloudant package includes Actions to read and write records to a Cloudant database.
- A Feed is used to configure an external event source to fire Trigger events. For example, the Alarm package includes a Feed that can fire a Trigger at a specified frequency.

Every {{site.data.keyword.openwhisk_short}} entity, including packages, belongs in a *namespace*, and the fully qualified name of an entity is `/namespaceName[/packageName]/entityName`. For more information, refer to the [naming guidelines](./openwhisk_reference.html#openwhisk_entities).

The following sections describe how to browse packages and use the Triggers and feeds in them. In addition, if you are interested in contributing your own packages to the catalog, read the sections on creating and sharing packages.

## Browse packages
{: #browse-packages}

Several packages are registered with {{site.data.keyword.openwhisk_short}}. You can get a list of packages in a namespace, list the entities in a package, and get a description of the individual entities in a package.

1. Get a list of packages in the `/whisk.system` namespace.
  ```
  ic wsk package list /whisk.system
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
  ic wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **Output:**
  ```
  package /whisk.system/cloudant: Cloudant database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  This output shows that the Cloudant package provides two Actions, `read` and `write`, and one Trigger feed called `changes`. The `changes` Feed causes Triggers to be fired when documents are added to the specified Cloudant database.

  The Cloudant package also defines the parameters `username`, `password`, `host`, and `port`. These parameters must be specified for the Actions and feeds to be meaningful. The parameters allow the Actions to operate on a specific Cloudant account, for example.

3. Get a description of the `/whisk.system/cloudant/read` Action.
  ```
  ic wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **Output:**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  This output shows that the Cloudant `read` Action requires three parameters, including the database and document ID to retrieve.

## Invoke Actions in a package
{: #openwhisk_package_invoke}

You can invoke Actions in a package, as with other Actions. The next few steps show how to invoke the `greeting` Action in the `/whisk.system/samples` package with different parameters.

1. Get a description of the `/whisk.system/samples/greeting` Action.
  ```
  ic wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}
  
  **Output:**
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  Notice that the `greeting` Action takes two parameters: `name` and `place`.

2. Invoke the Action without any parameters.
  ```
  ic wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  **Output:**
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  The output is a generic message because no parameters were specified.

3. Invoke the Action with parameters.
  ```
  ic wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  **Output:**
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Notice that the output uses the `name` and `place` parameters that were passed to the Action.

## Create and use package bindings
{: #openwhisk_package_bind}

Although you can use the entities in a package directly, you might find yourself passing the same parameters to the Action every time. You can simplify the process by binding to a package and specifying default parameters, which are inherited by the Actions in the package.

For example, in the `/whisk.system/cloudant` package, you can set default `username`, `password`, and `dbname` values in a package binding and these values are automatically passed to any Actions in the package.

In the following simple example, you bind to the `/whisk.system/samples` package.

1. Bind to the `/whisk.system/samples` package and set a default `place` parameter value.
  ```
  ic wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **Output:**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Get a description of the package binding.
  ```
  ic wsk package get --summary valhallaSamples
  ```
  {: pre}

  **Output:**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Notice that all the Actions in the `/whisk.system/samples` package are available in the `valhallaSamples` package binding.

3. Invoke an Action in the package binding.
  ```
  ic wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **Output:**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Notice from the result that the Action inherits the `place` parameter that you set when you created the `valhallaSamples` package binding.

4. Invoke an Action and overwrite the default parameter value.
  ```
  ic wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **Output:**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Notice that the `place` parameter value that is specified with the Action invocation overwrites the default value set in the `valhallaSamples` package binding.

## Create and use Trigger Feeds
{: #openwhisk_package_trigger}

Feeds offer a convenient way to configure an external event source to fire these events to a {{site.data.keyword.openwhisk_short}} Trigger. This example shows how to use a Feed in the Alarms package to fire a Trigger every second, and how to use a rule to invoke an Action every second.

1. Get a description of the Feed in the `/whisk.system/alarms` package.
  ```
  ic wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  **Output:**
  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ic wsk action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  **Output:**
  ```
  action /whisk.system/alarms/alarm: Fire Trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  The `/whisk.system/alarms/alarm` Feed takes two parameters:
  - `cron`: A crontab specification of when to fire the Trigger.
  - `trigger_payload`: The payload parameter value to set in each Trigger event.

2. Create a Trigger that fires every 8 seconds.
  ```
  ic wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  **Output:**
  ```
  ok: created trigger feed everyEightSeconds
  ```
  {: screen}

3. Create a file named **hello.js** with the following Action code:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Make sure that the Action exists.
  ```
  ic wsk action update hello hello.js
  ```
  {: pre}

5. Create a rule that invokes the **hello** Action every time the `everyEightSeconds` Trigger fires.
  ```
  ic wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}

  **Output:**
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Check that the Action is being invoked by polling for activation logs.
  ```
  ic wsk activation poll
  ```
  {: pre}

  You can see that the activations are observed every 8 seconds for the Trigger, the rule, and the Action. The Action receives the parameters `{"name":"Mork", "place":"Ork"}` on every invocation.

## Create a package
{: #openwhisk_packages_create}

A package is used to organize a set of related Actions and Feeds.
It also allows for parameters to be shared across all entities in the package.

To create a custom package with a simple Action in it, try the following example:

1. Create a package called **custom**.
  ```
  ic wsk package create custom
  ```
  {: pre}

  **Output:**
  ```
  ok: created package custom
  ```
  {: screen}

2. Get a summary of the package.
  ```
  ic wsk package get --summary custom
  ```
  {: pre}

  **Output:**
  ```
  package /myNamespace/custom
  ```
  {: screen}

  Notice that the package is empty.

3. Create a file that is called `identity.js` that contains the following Action code. This Action returns all input parameters.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Create an Action called **identity** in the `custom` package.
  ```
  ic wsk action create custom/identity identity.js
  ```
  {: pre}
  
  **Output:**
  ```
  ok: created action custom/identity
  ```
  {: screen}

  Creating an Action in a package requires that you prefix the Action name with a package name. Package nesting is not allowed. A package can contain only Actions and can't contain another package.

5. Get a summary of the package again.
  ```
  ic wsk package get --summary custom
  ```
  {: pre}

  **Output:**
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  You can see the **custom/identity** Action in your namespace now.

6. Invoke the Action in the package.
  ```
  ic wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  **Output:**
  ```
  {}
  ```
  {: screen}

You can set default parameters for all the entities in a package by setting package-level parameters that are inherited by all Actions in the package. To see how this inheritance works, try the following example:

1. Update the **custom** package with two parameters: `city` and `country`.
  ```
  ic wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  **Output:**
  ```
  ok: updated package custom
  ```
  {: screen}

2. Display the parameters in the **custom** package and **identidy** Action, and see how the **identity** Action in the package inherits parameters from the package.
  ```
  ic wsk package get custom parameters
  ```
  {: pre}

  **Output:**
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
  ic wsk action get custom/identity parameters
  ```
  {: pre}

  **Output:**
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

3. Invoke the **identity** Action without any parameters to verify that the action indeed inherits the parameters.
  ```
  ic wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  **Output:**
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. Invoke the **identity** Action with some parameters. Invocation parameters are merged with the package parameters; the invocation parameters override the package parameters.
  ```
  ic wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  **Output:**
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

After the Actions and Feeds that comprise a package are debugged and tested, the package can be shared with all {{site.data.keyword.openwhisk_short}} users. Sharing the package makes it possible for the users to bind the package, invoke Actions in the package, and author {{site.data.keyword.openwhisk_short}} Rules and sequence Actions.

1. Share the package with all users:
  ```
  ic wsk package update custom --shared yes
  ```
  {: pre}

  **Output:**
  ```
  ok: updated package custom
  ```
  {: screen}

2. Display the `publish` property of the package to verify that it is now true.
  ```
  ic wsk package get custom publish
  ```
  {: pre}

  **Output:**
  ```
  ok: got package custom, displaying field publish
  
  true
  ```
  {: screen}

Others can now use your **custom** package, including binding to the package or directly invoking an Action in it. Other users must know the fully qualified names of the package to bind it or invoke Actions in it. Actions and feeds within a shared package are _public_. If the package is private, then all of its contents are also private.

1. Get a description of the package to show the fully qualified names of the package and Action.
  ```
  ic wsk package get --summary custom
  ```
  {: pre}

  **Output:**
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  In the previous example, you're working with the **myNamespace** namespace, and this namespace appears in the fully qualified name.
