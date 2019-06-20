---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: nlc, natural language classifier, machine learning

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

# {{site.data.keyword.nlclassifiershort}}
{: #pkg_natlang_classifier}

{{site.data.keyword.nlclassifierfull}} 使用机器学习算法来返回简短文本输入的最匹配预定义类。您可以创建并训练一个分类器，用于将预定义的类连接到示例文本，以便该服务可以将这些类应用于新输入。
{: shortdesc}

{{site.data.keyword.nlclassifiershort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.nlclassifiershort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|[`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html)|包|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|使用 {{site.data.keyword.nlclassifiershort}} 服务。|
|[classify](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id、text|对一个短语进行分类。|
|[classify-collection](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id、collection|对多个短语进行分类。|
|[create-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、metadata、training_data|创建分类器。|
|[delete-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id|删除分类器。|
|[get-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、classifier_id|获取有关分类器的信息。|
|[list-classifiers](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|列出分类器。|

## 创建 {{site.data.keyword.nlclassifiershort}} 服务实例
{: #service_instance_classifier}

安装包之前，必须创建 {{site.data.keyword.nlclassifiershort}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.nlclassifiershort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/natural_language_classifier)。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.nlclassifiershort}} 包
{: #install_classifier}

具有 {{site.data.keyword.nlclassifiershort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.nlclassifiershort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #nlclassifier_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.nlclassifiershort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.nlclassifiershort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.nlclassifiershort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    根据在其中创建服务实例的区域，可能会以不同方式命名服务实例，因为它是 IAM 服务。如果以上命令失败，请将以下服务名称用于 bind 命令：
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    示例输出：
      ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.nlclassifiershort}} 服务实例凭证。
    ```
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    示例输出：
      ```
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #nlclassifier_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将包安装到其中的名称空间。 

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Natural Language Classifier** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **natural-language-classifier-v1** 的新包。

7. 要使用 **natural-language-classifier-v1** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **natural-language-classifier-v1** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.nlclassifiershort}} 包
{: #usage_classifier}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

请试用 `list-classifiers` 操作。
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}

