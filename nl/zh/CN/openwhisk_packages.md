---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 组织包中的操作
{: #openwhisk_packages}

在 {{site.data.keyword.openwhisk}} 中，可以使用包将一组相关操作捆绑在一起，然后与其他人共享。
{: shortdesc}

包可以包含*操作*和*订阅源*。
- 操作是在 {{site.data.keyword.openwhisk_short}} 上运行的一段代码。例如，{{site.data.keyword.cloudant}} 包中包含用于在 {{site.data.keyword.cloudant_short_notm}} 数据库中读写记录的操作。
- 订阅源用于配置外部事件源以触发触发器事件。例如，“警报”包中包含可按指定频率触发触发器的订阅源。

每个 {{site.data.keyword.openwhisk_short}} 实体（包括包）都属于*名称空间*，并且实体的标准名称为 `/namespaceName[/packageName]/entityName`。有关更多信息，请参阅[命名准则](./openwhisk_reference.html#openwhisk_entities)。

以下部分描述了如何浏览包以及使用包中的触发器和订阅源。此外，如果您想要将自己的包提供给目录，请阅读有关创建和共享包的部分。

## 浏览包
{: #browse-packages}

已向 {{site.data.keyword.openwhisk_short}} 注册了多个包。您可以获取名称空间中包的列表，列出包中的实体，以及获取包中个别实体的描述。

1. 获取 `/whisk.system` 名称空间中包的列表。
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  包列表输出：
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

2. 获取 `/whisk.system/cloudant` 包中实体的列表。
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  示例输出：
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  此输出显示了 {{site.data.keyword.cloudant_short_notm}} 包提供有两个操作（`read` 和 `write`）和一个名为 `changes` 的触发器订阅源。将文档添加到指定的{{site.data.keyword.cloudant_short_notm}} 数据库时，`changes` 订阅源会使触发器触发。

  {{site.data.keyword.cloudant_short_notm}} 包还定义了参数 `username`、`password`、`host` 和 `port`。必须指定这些参数，操作和订阅源才有意义。例如，这些参数允许操作对特定 {{site.data.keyword.cloudant_short_notm}} 帐户执行。

3. 获取 `/whisk.system/cloudant/read` 操作的描述。
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  示例输出：
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  此输出显示了 {{site.data.keyword.cloudant_short_notm}} `read` 操作需要三个参数，包括要检索的数据库和文档标识。

## 调用包中的操作
{: #openwhisk_package_invoke}

可以调用包中的操作，就像使用其他操作一样。下面几步显示如何使用不同参数调用 `/whisk.system/samples` 包中的 `greeting` 操作。

1. 获取 `/whisk.system/samples/greeting` 操作的描述。
  ```
  ibmcloud fn action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  示例输出：
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  请注意，`greeting` 操作接受两个参数：`name` 和 `place`。

2. 在不使用任何参数的情况下调用操作。
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  因为未指定任何参数，所以输出是通用消息。

3. 使用参数调用操作。
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  请注意，输出使用的是传递给操作的 `name` 和 `place` 参数。

## 创建和使用包绑定
{: #openwhisk_package_bind}

虽然可以直接使用包中的实体，但您可能会发现每次都要将相同的参数传递给操作。通过绑定到包并指定缺省参数（由包中的操作继承），可以简化此过程。

例如，在 `/whisk.system/cloudant` 包中，可以在包绑定中设置缺省 `username`、`password` 和 `dbname` 值，并且这些值会自动传递给包中的任何操作。

在以下简单示例中，将绑定到 `/whisk.system/samples` 包。

1. 绑定到 `/whisk.system/samples` 包，并设置缺省 `place` 参数值。
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  示例输出：
  ```
ok: created binding valhallaSamples
  ```
  {: screen}

2. 获取包绑定的描述。
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  示例输出：
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting：返回友好的问候
   action /myNamespace/valhallaSamples/wordCount：统计字符串中的字数
   action /myNamespace/valhallaSamples/helloWorld：演示日志记录工具
   action /myNamespace/valhallaSamples/curl：对主机 URL 执行 Curl
  ```
  {: screen}

  请注意，`/whisk.system/samples` 包中的所有操作在 `valhallaSamples` 包绑定中都可用。

3. 调用包绑定中的操作。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  请注意，从结果中可以看出，操作继承了创建 `valhallaSamples` 包绑定时所设置的 `place` 参数。

4. 调用操作，并覆盖缺省参数值。
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  示例输出：
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  请注意，使用操作调用指定的 `place` 参数值会覆盖在 `valhallaSamples` 包绑定中设置的缺省值。

## 创建并使用触发器订阅源
{: #openwhisk_package_trigger}

通过订阅源，可以方便地配置外部事件源来对 {{site.data.keyword.openwhisk_short}} 触发器触发这些事件。此示例显示了如何使用“警报”包中的订阅源来每秒触发一次触发器，以及如何使用规则来每秒调用一次操作。

1. 获取 `/whisk.system/alarms` 包中订阅源的描述。
  ```
  ibmcloud fn package get --summary /whisk.system/alarms
  ```
  {: pre}

  示例输出：
  ```
package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  示例输出：
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` 订阅源采用两个参数：
  - `cron`：关于何时触发触发器的 crontab 规范。
  - `trigger_payload`：要在每个触发器事件中设置的有效内容参数值。

2. 创建每 8 秒触发一次的触发器。
  ```
  ibmcloud fn trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  示例输出：
  ```
ok: created trigger feed everyEightSeconds
  ```
  {: screen}

3. 创建名为 **hello.js** 的文件并包含以下操作码：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. 确保该操作存在。
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

5. 创建规则，用于在每次 **everyEightSeconds** 触发器触发时调用 `hello` 操作。
  ```
  ibmcloud fn rule create myRule everyEightSeconds hello
  ```
  {: pre}

  示例输出：
  ```
  ok: created rule myRule
  ```
  {: screen}

6. 通过轮询激活日志来检查是否正在调用该操作。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  您可以看到针对触发器、规则和操作，每 8 秒激活一次。操作会在每次调用时收到参数 `{"name":"Mork", "place":"Ork"}`。

## 创建包
{: #openwhisk_packages_create}

包用于组织一组相关的操作和订阅源。
它还允许在包中的所有实体之间共享参数。

要创建定制包并在其中包含简单操作，请尝试以下示例：

1. 创建名为 **custom** 的包。
  ```
  ibmcloud fn package create custom
  ```
  {: pre}

  示例输出：
  ```
ok: created package custom
  ```
  {: screen}

2. 获取包的摘要。
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  示例输出：
  ```
package /myNamespace/custom
  ```
  {: screen}

  请注意，该包为空。

3. 创建名为 `identity.js` 的文件，此文件包含以下操作码。此操作会返回所有输入参数。
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. 在 `custom` 包中创建名为 **identity** 的操作。
  ```
  ibmcloud fn action create custom/identity identity.js
  ```
  {: pre}

  示例输出：
  ```
ok: created action custom/identity
  ```
  {: screen}

  在包中创建操作需要将包名添加为操作名称的前缀。不允许包嵌套。包只能包含操作，而不能包含其他包。

5. 再次获取包的摘要。
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  示例输出：
  ```
package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  现在，可以在名称空间中看到 **custom/identity** 操作。

6. 调用包中的操作。
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  示例输出：
  ```
  {}
  ```
  {: screen}

您可通过设置由包中所有操作继承的包级别参数，设置用于该包中所有实体的缺省参数。要了解此继承的工作方式，请尝试以下示例：

1. 使用以下两个参数来更新 **custom** 包：`city` 和 `country`。
  ```
  ibmcloud fn package update custom --param city Austin --param country USA
  ```
  {: pre}

  示例输出：
  ```
ok: updated package custom
  ```
  {: screen}

2. 显示 **custom** 包和 **identity** 操作中的参数，并查看包中的 **identity** 操作是如何从包继承参数的。
  ```
  ibmcloud fn package get custom parameters
  ```
  {: pre}

  示例输出：
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

  示例输出：
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

3. 在不使用任何参数的情况下调用 **identity** 操作，以验证该操作是否确实继承了这些参数。
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  示例输出：
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. 使用某些参数调用 **identity** 操作。调用参数会与包参数合并；调用参数会覆盖包参数。
  ```
  ibmcloud fn action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  示例输出：
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## 共享包
{: #openwhisk_packages_share}

调试并测试组成一个包的操作和订阅源之后，该包可以与所有 {{site.data.keyword.openwhisk_short}} 用户共享。通过共享包，用户可以绑定包，调用包中的操作，以及编写 {{site.data.keyword.openwhisk_short}} 规则和序列操作。

1. 与所有用户共享包：
  ```
  ibmcloud fn package update custom --shared yes
  ```
  {: pre}

  示例输出：
  ```
ok: updated package custom
  ```
  {: screen}

2. 显示包的 `publish` 属性以验证其现在是否为 true。
  ```
  ibmcloud fn package get custom publish
  ```
  {: pre}

  示例输出：
  ```
  ok: got package custom, displaying field publish
    true
  ```
  {: screen}

现在，其他人可以使用您的 **custom** 包，包括绑定到该包或直接调用该包中的操作。其他用户必须知道包的标准名称，才能绑定包或调用包中的操作。共享包中的操作和订阅源是_公共_的。如果包是私有的，那么其所有内容也是私有的。

1. 获取包的描述，以显示包和操作的标准名称。
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  示例输出：
  ```
package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  在上面的示例中，您使用的是 **myNamespace** 名称空间，并且此名称空间显示为标准名称。
