---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 入门
{: #index}

通过 {{site.data.keyword.openwhisk}}，您可以使用自己喜欢的编程语言来编写轻量代码，用于以可缩放方式运行应用程序逻辑。您可以使用来自应用程序的基于 HTTP 的 API 请求按需运行代码，也可以自动响应 {{site.data.keyword.Bluemix_notm}} 服务和第三方事件。函数即服务 (Faas) 编程平台基于开放式源代码项目 Apache OpenWhisk。
{: shortdesc}

由于 {{site.data.keyword.openwhisk_short}} 是无服务器的，因此对您可以使用的语言没有限制，而且您不必花时间显式供应后端基础架构。您可以专注于编写应用程序逻辑，而不必担心自动缩放、高可用性、更新或维护。IBM 提供了开箱即用的硬件、网络、软件管理、负载均衡、插件等。您只需提供代码即可！

## 使用操作
{: #creating_actions}
{: #openwhisk_actions}

通过 {{site.data.keyword.openwhisk}}，可以创建设置为执行特定任务（称为操作）的无状态代码片段。
{:shortdesc}

**什么是操作？**
{: #what_is_an_action}

操作是一小段代码，可以调用或设置为自动运行以响应事件。对于其中任一情况，每次运行都会生成一个由唯一激活标识进行识别的记录。操作的输入和结果可以视为键/值对。键为字符串，值为有效的 JSON 值。可以使用所选语言来编写操作，并将其作为源代码或 Docker 映像提供给服务。操作代码在由 Cloud Functions API、CLI 或 iOS SDK 直接调用时运行。操作可以自动响应来自 IBM Cloud 或第三方服务的事件。

**为什么要使用操作？**
{: #why_use_an_action}

通过使用操作，可以限制代码运行时间量，从而降低间接成本。

例如，可以使用操作来检测映像中的构面，响应数据库中的更改，聚集一组 API 调用，或者甚至可发布推文。

**一次可以使用多个操作吗？**
{: #more_than_one_action}

可以！您可以使用操作来调用其他操作，也可以将操作串接在一起以创建序列。要创建序列，一个操作的输出将是另一个操作的输入，而这另一个操作提供的输出又可用于触发另一个操作，依此类推。您甚至可以捆绑所创建的操作组来构成包。使用包时，可以通过调用包来复用常用操作或序列，而不用重新配置操作或序列。

单击某个选项以开始：

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="单击某个图标以迅速开始使用 {{site.data.keyword.openwhisk_short}}。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="/docs/openwhisk?topic=cloud-functions-index#openwhisk_start_hello_world" alt="创建操作" title="创建操作" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="设置 {{site.data.keyword.openwhisk_short}} CLI 插件" title="设置 {{site.data.keyword.openwhisk_short}} CLI 插件" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="查看平台体系结构" title="查看平台体系结构" shape="rect" coords="326, -10, 448, 218" />
</map>

## 在 GUI 中创建操作
{: #openwhisk_start_hello_world}

要开始使用 {{site.data.keyword.openwhisk_short}}，请尝试使用 HelloWorld 快速入门模板。

1.  在 [{{site.data.keyword.Bluemix_notm}} **目录** ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk) 中访问 Functions 仪表板。

2. 单击**开始创建** > **快速入门模板**，然后选择 **Hello World** 模板。

3. 通过在**包名**字段中输入唯一名称，为操作创建包。

4. 在**操作 helloworld** 部分中，从下拉列表中选择运行时。在部署模板之前，可以在每个可用运行时中预览样本操作的代码。

5. 单击**部署**。您已创建操作。太棒了！

6. 通过单击**调用**来运行该操作。调用操作会手动执行该操作所定义的应用程序逻辑。在**激活**面板中，可以看到该操作生成的“您好，新用户！”问候语。

7. 可选：单击**更改输入**以更改操作或尝试您自己的操作。然后，单击**调用**以运行包含更新的操作。结果会显示在**激活**面板中。可以根据需要多次重复此过程。

非常好！您已创建第一个操作。要清除此操作，请单击溢出菜单并选择**删除操作**。

## 在 CLI 中创建操作
{: #openwhisk_start_hello_world_cli}

快速入门并熟练运用 HelloWorld JavaScript 示例代码。此示例会创建一个基本 `hello` 操作，您可以手动调用该操作以运行其应用程序逻辑。

1. [设置 {{site.data.keyword.openwhisk_short}} CLI 插件](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)。

2. 将以下代码保存到名为 **hello.js** 的文件。

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
function main(params) {
    var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. 创建 `hello` 操作。
    

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. 在不使用任何参数的情况下调用该操作。


    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    输出：
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. 在传递 name 参数的情况下再次调用该操作，以测试应用程序逻辑。
    

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    输出：
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

非常好！您已创建第一个操作。要清除此操作，请运行 `ibmcloud fn action delete hello`。

## 接下来要做什么？
{: #next-steps}

现在，您已完成了第一个模板部署，接下来要做什么呢？您可以执行以下操作：

* 熟悉[术语](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology)。
* 开始使用[您自己的操作](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)。
* 了解有关在[包](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)中组织操作的信息。
* 高级选项：创建[无服务器 REST API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)。
