---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# 安裝 CLI 和外掛程式
{: #cli_install}

{{site.data.keyword.openwhisk}} 針對 {{site.data.keyword.cloud_notm}} CLI 提供功能強大的外掛程式，允許完全管理 {{site.data.keyword.openwhisk_short}} 系統。
您可以使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式來管理動作中程式碼 Snippet、建立觸發程式及規則以讓動作回應事件，並將動作組合為套件。
{:shortdesc}


## 設定 {{site.data.keyword.cloud_notm}} CLI
{: #cli_setup}

**開始之前**

您必須建立 [{{site.data.keyword.cloud_notm}} 帳戶](https://cloud.ibm.com/){: external}。

下載並安裝 {{site.data.keyword.cloud_notm}} CLI，然後登入。
{: shortdesc}

1. 下載並安裝 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli)。

2. 登入 {{site.data.keyword.cloud_notm}} CLI。若要指定 {{site.data.keyword.cloud_notm}} 地區，請[包含 API 端點](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)。

  ```
  ibmcloud login
  ```
  {: pre}

3. 按照提示選取您的 {{site.data.keyword.cloud_notm}} 帳戶。

4. 取得資源群組的清單。 

```
ibmcloud resource groups
```
{: pre}

**輸出範例**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. 選用：透過執行下列指令將非預設資源群組設定為目標。
```
ibmcloud target -g <resource_group>
```
{: pre}


**輸出範例**

```
Targeted resource group <resource_group>
```
{: screen}

## 設定 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cli_plugin_setup}

若要使用 {{site.data.keyword.openwhisk_short}}，請下載並安裝 CLI 外掛程式。
{: shortdesc}

可以使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式來執行下列作業。

* 在 {{site.data.keyword.openwhisk_short}} 上，執行您的程式碼 Snippet 或動作。請參閱[建立及呼叫動作](/docs/openwhisk?topic=cloud-functions-actions)。
* 建立觸發程式和規則，讓動作對事件進行回應。請參閱[建立觸發程式及規則](/docs/openwhisk?topic=cloud-functions-triggers)。
* 組合動作以及配置外部事件來源。請參閱[建立及使用套件](/docs/openwhisk?topic=cloud-functions-pkg_ov)。
* 瀏覽套件的型錄，並透過外部服務來增強應用程式的功能。請參閱[新增 {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services)。

完成下列步驟來安裝 {{site.data.keyword.openwhisk_short}} CLI 外掛程式

1. 安裝 {{site.data.keyword.openwhisk_short}} 外掛程式。

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. 驗證外掛程式已安裝。

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **輸出**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. 登入後，所有 {{site.data.keyword.openwhisk_short}} 指令都以 `ibmcloud fn` 開頭。若要查看您可以使用 {{site.data.keyword.openwhisk_short}} 外掛程式執行的所有作業，請執行不含任何引數的 `ibmcloud fn`。
  ```
  ibmcloud fn
  ```
  {: pre}




## 將 {{site.data.keyword.openwhisk_short}} 名稱空間設定為目標
{: #cli_regions}
依預設，{{site.data.keyword.openwhisk_short}} 使用[已啟用 IAM 的名稱空間](/docs/iam?topic=iam-iamoverview){: external}。您無法再建立以 Cloud Foundry 為基礎的名稱空間。
{: important}

### 建立名稱空間或將名稱空間設定為目標。
若要取得 {{site.data.keyword.openwhisk_short}} 名稱空間的清單，請執行 `ibmcloud fn namespace list`。

#### 建立已啟用 IAM 功能的名稱空間。
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**回應**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### 將啟用 IAM 的名稱空間設定為目標。 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**回應**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### 將以 Cloud Foundry 為基礎的名稱空間設定為目標。 
  
可以使用 `-o` 和 `-s` 旗標將特定 `org` 和 `space` 設定為目標，也可以按照提示進行操作。

* 透過在 `target` 指令中包含 `org` 和 `space` 名稱，將 Cloud Foundy 名稱空間設定為目標。

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* 將 Cloud Foundry 設定為目標，並按照提示選取 `org` 和 `space`。

```
ibmcloud target --cf
```
{: pre}


**回應**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### 選用：建立用於暫置和正式作業部署的名稱空間。

可以透過為每個預先正式作業（暫置）和正式作業 {{site.data.keyword.openwhisk_short}} 部署建立啟用 IAM 的名稱空間來處理這些部署。執行 [`ibmcloud fn namespace`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) 以在您的組織下建立更多名稱空間（例如 "staging" 和 "production" ）：

建立暫置名稱空間。
```
ibmcloud fn namespace create staging
```
{: pre}

建立正式作業名稱空間。
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} 對名稱空間的名稱有所限制。如需相關資訊，請參閱[系統詳細資料和限制](/docs/openwhisk?topic=cloud-functions-limits#limits_entities)文件。
{: tip}


## 配置 {{site.data.keyword.openwhisk_short}} CLI 以使用 HTTPS Proxy
{: #cli_proxy}

{{site.data.keyword.openwhisk_short}} CLI 可以設為使用 HTTPS Proxy。若要設定 HTTPS Proxy，必須建立一個稱為 `HTTPS_PROXY` 的環境變數。變數必須設為 HTTPS Proxy 的位址，其埠使用下列格式：`{PROXY IP}:{PROXY PORT}`。

變更 `org` 或 `space` 的名稱會根據已變更的名稱建立新的名稱空間。在新的名稱空間中看不到舊名稱空間中的實體，因此可能已被刪除。
{: important}


## 從 OpenWhisk CLI 移轉至 {{site.data.keyword.openwhisk_short}} CLI 外掛程式
{: #cli_migrate}

您現在可以使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式，來與 {{site.data.keyword.openwhisk_short}} 實體互動。雖然可以繼續使用獨立式 Openwhisk CLI，但此 CLI 不會有 {{site.data.keyword.openwhisk_short}} 支援的最新特性，例如以 IAM 為基礎的名稱空間和 `service bind`。
{: shortdesc}


### 指令語法
{: #cli_syntax}

Cloud Functions CLI 外掛程式中指令的所有指令選項和引數都與 [OpenWhisk 獨立式 CLI ](https://github.com/apache/incubator-openwhisk-cli){: external} 的選項相同。但是，請注意下列差異。

* {{site.data.keyword.openwhisk}} 外掛程式會自動利用您現行的登入和目標資訊。
* 現在，`wsk` 指令會作為 `ibmcloud fn` 執行。
* 不再需要 `wsk ibmcloud login` 指令。您可以使用 `ibmcloud login` 進行登入。
* 現在，可以使用 `ibmcloud fn API` 來管理 API。

如需相關資訊，請參閱 [{{site.data.keyword.openwhisk_short}} CLI 參考資料](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli)。

### API 鑑別及主機
{: #cli_api_auth}

使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式，您不需要明確配置 API 金鑰及 API 主機。您可以改為使用 `ibmcloud login` 登入。然後，透過執行 `ibmcloud fn property set --namespace <namespace_name>` 將啟用 IAM 的名稱空間設定為目標，或透過執行 `ibmcloud target --cf` 將以 Cloud Foundry 為基礎的名稱空間設定為目標。登入之後，所有指令的開頭都是 `ibmcloud fn`。


如果需要將鑑別 API 金鑰用於外部 HTTP 用戶端（例如，cURL 或 Postman）中的 {{site.data.keyword.openwhisk_short}}，可以使用下列指令來擷取該金鑰。

透過執行下列指令來取得現行 API 金鑰。
```
ibmcloud fn property get --auth
```
{: pre}

透過執行下列指令來取得現行 API 主機。
```
ibmcloud fn property get --apihost
```
{: pre}

對於 {{site.data.keyword.openwhisk_short}} CLI 外掛程式已設為目標的每個地區、組織及空間而言，API 金鑰是專用的。
{: tip}


### API 閘道鑑別
{: #cli_apigw_authentication}

OpenWhisk CLI 需要您執行 `wsk ibmcloud login` 才能配置 API 閘道授權，以使用 `wsk API` 指令來管理 API。使用 {{site.data.keyword.openwhisk_short}} CLI 外掛程式後，無需執行 `wsk ibmcloud login`。相反地，當您使用 `ibmcloud login` 指令來登入 {{site.data.keyword.cloud_notm}} 時，{{site.data.keyword.openwhisk}} 外掛程式會自動利用現行登入和目標資訊。現在，您可以使用 `ibmcloud fn api` 指令來管理 API。


### 移轉部署 Script
{: #cli_migrating_deploy_scripts}

如果您具有將 OpenWhisk CLI 與 `wsk` 指令一起使用的 Script，則所有指令的工作方式與使用 `ibmcloud fn` 指令相同。您可以修改 Script 來使用 {{site.data.keyword.cloud_notm}} CLI 外掛程式，或建立別名或封套，以將使用 `wsk` 的現行指令轉換為 `ibmcloud fn`。{{site.data.keyword.cloud_notm}} CLI 中的 `ibmcloud login` 和 `ibmcloud target` 指令以自動模式運作。使用自動模式，您可以在執行 `ibmcloud fn` 指令以部署及管理 {{site.data.keyword.openwhisk_short}} 實體之前，先配置環境。

## CLI 版本歷程
{: #cli_versions}

版本的歷程記錄，顯示強調顯示及錯誤修正程式。

1.0.30 版（2019 年 4 月 3 日）
* 已改善 IAM 與組織及空間型服務的 `service bind` 處理。
* 已新增處理 API 端點 https://cloud.ibm.com 的修正程式。

1.0.29 版（2019 年 2 月 6 日）
* 已新增 `deploy` 及 `undeploy` 指令，以透過資訊清單檔部署或取消部署 Functions 實體集合。如需相關資訊，請參閱[部署](/docs/openwhisk?topic=cloud-functions-deploy#deploy)文件。

1.0.28 版（2019 年 1 月 21 日）
* 已在 `update|delete|get namespace name` 存在多次時新增一則錯誤訊息。

1.0.27 版（2018 年 12 月 11 日）
* 已新增 `namespace get` 修正程式。
* 已在動作是 Blackbox 動作時新增 `--save-as` 的修正程式。

1.0.26 版（2018 年 11 月 30 日）
* 已啟用 `fn property get --auth`，正確地傳回新環境中的鑑別金鑰。

1.0.25 版（2018 年 11 月 23 日）
* 已改善錯誤訊息結果顯示。
* 已新增 `fn namespace get` 修正程式，以正確顯示名稱空間內容。

1.0.23（2018 年 10 月 15 日）
* 新增了對 ruby (`.rb`) 動作碼識別的支援。

1.0.22（2018 年 8 月 20 日）
* 已新增 us-east 地區支援。

1.0.21（2018 年 8 月 1 日）
* 別名 `fn` 和 `functions` 現在可用於 {{site.data.keyword.openwhisk_short}} 指令：`ibmcloud fn <command>` 和 `ibmcloud fn <command>`. 此外，您仍可以使用 `ibmcloud wsk <command>`。

1.0.19（2018 年 7 月 2 日）
* 次要錯誤修正程式及增進功能。

1.0.18（2018 年 6 月 20 日）
* 已新增取消連結使用者所提供服務實例的修正程式。
* 效能提升。

1.0.17（2018 年 6 月 12 日）
* 已新增支援，讓您可以連結 (`ibmcloud wsk service bind`) 及取消連結 (`ibmcloud wsk service unbind`) 使用 `ibmcloud cf create-user-provided-service` 指令所建立的使用者提供的服務實例。

1.0.16（2018 年 5 月 24 日）
* 次要錯誤修正程式及增進功能。

1.0.15（2018 年 5 月 21 日）
* 次要錯誤修正程式及增進功能。

1.0.14（2018 年 5 月 17 日）
* 已啟用組織和空間名稱中 `&` 字元的支援。

1.0.13（2018 年 5 月 7 日）
* 次要錯誤修正程式及錯誤處理增進功能。

1.0.12（2018 年 4 月 30 日）
* {{site.data.keyword.cloud_notm}} SDK 更新項目，用來維護 `bx` CLI 相容性。

1.0.11（2018 年 4 月 23 日）
* 次要錯誤修正程式及增進功能。

1.0.10（2018 年 4 月 9 日）
* 已將新的 `--web-secure` 選項新增至 `ibmcloud wsk action create|update` 指令，以保護 Web 動作端點。
* 已修正連續路徑參數 [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external}。

1.0.9（2018 年 3 月 16 日）
* 已啟用套件層次的服務連結支援。

1.0.8（2018 年 2 月 22 日）
* 已啟用 IAM 服務連結支援。

1.0.7（2018 年 2 月 2 日）
* 已更新 `ibmcloud wsk api` 以接受路徑參數，例如 `/api/{id}`。如需相關資訊，請參閱 [API 閘道](/docs/openwhisk?topic=cloud-functions-apigateway)。
* 已還原 Proxy 支援。
* 已移除 `swift:3`。

1.0.6（2018 年 1 月 30 日）
* 已修正套件內動作之 `ibmcloud wsk service bind` 指令的錯誤。



