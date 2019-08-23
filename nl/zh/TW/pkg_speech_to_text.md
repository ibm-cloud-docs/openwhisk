---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: package, cognitive, functions

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


# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## 套件

|名稱|可用性|說明|
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) |可安裝|使用 {{site.data.keyword.speechtotextshort}} V1 服務。|
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) |預先安裝（在東京無法使用）|呼叫 Watson API 以將語音轉換為文字|

## {{site.data.keyword.speechtotextshort}} 服務
{: #speech_to_text}

可安裝的 {{site.data.keyword.speechtotextfull}} 服務提供了一個 [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external}，該 API 使用 IBM 的語音識別功能來產生語音音訊的記錄。
{:shortdesc}

該服務可以轉錄不同語言和音訊格式的語音。除了基本謄本之外，該服務還可以產生音訊的許多方面的詳細資訊。對於大部分的語言，該服務支援兩個取樣率：寬頻和窄頻。它會以 UTF-8 字集傳回所有 JSON 回應內容。如需該服務的相關資訊，請參閱 [IBM&reg; Cloud 文件](/docs/services/speech-to-text?topic=speech-to-text-about)。

{{site.data.keyword.speechtotextshort}} 套件包含下列實體。您可以透過按一下實體名稱在 {{site.data.keyword.speechtotextshort}} API 參考資料中找到相關資訊。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 使用 {{site.data.keyword.speechtotextshort}} V1 服務。|
| [`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`model_id` | 取得模型。|
| [`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 列出模型。|
| [`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`audio`、`content_type`、`model`、`customization_id`、`acoustic_customization_id`、`base_model_version`、`customization_weight`、`inactivity_timeout`、`keywords`、`keywords_threshold`、`max_alternatives`、`word_alternatives_threshold`、`word_confidence`、`timestamps`、`profanity_filter`、`smart_formatting`、`speaker_labels`| 辨識音訊 (sessionless)。|
| [`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`id` | 檢查一項工作。|
| [`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 檢查工作。|
| [`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`audio`、`content_type`、`model`、`callback_url`、`events`、`user_token`、`results_ttl`、`customization_id`、`acoustic_customization_id`、`base_model_version`、`customization_weight`、`inactivity_timeout`、`keywords`、`keywords_threshold`、`max_alternatives`、`word_alternatives_threshold`、`word_confidence`、`timestamps`、`profanity_filter`、`smart_formatting`、`speaker_labels`| 建立工作。|
| [`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`id` | 刪除工作。|
| [`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`callback_url`、`user_secret` | 登錄回呼。|
| [`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`callback_url` | 取消登錄回呼。|
| [`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`base_model_name`、`dialect`、`description` | 建立自訂語言模型。|
| [`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 刪除自訂語言模型。|
| [`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 取得自訂語言模型。|
| [`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`language` | 列出自訂語言模型。|
| [`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 重設自訂語言模型。|
| [`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_type_to_add`、`customization_weight` | 訓練自訂語言模型。|
| [`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 升級自訂語言模型。|
| [`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`corpus_name`、`corpus_file`、`allow_overwrite`| 新增語料庫。|
| [`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`corpus_name` | 刪除語料庫。|
| [`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`corpus_name` | 取得語料庫。|
| [`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 列出語料庫。|
| [`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_name`、`word`、`sounds_like`、`display_as`| 新增自訂單字。|
| [`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`words` | 新增自訂單字。|
| [`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_name` | 刪除自訂單字。|
| [`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_name` | 取得自訂單字。|
| [`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_type`、`sort` | 列出自訂單字。|
| [`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`base_model_name`、` description`| 建立自訂原音模型。|
| [`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 刪除自訂原音模型。|
| [`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 取得自訂原音模型。|
| [`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`language` | 列出自訂原音模型。|
| [`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 重設自訂原音模型。|
| [`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`custom_language_model_id` | 訓練自訂原音模型。|
| [`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`custom_language_model_id` | 升級自訂原音模型。|
| [`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`audio_name`、`audio_resource`、`content_type`、`contained_content_type`、`allow_overwrite`| 新增音訊資源。|
| [`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`audio_name` | 刪除音訊資源。|
| [`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`audio_name` | 取得音訊資源。|
| [`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id` | 列出音訊資源。|
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id` | 刪除標籤資料。|

### 建立 {{site.data.keyword.speechtotextshort}} 服務實例
{: #service_instance_speechtotext}

在安裝套件之前，您必須先建立 {{site.data.keyword.speechtotextshort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.speechtotextshort}} 服務實例](https://cloud.ibm.com/catalog/services/speech_to_text){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

### 安裝 {{site.data.keyword.speechtotextshort}} 套件
{: #install_speechtotext}

在您具有 {{site.data.keyword.speechtotextshort}} 服務實例之後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.speechtotextshort}} 套件安裝到您的名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #speechtotext_cli}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.speechtotextshort}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.speechtotextshort}} 套件儲存庫。

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.speechtotextshort}} 實例連結到該套件。

    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    視您建立服務實例的地區而定，服務實例可能會以不同方式命名，因為它是 IAM 服務。如果指令失敗，請針對 bind 指令使用下列服務名稱：
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    **輸出範例**

    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.speechtotextshort}} 服務實例認證配置該套件。

    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    **輸出範例**

    ```
    ok: got package speech-to-text-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #speechtotext_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用名稱空間下拉功能表，選取您要在其中安裝套件的名稱空間。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Speech To Text** 套件。

5. 按一下**安裝**。

6. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 `speech-to-text-v1` 的新套件。

7. 若要使用 **speech-to-text-v1** 套件中的動作，必須將服務認證連結到這些動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 [CLI 指示](#speechtotext_cli)中的步驟 4 和 5。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。 
  
  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下您要使用的 **speech-to-text-v1** 套件中的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

### 使用 {{site.data.keyword.speechtotextshort}} 套件
{: #usage_speechtotext}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

請嘗試 `list-models` 動作。
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson：Speech to Text
{: #preinstall_speechtotext}

在東京地區無法使用此預先安裝的套件。請參閱可安裝的 [Speech to Text](#install_speechtotext)。
{: tip}

`/whisk.system/watson-speechToText` 套件提供一種簡便的方式來呼叫若要將語音轉換為文字的 Watson API。
{: shortdesc}

該套件包含下列動作。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` |套件| `username`、`password` |將語音轉換為文字的套件。|
|`/whisk.system/watson-speechToText/speechToText` |動作|`payload`、`content_type`、`encoding`、`username`、`password`、`continuous`、`inactivity_timeout`、`interim_results`、`keywords`、`keywords_threshold`、`max_alternatives`、`model`、`timestamps`、`watson-token`、`word_alternatives_threshold`、`word_confidence`、`X-Watson-Learning-Opt-Out`|將音訊轉換為文字。|

淘汰使用包含 `/whisk.system/watson/speechToText` 動作的 `/whisk.system/watson` 套件。請改為參閱 [{{site.data.keyword.speechtotextshort}} 套件](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud)。
{: deprecated}

### 在 {{site.data.keyword.cloud_notm}} 中設定 Watson Speech to Text 套件

如果您是從 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk}}，則會自動為 {{site.data.keyword.cloud_notm}} Watson 服務實例建立套件連結。

1. 在 {{site.data.keyword.cloud_notm}} [儀表板](https://cloud.ibm.com){: external}中，建立 Watson Speech to Text 服務實例。請務必記住服務實例名稱，以及您所在的 {{site.data.keyword.cloud_notm}} 組織及空間。

2. 重新整理名稱空間中的套件。重新整理會自動建立您所建立之 Watson 服務實例的套件連結。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **輸出範例**

  ```
  created bindings:
  Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. 列出套件，查看已建立連結。

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **輸出範例**

  ```
  packages
  /myOrg_mySpace/Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### 在 {{site.data.keyword.cloud_notm}} 外部設定 Watson Speech to Text 套件

如果不是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.cloud_notm}} 外部設定 Watson Speech to Text，則必須為 Watson Speech to Text 服務手動建立套件連結。您需要 Watson Speech to Text 服務使用者名稱及密碼。

建立針對 Watson Speech to Text 服務所配置的套件連結。
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### 將語音轉換為文字

`/whisk.system/watson-speechToText/speechToText` 動作會將音訊語音轉換為文字。支援下列參數。

|參數|說明|
| --- | --- |
| `username` |Watson API 使用者名稱。|
| `password` |Watson API 密碼。|
| `payload` |要轉換為文字的已編碼語音二進位資料。|
| `content_type` |音訊的 MIME 類型。|
| `encoding` |語音二進位資料的編碼。|
| `continuous` |指出是否傳回代表以長間歇分隔之連續詞組的多個最終結果。|
| `inactivity_timeout` |時間（秒），如果在所提交的音訊中只偵測到靜音，將在此期限之後結束連線。|
| `interim_results` |指出服務是否要傳回臨時結果。|
| `keywords` |要在音訊中標示的關鍵字清單。|
| `keywords_threshold` |信任值，其為標示關鍵字的下限。|
| `max_alternatives` |要傳回之替代記錄的數目上限。|
| `model` |要用於識別要求的模型 ID。|
| `timestamps` |指出是否要為每個單字傳回時間校準。|
| `watson-token` |提供服務的鑑別記號，以作為提供服務認證的替代方案。|
| `word_alternatives_threshold` |信任值，其為將假設識別為可能替代字的下限。|
| `word_confidence` |指出是否要為每個單字傳回範圍從 0 到 1 之間的信任測量值。|
| `X-Watson-Learning-Opt-Out` |指出是否要拒絕呼叫的資料收集。|

在套件連結中呼叫 `speechToText` 動作，以轉換已編碼的音訊。

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**輸出範例**
```
{
  "data": "Hello Watson"
}
```
{: screen}


