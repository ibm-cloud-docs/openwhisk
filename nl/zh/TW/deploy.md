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

# 使用資訊清單檔部署實體
{: #deploy}

您可以使用 {{site.data.keyword.openwhisk_short}}，以使用利用 YAML 撰寫的資訊清單檔來說明及部署所有名稱空間實體。可以使用此檔案透過一個指令部署所有 Functions [套件](/docs/openwhisk?topic=cloud-functions-pkg_ov)、[動作](/docs/openwhisk?topic=cloud-functions-actions)、[觸發程式](/docs/openwhisk?topic=cloud-functions-triggers)和規則](/docs/openwhisk?topic=cloud-functions-rules)。

資訊清單檔說明您要部署及取消部署為群組的實體集。資訊清單檔內容必須遵循 [OpenWhisk 部署 YAML 規格](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification)。定義之後，您可以使用資訊清單檔，將一組 Functions 實體部署或重新部署至相同或不同的 Functions 名稱空間。您可以使用 Functions 外掛程式指令 `ibmcloud fn deploy` 及 `ibmcloud fn undeploy` 來部署及取消部署資訊清單檔中所定義的 Functions 實體。

## 建立 Hello World API 範例
{: #deploy_helloworld_example}

此範例採用某個簡單 Node.js 程式碼 (`helloworld.js`)、在套件 (`hello_world_package`) 內建立一個 Web 動作 (`hello_world`)，並定義此動作的 REST API。
{: shortdesc}

1. 使用下列程式碼，建立 `helloworld.js` 檔案。

    ```javascript
function main() {
    return {body: 'Hello world'};
}
```
    {: codeblock}

    部署資訊清單檔會定義下列變數。
    * 套件名稱。
    * 動作名稱。
    * 指出它是 Web 動作的動作註釋。
    * 動作碼檔案名稱。
    * 基礎路徑為 `/hello` 的 API。
    * `/world` 的端點路徑。

2. 建立 `hello_world_manifest.yml` 檔案。

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

3. 使用 `deploy` 指令來部署套件、動作及 API。

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. 您可以列出動作、套件及 API，以確認已順利建立三個預期的實體。


    1. 使用下列指令來列出動作。

      ```sh
    ibmcloud fn action list
    ```
      {: pre}

    2. 使用下列指令來列出套件。

      ```sh
    ibmcloud fn package list
    ```
      {: pre}

    3. 使用下列指令來列出 API。

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. 呼叫 API。

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

選用：可以使用 `undeploy` 指令來取消部署這些實體。

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## 其他 OpenWhisk 部署範例
{: more_deploy_examples}

Functions 部署是以 OpenWhisk 部署專案為基礎，其具有可在 Functions 內使用的[多個部署資訊清單範例](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples)。您可以使用 `ibmcloud fn deploy` 指令，而非 `wskdeploy`。

## 部署資訊清單規格
{: manifest_specification}

Functions 部署資訊清單必須遵循 OpenWhisk 部署資訊清單規格。如需詳細資料，請參閱 [OpenWhisk 部署資訊清單規格文件](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification)。
