---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 開始使用
{: #getting_started}

使用 {{site.data.keyword.openwhisk}}，您可以使用最愛的程式設計語言來撰寫輕量型程式碼，以可擴充方式來執行應用程式邏輯。您可以從應用程式隨需使用 HTTP 型 API 要求來執行程式碼，或自動執行以回應 {{site.data.keyword.Bluemix_notm}} 服務及協力廠商事件。「函數即服務 (Faas)」程式設計平台是以開放程式碼專案 Apache OpenWhisk 為基礎。
{: shortdesc}

因為 {{site.data.keyword.openwhisk_short}} 無伺服器，所以您不會受限於您可以使用的語言，而且不需要花費時間明確地佈建後端基礎架構。您可以專注在撰寫應用程式邏輯，而不用擔心自動擴充、高可用性、更新或維護。IBM 預設會提供硬體、網路、軟體管理、負載平衡、外掛程式等。您只需要攜帶程式碼！

## 使用動作
{: #creating_actions}
{: #openwhisk_actions}

使用 {{site.data.keyword.openwhisk}}，您可以建立無狀態程式碼 Snippet，而無狀態程式碼 Snippet 設定成執行一個稱為動作的特定作業。
{:shortdesc}

**何謂動作？**

動作是可以呼叫或設定為自動執行以回應事件的一小部分程式碼。在任一情況下，每次執行都會導致由唯一啟動 ID 所識別的記錄。可以將動作的輸入及結果視為鍵值組。索引鍵是字串，而值是有效的 JSON 值。您可以使用所選擇的語言來撰寫動作，並將動作當成原始碼或 Docker 映像檔提供給服務。Cloud Functions API、CLI 或 iOS SDK 直接呼叫動作碼時，即會執行此動作碼。動作可以自動回應來自 IBM Cloud 或協力廠商服務的事件。

**為何要使用動作？**

使用動作，即可限制程式碼所執行的時間量，從而降低額外成本。

例如，您可以使用動作來偵測影像中的臉孔、回應資料庫中的變更、聚集一組 API 呼叫，甚至張貼推文。

**一次可以使用多個動作嗎？**

是！您可以使用動作來呼叫其他動作，也可以將動作串連在一起以建立序列。若要讓這項作業運作，一個動作的輸出將會是另一個動作的輸入，而後者提供可用來觸發另一個動作的輸出，依此類推。您甚至可以組合您建立以形成套件的動作群組。使用套件，您可以呼叫套件來重複使用一般動作或序列，而不需要重新配置動作或序列。

按一下選項以開始使用：

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="按一下圖示，以快速開始使用 {{site.data.keyword.openwhisk_short}}。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="建立動作" title="建立動作" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式" title="設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="查看平台架構" title="查看平台架構" shape="rect" coords="326, -10, 448, 218" />
</map>

## 在 GUI 中建立動作
{: #openwhisk_start_hello_world}

若要開始使用 {{site.data.keyword.openwhisk_short}}，請嘗試使用 HelloWorld 快速入門範本。

1.  造訪 [{{site.data.keyword.Bluemix_notm}} **型錄** ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/openwhisk) 中的 Functions 儀表板。

2. 按一下**開始建立** > **快速入門範本**，然後選取 **Hello World** 範本。

3. 在**套件名稱**欄位中輸入唯一名稱，以建立動作的套件。

4. 在**動作 helloworld** 區段中，從下拉清單中選取運行環境。在部署範本之前，您可以在每個可用的運行環境中預覽範例動作的程式碼。

5. 按一下**部署**。您已建立動作。做得很好！

6. 按一下**呼叫**來執行此動作。手動呼叫動作，會執行動作所定義的應用程式邏輯。在**啟動**畫面中，您可以看到動作所產生的 "Hello stranger!" 問候語。

7. 選用項目：按一下**變更輸入**以變更動作，或嘗試您自己的動作。然後，按一下**呼叫**，以使用您的更新項目來執行動作。結果會顯示在**啟動**畫面中。您可以依需要多次重複此處理程序。

做得好！您已建立第一個動作。若要清除此動作，請按一下溢位功能表，然後選取**刪除動作**。

## 在 CLI 中建立動作
{: #openwhisk_start_hello_world_cli}

使用 HelloWorld JavaScript 範例程式碼來快速開始進行。此範例會建立基本 `hello` 動作，您可以手動呼叫它來執行其應用程式邏輯。

1. [設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)。

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

## 下一步為何？
{: #getting_started_next_steps}

您已完成第一個範本部署，接下來要做什麼？您可以：

* 熟悉[術語](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology)。
* 開始使用[您自己的動作](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)。
* 瞭解如何將動作組織至[套件](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)。
* 進階選項：建立[無伺服器 REST API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)。
