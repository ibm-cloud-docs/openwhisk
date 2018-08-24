---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 開始使用
{: #index}

{{site.data.keyword.openwhisk}} 是以 Apache OpenWhisk 為基礎的多國語言「功能即服務 (FaaS)」程式設計平台。{{site.data.keyword.openwhisk_short}} 可讓開發人員撰寫可擴充執行應用程式邏輯的輕量型程式碼（稱為動作）。您可以根據來自 Web 應用程式或行動應用程式的 HTTP 型 API 要求設定依需求發生動作，或自動從 {{site.data.keyword.Bluemix_notm}} 服務及協力廠商事件回應事件型要求。
{: shortdesc}

因為 {{site.data.keyword.openwhisk_short}} 是一種無伺服器的事件驅動平台，所以您不需要明確地佈建伺服器。使用微服務、行動、IoT 及其他許多應用程式的開發人員，都可以專注在撰寫應用程式邏輯，而不用擔心自動擴充、高可用性、更新及維護。現成可用的自動擴充及負載平衡表示您不需要手動配置叢集、HTTP 外掛程式等等！IBM 會處理所有硬體、網路及軟體管理。您要做的只是提供程式碼。

按一下選項以開始使用：

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="按一下圖示，馬上開始使用 {{site.data.keyword.openswhisk_short}}。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="建立動作" title="建立動作" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式" title="設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="查看平台架構" title="查看平台架構" shape="rect" coords="326, -10, 448, 218" />
</map>


## 在 GUI 中建立動作
{: #openwhisk_start_hello_world}

若要開始使用 {{site.data.keyword.openwhisk_short}}，請嘗試使用 HelloWorld 快速入門範本。

1.  從 [{{site.data.keyword.Bluemix_notm}} **型錄** ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/?category=whisk) **函數**種類中，按一下 Functions。

2. 按一下**開始建立** > **快速入門範本**，然後選取 **Hello World** 範本。

5. 查看動作的程式碼，然後按一下**部署**來建立動作。您現在會有一個稱為 `hello` 的動作。

6. 按一下**呼叫**來執行此動作。手動呼叫動作，會執行動作所定義的應用程式邏輯。在**啟動**畫面中，您可以看到動作所產生的 "Hello stranger!" 問候語。

做得好！您已建立第一個動作。若要清除此動作，請按一下溢位功能表，然後選取**刪除動作**。

## 在 CLI 中建立動作
{: #openwhisk_start_hello_world_cli}

使用 HelloWorld JavaScript 範例程式碼來快速開始進行。此範例會建立基本 `hello` 動作，您可以手動呼叫它來執行其應用程式邏輯。

1. [設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式](bluemix_cli.html)。

2. 將下列程式碼儲存至名為 **hello.js** 的檔案中。

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

3. 建立 `hello` 動作。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. 呼叫動作，但未傳遞任何參數。
    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    輸出：
        ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. 重新呼叫動作並傳遞 name 參數，以測試應用程式邏輯。
    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    輸出：
        ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

做得好！您已建立第一個動作。若要清除此動作，請執行 `ibmcloud fn action delete hello`。

**下一步為何？**
* [試用警示服務範例，以在每次產生定期事件時呼叫 **hello** 動作。](./openwhisk_packages.html#openwhisk_package_trigger)
* [建立無伺服器 REST API。](openwhisk_apigateway.html)
* [查看 {{site.data.keyword.Bluemix_notm}} 服務（例如 Cloudant）的預先安裝動作套件。](cloudant_actions.html)
