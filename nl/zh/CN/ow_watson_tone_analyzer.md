---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.toneanalyzershort}} 包

{{site.data.keyword.toneanalyzerfull}} 服务使用语言分析来检测书面文本中的情绪和语气。
{:shortdesc}

此服务可以分析文档级别和语句级别的语气。您可以使用此服务来了解对方是如何理解自己的书面沟通的，然后相应地改进沟通语气。企业可以使用此服务来了解与其客户沟通的语气，并对每位客户进行相应的响应，或者理解并改进客户对话。

**注：**Tone Analyzer 服务禁用了请求日志记录。无论是否设置 `X-Watson-Learning-Opt-Out` 请求头，此服务都不会记录和保留来自请求与响应的数据。

{{site.data.keyword.toneanalyzershort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.toneanalyzershort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|[`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html)|包|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|使用 {{site.data.keyword.toneanalyzershort}} 服务。|
|[tone](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、tone_input、content_type、sentences、tones、content_language、accept_language|分析常规语气。|
|[tone-chat](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、utterances、content_language、accept_language|分析客户参与语气.|

## 创建 {{site.data.keyword.toneanalyzershort}} 服务实例
{: #service_instance}

安装包之前，必须创建 {{site.data.keyword.toneanalyzershort}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.toneanalyzershort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/catalog/services/tone_analyzer)。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.toneanalyzershort}} 包
{: #install}

具有 {{site.data.keyword.toneanalyzershort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.toneanalyzershort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #toneanalyzer_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](bluemix_cli.html#cloudfunctions_cli)。
  2. 安装 [`wskdeploy` 命令 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)，并将下载的二进制文件添加到 PATH 中。

要安装 {{site.data.keyword.toneanalyzershort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.toneanalyzershort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    wskdeploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.toneanalyzershort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    根据在其中创建服务实例的区域，可能会以不同方式命名服务实例，因为它是 IAM 服务。如果以上命令失败，请将以下服务名称用于 bind 命令：
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}
示例输出：
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.toneanalyzershort}} 服务实例凭证。
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    示例输出：
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
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
{: #toneanalyzer_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将 {{site.data.keyword.cos_short}} 包安装到其中的名称空间。名称空间由组合的组织和空间名称构成。

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Tone Analyzer** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **tone-analyzer-v3** 的新包。

7. 要使用 **tone-analyzer-v3** 包中的操作，必须将服务凭证绑定到操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。 
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **tone-analyzer-v3** 包中要使用的操作。这将打开该操作的详细信息页面。 
    2. 在左侧导航中，单击**参数**部分。 
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.toneanalyzershort}} 包
{: #usage}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有操作都需要格式为 YYYY-MM-DD 的版本参数。以向后不兼容方式更改 API 后，会发布一个新版本日期。请在 [API 参考](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning)中查看更多详细信息。

此包的函数使用当前版本的 Tone Analyzer (2017-09-21)。请试用 `tone` 操作。
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}
