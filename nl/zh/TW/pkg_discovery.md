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


# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

可安裝的 {{site.data.keyword.discoveryfull}} 服務是一種認知搜尋和內容分析引擎，可以將其新增至應用程式以識別型樣、趨勢和切實可行的洞察，以推動更好的決策。將結構化及非結構化資料與預先強化的內容安全地統一，並使用簡化的查詢語言來省掉手動過濾結果這一程序。
{: shortdesc}

{{site.data.keyword.discoveryshort}} 套件包含下列實體。您可以透過按一下實體名稱在 {{site.data.keyword.discoveryshort}} API 參考資料中找到更多詳細資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 使用 {{site.data.keyword.discoveryshort}} 服務。|
| [`create-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`description`、`size` | 建立環境。|
| [`delete-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id` | 刪除環境。|
| [`get-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id` | 取得環境的相關資訊。|
| [`list-environments`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name` | 列出環境。|
| [`list-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_ids` | 列出集合間的欄位。|
| [`update-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name`、`description` | 更新環境。|
| [`create-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name`、`description`、`conversions`、`enrichments`、`normalizations`| 新增配置。|
| [`delete-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration_id` | 刪除配置。|
| [`get-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration_id` | 取得配置詳細資料。|
| [`list-configurations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name` | 列出配置。|
| [`update-configuration`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration_id`、`name`、`description`、`conversions`、`enrichments`、`normalizations`| 更新配置。|
| [`test-configuration-in-environment`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`configuration`、`step`、`configuration_id`、`file`、`metadata`、`file_content_type`| 測試配置。|
| [`create-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name`、`description`、`configuration_id`、`language`| 建立集合。|
| [`delete-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 刪除集合。|
| [`get-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 取得集合詳細資料。|
| [`list-collection-fields`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 列出集合欄位。|
| [`list-collections`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`name` | 列出集合。|
| [`update-collection`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`name`、`description`、`configuration_id`| 更新集合。|
| [`create-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`expansions`| 建立或更新擴充清單。|
| [`delete-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 刪除擴充清單。|
| [`list-expansions`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 取得擴充清單。|
| [`add-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`file`、`metadata`、`file_content_type`| 新增文件。|
| [`delete-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`document_id` | 刪除文件。|
| [`get-document-status`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`document_id` | 取得文件詳細資料。|
| [`update-document`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`document_id`、`file`、`metadata`、`file_content_type`| 更新文件。|
| [`federated-query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_ids`、`filter`、`query`、`natural_language_query`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`deduplicate`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields`| 查詢多個集合中的文件。|
| [`federated-query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_ids`、`filter`、`query`、`natural_language_query`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields`| 查詢多個集合系統注意事項。|
| [`query`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`filter`、`query`、`natural_language_query`、`passages`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`passages_fields`、`passages_count`、`passages_characters`、`deduplicate`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields`| 查詢集合。|
| [`query-entities`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`feature`、`entity`、`context`、`count`、`evidence_count`| 查詢 Knowledge Graph 實體。|
| [`query-notices`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`filter`、`query`、`natural_language_query`、`passages`、`aggregation`、`count`、`return_fields`、`offset`、`sort`、`highlight`、`passages_fields`、`passages_count`、`passages_characters`、`deduplicate_field`、`similar`、`similar_document_ids`、`similar_fields`| 查詢系統注意事項。|
| [`query-relations`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`entities`、`context`、`sort`、`filter`、`count`、`evidence_count`| 查詢 Knowledge Graph 關係。|
| [`add-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey、iam_url`、`headers、headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`natural_language_query`、`filter`、`examples`| 將查詢新增至訓練資料。|
| [`create-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`document_id`、`cross_reference`、`relevance`| 將範例新增至訓練資料查詢。|
| [`delete-all-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 刪除所有訓練資料。|
| [`delete-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id` | 刪除訓練資料查詢。|
| [`delete-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`example_id` | 刪除訓練資料查詢的範例。|
| [`get-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id` | 取得有關查詢的詳細資料。|
| [`get-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`example_id` | 取得訓練資料範例的詳細資料。|
| [`list-training-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id` | 列出訓練資料。|
| [`list-training-examples`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id` | 列出訓練資料查詢的範例。|
| [`update-training-example`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`environment_id`、`collection_id`、`query_id`、`example_id`、`cross_reference`、`relevance`| 變更範例的標籤或交互參照。|
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id` | 刪除標籤資料。|

## 建立 {{site.data.keyword.discoveryshort}} 服務實例
{: #service_instance_discovery}

在安裝套件之前，您必須先建立 {{site.data.keyword.discoveryshort}} 服務實例和服務認證。

1. [建立 {{site.data.keyword.discoveryshort}} 服務實例](https://cloud.ibm.com/catalog/services/discovery){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.discoveryshort}} 套件
{: #install_discovery}

具有 {{site.data.keyword.discoveryshort}} 服務實例後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.discoveryshort}} 套件安裝到名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #discovery_cli}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.discoveryshort}} 套件，請執行下列指令：

1. 複製 {{site.data.keyword.discoveryshort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
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
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.discoveryshort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    **輸出範例**
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.discoveryshort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "discovery": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 透過 {{site.data.keyword.openwhisk_short}} 使用者介面安裝
{: #discovery_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用名稱空間下拉功能表，選取您要在其中安裝套件的名稱空間。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Discovery** 套件。

6. 按一下**安裝**。

7. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 **discovery-v1** 的新套件。

8. 若要使用 `discovery-v1` 套件中的動作，必須將服務認證連結到這些動作。
  * 若要將服務認證連結到套件中的所有動作，請完成 CLI 指示中的步驟 4。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。

  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下 `discovery-v1` 套件中要使用的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.discoveryshort}} 套件
{: #usage_discovery}

若要使用此套件中的動作，請以下列格式執行指令：

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning){: external}中的其他詳細資料。

此套件的函數使用現行版本的 Discovery (`2018-03-05`)。請嘗試 `list-environments` 動作。
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}



