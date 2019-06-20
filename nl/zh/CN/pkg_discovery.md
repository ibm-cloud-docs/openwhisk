---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: watson discovery, functions, watson, cognitive

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

# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

可安装的 {{site.data.keyword.discoveryfull}} 服务是一种认知搜索和内容分析引擎，可以将其添加到应用程序以识别模式、趋势和切实可行的洞察，从而推动更好的决策。将结构化数据和非结构化数据与预先扩充的内容进行安全整合，并使用简化查询语言，从而不必手动过滤结果。
{: shortdesc}

{{site.data.keyword.discoveryshort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.discoveryshort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|[`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html)|包|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|使用 {{site.data.keyword.discoveryshort}} 服务。|
|[create-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、description、size|创建环境。|
|[delete-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id|删除环境。|
|[get-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id|获取有关环境的信息。|
|[list-environments](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name|列出环境。|
|[list-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_ids|列出集合中的字段。|
|[update-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、name、description|更新环境。|
|[create-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、name、description、conversions、enrichments、normalizations|添加配置。|
|[delete-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、configuration_id|删除配置。|
|[get-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、configuration_id|获取配置详细信息。|
|[list-configurations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、name|列出配置。|
|[update-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、configuration_id、name、description、conversions、enrichments、normalizations|更新配置。|
|[test-configuration-in-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、configuration、step、configuration_id、file、metadata、file_content_type|测试配置。|
|[create-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、name、description、configuration_id、language|创建集合。|
|[delete-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|删除集合。|
|[get-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|获取集合详细信息。|
|[list-collection-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|列出集合字段。|
|[list-collections](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、name|列出集合。|
|[update-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、name、description、configuration_id|更新集合。|
|[create-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、expansions|创建或更新扩展列表。|
|[delete-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|删除扩展列表。|
|[list-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|获取扩展列表。|
|[add-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、file、metadata、file_content_type|添加文档。|
|[delete-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、document_id|删除文档。|
|[get-document-status](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、document_id|获取文档详细信息。|
|[update-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、document_id、file、metadata、file_content_type|更新文档。|
|[federated-query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_ids、filter、query、natural_language_query、aggregation、count、return_fields、offset、sort、highlight、deduplicate、deduplicate_field、similar、similar_document_ids、similar_fields|查询多个集合中的文档。|
|[federated-query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_ids、filter、query、natural_language_query、aggregation、count、return_fields、offset、sort、highlight、deduplicate_field、similar、similar_document_ids、similar_fields|查询多个集合系统通知。|
|[query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、filter、query、natural_language_query、passages、aggregation、count、return_fields、offset、sort、highlight、passages_fields、passages_count、passages_characters、deduplicate、deduplicate_field、similar、similar_document_ids、similar_fields|查询集合。|
|[query-entities](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、feature、entity、context、count、evidence_count|查询知识图实体。|
|[query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、filter、query、natural_language_query、passages、aggregation、count、return_fields、offset、sort、highlight、passages_fields、passages_count、passages_characters、deduplicate_field、similar、similar_document_ids、similar_fields|查询系统通知。|
|[query-relations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、entities、context、sort、filter、count、evidence_count|查询知识图关系。|
|[add-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、natural_language_query、filter、examples|向训练数据添加查询。|
|[create-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id、document_id、cross_reference、relevance|向训练数据查询添加示例。|
|[delete-all-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|删除所有训练数据。|
|[delete-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id|删除训练数据查询。|
|[delete-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id、example_id|删除训练数据查询的示例。|
|[get-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id|获取有关查询的详细信息。|
|[get-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id、example_id|获取训练数据示例的详细信息。|
|[list-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id|列出训练数据。|
|[list-training-examples](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id|列出训练数据查询的示例。|
|[update-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、environment_id、collection_id、query_id、example_id、cross_reference、relevance|更改示例的标签或交叉引用。|
|[delete-user-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id|删除标注的数据。|

## 创建 {{site.data.keyword.discoveryshort}} 服务实例
{: #service_instance_discovery}

安装包之前，必须创建 {{site.data.keyword.discoveryshort}} 服务实例和服务凭证。

1. [创建 {{site.data.keyword.discoveryshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/discovery)。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.discoveryshort}} 包
{: #install_discovery}

具有 {{site.data.keyword.discoveryshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.discoveryshort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #discovery_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.discoveryshort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.discoveryshort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
    ```
    {: pre}

3. 验证包是否已添加到包列表中。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    输出：
    ```
    packages
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.discoveryshort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    示例输出：
      ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.discoveryshort}} 服务实例凭证。
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    示例输出：
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

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #discovery_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将包安装到其中的名称空间。 

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Discovery** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **discovery-v1** 的新包。

7. 要使用 **discovery-v1** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请完成上面列出的 CLI 指示信息中的步骤 4。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **discovery-v1** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.discoveryshort}} 包
{: #usage_discovery}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

所有操作都需要格式为 YYYY-MM-DD 的版本参数。以向后不兼容方式更改 API 后，会发布一个新版本日期。请在 [API 参考](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning)中查看更多详细信息。

此包的函数使用当前版本的 Discovery (2018-03-05)。请试用 `list-environments` 操作。
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}

