---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploy, deployment templates, templates, example, quickstart, functions, serverless

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


# 部署快速入門範本
{: #templates}

{{site.data.keyword.openwhisk}} 提供範本型錄，以協助您在下一個專案中快速開始。範本是動作、觸發程式與序列的組合。有些範本也會併入 {{site.data.keyword.cloud_notm}} 中的其他服務實例。使用範本，您可以快速且輕鬆地建立專案，並立即開始編碼。
{: shortdesc}


## 可用的快速入門範本
{: #available-templates}

|名稱|說明|支援的運行環境 |來源儲存庫|
|:-----------------|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} 事件](#cloudant-template) | 在 {{site.data.keyword.cloudantfull}} 資料庫中編輯或新增文件時，請在主控台中記載該變更。|Node.js、Swift、Python、PHP |[程式碼](https://github.com/ibm-functions/template-cloudant-trigger){: external}|
| [上傳影像](#image-template) | 使用 Web 動作將影像上傳至 {{site.data.keyword.cos_full}} 實例的儲存區，然後擷取該影像的縮圖。| Node.js |[程式碼](https://github.com/ibm-functions/template-cloud-object-storage){: external}
| [取得 HTTP 資源](#get-http-resource-template) | 使用 HTTP 事件來呼叫 Web 動作，並從 Yahoo Weather API 取得資料。|Node.js、Python |[程式碼](https://github.com/ibm-functions/template-get-external-resource){: external}
| [Hello World](#hello-world-template) | 建立基本動作，以接受 JSON 物件作為單一參數。|Node.js、Swift、Python、PHP |[程式碼](https://github.com/ibm-functions/template-hello-world){: external}
| [{{site.data.keyword.messagehub}} 事件](#messagehub-events-template) | 在 {{site.data.keyword.messagehub_full}} 主題中新增資料時，請在主控台中記載該變更。|Node.js、Swift、Python、PHP |[程式碼](https://github.com/ibm-functions/template-messagehub-trigger){: external}
| [定期 Slack 提示](#slack-reminder-template) | 根據定期觸發程式使用 Slack 的 Webhook。|Node.js、Swift、Python、PHP |[程式碼](https://github.com/ibm-functions/template-reminder-slack){: external}
可以在每個範本的程式碼儲存庫中存取範本的程式碼，根據需要進行修改，並據以建立您自己的函數。
{: tip}


## 部署 {{site.data.keyword.cloudant_short_notm}} 事件範本
{: #cloudant-template}

{{site.data.keyword.cloudant_short_notm}} 範本會建立動作序列，以及開始該序列的觸發程式。在所連接的 {{site.data.keyword.cloudant_short_notm}} cats 資料庫範例中進行變更時，會發動觸發程式。預期的資料項目是 cat，其具有已定義的名稱和顏色。將新的 cat 新增至資料庫或已編輯現行的 cat 時，資料會記載至主控台。

### 從使用者介面部署 {{site.data.keyword.cloudant_short_notm}} 事件範本
{: #cloudant-template-ui}

1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 按一下**快速入門範本**。

3. 按一下 **Cloudant 事件**。

4. 建立 {{site.data.keyword.cloudant_short_notm}} 動作。
    1. 提供套件的名稱，或使用提供的預設名稱 `new-cloudant-item`。
    2. 在**動作**清單中，選取 `Node.js 6` 作為 cats 資料庫範例，然後按**下一步**。

5. 建立 {{site.data.keyword.cloudant_short_notm}} 觸發程式。觸發程式會在接收到事件來源中的事件時呼叫動作。若要建立觸發程式，您必須提供 {{site.data.keyword.messagehub}} 服務實例及服務認證。
    1. 在 **Cloudant 實例**清單中，選取**建立您自己的實例**。即會開啟 {{site.data.keyword.cloudant_short_notm}} 設定頁面。
    2. 建立 {{site.data.keyword.cloudant_short_notm}} 服務實例。
    3. 建立一組「服務認證」。
    4. 按一下**確定**以關閉 {{site.data.keyword.cloudant_short_notm}} 設定頁面，並回到 {{site.data.keyword.openwhisk_short}} 主控台。
    5. 在 **Cloudant 實例**清單中，您現在可以選取**輸入您自己的認證**，並提供下列資訊。
        
        * `Username` - 您的 {{site.data.keyword.cloudant_short_notm}} 使用者名稱。
        * `Password` - 您的 {{site.data.keyword.cloudant_short_notm}} 密碼。
        * `Host` - `<username>.cloudant.com`。 
        * `Database` - {{site.data.keyword.cloudant_short_notm}} 資料庫的名稱。

6. 按一下**部署**。部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或傳回並查看可用範本的型錄。

### 從 CLI 部署 {{site.data.keyword.cloudant_short_notm}} 事件範本
{: #cloudant-template-cli}

1. 複製範本儲存庫。
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. 導覽至您要使用之動作運行環境的目錄：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. 使用下列環境變數，部署範本。
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>瞭解環境變數</caption>
    <thead>
    <th colspan=2>瞭解環境變數</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>您的 {{site.data.keyword.cloudant_short_notm}} 使用者名稱</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>您的 {{site.data.keyword.cloudant_short_notm}} 密碼</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>您的 {{site.data.keyword.cloudant_short_notm}} 資料庫的名稱</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>套件的自訂名稱</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>規則的自訂名稱</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>觸發程式的自訂名稱</td></tr>
    </tbody></table>

## 部署上傳影像範本
{: #image-template}

「上傳影像」範本會建立一個 Web 動作，可用於透過小型介面將影像上傳到 {{site.data.keyword.cos_full_notm}} 儲存區。然後，範本會將影像擷取為縮圖，並將它顯示在 Web 動作的介面中。


### 從使用者介面部署上傳影像範本
{: #image-template-ui}

1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 按一下**快速入門範本**。

3. 按一下**上傳影像**。 

4. 輸入套件的名稱，或使用預設名稱 `upload-image`，然後按**下一步**。

6. 範本需要 {{site.data.keyword.cos_full_notm}} 服務實例中的服務認證。在 **{{site.data.keyword.cos_full_notm}}** 清單中，選取下列其中一個選項：
  * **建立實例**。如果您沒有現有的服務實例，請選取此選項以建立服務實例。
      1. 在開啟的 {{site.data.keyword.cos_full_notm}} 服務實例建立頁面中，建立服務實例。
      2. [建立一組 HMAC 服務認證](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。
      3. [建立至少一個儲存區](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets)。
  * **輸入您自己的認證**。選取此選項以手動輸入您自己的 {{site.data.keyword.cos_full_notm}} 服務實例認證。認證必須具有 HMAC 金鑰，且服務實例必須至少有一個儲存區。
  * **現有實例**。如果您有任何現有的 {{site.data.keyword.cos_full_notm}} 實例，請從清單中選取其中一個實例。認證必須具有 HMAC 金鑰，且服務實例必須至少有一個儲存區。

7. 按一下**部署**。

8. 在左側導覽中，按一下**端點**

9. 在 **Web 動作**區段中，複製不含 .json 字尾的鏈結，並將它貼到瀏覽器的位址列中。即會顯示範本 Web 動作的介面。

10. 選用項目：在範本部署之後，您可以導覽至**動作**儀表板，以自訂兩個新套件中的程式碼：
    * `cloud-object-storage` 套件，其包含使用 {{site.data.keyword.cos_full_notm}} 實例的動作
    * 範本套件（預設名稱 `upload-image`），其包含 `app` 動作

### 從 CLI 部署上傳影像範本
{: #image-template-cli}

1. [安裝 {{site.data.keyword.cos_full_notm}} 套件](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)。

2. 複製範本儲存庫。
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. 導覽至 `nodejs` 運行環境目錄。
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 將自訂套件名稱和 {{site.data.keyword.cos_full_notm}} 儲存區的名稱用作環境變數來部署範本。
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 在 {{site.data.keyword.openwhisk_short}} 主控台的[「動作」頁面](https://cloud.ibm.com/openwhisk/actions){: external}中，按一下 `process-change` 動作。

5. 在左側導覽中，按一下**端點**。

6. 在 **Web 動作**區段中，複製不含 .json 字尾的鏈結，並將它貼到瀏覽器的位址列中。即會顯示範本 Web 動作的介面。

## 部署取得 HTTP 資源範本
{: #get-http-resource-template}

「取得 HTTP 資源」範本會建立動作來提取外部資源、Yahoo Weather API，然後傳回資料。該動作啟用為 Web 動作，來容許使用已啟用了 CORS 的 URL 來呼叫該動作，而無需鑑別金鑰，這對於為 Web 應用程式建置後端非常有用。

依預設，`get-http-resource` 端點對於任何要呼叫該端點的使用者都公開可用。
{: note}

### 從使用者介面部署取得 HTTP 資源範本
{: #get-http-resource-template-ui}

1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 按一下**快速入門範本**。

3. 按一下**取得 HTTP 資源**。

3. 輸入套件的名稱，或使用預設名稱 `get-http-resource`。

4. 選擇您的動作所適用的運行環境：Node.js 8、Node.js 6 或 Python 3。

5. 按一下**部署**。

6. 對下列 URL 進行 curl 處理，以呼叫動作：`https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`。例如：
```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或傳回並查看可用範本的型錄。

### 從 CLI 部署取得 HTTP 資源範本
{: #get-http-resource-template-cli}

1. 複製範本儲存庫。
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. 導覽至您要使用之動作運行環境的目錄：`nodejs-6`、`nodejs` 或 `python`。
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. 使用自訂套件名稱作為環境變數，來部署範本。
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. 對下列 URL 進行 curl 處理，以呼叫動作：`https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`。例如：
```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## 部署 Hello World 範本
{: #hello-world-template}

您可以部署此基本 Hello World 動作，以開始使用 {{site.data.keyword.openwhisk_short}} 或測試您建立的其他實體，例如觸發程式及規則。

### 從使用者介面部署 Hello World 範本
{: #hello-world-template-ui}

1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 按一下**快速入門範本**。

3. 按一下 **Hello World**。

4. 輸入套件的名稱，或使用預設名稱 `hello-world`。

5. 選擇您的動作所適用的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。

6. 按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或傳回並查看可用範本的型錄。

### 從 CLI 部署 Hello World 範本
{: #hello-world-template-cli}

1. 複製 Hello World 範本儲存庫。
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. 導覽至您要使用之動作運行環境的目錄：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. 部署範本。
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## 部署 {{site.data.keyword.messagehub}} 事件範本
{: #messagehub-events-template}

「{{site.data.keyword.messagehub}} 事件」範本會建立一個動作，以及一個將開始該動作的觸發程式。只要將新項目新增至建立範本期間選擇的 {{site.data.keyword.messagehub}} 主題中，就會發動該觸發程式。

### 從使用者介面部署 {{site.data.keyword.messagehub}} 事件範本
{: #messagehub-events-template-ui}

1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 按一下**快速入門範本**。

3. 按一下 **{{site.data.keyword.messagehub}} 事件**。

4. 建立 {{site.data.keyword.messagehub}} 動作。
    1. 提供套件的名稱，或使用提供的預設名稱 `message-hub-events`。
    2. 選擇您的動作所適用的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。
    3. 按**下一步**。

5. 建立 {{site.data.keyword.messagehub}} 觸發程式。觸發程式會在接收到事件來源中的事件時呼叫動作。若要建立觸發程式，必須提供 {{site.data.keyword.messagehub}} 服務實例和服務認證。
    1. 在 **MessageHub 實例**清單中，選取**建立您自己的實例**。即會開啟 {{site.data.keyword.messagehub}} 建立頁面。
    2. 建立 {{site.data.keyword.messagehub}} 服務實例。
    3. 建立一組「服務認證」。
    4. 按一下**確定**以關閉 {{site.data.keyword.messagehub}} 建立頁面，並回到 {{site.data.keyword.messagehub}} 主控台。
    5. 在 **MessageHub 實例**清單中，您現在可以選取**輸入您自己的認證**，並提供下列資訊：
        * `Username` - 您的 {{site.data.keyword.messagehub}} 使用者名稱。
        * `Password` - 您的 {{site.data.keyword.messagehub}} 密碼。
        * `kafka_admin_url` - {{site.data.keyword.messagehub}} 管理 REST URL。
        * `Database` - {{site.data.keyword.messagehub}} 資料庫的名稱。
        * `主題` - 若要訂閱的主題。

6. 按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或傳回並查看可用範本的型錄。

### 從 CLI 部署 {{site.data.keyword.messagehub}} 事件範本
{: #messagehub-events-template-cli}

1. 複製範本儲存庫。
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. 導覽至您要使用之動作運行環境的目錄：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. 使用下列環境變數來部署範本。
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>瞭解環境變數</caption>
    <thead>
    <th colspan=2>瞭解環境變數</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>您的 {{site.data.keyword.messagehub}} 管理 REST URL</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>要訂閱的主題</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>您的 {{site.data.keyword.messagehub}} 使用者名稱</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>您的 {{site.data.keyword.messagehub}} 密碼</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>套件的自訂名稱</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>規則的自訂名稱</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>觸發程式的自訂名稱</td></tr>
    </tbody></table>

## 部署定期 Slack 提示範本
{: #slack-reminder-template}

「定期 Slack 提示」範本會根據使用者在建立觸發程式期間所提供的間隔而張貼至 Slack。

### 從使用者介面部署定期 Slack 提示範本
{: #slack-reminder-template-ui}

1. 請前往 https://api.slack.com/incoming-webhooks 來設定所需的送入 Webhook URL。

1. 移至 {{site.data.keyword.openwhisk_short}} 主控台中的[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 按一下**快速入門範本**。

3. 按一下**定期 Slack 提示事件**。

4. 建立「定期 Slack 提示」動作。
    1. 提供套件的名稱，或使用提供的預設名稱 `periodic-slack-reminder`。
    2. 選擇您的動作所適用的運行環境：Node.js 8、Node.js 6、Python 3、Swift 4 或 PHP 7.1。
    3. 在「參數」區段的**參數值**欄位中，輸入 Webhook URL，例如 `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`。
    4. 按**下一步**。

5. 建立「定期 Slack 提示」觸發程式。觸發程式會在接收到事件來源中的事件時呼叫動作。
    1. 使用型樣或 `Cron` 表示式來指定觸發程式的時間間隔。
        * `型樣` - 選取「平日」、「小時」和「分鐘」的 UTC 時間。
        * `Cron` - 指定基於 <a href="http://crontab.org">UNIX crontab 語法</a>的 cron 序列。依 `X X X X X` 格式，使用以空格區隔的 5 個或以下項目。
    2. 新增觸發程式 JSON 有效負載。

6. 按一下**部署**。

部署範本之後，您可以進一步編輯程式碼，以根據需要加以自訂，或傳回並查看可用範本的型錄。

### 從 CLI 部署定期 Slack 提示範本
{: #slack-reminder-template-cli}

1. 請前往 https://api.slack.com/incoming-webhooks 來設定所需的送入 Webhook URL。

1. 複製範本儲存庫。
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. 導覽至您要使用之動作運行環境的目錄：`nodejs-6`、`nodejs`、`php`、`python` 或 `swift`。
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. 使用下列環境變數，部署範本。
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>瞭解環境變數</caption>
    <thead>
    <th colspan=2>瞭解環境變數</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>Webhook URL，例如 <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>基於 <a href="http://crontab.org">UNIX crontab 語法</a>的 cron 序列。使用的欄位不超過 5 個，並且格式為 <code>X X X X X</code>。</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>套件的自訂名稱</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>規則的自訂名稱</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>觸發程式的自訂名稱</td></tr>
    </tbody></table>


