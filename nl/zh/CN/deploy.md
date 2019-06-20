---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: deploying actions, manifest, manifest file

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

# 使用清单文件部署实体
{: #deploy}

您可以使用 {{site.data.keyword.openwhisk_short}} 通过用 YAML 编写的清单文件来描述和部署所有名称空间实体。可以使用此文件通过一个命令部署所有 Functions [包](/docs/openwhisk?topic=cloud-functions-pkg_ov)、[操作](/docs/openwhisk?topic=cloud-functions-actions)、[触发器](/docs/openwhisk?topic=cloud-functions-triggers)和规则](/docs/openwhisk?topic=cloud-functions-rules)。

清单文件描述了要作为组部署和取消部署的实体集。清单文件内容必须遵循 [OpenWhisk 部署 YAML 规范](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification)。清单文件定义后，可以用于将一组 Functions 实体部署或重新部署到相同或不同的 Functions 名称空间中。可以使用 Functions 插件命令 `ibmcloud fn deploy` 和 `ibmcloud fn undeploy` 来部署和取消部署清单文件中定义的 Functions 实体。

## 创建 Hello World API 示例
{: #deploy_helloworld_example}

此示例采用一些简单的 Node.js 代码 (`helloworld.js`)，在包 (`hello_world_package`) 内创建 Web 操作 (`hello_world`)，并为此操作定义 REST API。
{: shortdesc}

1. 创建包含以下代码的 `helloworld.js` 文件。

    ```javascript
function main() {
    return {body: 'Hello world'};
}
```
    {: codeblock}

    该部署清单文件定义了以下变量。
    * 包名。
    * 操作名称。
    * 指示该操作为 Web 操作的操作注释。
    * 操作代码文件名。
    * 使用基本路径 `/hello` 的 API。
    * 端点路径 `/world`。

2. 创建 `hello_world_manifest.yml` 文件。

    ```yaml
packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
```
    {: codeblock}

3. 使用 `deploy` 命令来部署包、操作和 API。

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. 可以列出操作、包和 API，以确认这三种所需实体是否已成功创建。


    1. 使用以下命令来列出操作。

      ```sh
      ibmcloud fn action list
      ```
      {: pre}

    2. 使用以下命令来列出包。

      ```sh
      ibmcloud fn package list
      ```
      {: pre}

    3. 使用以下命令来列出 API。

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. 调用 API。

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

可选：可以使用 `undeploy` 命令来取消部署这些实体。

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## 其他 OpenWhisk 部署示例
{: more_deploy_examples}

Functions 部署基于 OpenWhisk 部署项目，该项目具有可在 Functions 中使用的[多个部署清单示例](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples)。您可以使用 `ibmcloud fn deploy` 命令，而不使用 `wskdeploy`。

## 部署清单规范
{: manifest_specification}

Functions 部署清单必须遵循 OpenWhisk 部署清单规范。请参阅 [OpenWhisk 部署清单规范文档](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification)以获取详细信息。
