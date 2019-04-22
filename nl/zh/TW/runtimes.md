---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# 運行環境

## 動作運行環境
您可以使用數種程式設計語言（例如 Javascript、Python 等）來編寫及執行動作。下列各節會顯示可用的運行環境。

下列鏈結會傳回 JSON 回應，以顯示 IBM Cloud Functions 在每個地區中可用的運行環境。

回應的 `runtimes` 區段包含一組可用的運行環境。

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

`image` 區段包含 [DockerHub](https://hub.docker.com/) 上的運行環境映像檔名稱以及使用的標籤。 

下列範例指向 `ibmfunctions/action-nodejs-v10` 及 `openwhisk/nodejs6action` 映像檔。
標籤可以是 `1.9.0` 這類版本號碼或 git commit 雜湊的簡短格式（例如 `b99d71e`）。

映像檔欄位範例。
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- 定期更新動作運行環境。這些更新項目包括安全修正程式及運行環境內套件的次要版本更新項目。次要版本更新項目可能會造成舊版相容性岔斷。運行環境更新項目可能會影響您的動作。不會自動移轉至相同類型的較新運行環境。
- 除非將運行環境更新為支援的運行環境，否則無法順利完成對已淘汰運行環境的動作。對失敗動作進行疑難排解時，若要識別運行環境是否已被淘汰，請檢查查詢回應中的 `deprecated=true`。若要更新運行環境，請參閱[變更動作運行環境](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)

## JavaScript 執行時期
{: #openwhisk_ref_javascript_environments}

JavaScript 動作可以在 Node.js 第 8 版或第 10 版中執行。 

Node.js 第 6 版是預設版本，但已在 2018 年 12 月 6 日淘汰。若要繼續使用 JavaScript 動作，請更新為 Node.js 第 8 版或第 10 版。
{: deprecated}

### 具有 IBM SDK 的 Node.js 第 10 版環境
{: #openwhisk_ref_javascript_environments_10}
如果在建立或更新動作時明確指定 `--kind` 旗標，且值為 `nodejs:10`，則會使用 Node.js 第 10 版環境。

#### 從 `nodejs:8` 移轉至 `nodejs:10`
- `nodejs:10` 中無法使用 `ibm_db` npm 套件。`ibm_db` 套件不支援 Node.js 10。您可以追蹤此問題 [ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541) 的進度。
- `nodejs:10` 中無法使用 `cloudant` npm 套件，此套件已被淘汰，您需要在匯入 nodejs 模組（即 `require('@cloudant/cloudant')`）時使用正式 npm 套件 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) 3.0.0 版，而且 [3.x 版只會傳回 Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)。
- `nodejs:10` 中無法使用 `cradle` npm 套件。
- `nodejs:10` 中無法使用 `log4js` npm 套件。您可以在 [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805) 追蹤此問題
- `nodejs:10` 中無法使用 `watson-developer-cloud` npm 套件。您可以追蹤此問題 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780) 中新版本的進度

nodejs 第 10 版運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md) 中找到。

### 具有 IBM SDK 的 Node.js 第 8 版環境
{: #openwhisk_ref_javascript_environments_8}
如果在建立或更新動作時明確指定 `--kind` 旗標，且值為 `nodejs:8`，則會使用 Node.js 第 8 版環境。

Node.js 第 8 版處於維護模式，且在 2019 年 12 月之前可供使用。請參閱 [Node.js 版本排程](https://github.com/nodejs/Release)。
{: deprecated}
 
Node.js 第 8 版運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md) 中找到。

### Node.js 第 6 版環境（已淘汰）
{: #openwhisk_ref_javascript_environments_6}
Node.js 第 6 版是預設版本，但已被淘汰。若要繼續使用 JavaScript 動作，請更新為 Node.js 第 8 版或第 10 版。
{: deprecated}

nodejs 第 6 版運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md) 中找到。

## Python 執行時期
{: #openwhisk_ref_python_environments}

OpenWhisk 支援使用兩個不同的運行環境版本來執行 Python 動作。

### Python 3.7 動作（Debian Stretch 型）
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 動作是使用 Python 3.7.x 來執行。若要使用此運行環境，請在建立或更新動作時指定 `wsk` CLI 參數 `--kind python:3.7`。

除了 Python 3.7 標準程式庫之外，運行環境還包含可供 Python 動作使用之 IBM Cloud 服務的 SDK 套件。

Python 3.7 運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md) 中找到。

### Python 3.6 動作（Debian Jessie 型）
{: #openwhisk_ref_python_environments_3.6}

Python 3 動作是使用 Python 3.6.x 來執行。若要使用此運行環境，請在建立或更新動作時指定 `wsk` CLI 參數 `--kind python:3.6`。

除了 Python 3.6 標準程式庫之外，運行環境還包含可供 Python 動作使用之 IBM Cloud 服務的 SDK 套件。

Python 3.6 運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md) 中找到。

### Python 2 動作

除非您在建立或更新動作時指定 `--kind` 旗標，否則會使用 Python 2.7.15 來執行 Python 2 動作。若要明確地選取此執行時期，請使用 `--kind python:2`。

使用 virtualenv 建立 Python 動作時，請使用 Docker 映像檔 `openwhisk/python2action`。
除了 Python 2.7 標準程式庫之外，Python 2 動作還可以使用下列套件。

Python 2 運行環境的詳細資訊可以在 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md) 中找到。

