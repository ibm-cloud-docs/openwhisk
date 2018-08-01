---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 入门
{: #index}

{{site.data.keyword.openwhisk}} 是一种基于 Apache OpenWhisk 的多语言功能即服务 (FaaaS) 编程平台。{{site.data.keyword.openwhisk_short}} 支持开发者编写能以可扩展方式执行应用程序逻辑的轻量级代码（称为“操作”）。可以将操作设置为根据来自 Web 应用程序或移动应用程序的基于 HTTP 的 API 请求按需执行，或者自动执行以响应来自 {{site.data.keyword.Bluemix_notm}} 服务和第三方事件的基于事件的请求。
{: shortdesc}

由于 {{site.data.keyword.openwhisk_short}} 是一种事件驱动型无服务器平台，因此无需显式供应服务器。使用微服务、移动、IoT 和其他许多应用程序的开发者可以专注于编写应用程序逻辑，而不必担心自动扩展、高可用性、更新和维护。通过开箱即用的自动扩展和负载均衡功能，您不必手动配置集群、HTTP 插件等！IBM 会负责所有硬件、网络和软件管理。您只需要提供代码即可。

单击某个选项以开始：

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="单击某个图标以迅速开始使用 {{site.data.keyword.openswhisk_short}}。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="创建操作" title="创建操作" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="设置 {{site.data.keyword.openwhisk_short}} CLI 插件" title="设置 {{site.data.keyword.openwhisk_short}} CLI 插件" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="查看平台体系结构" title="查看平台体系结构" shape="rect" coords="326, -10, 448, 218" />
</map>


## 在 GUI 中创建操作
{: #openwhisk_start_hello_world}

要开始使用 {{site.data.keyword.openwhisk_short}}，请尝试使用 HelloWorld 快速入门模板。

1.  在 [{{site.data.keyword.Bluemix_notm}} **目录** ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/?category=whisk) 的**功能**类别中，单击 Functions。

2. 单击**开始创建** > **快速入门模板**，然后选择 **Hello World** 模板。

5. 查看操作的代码，然后通过单击**部署**来创建操作。现在，您具有一个名为 `hello` 的操作。

6. 通过单击**调用**来运行该操作。调用操作会手动执行该操作所定义的应用程序逻辑。在**激活**面板中，可以看到该操作生成的“您好，新用户！”问候语。

非常好！您已创建第一个操作。要清除此操作，请单击溢出菜单并选择**删除操作**。

## 在 CLI 中创建操作
{: #openwhisk_start_hello_world_cli}

快速入门和熟悉运用 HelloWorld JavaScript 示例代码。此示例会创建一个基本 `hello` 操作，您可以手动调用该操作以运行其应用程序逻辑。

1. [设置 {{site.data.keyword.openwhisk_short}} CLI 插件](bluemix_cli.html)。

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

**接下来要做什么？**
* [试用警报服务示例，以在每次生成定期事件时都调用 **hello** 操作](./openwhisk_packages.html#openwhisk_package_trigger)。
* [创建无服务器 REST API](openwhisk_apigateway.html)。
* [查看 Cloudant 等 {{site.data.keyword.Bluemix_notm}} 服务的预安装操作包](cloudant_actions.html)。
