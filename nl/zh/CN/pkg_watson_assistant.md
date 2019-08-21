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

可安装的 {{site.data.keyword.conversationfull}} 服务将机器学习、自然语言理解和集成对话工具组合在一起，用于创建应用程序与用户之间的会话流。
{: shortdesc}

{{site.data.keyword.conversationshort}} 包中包含以下实体。有关更多信息，请通过单击实体名称来查看 {{site.data.keyword.conversationshort}} API 参考。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|[`assistant-v1`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html){: external}|包|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`|使用 {{site.data.keyword.conversationshort}} 服务。|
|[`message`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`input`、`alternate_intents`、`context`、`entities`、`intents`、`output`、`nodes_visited_details`|获取对用户输入的响应。|
|[`create-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-workspace){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`description`、`language`、`intents`、`entities`、`dialog_nodes`、`counterexamples`、`metadata`、`learning_opt_out`|创建工作空间。|
|[`delete-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-workspace){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`|删除工作空间。|
|[`get-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-workspace){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`export`、`include_audit`|获取有关工作空间的信息。|
|[`list-workspaces`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-workspaces){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出工作空间。|
|[`update-workspace`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-workspace){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`name`、`description`、`language`、`intents`、`entities`、dialog_nodes、counterexamples、`metadata`、`learning_opt_out`、`append`|更新工作空间。|
|[`create-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-intent){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`description`、`examples`|创建意向。|
|[`delete-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-intent){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`|删除意向。|
|[`get-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-intent){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`export`、`include_audit`|获取有关意向的信息。|
|[`list-intents`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-intents){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`export`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出意向。|
|[`update-intent`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-intent){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`new_intent`、`new_description`、`new_examples`|更新意向。|
|[`create-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-example){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text`|创建用户输入示例。|
|[`delete-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-example){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text`|删除用户输入示例。|
|[`get-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-example){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text`、`include_audit`|获取有关用户输入示例的信息。|
|[`list-examples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-examples){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出用户输入示例。|
|[`update-example`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-example){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`intent`、`text`、`new_text`|更新用户输入示例。|
|[`create-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-counterexample){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`|创建反例。|
|[`delete-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-counterexample){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`|删除反例。|
|[`get-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-counterexample){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`、`include_audit`|获取有关反例的信息。|
|[`list-counterexamples`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-counterexamples){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出反例。|
|[`update-counterexample`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-counterexample){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`text`、`new_text`|更新反例。|
|[`create-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-entity){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`description`、`metadata`、`values`、`fuzzy_match`|创建实体。|
|[`delete-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-entity){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`|删除实体。|
|[`get-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-entity){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`export`、`include_audit`|获取有关实体的信息。|
|[`list-entities`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-entities){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`export`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出实体。|
|[`update-entity`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-entity){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`new_entity`、`new_description`、`new_metadata`、`new_fuzzy_match`、`new_values`|更新实体。|
|[`create-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-value){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`metadata`、`synonyms`、`patterns`、`value_type`|添加实体值。|
|[`delete-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-value){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`|删除实体值。|
|[`get-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-value){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`export`、`include_audit`|获取实体值。|
|[`list-values`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-values){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`export`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出实体值。|
|[`update-value`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-value){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`new_value`、`new_metadata`、`new_type`、`new_synonyms`、`new_patterns`|更新实体值。|
|[`create-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-synonym){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`|添加实体值同义词。|
|[`delete-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-synonym){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`|删除实体值同义词。|
|[`get-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-synonym){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`、`include_audit`|获取实体值同义词。|
|[`list-synonyms`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-synonyms){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出实体值同义词。|
|[`update-synonym`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-synonym){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`entity`、`value`、`synonym`、`new_synonym`|更新实体值同义词。|
|[`create-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-dialog-node){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`、`description`、`conditions`、`parent`、`previous_sibling`、`output`、`context`、`metadata`、`next_step`、`actions`、`title`、`node_type`、`event_name`、`variable`、`digress_in`、`digress_out`、`digress_out_slots`|创建对话节点。|
|[`delete-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-dialog-node){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、dialog_node|删除对话节点。|
|[`get-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-dialog-node){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`、`include_audit`|获取对话节点。|
|[`list-dialog-nodes`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-dialog-nodes){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`page_limit`、`include_count`、`sort`、`cursor`、`include_audit`|列出对话节点。|
|[`update-dialog-node`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-dialog-node){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`dialog_node`、`new_dialog_node`、`new_description`、`new_conditions`、`new_parent`、`new_previous_sibling`、`new_output`、`new_context`、`new_metadata`、`new_next_step`、`new_title`、`new_type`、`new_event_name`、`new_variable`、`new_actions`、`new_digress_in`、`new_digress_out`、`new_digress_out_slots`|更新对话节点。|
|[`list-all-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-all-logs){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`filter`、`sort`、`page_limit`、`cursor`|列出所有工作空间中的日志事件。|
|[`list-logs`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-logs){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`workspace_id`、`sort`、`filter`、`page_limit`、`cursor`|列出某个工作空间中的日志事件。|
|[`delete-user-data`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id`|删除标注的数据。|

## 创建 {{site.data.keyword.conversationshort}} 服务实例
{: #service_instance_conversation}

安装包之前，必须创建 {{site.data.keyword.conversationshort}} 服务实例和服务凭证。

1. [创建 {{site.data.keyword.conversationshort}} 服务实例](https://cloud.ibm.com/catalog/services/conversation){: external}。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.conversationshort}} 包
{: #install_conversation}

具有 {{site.data.keyword.conversationshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.conversationshort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #conversation_cli}

**开始之前**
[安装 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.conversationshort}} 包，请运行以下命令。

1. 克隆 {{site.data.keyword.conversationshort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
    ```
    {: pre}

3. 验证包是否已添加到包列表中。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **输出**
    ```
    packages
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.conversationshort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    **示例输出**
    ```
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.conversationshort}} 服务实例凭证。
    ```
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    **示例输出**
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

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #conversation_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用“名称空间”下拉菜单，选择要将包安装到其中的名称空间。

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Watson Assistant** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **assistant-v1** 的新包。

7. 要使用 **assistant-v1** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循 [CLI 指示信息](#conversation_cli)中的步骤 4 和 5。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。 
  
  对于要使用的每个操作，必须完成以下步骤。
  {: note}

    1. 单击 **assistant-v1** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.conversationshort}} 包
{: #usage_conversation}

要使用此包中的操作，请运行以下格式的命令。

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

所有操作都需要格式为 YYYY-MM-DD 的 version 参数。以向后不兼容方式更改 API 后，会发布一个新版本日期。有关更多信息，请参阅 [API 参考](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning){: external}。

此包的函数使用当前版本的 Watson Assistant (`2018-07-10`)。请试用 `list-workspaces` 操作。
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}


