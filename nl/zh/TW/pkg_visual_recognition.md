---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: functions, cognitive,

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


# {{site.data.keyword.visualrecognitionshort}}
{: #pkg_visual_recognition}

可安裝的 {{site.data.keyword.visualrecognitionfull}} 服務使用深度學習演算法來識別上傳到此服務的影像中的場景、物件和臉孔。您可以建立及訓練自訂分類器，以識別符合您需求的主題。
{:shortdesc}

{{site.data.keyword.visualrecognitionshort}} 套件包含下列實體。如需相關資訊，請透過按一下實體名稱來查看 {{site.data.keyword.visualrecognitionshort}} API 參考資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 使用 {{site.data.keyword.visualrecognitionshort}} 服務。|
| [`classify`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`images_file`、`accept_language`、`url`、`threshold`、`owners`、`classifier_ids`、`images_file_content_type`| 分類影像。|
| [`detect-faces`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`images_file`、`url`、`images_file_content_type`| 偵測影像中的臉孔。|
| [`create-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`classname_positive_examples`、`negative_examples`| 建立分類器。|
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id` | 刪除分類器。|
| [`get-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id` | 擷取分類器詳細資料。|
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`verbose` | 擷取分類器清單。|
| [`update-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id`、`classname_positive_examples`、`negative_examples`| 更新分類器。|
| [`get-core-ml-model`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`classifier_id` | 擷取分類器的核心 ML 模型。|
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id` | 刪除標籤資料。|

## 建立 {{site.data.keyword.visualrecognitionshort}} 服務實例
{: #service_instance_recognition}

在安裝套件之前，您必須先建立 {{site.data.keyword.visualrecognitionshort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.visualrecognitionshort}} 服務實例](https://cloud.ibm.com/catalog/services/watson_vision_combined){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.visualrecognitionshort}} 套件
{: #install_recognition}

具有 {{site.data.keyword.visualrecognitionshort}} 服務實例後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.visualrecognitionshort}} 套件安裝到名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #visualrecognition_cli}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.visualrecognitionshort}} 套件，請執行下列指令。

1. 複製 {{site.data.keyword.visualrecognitionshort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
    ```
    {: pre}

3. 驗證該套件已新增至套件清單。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **輸出**
    ```
    packages
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.visualrecognitionshort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    **輸出範例**
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.visualrecognitionshort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #visualrecognition_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用 **Cloud Foundry 組織**和 **Cloud Foundry 空間**清單，選取您要在其中安裝此套件的名稱空間。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Visual Recognition** 套件。

5. 按一下**安裝**。

6. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 **visual-recognition-v3** 的新套件。

7. 若要使用 **visual-recognition-v3** 套件中的動作，您必須將服務認證連結至動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 [CLI 指示](#visualrecognition_cli)中的步驟 4 和 5。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。
   
  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下您要使用的 **visual-recognition-v3** 套件中的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.visualrecognitionshort}} 套件
{: #usage_recognition}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning){: external}中的其他詳細資料。

此套件的功能使用 Visual Recognition 現行版本：`2018-03-19`。請嘗試 `list-classifiers` 動作。
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}


