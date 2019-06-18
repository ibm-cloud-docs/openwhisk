---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: text to speech, watson, cognitive, functions, packages

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Text to Speech
{: #pkg_text_to_speech}

## 套件選項

|套件|可用性|說明|
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) |預先安裝（在東京無法使用）|將文字轉換為語音的套件|
| [`text-to-speech-v1`](#text_to_speech_ins) |可安裝| 使用 {{site.data.keyword.texttospeechshort}} 服務。|

## Watson Text to Speech
{: #text_to_speech}

在東京地區無法使用此預先安裝的套件。請參閱使用 IAM 鑑別可安裝的 [Text to Speech](#text_to_speech_ins) 套件。
{: tip}

`/whisk.system/watson-textToSpeech` 套件提供一種簡便的方式來呼叫若要將文字轉換為語音的 Watson API。
{: shortdesc}

該套件包含下列動作。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` |套件|username、password|將文字轉換為語音的套件|
|`/whisk.system/watson-textToSpeech/textToSpeech` |動作|payload、voice、accept、encoding、username、password|將文字轉換為音訊|

**附註**：已淘汰套件 `/whisk.system/watson`（包括 `/whisk.system/watson/textToSpeech` 動作）。請改為參閱[可安裝的 {{site.data.keyword.texttospeechshort}} 套件](#text_to_speech_ins)。

### 在 {{site.data.keyword.Bluemix_notm}} 中設定 Watson Text to Speech 套件

如果您是從 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk}}，則會自動為 {{site.data.keyword.Bluemix_notm}} Watson 服務實例建立套件連結。

1. 在 {{site.data.keyword.Bluemix_notm}} [儀表板](http://cloud.ibm.com)中，建立 Watson Text to Speech 服務實例。

  請務必記住服務實例名稱，以及您所在的 {{site.data.keyword.Bluemix_notm}} 組織及空間。

2. 重新整理名稱空間中的套件。重新整理會自動建立您所建立之 Watson 服務實例的套件連結。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  輸出範例：
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  列出套件，查看已建立套件連結：
  ```
  ibmcloud fn package list
  ```
  {: pre}

  輸出範例：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### 在 {{site.data.keyword.Bluemix_notm}} 外部設定 Watson Text to Speech 套件

如果您不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者要在 {{site.data.keyword.Bluemix_notm}} 外部設定 Watson Text to Speech，則必須手動建立 Watson Text to Speech 服務的套件連結。您需要 Watson Text to Speech 服務使用者名稱及密碼。

建立針對 Watson Speech to Text 服務所配置的套件連結。
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### 將文字轉換為語音

`/whisk.system/watson-textToSpeech/textToSpeech` 動作會將某串文字轉換為音訊語音。參數如下所示：

- `username`：Watson API 使用者名稱。
- `password`：Watson API 密碼。
- `payload`：要轉換為語音的文字。
- `voice`：說話者的聲音。
- `accept`：語音檔的格式。
- `encoding`：語音二進位資料的編碼。

在套件連結中呼叫 **textToSpeech** 動作，以轉換文字。
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
  ```
{: pre}

輸出範例：
```
{
      "payload": "<base64 encoding of a .wav file>"
  }
  ```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

可安裝的 {{site.data.keyword.texttospeechfull}} 服務提供了一個 API，該 API 使用 IBM 的語音合成功能，將文字合成為各種語言、用語和聲音形式的比較自然的語音。
{:shortdesc}

該服務支援每個語言至少一種男性或女性聲音，有時候是兩種。音訊會以最小延遲流回用戶端。如需該服務的相關資訊，請參閱 [IBM Cloud 文件](/docs/services/text-to-speech?topic=text-to-speech-about)。

{{site.data.keyword.texttospeechshort}} 套件包含下列實體。您可以按一下實體名稱，在 {{site.data.keyword.texttospeechshort}} API 參考資料中找到其他詳細資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) |套件| username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、 | 使用 {{site.data.keyword.texttospeechshort}} 服務。|
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、voice、customization_id、 | 取得語音。|
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) |動作| username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、 | 列出語音。|
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、accept、voice、customization_id、 | 合成音訊。|
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、voice、format、customization_id、 | 取得發音。|
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、language、description、 | 建立自訂模型。|
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、 | 刪除自訂模型。|
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、 | 取得自訂模型。|
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、language、 | 列出自訂模型。|
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、name、description、words、 | 更新自訂模型。|
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word、translation、part_of_speech、 | 新增自訂單字。|
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、words、 | 新增自訂單字。|
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word、 | 刪除自訂單字。|
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word、 | 取得自訂單字。|
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、 | 列出自訂單字。|
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) |動作|  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id、 | 刪除標籤資料。|

### 建立 {{site.data.keyword.texttospeechshort}} 服務實例
{: #service_instance_texttospeech}

在安裝套件之前，您必須先建立 {{site.data.keyword.texttospeechshort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.texttospeechshort}} 服務實例 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/catalog/services/text_to_speech)。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

### 安裝 {{site.data.keyword.texttospeechshort}} 套件
{: #install_texttospeech}

在您具有 {{site.data.keyword.texttospeechshort}} 服務實例之後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.texttospeechshort}} 套件安裝到您的名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #texttospeech_cli}

開始之前：
  1. [安裝 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.texttospeechshort}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.texttospeechshort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.texttospeechshort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    視您建立服務實例的地區而定，服務實例可能會以不同方式命名，因為它是 IAM 服務。如果上述指令失敗，請針對 bind 指令使用下列服務名稱：
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
    輸出範例：
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.texttospeechshort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #texttospeech_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://cloud.ibm.com/openwhisk/create)。

2. 使用 **Cloud Foundry 組織**和 **Cloud Foundry 空間**清單，選取您要在其中安裝此套件的名稱空間。 

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Text To Speech** 套件。

5. 按一下**安裝**。

6. 安裝好套件之後，您會被重新導向至動作頁面，而且您可以搜尋新的套件，其名稱為 **text-to-speech-v1**。

7. 若要使用 **text-to-speech-v1** 套件中的動作，您必須將服務認證連結至動作。
  * 若要將服務認證連結至套件中的所有動作，請遵循上述 CLI 指令中的步驟 5 及 6。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。**附註**：您必須針對要使用的每一個動作，完成下列步驟。
    1. 按一下您要使用的 **text-to-speech-v1** 套件中的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.texttospeechshort}} 套件
{: #usage_texttospeech}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

請嘗試 `list-voices` 動作。
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}

