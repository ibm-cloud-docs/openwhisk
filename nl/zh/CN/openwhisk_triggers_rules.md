---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 一般概念
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} 触发器和规则为平台带来了事件驱动型功能。来自外部和内部事件源的事件将通过触发器进行传递，并且规则允许操作对这些事件做出反应。
{: shortdesc}

## 什么是触发器？
{: #openwhisk_triggers_create}

触发器是为某类事件指定的通道。下面是触发器示例：
- 位置更新事件的触发器。
- 关于文档上传到 Web 站点的触发器。
- 传入电子邮件的触发器。

触发器可以使用键/值对的字典来*触发*（激活）。有时，此字典称为*事件*。与操作一样，每次触发触发器都会生成一个**激活标识**。

触发器可以由用户显式触发，也可以由外部事件源代表用户触发。
通过*订阅源*，可以方便地配置外部事件源来触发 {{site.data.keyword.openwhisk_short}} 可使用的触发器事件。请参阅以下示例订阅源：
- {{site.data.keyword.cloudant}} 数据更改订阅源，用于每次在数据库中添加或修改文档时，触发触发器事件。
- Git 订阅源，用于针对 Git 存储库中的每次落实，触发触发器事件。

## 规则如何影响触发器？
{: #openwhisk_rules_use}

规则用于将一个触发器与一个操作关联在一起，每次触发触发器时，都会将触发器事件作为输入来调用相应的操作。

使用相应的规则集时，可以通过单个触发器事件来调用多个操作，也可以调用一个操作以响应来自多个触发器的事件。

例如，假设一个系统包含以下操作：
- `classifyImage` - 此操作用于检测图像中的对象并对其进行分类。
- `thumbnailImage` - 此操作用于创建图像的缩略图版本。

此外，假设有两个事件源将触发以下触发器：
- `newTweet` - 此触发器在发布新推文时触发。
- `imageUpload` - 此触发器在将图像上传到 Web 站点时触发。

可以设置规则，以使单个触发器事件调用多个操作，以及使多个触发器调用同一操作：
- `newTweet -> classifyImage` 规则。
- `imageUpload -> classifyImage` 规则。
- `imageUpload -> thumbnailImage` 规则。

这三个规则确定以下行为：
- 对两个推文中的图像分类。
- 对上传的图像分类。
- 生成缩略图版本。

## 创建和触发触发器
{: #openwhisk_triggers_fire}

触发器可以在发生特定事件时触发，也可以手动触发。

例如，创建用于发送用户位置更新的触发器，然后手动触发该触发器。
1. 输入以下命令来创建触发器：
  ```
  ibmcloud fn trigger create locationUpdate
  ```
  {: pre}

  示例输出：
  ```
ok: created trigger locationUpdate
  ```
  {: screen}

2. 通过列出触发器集来检查是否已创建该触发器。
  ```
  ibmcloud fn trigger list
  ```
  {: pre}

  示例输出：
  ```
triggers
  /someNamespace/locationUpdate                            private
  ```
  {: screen}

  现在，已创建可向其触发事件的指定“通道”。

3. 接下来，通过指定触发器名称和参数来触发触发器事件：
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  示例输出：
  ```
ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

触发没有伴随规则与之匹配的触发器不会产生可见的效果。
触发器无法在包内创建；必须直接在**名称空间**下创建触发器。

## 使用规则将触发器与操作相关联
{: #openwhisk_rules_assoc}

规则用于将触发器与操作关联在一起。每次触发触发器事件时，都会通过事件参数来调用操作。

例如，创建一个规则，用于在每次发布位置更新时都调用 `hello` 操作。
1. 创建名为“hello.js”的文件并包含以下操作码：
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 确保触发器和操作存在：
  ```
  ibmcloud fn trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. 下一步是创建规则。规则创建后即会启用，这意味着规则将立即可用于响应触发器激活。三个参数分别是：_规则名称_、_触发器名称_和_操作名称_。
  ```
  ibmcloud fn rule create myRule locationUpdate hello
  ```
  {: pre}

  您可以随时选择禁用规则：
  ```
  ibmcloud fn rule disable myRule
  ```
  {: pre}

4. 触发 **locationUpdate** 触发器。每次触发事件时，都会通过事件参数来调用 **hello** 操作。
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  示例输出：
  ```
ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. 通过检查最新的激活来验证是否调用了 **hello** 操作。
  ```
  ibmcloud fn activation list --limit 1 hello
  ```
  {: pre}

  示例输出：
  ```
activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  现在，查询先前命令输出中列出的激活标识：
  ```
  ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  示例输出：
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  您将看到 **hello** 操作收到了事件有效内容，并返回了期望的字符串。

可以创建多个规则，用于将同一触发器与不同操作相关联。触发器和规则不能属于包。但是，规则可以与属于包的操作相关联，例如：
  ```
  ibmcloud fn rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

您还可以将规则与序列配合使用。例如，可以创建由规则 `anotherRule` 激活的操作序列 `recordLocationAndHello`。
  ```
  ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
