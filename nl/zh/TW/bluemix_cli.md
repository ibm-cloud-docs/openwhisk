---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# 設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} 針對 {{site.data.keyword.Bluemix_notm}} CLI 提供功能強大的外掛程式，允許完全管理 {{site.data.keyword.openwhisk_short}} 系統。
{: shortdesc}

## 設定 {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

下載並安裝 {{site.data.keyword.Bluemix_notm}} CLI，然後登入。
{: shortdesc}

1. 下載並安裝 [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)。

2. 登入 {{site.data.keyword.Bluemix_notm}} CLI。美國南部及英國 {{site.data.keyword.Bluemix_notm}} 地區提供 {{site.data.keyword.openwhisk_short}}。如果未指定 {{site.data.keyword.Bluemix_notm}} API 端點，則請使用 `-a` 旗標予以指定。

    * 若要登入美國南部地區，請執行下列指令：
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * 若要登入英國地區，請執行下列指令：
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  您可以使用 `ibmcloud api` 指令來明確設定 {{site.data.keyword.Bluemix_notm}} API 端點。若要顯示現行 API 端點設定，請使用 `ibmcloud target` 指令。
  {: tip}

3. 如果未指定組織、空間及密碼等資訊，則 `ibmcloud login` 指令會提示您輸入。

  您可以在登入時指定組織及空間，以跳過組織及空間的輸入提示。請使用下列旗標：`ibmcloud login -o <ORG> -s <SPACE>`。
  {: tip}

您也可以使用 {{site.data.keyword.Bluemix_notm}} API 金鑰來登入。當您的帳戶是以聯合登入配置，並需要使用 `--sso` 旗標來登入時，此方法很有用。如果您要設定持續整合 (CI)，且想要配置自動式管線，則[使用 API 金鑰](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key)也會有所助益。

1. 建立新 API 金鑰。
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. 使用產生的 API 金鑰值來登入。
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
如需 `ibmcloud login` 指令的相關資訊，請使用 `ibmcloud login --help` 或檢閱 [IBM Cloud (bx) 指令](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login)主題。

