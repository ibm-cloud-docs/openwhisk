---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} 針對 {{site.data.keyword.Bluemix_notm}} CLI 提供功能強大的外掛程式，允許完全管理 {{site.data.keyword.openwhisk_short}} 系統。
{: shortdesc}

## 設定 {{site.data.keyword.Bluemix_notm}} CLI
{: #bluemix_cli_setup}

下載並安裝 [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)。

## 設定 {{site.data.keyword.openwhisk_short}} 外掛程式
{: #cloudfunctions_plugin_setup}

若要安裝 {{site.data.keyword.openwhisk_short}} 外掛程式，請執行下列指令：
```
ibmcloud plugin install cloud-functions
```
{: pre}


若要驗證 {{site.data.keyword.openwhisk_short}} 外掛程式是否順利完成安裝，請執行下列指令：
```
ibmcloud plugin list cloud-functions
```
{: pre}


輸出會顯示已安裝的 {{site.data.keyword.openwhisk_short}} 版本資訊：
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

您可以執行下列指令來升級 {{site.data.keyword.openwhisk_short}} 外掛程式：
```
ibmcloud plugin update Cloud-Functions
```
{: pre}


如需外掛程式指令的相關資訊，請使用 `ibmcloud plugin --help` 或查看下列文件：
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}} CLI 鑑別
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} 可在兩個 {{site.data.keyword.Bluemix_notm}} 地區中使用。

當您登入 {{site.data.keyword.Bluemix_notm}} CLI 時，您可以指定目標地區的 {{site.data.keyword.Bluemix_notm}} API 端點與組織及空間。

若要登入美國南部地區，請執行下列指令：
```
ibmcloud login -a api.ng.bluemix.net
```
{: pre}


若要登入英國地區，請執行下列指令：
```
ibmcloud login -a api.eu-gb.bluemix.net
```
{: pre}


`-a` 旗標指定要使用的 {{site.data.keyword.Bluemix_notm}} API 端點。如果指定 API 端點，則不需要 `-a` 選項。您可以使用 `ibmcloud api` 指令來明確設定 {{site.data.keyword.Bluemix_notm}} API 端點。若要顯示現行 API 端點設定，請使用 `ibmcloud target` 指令。

如果未指定組織、空間及密碼等資訊，則 `login` 指令會提示您輸入。您可以在指令行中指定組織及空間，以跳過其提示。
```
ibmcloud login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


您也可以使用 {{site.data.keyword.Bluemix_notm}} API 金鑰來登入。當您的帳戶是以「聯合登入」配置，並需要使用 `--sso` 旗標來登入時，此方法很有用。如果您要設定「繼續整合 (CI)」，且想要配置自動式管線，則使用 API 金鑰也會有所助益。
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

若要使用 {{site.data.keyword.Bluemix_notm}} CLI 建立新的 API 金鑰，請執行下列指令：
```
ibmcloud iam api-key-create MyKey
```
{: pre}


然後，使用 API 金鑰產生的值來登入，如下列範例所示：
```
ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


如需登入指令的相關資訊，請使用 `ibmcloud login --help` 或檢閱下列文件：
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## 使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cloudfunctions_plugin_usage}

驗證您的設定。執行回應的區塊處理（同步）呼叫，並將 `hello` 作為引數傳遞給它，如下列範例所示：
```
ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


請參閱下列輸出範例：
```
{
    "message":"hello"
}
```

配置環境之後，您可以使用 {{site.data.keyword.openwhisk_short}} CLI 來執行下列作業：

* 在 {{site.data.keyword.openwhisk_short}} 上，執行您的程式碼 Snippet 或「動作」。請參閱[建立及呼叫動作](./openwhisk_actions.html)。
* 使用「觸發程式」和「規則」，讓「動作」回應事件。請參閱[建立觸發程式及規則](./openwhisk_triggers_rules.html)。
* 瞭解套件如何組合動作及配置外部事件來源。請參閱[建立及使用套件](./openwhisk_packages.html)。
* 探索套件的型錄，以及使用外部服務（例如 [Cloudant 事件來源](./openwhisk_cloudant.html)）來加強應用程式。請參閱[使用 {{site.data.keyword.openwhisk_short}} 啟用的服務](./openwhisk_catalog.html)。

若要取得 {{site.data.keyword.openwhisk_short}} 外掛程式的指令清單，請執行 `ibmcloud wsk`（不含引數）。

## 使用動作中的服務
{: #binding_services}

{{site.data.keyword.openwhisk_short}} 提供 `service bind` 指令，讓您的 {{site.data.keyword.Bluemix_notm}} 服務認證可供您的程式碼在執行時期使用。然後，可以利用 `service bind` 指令，讓您將任何 {{site.data.keyword.Bluemix_notm}} 服務連結至 {{site.data.keyword.openwhisk_short}} 中所定義的任何「動作」。

如需如何使用「動作」中的服務的詳細步驟，請參閱[使用動作中的服務](./binding_services.html)主題。

## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS Proxy
{: #cli_https_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以設為使用 HTTPS Proxy。若要設定 HTTPS Proxy，必須建立一個稱為 `HTTPS_PROXY` 的環境變數。變數必須設為 HTTPS Proxy 的位址，其埠使用下列格式：`{PROXY IP}:{PROXY PORT}`。

## 使用地區、組織及空間
{: #region_info}

您可以建立空間來處理正式作業前（暫置）和正式作業部署，作法是為每一個建立空間。建立空間可讓 {{site.data.keyword.openwhisk_short}} 擁有兩個為您定義的不同名稱空間。

您可以使用 `ibmcloud iam space-create`，在組織下建立更多空間，例如，"staging" 和 "production"。請參閱下列範例：
```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```

如需 `ibmcloud iam` 指令的相關資訊，請參閱下列文件： https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} 對名稱空間的名稱有所限制。如需這些限制的相關資訊，請參閱下列文件：https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

如果您已登入，則可以在 {{site.data.keyword.Bluemix_notm}} CLI 中執行 `ibmcloud target` 指令，以切換地區、組織及空間。

使用下列指令可顯示所有 {{site.data.keyword.Bluemix_notm}} 地區。

只有 `us-south` 和 `eu-gb` 地區才支援 {{site.data.keyword.openwhisk_short}}。
{: tip}

```
ibmcloud regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

