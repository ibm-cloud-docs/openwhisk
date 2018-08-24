---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.toneanalyzershort}} 套件

{{site.data.keyword.toneanalyzerfull}} 服務會使用語言分析，在撰寫的文字中偵測情緒和語氣。
{:shortdesc}

該服務可以同時在文件和文句層次上分析語氣。您可以利用該服務來瞭解如何感知您所撰寫的溝通文字，然後再改進溝通的語氣。企業可以使用此服務來瞭解客戶溝通的語氣，以適當的回應每位客戶，或瞭解及改進其客戶交談。

**附註：**已針對 Tone Analyzer 服務停用要求記載。不論是否設定 `X-Watson-Learning-Opt-Out` 要求標頭，該服務不會記載要求和回應的資料，也不會保留資料。

{{site.data.keyword.toneanalyzershort}} 套件包含下列實體。您可以按一下實體名稱，在 {{site.data.keyword.toneanalyzershort}} API 參照中找到其他詳細資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html) |套件| username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、 | 使用 {{site.data.keyword.toneanalyzershort}} 服務。|
| [tone](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、tone_input、content_type、sentences、tones、content_language、accept_language、 | 分析一般語氣。|
| [tone-chat](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、utterances、content_language、accept_language、 | 分析客戶參與語氣。|

## 建立 {{site.data.keyword.toneanalyzershort}} 服務實例
{: #service_instance}

在安裝套件之前，您必須先建立 {{site.data.keyword.toneanalyzershort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.toneanalyzershort}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/catalog/services/tone_analyzer)。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.toneanalyzershort}} 套件
{: #install}

在您具有 {{site.data.keyword.toneanalyzershort}} 服務實例之後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.toneanalyzershort}} 套件安裝到您的名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #toneanalyzer_cli}

開始之前：
  1. [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](bluemix_cli.html#cloudfunctions_cli)。
  2. 安裝 [`wskdeploy` 指令 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)，並將下載的二進位檔新增至您的 PATH。

若要安裝 {{site.data.keyword.toneanalyzershort}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.toneanalyzershort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    wskdeploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
    ```
    {: pre}

3. 驗證該套件已新增至套件清單。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    輸出：
    ```
    packages
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.toneanalyzershort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    視您建立服務實例的地區而定，服務實例可能會以不同方式命名，因為它是 IAM 服務。如果上述指令失敗，請針對 bind 指令使用下列服務名稱：
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}
    輸出範例：
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.toneanalyzershort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #toneanalyzer_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/openwhisk/create)。

2. 使用 **Cloud Foundry 組織**和 **Cloud Foundry 空間**清單，選取您要在其中安裝 {{site.data.keyword.cos_short}} 套件的名稱空間。名稱空間是由合併的組織及空間名稱組成。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Tone Analyzer** 套件。

5. 按一下**安裝**。

6. 安裝好「套件」之後，您將被重新導向至「動作」頁面，您可以搜尋新的套件，其名稱為 **tone-analyzer-v3**。

7. 若要使用 **tone-analyzer-v3** 套件中的「動作」，您必須將服務認證連結至動作。
  * 若要將服務認證連結至套件中的所有動作，請遵循上述 CLI 指令中的步驟 5 及 6。 
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。**附註**：您必須針對要使用的每一個動作，完成下列步驟。
    1. 按一下您要使用的 **tone-analyzer-v3** 套件中的「動作」。即會開啟該「動作」的詳細資料頁面。 
    2. 在左側導覽中，按一下**參數**區段。 
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.toneanalyzershort}} 套件
{: #usage}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作將需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning)中的其他詳細資料。

此套件的功能使用 Tone Analyzer 現行版本：2017-09-21。請嘗試 `tone` 動作。
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}