## Swift 動作
{: #swift-actions}

Swift 3.1.1 及 4.1 運行環境已淘汰，在 2019 年 2 月 28 日之前可供使用。
使用 kind `swift:4.2` 及新的編譯處理程序，啟動任何新的動作，或將任何現有動作移轉至 Swift 4.2 運行環境。
{: tip}

### Swift 3
Swift 3 動作是使用 Swift 3.1.1 `--kind swift:3.1.1` 來執行。由於不支援舊版 Swift，請一律指定 `swift:3.1.1` 類型。

您必須移轉所有 Swift 動作，才能使用 `swift:3.1.1` 類型。最佳作法是在建立或更新動作時，一律提供特定類型。
{: tip}

使用單一 Swift 原始檔時，Swift 3.1.1 動作可以使用下列套件：
- KituraNet 1.7.6 版：https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON 15.0.1 版：https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK 0.16.0 版：https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
分別使用 `--kind swift:4.1` 或 `--kind swift:4.2`，可以使用 Swift 4.1 或 4.2 來執行 Swift 4 動作。
預設 `--kind swift:default` 為 Swift 4.2。

Swift 4.x 動作運行環境未內嵌任何套件，請遵循[包裝的 swift 動作](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable)的指示，以包括使用 Package.swift 的相依關係。

使用單一 Swift 原始檔時，Swift 4.1 動作可以使用下列套件：
- Watson Developer Cloud SDK 0.38.1 版：https://github.com/watson-developer-cloud/swift-sdk

使用單一 Swift 原始檔時，Swift 4.2 動作可以使用下列套件：
- Watson Developer Cloud SDK 1.2.0 版：https://github.com/watson-developer-cloud/swift-sdk

### 將 Swift 3.1.1 移轉至 Swift 4.1

#### 使用單一來源動作檔的 SwiftyJSON
如果您有未編譯的 `swift:3.1.1` 動作，並使用 **SwiftyJSON** 套件，則需要前置編譯動作，並指定您要用於 `swift:4.2` 類型動作的 SwiftyJSON 版本。從 Swift 4.1 開始列入考量，可以改善 JSON 資料的管理。

## PHP 動作
{: #openwhisk_ref_php}

PHP 7.1 及 7.2 將會在 2019 年 1 月 11 日淘汰。若要繼續使用 PHP 動作，請更新為 PHP 7.3。
{: deprecated}

PHP 動作是使用 PHP 7.3.0 來執行。若要使用此運行環境，請在建立或更新動作時指定 `wsk` CLI 參數 `--kind php:7.3`。當您使用副檔名為 `.php` 的檔案來建立動作時，這個行為是預設值。

PHP 7.1 及 7.2 運行環境已淘汰。將所有動作都移轉至 PHP 7.3，以獲得較佳的延遲，並更快地完整執行。

除了標準副檔名之外，也能使用下列 PHP 副檔名：

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Docker 動作
{: #openwhisk_ref_docker}

Docker 動作是在 Docker 容器中執行使用者提供的二進位檔。二進位檔是在根據 [python:3.6-alpine](https://hub.docker.com/r/library/python) 的 Docker 映像檔中執行，因此二進位檔必須與此發行套件相容。

Docker 架構是建置 OpenWhisk 相容 Docker 映像檔的便利方法。您可以使用 `ibmcloud fn sdk install docker` CLI 外掛程式指令來安裝架構。

主要二進位程式必須位在容器的 `/action/exec` 中。執行檔會從可解除序列化為 `JSON` 物件的單一指令行引數字串接收到輸入引數。它必須使用 `stdout`，以已序列化 `JSON` 的單行字串形式來傳回結果。

您可以修改 `dockerSkeleton` 中包含的 `Dockerfile`，來包含任何編譯步驟或相依關係。
