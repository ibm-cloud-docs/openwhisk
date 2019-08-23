---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# 規劃無伺服器 APP
{: #plan}

開始建立函數之前，請先瞭解在此過程中必須進行的決策。
{: shortdesc}

## 檢閱運行環境支援
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} 提供了標準[運行環境](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)來執行程式碼。但是，由於 {{site.data.keyword.openwhisk_short}} 是無伺服器的，因此對您可以使用的運行環境沒有限制。您可以透過建立自己的[自訂 Docker 映像檔](/docs/openwhisk?topic=cloud-functions-actions#actions-docker)來包裝程式碼，以便能建立自訂運行環境。
{: shortdesc}



## 建構程式碼
{: #plan_architect}

您可能需要對現有程式碼進行一些調整，使其能在 {{site.data.keyword.openwhisk}} 中執行。如果您尚未有程式碼，請在撰寫程式碼時牢記以下事項。
{: shortdesc}

1. 對程式碼進行元件化。

    如果您已有一個應用程式並且要使其成為無伺服器的，請考慮可能需要如何將該應用程式分解成更小的部分。每個函數都包含一組針對您要執行的程式碼的觸發程式。 例如，如果在 GitHub 上建立了問題，請執行此 JavaScript 程式碼。如果應用程式包含其中多個事件或動作，請考慮將其分成不同的函數。

2. 使用 {{site.data.keyword.cloud_notm}} 服務，而不使用架構。

    可以不要使用架構來使您在運行環境的功能中使用這些能力，而改為使用 {{site.data.keyword.cloud}} 服務。架構可協助您完成的許多常見作業，在 {{site.data.keyword.cloud}} 上作為服務提供。
    {: shortdesc}

    例如，不使用架構進行鑑別，而改為嘗試 {{site.data.keyword.appid_full}}。如果需要外部檔案儲存空間，請嘗試 {{site.data.keyword.cos_full}}。

    如果要合併的功能未作為 {{site.data.keyword.cloud}} 上的服務提供，您一律可以將該功能與資訊來源和 API 整合，而無需架構。

3. [驗證碼是否符合系統限制](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)。

    程式碼必須符合系統限制才能執行。例如，如果包裝的程式碼大於 48 MB，則可能需要繼續將其分解成更小的部分，或將其包裝為 Docker 映像檔。

    包含許多協力廠商模組、原生程式庫或外部工具的應用程式可能會達到此限制。如果您建立了大於 48 MB 的 .zip 或 .jar 套件動作，則必須使用相依關係來延伸運行環境映像檔。然後，使用單一原始檔或小於 48 MB 的保存。例如，透過建置包含必要共用程式庫的自訂 Docker 運行環境，相依關係並不需要存在於保存檔中。專用原始檔仍可組合在保存檔中，並在運行環境注入。

4. 確定必須注入到程式碼中的參數。

    在無伺服器動作中，資料是藉由將參數新增至動作來提供。參數宣告為主要無伺服器函數的引數。通常，這些參數是服務的認證，但也可能是使程式碼可重複使用於不同觸發程式的任何內容。

5. [驗證在函數中使用程式碼的結構需求](/docs/openwhisk?topic=cloud-functions-prep)。

    無論您是已有應用程式還是計劃開發 Script 來使用，程式碼都可能需要一些調整，使其可供 {{site.data.keyword.openwhisk}} 使用。程式碼本身必須符合一些結構需求，例如輸入參數和輸出結果。程式碼還可能需要包裝成單一檔案，該檔案中包含其所有相依關係。







## 確定事件來源
{: #plan_source}

考慮函數中與您希望發生以觸發程式碼執行的事件相關的方面。您可能希望在每次確定到 GitHub 儲存庫時執行程式碼。或者，您可能希望每次在 Cloudant 資料庫中進行更新時執行程式碼。
{: shortdesc}

選擇用於觸發函數的事件後，請檢閱可供使用的套件。您或許能夠使用其中一個套件來簡化函數開發。如果不能，您可以為事件來源建立自己的套件。

您可能需要回到程式碼，並根據事件選取進行一些修訂。


## 組織不同環境中的部署
{: #plan_environments}

決定如何在不同環境中部署函數，例如在開發環境、暫置環境和正式作業環境中。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 是一個以 Cloud Foundry 為基礎的服務，因此可以在 {{site.data.keyword.cloud_notm}} 中提供的 Cloud Foundry 組織和空間中管理函數部署。若要在這些環境中組織函數，可選擇為每個函數建立一個組織。然後，為所需的每個環境建立一個空間。您還可以改為每個環境擁有一個組織，並為每個函數建立一個空間。不管如何安排組織和空間，都請選擇可以用於有效管理函數實體的結構。

您還可以使用[名稱空間](/docs/openwhisk?topic=cloud-functions-namespaces)來隔離資源。依預設，每個 {{site.data.keyword.cloud_notm}} 空間都包含一個 Open Whisk 名稱空間。可以對實體（例如，名稱空間中的動作或觸發程式）進行分組，然後建立 Identity and Access (IAM) 原則來管理該群組的使用者許可權。




