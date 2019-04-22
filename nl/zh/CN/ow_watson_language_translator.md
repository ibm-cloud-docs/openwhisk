---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: language translator, functions, actions, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.languagetranslatorshort}} 包

{{site.data.keyword.languagetranslatorfull}} 将文本从一种语言翻译为另一种语言。此服务有多个 IBM 提供的翻译模型，您可以根据自己的独特术语和语言进行定制。
{: shortdesc}

{{site.data.keyword.languagetranslatorshort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.languagetranslatorshort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|[`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html)|包|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|使用 {{site.data.keyword.languagetranslatorshort}} 服务。|
|[translate](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、model_id、source、target|翻译文本。|
|[identify](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text|识别文本的语言。|
|[list-identifiable-languages](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|列出可识别的语言。|
|[create-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、base_model_id、name、forced_glossary、parallel_corpus|创建模型。|
|[delete-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、model_id|删除模型。|
|[get-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、model_id|获取模型详细信息。|
|[list-models](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、source、target、default_models|列出模型。|

## 创建 {{site.data.keyword.languagetranslatorshort}} 服务实例
{: #service_instance_translator}

安装包之前，必须创建 {{site.data.keyword.languagetranslatorshort}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.languagetranslatorshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/language_translator)。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.languagetranslatorshort}} 包
{: #install_translator}

具有 {{site.data.keyword.languagetranslatorshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.languagetranslatorshort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #languagetranslator_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli)。

要安装 {{site.data.keyword.languagetranslatorshort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.languagetranslatorshort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.languagetranslatorshort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    示例输出：
    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.languagetranslatorshort}} 服务实例凭证。
    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    示例输出：
    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #languagetranslator_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将包安装到其中的名称空间。名称空间由组合的组织和空间名称构成。

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Language Translator** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **language-translator-v3** 的新包。

7. 要使用 **language-translator-v3** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **language-translator-v3** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.languagetranslatorshort}} 包
{: #usage_translator}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有操作都需要格式为 YYYY-MM-DD 的版本参数。以向后不兼容方式更改 API 后，会发布一个新版本日期。请在 [API 参考](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning)中查看更多详细信息。

此包的函数使用当前版本的 Language Translator (2018-05-01)。请试用 `identify` 操作。
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}
