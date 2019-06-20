
---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: getting started, creating actions, invoking actions,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# 入门
{: #getting-started}

通过 {{site.data.keyword.openwhisk}}，您可以使用自己喜欢的编程语言来编写轻量代码，用于以可缩放方式运行应用程序逻辑。您可以使用来自应用程序的基于 HTTP 的 API 请求按需运行代码，也可以根据对 {{site.data.keyword.Bluemix_notm}} 服务和第三方事件的响应来运行代码。函数即服务 (Faas) 编程平台基于开放式源代码项目 Apache OpenWhisk。
{: shortdesc}

## 使用操作
{: #gs_actions}

通过 {{site.data.keyword.openwhisk}}，可以创建设置为执行特定任务（称为操作）的无状态代码片段。
要了解有关操作和其他 Functions 术语的更多信息，请参阅[术语](/docs/openwhisk?topic=cloud-functions-about)。
{:shortdesc}

单击某个选项以开始：

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="单击某个图标以迅速开始使用 {{site.data.keyword.openwhisk_short}}。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="创建操作" title="创建操作" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="设置 {{site.data.keyword.openwhisk_short}} CLI 插件" title="设置 {{site.data.keyword.openwhisk_short}} CLI 插件" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="查看平台体系结构" title="查看平台体系结构" shape="rect" coords="326, -10, 448, 218" />
</map>

## 在 GUI 中创建操作
{: #gs_hello_world}

要开始使用 {{site.data.keyword.openwhisk_short}}，请尝试使用 HelloWorld 快速入门模板。

1. 创建 [{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration) 帐户或登录到现有帐户。

2. 导航至 [{{site.data.keyword.openwhisk_short}} 仪表板 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk)。

2. 单击**开始创建** > **快速入门模板**，然后选择 **Hello World** 模板。

3. 通过在**包名**字段中输入唯一名称，为操作创建包。

4. 在**操作 helloworld** 部分中，从下拉列表中选择运行时。在部署模板之前，可以在每个可用运行时中预览样本操作的代码。

5. 单击**部署**。您已创建操作。太棒了！

6. 通过单击**调用**来运行该操作。调用操作会手动执行该操作所定义的应用程序逻辑。在**激活**面板中，可以看到该操作生成的“您好，新用户！”问候语。

7. 可选：单击**更改输入**以更改操作或尝试您自己的操作。

  a. 将以下代码粘贴到**更改输入**框中，然后更改 name 值。
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. 然后，单击**调用**以运行包含更新的操作。结果会显示在**激活**面板中。可以根据需要多次重复此过程。

非常好！您已创建第一个操作。要清除此操作，请单击溢出菜单并选择**删除操作**。

## 在 CLI 中创建操作
{: #gs_hello_world_cli}

快速入门并熟练运用 [HelloWorld JavaScript](/docs/openwhisk?topic=cloud-functions-prep#prep-js) 示例代码。此示例会创建一个基本 `hello` 操作，您可以手动调用该操作以运行其应用程序逻辑。

## 接下来要做什么？
{: #gs_next_steps}

现在，您已完成了第一个模板部署，接下来要做什么呢？您可以执行以下操作：

* 熟悉[术语](/docs/openwhisk?topic=cloud-functions-about#about_technology)。
* 开始使用[您自己的操作](/docs/openwhisk?topic=cloud-functions-actions)。
* 了解有关在[包](/docs/openwhisk?topic=cloud-functions-pkg_ov)中组织操作的信息。
* 高级选项：创建[无服务器 REST API](/docs/openwhisk?topic=cloud-functions-apigateway)。
