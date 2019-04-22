---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}} 針對 {{site.data.keyword.Bluemix_notm}} CLI 提供功能強大的外掛程式，允許完全管理 {{site.data.keyword.openwhisk_short}} 系統。
您可以使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式來管理動作中程式碼 Snippet、建立觸發程式及規則以讓動作回應事件，並將動作組合為套件。
{:shortdesc}

您現在可以在 {{site.data.keyword.openwhisk_short}} 指令使用別名 `fn`：`ibmcloud fn <command>`
{: tip}

## 設定 {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

下載並安裝 {{site.data.keyword.Bluemix_notm}} CLI，然後登入。
{: shortdesc}

1. 下載並安裝 [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli)。

2. 登入 {{site.data.keyword.Bluemix_notm}} CLI。若要指定 IBM Cloud 地區，請[包括 API 端點](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)。

  ```
  ibmcloud login
  ```
  {: pre}

  您可以使用下列旗標，在登入時指定組織及空間，以跳過組織及空間的輸入提示：`ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. 如果您未指定組織及空間，請完成 login 指令之後的輸入提示。


## 設定 {{site.data.keyword.openwhisk_short}} 外掛程式
{: #cloudfunctions_plugin_setup}

若要使用 {{site.data.keyword.openwhisk_short}}，請下載並安裝 CLI 外掛程式。
{: shortdesc}

您可以使用外掛程式：

* 在 {{site.data.keyword.openwhisk_short}} 上，執行您的程式碼 Snippet 或動作。請參閱[建立及呼叫動作](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)。
* 使用觸發程式及規則，讓動作回應事件。請參閱[建立觸發程式及規則](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers)。
* 瞭解套件如何組合動作以及配置外部事件來源。請參閱[建立及使用套件](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)。
* 探索套件的型錄，以及使用外部服務（例如 [{{site.data.keyword.cloudant}} 事件來源](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant)）來加強應用程式。

若要查看您可以使用 {{site.data.keyword.openwhisk_short}} 外掛程式執行的所有作業，請執行不含任何引數的 `ibmcloud fn`。
{: tip}

1. 安裝 {{site.data.keyword.openwhisk_short}} 外掛程式。
    

  ```
    ibmcloud plugin install cloud-functions
    ```
  {: pre}

2. 驗證外掛程式已安裝。
    

  ```
    ibmcloud plugin list cloud-functions
    ```
  {: pre}

  輸出：
  ```
