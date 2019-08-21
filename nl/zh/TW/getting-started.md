---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions, ibm cloud functions, cloud functions, getting started, creating actions

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


# IBM {{site.data.keyword.openwhisk_short}} 開始使用
{: #getting-started}

使用 {{site.data.keyword.openwhisk}}，您可以使用最愛的程式設計語言來撰寫輕量型程式碼，以可擴充方式來執行應用程式邏輯。您可以使用來自應用程式且以 HTTP 為基礎的 API 要求依需要執行程式碼，也可以執行程式碼以回應 {{site.data.keyword.cloud_notm}} 服務及協力廠商事件。「函數即服務 (Faas)」程式設計平台是以開放程式碼專案 Apache OpenWhisk 為基礎。
{: shortdesc}

## 使用動作
{: #gs_actions}

使用 {{site.data.keyword.openwhisk}}，您可以建立無狀態程式碼 Snippet，而無狀態程式碼 Snippet 設定成執行一個稱為動作的特定作業。
若要瞭解有關動作和其他 Functions 術語的更多資訊，請參閱[術語](/docs/openwhisk?topic=cloud-functions-about)。
{:shortdesc}

按一下某個選項以開始。

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="按一下圖示，以快速開始使用 {{site.data.keyword.openwhisk_short}}。" style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="建立動作" title="建立動作" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式" title="設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="查看平台架構" title="查看平台架構" shape="rect" coords="326, -10, 448, 218" />
</map>

## 在 GUI 中建立動作
{: #gs_hello_world}

若要開始使用 {{site.data.keyword.openwhisk_short}}，請嘗試建立 Hello World 快速開始使用範本。

1. 建立 [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/registration){: external} 帳戶或登入到現有帳戶。

2. 導覽至 [{{site.data.keyword.openwhisk_short}} 儀表板](https://cloud.ibm.com/openwhisk){: external}。

2. 按一下**開始建立** > **快速入門範本**，然後選取 **Hello World** 範本。

3. 在**套件名稱**欄位中輸入唯一名稱，以建立動作的套件。

4. 在**動作 helloworld** 區段中，從下拉功能表中選取運行環境。在部署範本之前，您可以在每個可用的運行環境中預覽範例動作的程式碼。

5. 按一下**部署**。您已建立動作。做得很好！

6. 按一下**呼叫**來執行此動作。手動呼叫動作，會執行動作所定義的應用程式邏輯。在**啟動**畫面中，可以看到該動作產生的「新使用者您好！」問候語。

7. 選用項目：按一下**變更輸入**以變更動作，或嘗試您自己的動作。

  a. 將下列程式碼貼上到**變更輸入**方框中，然後變更 name 值。
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. 然後，按一下**呼叫**，以使用您的更新項目來執行動作。結果會顯示在**啟動**畫面中。您可以依需要多次重複此處理程序。

做得好！您已建立第一個動作。若要清除此動作，請按一下溢位功能表，然後選取**刪除動作**。

## 在 CLI 中建立動作
{: #gs_hello_world_cli}

快速入門和熟悉運用 [Hello World JavaScript](/docs/openwhisk?topic=cloud-functions-prep#prep-js) 程式碼範例。此範例會建立基本 `hello` 動作，您可以手動呼叫它來執行其應用程式邏輯。

## 下一步為何？
{: #gs_next_steps}

現在，您已完成了第一個範本部署，接下來要做什麼呢？

* 熟悉[術語](/docs/openwhisk?topic=cloud-functions-about#about_technology)。
* 開始使用[您自己的動作](/docs/openwhisk?topic=cloud-functions-actions)。
* 瞭解如何將動作組織至[套件](/docs/openwhisk?topic=cloud-functions-pkg_ov)。
* 進階選項 - 建立[無伺服器 REST API](/docs/openwhisk?topic=cloud-functions-apigateway)。



