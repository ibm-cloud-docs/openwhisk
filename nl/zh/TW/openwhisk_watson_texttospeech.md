---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: text to speech, watson, cognitive, converting, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
# Watson：Text to Speech 套件
{: #openwhisk_catalog_watson_texttospeech}

在東京地區無法使用此預先安裝的套件。請參閱使用 IAM 鑑別可安裝的 [Text to Speech](/docs/openwhisk?topic=cloud-functions-text-to-speech-package) 套件。
{: tip}

`/whisk.system/watson-textToSpeech` 套件提供一種簡便的方式來呼叫要將文字轉換為語音的 Watson API。
{: shortdesc}

該套件包含下列動作。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` |套件|username、password|將文字轉換為語音的套件|
|`/whisk.system/watson-textToSpeech/textToSpeech` |動作|payload、voice、accept、encoding、username、password|將文字轉換為音訊|

**附註**：已淘汰套件 `/whisk.system/watson`（包括 `/whisk.system/watson/textToSpeech` 動作）。請改為參閱[可安裝的 {{site.data.keyword.texttospeechshort}} 套件](/docs/openwhisk?topic=cloud-functions-text-to-speech-package)。

## 在 {{site.data.keyword.Bluemix_notm}} 中設定 Watson Text to Speech 套件

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

## 在 {{site.data.keyword.Bluemix_notm}} 外部設定 Watson Text to Speech 套件

如果您不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者要在 {{site.data.keyword.Bluemix_notm}} 外部設定 Watson Text to Speech，則必須手動建立 Watson Text to Speech 服務的套件連結。您需要 Watson Text to Speech 服務使用者名稱及密碼。

建立針對 Watson Speech to Text 服務所配置的套件連結。
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 將某串文字轉換為語音

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