Plugin Name          Version
    Cloud-Functions      1.0.16
    ```
  {: screen}

已具有外掛程式，但需要更新？請執行 `ibmcloud plugin update cloud-functions`。
{:tip}



## 使用動作中的服務
{: #binding_services_cli}

{{site.data.keyword.openwhisk_short}} 提供 `service bind` 指令，讓您的 {{site.data.keyword.Bluemix_notm}} 服務認證可供您的程式碼在執行時期使用。您接著可以使用 `service bind` 指令，將任何 {{site.data.keyword.Bluemix_notm}} 服務連結至 {{site.data.keyword.openwhisk_short}} 中所定義的任何動作。

如需如何使用動作中服務的詳細步驟，請參閱[將服務連結至動作](/docs/openwhisk?topic=cloud-functions-binding_services)。


## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS Proxy
{: #cli_https_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以設為使用 HTTPS Proxy。若要設定 HTTPS Proxy，必須建立一個稱為 `HTTPS_PROXY` 的環境變數。變數必須設為 HTTPS Proxy 的位址，其埠使用下列格式：`{PROXY IP}:{PROXY PORT}`。



## 切換至不同的地區、組織及空間
{: #region_info}

如果您已登入，則可以在 {{site.data.keyword.Bluemix_notm}} CLI 中執行 `ibmcloud target` 指令，以切換地區、組織及空間。


若要建立及管理實體，您必須將目標設為名稱空間。在某些狀況下，可使用底線 (`_`) 表示的預設名稱空間，會對應至目前設為目標的 Cloud Foundry 型名稱空間。

您可以建立空間來處理正式作業前（暫置）和正式作業部署，作法是為每一個建立空間。建立空間可讓 {{site.data.keyword.openwhisk_short}} 擁有兩個為您定義的不同名稱空間。執行 [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create)，在組織下建立更多空間，例如 "staging" 及 "production"：

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} 對名稱空間的名稱有所限制。如需相關資訊，請參閱[系統詳細資料及限制](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities)文件。
{: tip}

**警告**：變更組織或空間的名稱會根據已變更的名稱來建立新的名稱空間。在新的名稱空間中看不到舊名稱空間中的實體，因此可能已被刪除。


## 從 OpenWhisk CLI 移轉至 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cli_migration}

您現在可以使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式，來與 {{site.data.keyword.openwhisk_short}} 實體互動。雖然您可以繼續使用獨立式 OpenWhisk CLI，但它沒有 {{site.data.keyword.openwhisk_short}} 所支援的最新特性，例如 IAM 型名稱空間及 `service bind`。
{: shortdesc}

### 指令語法
{: #command_syntax}

除了不再需要的 `wsk bluemix login` 指令之外，所有 `wsk` 指令都會使用 `ibmcloud fn` 指令，以相同的方式運作。Cloud Functions CLI 外掛程式中指令的所有指令選項及引數，都與 OpenWhisk 獨立式 CLI 的指令相同。如需相關資訊，請參閱 [Apache OpenWhisk CLI 專案 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk-cli)。

### API 鑑別及主機
{: #api_authentication}

OpenWhisk CLI 需要您配置鑑別 API 金鑰及 API 主機。使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式，您不需要明確配置 API 金鑰及 API 主機。相反地，您可以使用 `ibmcloud login` 來登入，並使用 `ibmcloud target` 指令，將您的地區及名稱空間設為目標。登入之後，所有指令的開頭都是 `ibmcloud fn`。


如果您需要在外部 HTTP 用戶端（例如 cURL 或 Postman）中使用 {{site.data.keyword.openwhisk_short}} 的鑑別 API 金鑰，則可以使用下列指令進行擷取：

若要取得現行 API 金鑰，請執行下列指令：
```
ibmcloud fn property get --auth
```
{: pre}

若要取得現行 API 主機，請執行下列指令：
```
ibmcloud fn property get --apihost
```
{: pre}

對於 {{site.data.keyword.openwhisk_short}} CLI 外掛程式已設為目標的每個地區、組織及空間而言，API 金鑰是專用的。
{: tip}

### API 閘道鑑別
{: #apigw_authentication}

OpenWhisk CLI 需要您執行 `wsk bluemix login`，才能使用 `wsk api` 指令來配置用於管理 API 的「API 閘道」授權。使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式，就不需要執行 `wsk bluemix login`。相反地，當您使用 `ibmcloud login` 指令來登入 {{site.data.keyword.Bluemix_notm}} 時，{{site.data.keyword.openwhisk}} 外掛程式會自動利用現行登入和目標資訊。現在，您可以使用 `ibmcloud fn api` 指令來管理 API。

### 移轉部署 Script
{: #migrating_deploy_scripts}

如果您的 Script 使用具有 `wsk` 二進位的 OpenWhisk CLI，則所有指令都會使用 `ibmcloud fn` 指令，以相同的方式運作。您可以修改 Script 來使用 {{site.data.keyword.Bluemix_notm}} CLI 外掛程式，或建立別名或封套，以將使用 `wsk` 的現行指令轉換為 `ibmcloud fn`。{{site.data.keyword.Bluemix_notm}} CLI 中的 `ibmcloud login` 和 `ibmcloud target` 指令以自動模式運作。使用自動模式，您可以在執行 `ibmcloud fn` 指令以部署及管理 {{site.data.keyword.openwhisk_short}} 實體之前，先配置環境。





## CLI 版本歷程
{: #version_history}

版本的歷程記錄，顯示強調顯示及錯誤修正程式。

1.0.30 版 (2019-04-03)
* 已改善 IAM 與組織及空間型服務的 `service bind` 處理。
* 已新增處理 API 端點 https://cloud.ibm.com 的修正程式。

1.0.29 版 (2019-02-06)
* 已新增 `deploy` 及 `undeploy` 指令，以透過資訊清單檔部署或取消部署 Functions 實體集合。如需相關資訊，請參閱[部署](/docs/openwhisk?topic=cloud-functions-deploy#deploy)文件。

1.0.28 版 (2019-01-21)
* 已在 `update|delete|get namespace name` 存在多次時新增一則錯誤訊息。

1.0.27 版 (2018-12-11)
* 已新增 `namespace get` 修正程式。
* 已在動作是 Blackbox 動作時新增 `--save-as` 的修正程式。
* 已新增 action create 及 action update 指令的 `--concurrency` 旗標。

1.0.26 版 (2018-11-30)
* 已啟用 `fn property get --auth`，正確地傳回新環境中的鑑別金鑰。

1.0.25 版 (2018-11-23)
* 已改善錯誤訊息結果顯示。
* 已新增 `fn namespace get` 修正程式，以正確顯示名稱空間內容。

1.0.23 (2018-10-15)
* 已新增 ruby (.rb) 動作碼辨識支援。

1.0.22 (2018-08-20)
* 已新增 us-east 地區支援。

1.0.21 (2018-08-01)
* 別名 `fn` 及 `functions` 現在可以用於 {{site.data.keyword.openwhisk_short}} 指令：`ibmcloud fn <command>` 及 `ibmcloud fn <command>`。您仍可以使用 `ibmcloud wsk <command>`。

1.0.19 (2018-07-02)
* 次要錯誤修正程式及增進功能。

1.0.18 (2018-06-20)
* 已新增取消連結使用者所提供服務實例的修正程式。
* 效能提升。

1.0.17 (2018-06-12)
* 已新增支援，讓您可以連結 (`ibmcloud wsk service bind`) 及取消連結 (`ibmcloud wsk service unbind`) 使用 `ibmcloud cf create-user-provided-service` 指令所建立的使用者提供的服務實例。

1.0.16 (2018-05-24)
* 次要錯誤修正程式及增進功能。

1.0.15 (2018-05-21)
* 次要錯誤修正程式及增進功能。

1.0.14 (2018-05-17)
* 已啟用組織和空間名稱中 `&` 字元的支援。

1.0.13 (2018-05-07)
* 次要錯誤修正程式及錯誤處理增進功能。

1.0.12 (2018-04-30)
* {{site.data.keyword.Bluemix_notm}} SDK 更新項目，用來維護 `bx` CLI 相容性。

1.0.11 (2018-04-23)
* 次要錯誤修正程式及增進功能。

1.0.10 (2018-04-09)
* 已將新的 `--web-secure` 選項新增至 `ibmcloud wsk action create|update` 指令，以保護 Web 動作端點。
* 已修正連續路徑參數 [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237)。

1.0.9 (2018-03-16)
* 已啟用套件層次的服務連結支援。

1.0.8 (2018-02-22)
* 已啟用 IAM 服務連結支援。

1.0.7 (2018-02-02)
* 已更新 `ibmcloud wsk api` 以接受路徑參數，例如 `/api/{id}`。如需相關資訊，請參閱 [API 閘道](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)。
* 已還原 Proxy 支援。
* 已移除 `swift:3`。

1.0.6 (2018-01-30)
* 已修正套件內動作之 `ibmcloud wsk service bind` 指令的錯誤。
