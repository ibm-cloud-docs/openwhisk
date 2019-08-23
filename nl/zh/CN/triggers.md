---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: triggers, serverless, functions

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



# 为事件创建触发器
{: #triggers}

触发器是指要对特定类型的事件做出反应的声明，事件可来自用户或来自事件源。
{: shortdesc}

下面是触发器示例。
- 位置更新事件
- 文档上传到 Web 站点
- 传入电子邮件



## 通过 CLI 创建触发器
{: #triggers_create}


1. 创建触发器。触发器必须直接在名称空间中进行创建，而不能在包内部进行创建。
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    **示例输出**
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. 验证是否已创建触发器。
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    **示例输出**
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



接下来，可以[测试触发器](/docs/openwhisk?topic=cloud-functions-test#test_triggers)或[创建规则](/docs/openwhisk?topic=cloud-functions-rules)，以将触发器与操作关联在一起。



## 订阅源与触发器的区别
{: #triggers_difference}

订阅源和触发器是紧密相关的，但在技术上是不同的概念。

- {{site.data.keyword.openwhisk_short}} 处理流入系统的**事件**。

- **触发器**是指某类事件的名称。每个事件属于恰好一个触发器；若用类比说明，触发器类似于基于主题的发布/预订系统中的主题。**规则**表示每当来自触发器的事件到达时，通过触发器有效内容调用操作。

- 通过**订阅源**，可以方便地配置外部事件源来触发 {{site.data.keyword.openwhisk_short}} 可使用的触发器事件。订阅源是全部属于某个触发器的事件的流。预安装的包、可安装的包和您自己的定制包可能包含订阅源。订阅源通过**订阅源操作**进行控制，订阅源操作用于处理组成订阅源的事件流的创建、删除、暂停和恢复。通常，订阅源操作通过使用管理通知的 REST API 来与生成事件的外部服务进行交互。

订阅源示例：
- {{site.data.keyword.cloudant}} 数据更改订阅源，用于每次在数据库中添加或修改文档时，触发触发器事件
- Git 订阅源，用于针对 Git 存储库中的每次落实，触发触发器事件



## 为订阅源创建触发器
{: #triggers_feeds}

此示例显示了如何使用“警报”包中的订阅源来每分钟触发一次触发器，以及如何使用规则来每分钟调用一次操作。

1. 获取 `/whisk.system/alarms` 包中实体的描述列表。

    ```
  ibmcloud fn package get --summary /whisk.system/alarms
  ```
    {: pre}

    **示例输出**
    ```
    package /whisk.system/alarms
     feed   /whisk.system/alarms/alarm
    ```
    {: screen}
2. 获取 `/whisk.system/alarms` 包中订阅源的描述，以查看可以使用的参数。

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  **示例输出**
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` 订阅源采用两个参数：
  - `cron`：关于何时触发触发器的 crontab 规范。
  - `trigger_payload`：要在每个触发器事件中设置的有效内容参数值。

2. 创建每一分钟触发一次的触发器。
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  **示例输出**
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. 创建应用程序。示例 `hello.js`：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. 创建操作。
  ```
    ibmcloud fn action create hello hello.js
    ```
  {: pre}

5. 创建规则，用于在每次 `everyOneMinute` 触发器触发时调用 `hello` 操作。
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  **示例输出**
  ```
  ok: created rule myRule
  ```
  {: screen}

6. 通过轮询激活日志来检查是否正在调用该操作。
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  您可以看到针对触发器、规则和操作，每一分钟激活一次。操作会在每次调用时收到参数 `{"name":"Mork", "place":"Ork"}`。