使用 `ibmcloud target` 指令可變更地區。例如，若您想要切換至英國地區以及空間 `staging`：
```
ibmcloud target -r eu-gb -s staging
```
{: pre}


如果您需要在相同地區內變更空間（例如從 staging 到 production），請執行下列指令：
```
ibmcloud target -s production
```
{: pre}


如需 `target` 指令的相關資訊，請使用 `ibmcloud target --help` 或檢閱下列文件：
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## 從 OpenWhisk CLI 移轉至 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cli_migration}

引進 {{site.data.keyword.openwhisk_short}} CLI 外掛程式之後，便不再需要 OpenWhisk CLI。

### 指令語法
{: #command_syntax}

除了不再需要的 `wsk bluemix login` 指令之外，所有 `wsk` 指令都使用 `ibmcloud wsk` 指令，以相同的方式運作。所有指令選項和引數都相同。

### API 鑑別及主機
{: #api_authentication}

OpenWhisk CLI 要求您配置鑑別 API 金鑰及 API 主機。
使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式，您不需要明確配置 API 金鑰及 API 主機。您需要使用 `ibmcloud login` 來登入，並使用 `ibmcloud target` 指令，將您的地區和名稱空間設為目標。之後，所有後續指令的開頭都是 `ibmcloud wsk`。以此方式鑑別也可避免存取 {{site.data.keyword.Bluemix_notm}} 網頁，以免取得配置 CLI 環境的特定資訊。

如果您需要取得 {{site.data.keyword.openwhisk_short}} 的鑑別 API 金鑰，以從外部 HTTP 用戶端（例如 cURL 或 Postman）使用該金鑰，您可以使用下列指令擷取之：

若要取得現行 API 金鑰，請執行下列指令：
```
ibmcloud wsk property get --auth
```
{: pre}


若要取得現行 API 主機，請執行下列指令：
```
ibmcloud wsk property get --apihost
```
{: pre}


對於 {{site.data.keyword.openwhisk_short}} CLI 外掛程式已設為目標的每個地區、組織及空間而言，API 金鑰是專用的。
{: tip}

### API 閘道鑑別
{: #apigw_authentication}

OpenWhisk CLI 目前需要您執行 `wsk bluemix login`，才能使用 `wsk api` 指令來配置用於管理 API 的「API 閘道」授權。

使用 {{site.data.keyword.openwhisk}} CLI 外掛程式，不再需要執行 `wsk bluemix login`。而是改用 `ibmcloud login` 指令來登入 {{site.data.keyword.Bluemix_notm}}，{{site.data.keyword.openwhisk}} 外掛程式會自動利用現行登入和目標資訊。現在，您可以使用 `ibmcloud wsk api` 指令來管理您的 API。

### 移轉部署 Script
{: #migrating_deploy_scripts}

如果您的 Script 使用具有 `wsk` 二進位的 OpenWhisk CLI，則所有指令都使用 `ibmcloud wsk` 指令，以相同的方式來運作。您可以修改 Script 來使用 {{site.data.keyword.Bluemix_notm}} CLI 外掛程式，或建立別名或封套，使現行的 `wsk` 執行轉換為 `ibmcloud wsk`。{{site.data.keyword.Bluemix_notm}} CLI 中的 `ibmcloud login` 和 `ibmcloud target` 指令以自動模式運作。使用自動模式，您可以在執行 `ibmcloud wsk` 指令以進行部署及管理 {{site.data.keyword.openwhisk_short}} 實體之前，先配置環境。


## 版本歷程
{: #version_history}

版本的歷程記錄，顯示強調顯示及錯誤修正程式。

1.0.7 (2018-02-02)
* `ibmcloud wsk api` 現在接受路徑參數，例如，`/api/{id}`。如需相關資訊，請參閱 [API 閘道](./openwhisk_apigateway.html)
* 還原 Proxy 支援。
* 移除 `swift:3`。

1.0.6 (2018-01-30)
* 針對套件內的動作，在 `ibmcloud wsk service bind` 指令中修正錯誤。
