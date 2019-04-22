---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: trigger rules, triggers, actions, channel events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 通过触发器和规则响应事件
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk}} 触发器和规则为平台带来了事件驱动型功能。来自外部和内部事件源的事件将通过触发器进行传递，并且规则允许操作对这些事件做出反应。
{: shortdesc}

## 一般概念
{: #definitions}

### 触发器
{: #openwhisk_triggers_create}

触发器是为某类事件指定的通道。
{: shortdesc}

触发器是指要对特定类型的事件做出反应的声明，事件可来自用户或来自事件源。下面是触发器示例。
- 位置更新事件的触发器
- 关于文档上传到 Web 站点的触发器
- 传入电子邮件的触发器

触发器可以使用键/值对的字典来触发（或激活）。有时，此字典称为事件。触发器可以由用户显式触发，也可以由外部事件源代表用户触发。
与操作一样，每次触发与规则关联的触发器都会生成一个激活标识。未与规则关联的触发器在触发时没有任何可视效果。

通过订阅源，可以方便地配置外部事件源来触发 {{site.data.keyword.openwhisk_short}} 可使用的触发器事件。下面是订阅源示例。
- {{site.data.keyword.cloudant}} 数据更改订阅源，用于每次在数据库中添加或修改文档时，触发触发器事件
- Git 订阅源，用于针对 Git 存储库中的每次落实，触发触发器事件

### 规则
{: #openwhisk_rules_use}

规则将触发器与操作关联在一起。
{: shortdesc}

每次触发器触发时，规则都会将触发器事件作为输入，并调用关联的操作。使用相应的规则集时，可以通过单个触发器事件来调用多个操作，也可以调用一个操作以响应来自多个触发器的事件。

例如，假设一个系统包含以下操作。
- `classifyImage` - 此操作用于检测图像中的对象并对其进行分类。
- `thumbnailImage` - 此操作用于创建图像的缩略图版本。

此外，假设有两个事件源将触发以下触发器。
- `newTweet` - 此触发器在发布新推文时触发。
- `imageUpload` - 此触发器在将图像上传到 Web 站点时触发。

可以设置规则，以使单个触发器事件调用多个操作，以及使多个触发器调用同一操作。
- `newTweet -> classifyImage` 规则
- `imageUpload -> classifyImage` 规则
- `imageUpload -> thumbnailImage` 规则

这三个规则确定以下行为。
- 对两个推文中的图像分类。
- 对上传的图像分类。
- 生成缩略图版本。

## 创建触发器作为事件的通道
{: #openwhisk_triggers_fire}

以下步骤说明如何创建用于发送用户位置更新的示例触发器，以及如何手动触发该触发器。

1. 创建触发器。触发器必须直接在名称空间中进行创建，而不能在包内部进行创建。
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    示例输出：
    ```
    ok: created trigger locationUpdate
  ```
    {: screen}

2. 验证是否已创建触发器。
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
    触发器充当可在其中触发事件的指定通道。

3. 触发触发器事件。
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    由于不存在与此触发器相关联的规则，因此传递的参数不会用作任何操作的输入。示例输出：
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
    {: screen}

在下一部分中，可以通过创建规则将触发器与操作相关联。

## 使用规则将触发器与操作相关联
{: #openwhisk_rules_assoc}

规则用于将触发器与操作关联在一起。每次触发触发器事件时，都会通过触发器事件中的参数来调用相应操作。

以下步骤说明在创建 [`locationUpdate` 触发器](#openwhisk_triggers_fire)后，如何创建示例规则，用于在每次发布位置更新时都调用 `hello` 操作。

1. 创建名为“hello.js”的文件并包含以下操作码：
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
    {: pre}

2. 创建 `hello` 操作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. 创建 `myRule` 规则，用于将 `locationUpdate` 触发器与 `hello` 操作相关联。规则必须直接在名称空间中进行创建，而不能在包内部进行创建。
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. 触发 `locationUpdate` 触发器。每次发生触发器事件时，都会通过事件参数来调用 `hello` 操作。
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    示例输出：
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
    {: screen}

5. 通过检查最新的激活记录来验证是否调用了 `hello` 操作。
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

6. 从上一个命令输出中获取有关激活标识的更多信息。
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    示例输出：
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
您将看到 `hello` 操作收到了事件有效内容，并返回了期望的字符串。

7. 要禁用规则，可以运行以下命令。
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

您还可以使用规则将触发器与序列相关联。例如，可以创建由规则 `anotherRule` 激活的名为 `recordLocationAndHello` 的操作序列。
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
