---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

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



# 通过规则关联触发器和操作
{: #rules}

每次触发器触发时，规则都会将触发器事件作为输入，并调用关联的操作。使用相应的规则集时，可以通过单个触发器来调用多个操作，也可以调用一个操作以响应来自多个触发器的事件。
{: shortdesc}


## 通过 UI 创建规则
{: #rules_ui}

在用户界面中，系统已创建用于关联操作和触发器的规则。
{: shortdesc}

创建操作或触发器或者访问其详细信息时，可以选择连接现有或新的操作或触发器。建立连接时，系统会创建规则，并以 `ACTION_NAME-TRIGGER_NAME` 格式对其命名。

在 CLI 中，可以运行 `ibmcloud fn rule list` 来验证是否已创建该规则。


## 通过 CLI 创建规则
{: #rules_create}

规则用于将触发器与操作关联在一起。每次触发触发器事件时，都会通过触发器事件中的参数来调用相应操作。

开始之前，请先创建[操作](/docs/openwhisk?topic=cloud-functions-actions)和[触发器](/docs/openwhisk?topic=cloud-functions-triggers)。


创建规则以将触发器与操作相关联。规则必须直接在名称空间中进行创建，而不能在包内部进行创建。
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


要禁用规则，可以运行以下命令。
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## 为操作序列创建规则
{: #rules_seq}

还可以使用规则将触发器与操作序列关联在一起。

开始之前，请先创建[操作序列](/docs/openwhisk?topic=cloud-functions-actions#actions_seq)和[触发器](/docs/openwhisk?topic=cloud-functions-triggers)。

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## 关联多个触发器和操作
{: #rules_assoc}

可以通过为操作和触发器的每种组合创建一个规则，从而使用不同的组合。操作和触发器无需按 1:1 的比率组合。

例如，假设使用以下操作。

|操作|描述|
| --- | --- |
|`classifyImage`|此操作用于检测图像中的对象并对其进行分类。|
|`thumbnailImage`|此操作用于创建图像的缩略图版本。|

此外，假设有两个事件源将触发以下触发器。

|触发器|描述|
| --- | --- |
|`newTweet`|此触发器在发布新推文时触发。|
|`imageUpload`|此触发器在将图像上传到 Web 站点时触发。|

可以设置规则，以使单个触发器事件调用多个操作，以及使多个触发器调用同一操作。
- `newTweet -> classifyImage` 规则
- `imageUpload -> classifyImage` 规则
- `imageUpload -> thumbnailImage` 规则

这三个规则确定以下行为。
- 对两个推文中的图像分类。
- 对上传的图像分类。
- 生成缩略图版本。

