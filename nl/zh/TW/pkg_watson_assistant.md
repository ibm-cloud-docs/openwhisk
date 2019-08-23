---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: openwhisk, functions

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


# {{site.data.keyword.conversationshort}}
{: #pkg_watson_assistant}

可安裝的 {{site.data.keyword.conversationfull}} 服務將機器學習、自然語言理解和整合對話工具結合在一起，用於建立應用程式與使用者之間的會話流程。
{: shortdesc}

{{site.data.keyword.conversationshort}} 套件包含下列實體。如需相關資訊，請透過按一下實體名稱來查看 {{site.data.keyword.conversationshort}} API 參考資料。

|實體|類型|參數|說明|
| --- | --- | --- | --- |
| [`assistant-v1`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html){: external} |套件| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`  | 使用 {{site.data.keyword.conversationshort}} 服務。|
| [`message`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`input`、`alternate_intents`、`context`、`entities`、`intents`、`output`、`nodes_visited_details`| 取得對使用者輸入的回應。|
| [`create-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-workspace){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`description`、`language`、`intents`、`entities`、`dialog_nodes`、`counterexamples`、`metadata`、`learning_opt_out`| 建立工作區。|
| [`delete-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-workspace){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`| 刪除工作區。|
| [`get-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-workspace){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`export`、`include_audit`| 取得工作區的相關資訊。|
| [`list-workspaces`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-workspaces){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`| 列出工作區。|
| [`update-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-workspace){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`name`、`description`、`language`、`intents`、`entities`、dialog_nodes、counterexamples、`metadata`、`learning_opt_out`、`append`| 更新工作區。|
| [`create-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-intent){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`description`、`examples`| 建立意圖。|
| [`delete-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-intent){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`| 刪除意圖。|
| [`get-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-intent){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`export`、`include_audit`| 取得意圖的相關資訊。|
| [`list-intents`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-intents){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`export`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`  | 列出意圖。|
| [`update-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-intent){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`new_intent`、`new_description`、`new_examples`  | 更新意圖。|
| [`create-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-example){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text` | 建立使用者輸入範例。|
| [`delete-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-example){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text` | 刪除使用者輸入範例。|
| [`get-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-example){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text`、`include_audit`  | 取得使用者輸入範例的相關資訊。|
| [`list-examples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-examples){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`  | 列出使用者輸入範例。|
| [`update-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-example){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text`、`new_text`  | 更新使用者輸入範例。|
| [`create-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-counterexample){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`  | 建立反例。|
| [`delete-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-counterexample){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`  | 刪除反例。|
| [`get-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-counterexample){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`、`include_audit` | 取得反例的相關資訊。|
| [`list-counterexamples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-counterexamples){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`  | 列出反例。|
| [`update-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-counterexample){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`、`new_text` | 更新反例。|
| [`create-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-entity){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`description`、`metadata`、`values`、`fuzzy_match`  | 建立實體。|
| [`delete-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-entity){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`  | 刪除實體。|
| [`get-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-entity){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`export`、`include_audit`  | 取得實體的相關資訊。|
| [`list-entities`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-entities){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`export`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`  | 列出實體。|
| [`update-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-entity){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`new_entity`、`new_description`、`new_metadata`、`new_fuzzy_match`、`new_values` | 更新實體。|
| [`create-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-value){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`metadata`、`synonyms`、`patterns`、`value_type` | 新增實體值。|
| [`delete-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-value){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value` | 刪除實體值。|
| [`get-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-value){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`export`、`include_audit`  | 取得實體值。|
| [`list-values`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-values){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`export`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit` | 列出實體值。|
| [`update-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-value){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`new_value`、`new_metadata`、`new_type`、`new_synonyms`、`new_patterns` | 更新實體值。|
| [`create-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-synonym){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`  | 新增實體值同義字。|
| [`delete-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-synonym){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`  | 刪除實體值同義字。|
| [`get-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-synonym){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`、`include_audit`  | 取得實體值同義字。|
| [`list-synonyms`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-synonyms){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit` | 列出實體值同義字。|
| [`update-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-synonym){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`、`new_synonym`  | 更新實體值同義字。|
| [`create-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-dialog-node){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`、`description`、`conditions`、`parent`、`previous_sibling`、`output`、`context`、`metadata`、`next_step`、`actions`、`title`、`node_type`、`event_name`、`variable`、`digress_in`、`digress_out`、`digress_out_slots` | 建立對話節點。|
| [`delete-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-dialog-node){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`  | 刪除對話節點。|
| [`get-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-dialog-node){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`、`include_audit` | 取得對話節點。|
| [`list-dialog-nodes`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-dialog-nodes){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`  | 列出對話節點。|
| [`update-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-dialog-node){: external} |動作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`、`new_dialog_node`、`new_description`、`new_conditions`、`new_parent`、`new_previous_sibling`、`new_output`、`new_context`、`new_metadata`、`new_next_step`、`new_title`、`new_type`、`new_event_name`、`new_variable`、`new_actions`、`new_digress_in`、`new_digress_out`、`new_digress_out_slots`| 更新對話節點。|
| [`list-all-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-all-logs){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`filter`、`sort`、`page_limit`、`cursor`  | 列出所有工作區中的日誌事件。|
| [`list-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-logs){: external} |動作|  `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`sort`、`filter`、`page_limit`、`cursor`  | 列出工作區中的日誌事件。|
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data){: external} |動作| `username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id` | 刪除標籤資料。|

## 建立 {{site.data.keyword.conversationshort}} 服務實例
{: #service_instance_conversation}

在安裝套件之前，您必須先建立 {{site.data.keyword.conversationshort}} 服務實例和服務認證。

1. [建立 {{site.data.keyword.conversationshort}} 服務實例](https://cloud.ibm.com/catalog/services/conversation){: external}。
2. 建立服務實例時，也會為您建立自動產生的服務認證。

## 安裝 {{site.data.keyword.conversationshort}} 套件
{: #install_conversation}

具有 {{site.data.keyword.conversationshort}} 服務實例後，請使用 {{site.data.keyword.openwhisk}} CLI 將 {{site.data.keyword.conversationshort}} 套件安裝到名稱空間中。
{: shortdesc}

### 透過 {{site.data.keyword.openwhisk_short}} CLI 安裝
{: #conversation_cli}

**開始之前**

[安裝 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 外掛程式](/docs/openwhisk?topic=cloud-functions-cli_install)。

若要安裝 {{site.data.keyword.conversationshort}} 套件，請執行下列指令。

1. 複製 {{site.data.keyword.conversationshort}} 套件儲存庫。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署套件。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
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
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. 將認證從您建立的 {{site.data.keyword.conversationshort}} 實例連結到該套件。
    ```
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    **輸出範例**
    ```
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. 驗證已使用 {{site.data.keyword.conversationshort}} 服務實例認證配置該套件。
    ```
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    **輸出範例**
    ```
    ok: got package assistant-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "conversation": {
            "credentials": "Credentials-1",
            "instance": "Watson Assistant (formerly Conversation)-8h",
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
{: #conversation_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 主控台中，移至[建立頁面 ](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用名稱空間下拉功能表，選取您要在其中安裝套件的名稱空間。

3. 按一下**安裝套件**。

4. 按一下 **Watson** 套件群組。

5. 按一下 **Watson Assistant** 套件。

5. 按一下**安裝**。

6. 安裝套件後，會將您重新導向到「動作」頁面，您可以在其中搜尋名稱為 **assistant-v1** 的新套件。

7. 若要使用 **assistant-v1** 套件中的「動作」，您必須將服務認證連結至動作。
  * 若要將服務認證連結到套件中的所有動作，請遵循 [CLI 指示](#conversation_cli)中的步驟 4 和 5。
  * 若要將服務認證連結至個別動作，請在使用者介面中完成下列步驟。 
  
  您必須針對要使用的每一個動作，完成下列步驟。
  {: note}

    1. 按一下您要使用的 **assistant-v1** 套件中的動作。即會開啟該動作的詳細資料頁面。
    2. 在左側導覽中，按一下**參數**區段。
    3. 輸入新的**參數**。對於索引鍵，輸入 `__bx_creds`。針對該值，請貼上先前建立之服務實例中的服務認證 JSON 物件。

## 使用 {{site.data.keyword.conversationshort}} 套件
{: #usage_conversation}

若要使用此套件中的動作，請以下列格式執行指令。

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

所有動作需要格式為 YYYY-MM-DD 的版本參數。當以舊版不相容的方式變更 API 時，會釋出新的版本日期。如需相關資訊，請參閱 [API 參考資料](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning){: external}。

此套件的函數使用現行版本的 Watson Assistant (`2018-07-10`)。請嘗試 `list-workspaces` 動作。
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}