## 設定 {{site.data.keyword.openwhisk_short}} 外掛程式
{: #cloudfunctions_plugin_setup}

下載並安裝 {{site.data.keyword.openwhisk_short}} 外掛程式。
{: shortdesc}

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

3. 執行 `echo` 的區塊處理（同步）呼叫。傳遞 `hello` 作為引數。
    ```
    ibmcloud fn action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. 驗證輸出中傳回 `hello` 訊息。
    輸出範例：
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. 您可以執行下列指令來升級 {{site.data.keyword.openwhisk_short}} 外掛程式：
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

您可以使用 {{site.data.keyword.openwhisk_short}} CLI：

* 在 {{site.data.keyword.openwhisk_short}} 上，執行您的程式碼 Snippet 或動作。請參閱[建立及呼叫動作](./openwhisk_actions.html)。
* 使用觸發程式及規則，讓動作回應事件。請參閱[建立觸發程式及規則](./openwhisk_triggers_rules.html)。
* 瞭解套件如何組合動作以及配置外部事件來源。請參閱[建立及使用套件](./openwhisk_packages.html)。
* 探索套件的型錄，以及使用外部服務（例如 [{{site.data.keyword.cloudant}} 事件來源](./openwhisk_cloudant.html)）來加強應用程式。

若要取得 {{site.data.keyword.openwhisk_short}} 外掛程式的指令清單，請執行不含任何引數的 `ibmcloud fn`。
{: tip}

## 使用動作中的服務
{: #binding_services}

{{site.data.keyword.openwhisk_short}} 提供 `service bind` 指令，讓您的 {{site.data.keyword.Bluemix_notm}} 服務認證可供您的程式碼在執行時期使用。您接著可以使用 `service bind` 指令，將任何 {{site.data.keyword.Bluemix_notm}} 服務連結至 {{site.data.keyword.openwhisk_short}} 中所定義的任何動作。

如需如何使用動作中服務的詳細步驟，請參閱[將服務連結至動作](./binding_services.html)。

## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS Proxy
{: #cli_https_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以設為使用 HTTPS Proxy。若要設定 HTTPS Proxy，必須建立一個稱為 `HTTPS_PROXY` 的環境變數。變數必須設為 HTTPS Proxy 的位址，其埠使用下列格式：`{PROXY IP}:{PROXY PORT}`。

## 切換至不同的地區、組織及空間
{: #region_info}

如果您已登入，則可以在 {{site.data.keyword.Bluemix_notm}} CLI 中執行 `ibmcloud target` 指令，以切換地區、組織及空間。

美國南部及英國 {{site.data.keyword.Bluemix_notm}} 地區提供 {{site.data.keyword.openwhisk_short}}。若要變更地區，請使用 `ibmcloud target` 指令。例如，若要切換至英國地區，以及該地區中的空間 `staging`，請執行下列指令：
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

您可以建立空間來處理正式作業前（暫置）和正式作業部署，作法是為每一個建立空間。建立空間可讓 {{site.data.keyword.openwhisk_short}} 擁有兩個為您定義的不同名稱空間。執行 [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create)，在組織下建立更多空間，例如 "staging" 及 "production"：

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} 對名稱空間的名稱有所限制。如需相關資訊，請參閱[系統詳細資料及限制](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities)文件。
{: tip}

## 從 OpenWhisk CLI 移轉至 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cli_migration}

引進 {{site.data.keyword.openwhisk_short}} CLI 外掛程式之後，就不再需要 OpenWhisk 獨立式 CLI。

### 指令語法
{: #command_syntax}

除了不再需要的 `wsk bluemix login` 指令之外，所有 `wsk` 指令都會使用 `ibmcloud fn` 指令，以相同的方式運作。所有指令選項和引數都相同。

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

## 版本歷程
{: #version_history}

版本的歷程記錄，顯示強調顯示及錯誤修正程式。

1.0.18 (2018-06-20)
* 取消連結使用者所提供服務實例的修正程式。
* 效能提升。

1.0.17 (2018-06-12)
* 新增支援，讓您可以連結 (`ibmcloud wsk service bind`) 及取消連結 (`ibmcloud wsk service unbind`) 使用 `ibmcloud cf create-user-provided-service` 指令所建立的使用者提供的服務實例。

1.0.16 (2018-05-24)
* 次要錯誤修正程式及增進功能。

1.0.15 (2018-05-21)
* 次要錯誤修正程式及增進功能。

1.0.14 (2018-05-17)
* 啟用組織和空間名稱中 `&` 字元的支援。

1.0.13 (2018-05-07)
* 次要錯誤修正程式及錯誤處理增進功能。

1.0.12 (2018-04-30)
* {{site.data.keyword.Bluemix_notm}} SDK 更新項目，用來維護 `bx` CLI 相容性。

1.0.11 (2018-04-23)
* 次要錯誤修正程式及增進功能。

1.0.10 (2018-04-09)
* 將新的 `--web-secure` 選項新增至 `ibmcloud wsk action create|update` 指令，以保護 Web 動作端點。
* 修正連續路徑參數 [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237)。

1.0.9 (2018-03-16)
* 啟用套件層次的服務連結支援。

1.0.8 (2018-02-22)
* 啟用 IAM 服務連結支援。

1.0.7 (2018-02-02)
* `ibmcloud wsk api` 現在接受路徑參數，例如，`/api/{id}`。如需相關資訊，請參閱 [API 閘道](./openwhisk_apigateway.html)。
* 還原 Proxy 支援。
* 移除 `swift:3`。

1.0.6 (2018-01-30)
* 套件內動作之 `ibmcloud wsk service bind` 指令的錯誤修正程式。
