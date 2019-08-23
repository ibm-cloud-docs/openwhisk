---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, serverless, functions

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


# {{site.data.keyword.personalityinsightsshort}}
{: #pkg_person_insights}

可安裝的 {{site.data.keyword.personalityinsightsfull}} 服務可讓應用程式從社交媒體、企業資料或其他數位通訊中衍生洞察。此服務使用語言分析，從數位通訊（例如電子郵件、文字訊息、推文及討論區貼文）中推斷個人的內在人格特徵，包括五大性格特質、需求和價值。
{: shortdesc}

該服務可從嘈雜的社群媒體中自動推斷能反映其人格特徵的個人寫真。該服務可根據其分析結果來推斷使用偏好，並可針對有時間戳記的 JSON 內容報告時間行為。
* 如需此服務用於說明人格特徵的模型意義的相關資訊，請參閱[特徵模型](/docs/services/personality-insights?topic=personality-insights-models)。
* 如需消費喜好設定意義的相關資訊，請參閱[消費喜好設定](/docs/services/personality-insights?topic=personality-insights-preferences)。

{{site.data.keyword.personalityinsightsshort}} 服務停用了要求記載。不論是否設定 `X-Watson-Learning-Opt-Out` 要求標頭，該服務不會記載要求和回應的保存資料。
{: note}

{{site.data.keyword.personalityinsightsshort}} 套件包含下列實體。您可以按一下實體名稱，在 {{site.data.keyword.personalityinsightsshort}} API 參考資料中找到其他詳細資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 使用 {{site.data.keyword.personalityinsightsshort}} V3 服務。|
| [`profile`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`content`、`content_type`、`content_language`、`accept_language`、`raw_scores`、`csv_headers`、`consumption_preferences` | 取得設定檔。|
| [`profile-as-csv`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`content`、`content_type`、`content_language`、`accept_language`、`raw_scores`、`csv_headers`、`consumption_preferences` | 取得 CSV 檔的設定檔。|

## 建立 {{site.data.keyword.personalityinsightsshort}} 服務實例
{: #service_instance_insights}

在安裝套件之前，您必須先建立 {{site.data.keyword.personalityinsightsshort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.personalityinsightsshort}} 服務實例](https://cloud.ibm.com/catalog/services/personality_insights){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.personalityinsightsshort}} 套件
{: #install_insights}

具有 {{site.data.keyword.personalityinsightsshort}} 服務實例後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.personalityinsightsshort}} 套件安裝到名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #personalityinsights_cli}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.personalityinsightsshort}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.personalityinsightsshort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.personalityinsightsshort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    視您建立服務實例的地區而定，服務實例可能會以不同方式命名，因為它是 IAM 服務。如果指令失敗，請針對 bind 指令使用下列服務名稱：
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    **輸出範例**
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.personalityinsightsshort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #personalityinsights_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用「名稱空間」下拉功能表，選取要將套件安裝到其中的名稱空間。 

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Personality Insights** 套件。

6. 按一下**安裝**。

7. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 `personality-insights-v3` 的新套件。

8. 若要使用 `personality-insights-v3` 套件中的動作，必須將服務認證連結到這些動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 CLI 指示中的步驟 5 和 6。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。

  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下 `personality-insights-v3` 套件中要使用的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的參數。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.personalityinsightsshort}} 套件
{: #usage_insights}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning){: external}中的其他詳細資料。

此套件的函數使用現行版本的 {{site.data.keyword.personalityinsightsshort}} (2017-10-13)。請嘗試 `profile` 動作。
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "You can write an excerpt about yourself here, but it will need to be at least 100 words long. This excerpt is just some filler text and probably won't return anything very interesting from the personality insights service. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts. The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and for JSON content that is timestamped, can report temporal behavior."
```
{: pre}



