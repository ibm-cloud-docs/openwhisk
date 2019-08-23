---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: natural language, understanding, watson knowledge studio, functions

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


# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

可安裝的 {{site.data.keyword.nlufull}} 服務會協助您大規模地分析文字內容的各種特性。
{: shortdesc}

提供文字、原始 HTML 或公用 URL，{{site.data.keyword.nlushort}} 就會為您提供所要求之功能的結果。依預設，該服務會在分析之前清除 HTML 內容，因此，結果可以忽略大部分廣告及其他不想要的內容。您可以使用 Watson Knowledge Studio 來建立<a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">自訂模型</a>，並且用來在 Natural Language Understanding 中偵測自訂實體和關係。

{{site.data.keyword.nlushort}} 套件包含下列實體。如需相關資訊，請透過按一下實體名稱來查看 {{site.data.keyword.nlushort}} API 參考資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、| 使用 {{site.data.keyword.nlushort}} 服務。|
| [`analyze`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`features`、`text`、`html`、`url`、`clean`、`xpath`、`fallback_to_raw`、`return_analyzed_text`、`language`、`limit_text_characters`、|分析文字、HTML 或公用網頁。|
| [`delete-model`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`model_id`、|刪除模型。|
| [`list-models`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、| 列出模型。|

## 建立 {{site.data.keyword.nlushort}} 服務實例
{: #service_instance_understanding}

在安裝套件之前，您必須先建立 {{site.data.keyword.nlushort}} 服務實例和服務認證。
{: shortdesc}

1. [建立 {{site.data.keyword.nlushort}} 服務實例](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.nlushort}} 套件
{: #install_understanding}

具有 {{site.data.keyword.nlushort}} 服務實例後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.nlushort}} 套件安裝到名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #nlus_cli}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.nlushort}} 套件，請執行下列動作：

1. 複製 {{site.data.keyword.nlushort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.nlushort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    **輸出範例**
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.nlushort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #nlus_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用名稱空間下拉功能表，選取您要在其中安裝套件的名稱空間。名稱空間是由合併的組織及空間名稱組成。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Natural Language Understanding** 套件。

5. 按一下**安裝**。

6. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 **natural-language-understanding-v1** 的新套件。

7. 若要使用 **natural-language-understanding-v1** 套件中的動作，您必須將服務認證連結至動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 [CLI 指示](#nlus_cli)中的步驟 4 和 5。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。 

  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下您要使用的 **natural-language-understanding-v1** 套件中的「動作」。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.nlushort}} 套件
{: #usage_understanding}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning){: external}中的其他詳細資料。

此套件的功能使用 Natural Language Understanding 現行版本：2018-03-16。請嘗試 `list-models` 動作。
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}



