---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: watson, translator, cognitive, translating text, language, functions

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


# Translator

## {{site.data.keyword.languagetranslatorshort}} 套件

{: #pkg_translator}

可安裝的 {{site.data.keyword.languagetranslatorfull}} 套件將文字從一種語言翻譯為另一種語言。該服務提供多個 IBM 提供的翻譯模型，您可以根據自己獨特的術語和語言來自訂它們。
{: shortdesc}

{{site.data.keyword.languagetranslatorshort}} 套件包含下列實體。您可以透過按一下實體名稱在 {{site.data.keyword.languagetranslatorshort}} API 參考資料中找到更多詳細資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 使用 {{site.data.keyword.languagetranslatorshort}} 服務。|
| [`translate`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`text`、`model_id`、`source`、`target`| 翻譯文字。|
| [`identify`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`text`| 識別文字的語言。|
| [`list-identifiable-languages`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 列出可識別的語言。|
| [`create-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`base_model_id`、`name`、`forced_glossary`、`parallel_corpus`| 建立模型。|
| [`delete-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`model_id` | 刪除模型。|
| [`get-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`model_id` | 取得模型詳細資料。|
| [`list-models`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`source`、`target`、`default_models`| 列出模型。|

## 建立 {{site.data.keyword.languagetranslatorshort}} 服務實例
{: #service_instance_translator}

在安裝套件之前，您必須先建立 {{site.data.keyword.languagetranslatorshort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.languagetranslatorshort}} 服務實例](https://cloud.ibm.com/catalog/services/language_translator){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.languagetranslatorshort}} 套件
{: #install_translator}

具有 {{site.data.keyword.languagetranslatorshort}} 服務實例後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.languagetranslatorshort}} 套件安裝到名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #languagetranslator_cli}

**開始之前**

為 {{site.data.keyword.cloud_notm}} CLI [安裝 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

安裝 {{site.data.keyword.languagetranslatorshort}} 套件。

1. 複製 {{site.data.keyword.languagetranslatorshort}} 套件儲存庫。

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.languagetranslatorshort}} 實例連結到該套件。

    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    **輸出範例**

    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.languagetranslatorshort}} 服務實例認證配置該套件。

    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    **輸出範例**

    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #languagetranslator_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用名稱空間下拉功能表，選取您要在其中安裝套件的名稱空間。名稱空間是由合併的組織及空間名稱組成。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Language Translator** 套件。

6. 按一下**安裝**。

7. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 `language-translator-v3` 的新套件。

8. 若要使用 `language-translator-v3` 套件中的動作，必須將服務認證連結到這些動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 [CLI 指示中的步驟 5 和 6](#languagetranslator_cli)。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。
  
  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下 `language-translator-v3` 套件中要使用的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的`參數`。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.languagetranslatorshort}} 套件
{: #usage_translator}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning){: external}中的其他詳細資料。

此套件的函數使用現行版本的 Language Translator (`2018-05-01`)。請嘗試 `identify` 動作。
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}




