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

可安装的 {{site.data.keyword.personalityinsightsfull}} 服务支持应用程序从社交媒体、企业数据或其他数字通信中派生洞察。此服务使用语言分析从数字通信（例如，电子邮件、文本消息、推文和论坛帖子）中推断个人的内在个性特征，包括“五大性格”、“需求”和“价值观”。
{: shortdesc}

此服务可以从潜在嘈杂的社交媒体中，自动推断出反映个性特征的个人情况。此服务可以基于其分析结果推断消费偏好，而对于带有时间戳记的 JSON 内容，可以报告与时间相关的行为。
* 有关此服务用于描述个性特征的模型的含义的更多信息，请参阅[个性模型](/docs/services/personality-insights?topic=personality-insights-models)。
* 有关消费偏好含义的更多信息，请参阅[消费偏好](/docs/services/personality-insights?topic=personality-insights-preferences)。

{{site.data.keyword.personalityinsightsshort}} 服务禁用了请求日志记录。无论是否设置 `X-Watson-Learning-Opt-Out` 请求头，此服务都不会记录或保留来自请求与响应的数据。
{: note}

{{site.data.keyword.personalityinsightsshort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.personalityinsightsshort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|[`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html){: external}|包|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`|使用 {{site.data.keyword.personalityinsightsshort}} V3 服务。|
|[`profile`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`content`、`content_type`、`content_language`、`accept_language`、`raw_scores`、`csv_headers`、`consumption_preferences`|获取个人档案。|
|[`profile-as-csv`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`content`、`content_type`、`content_language`、`accept_language`、`raw_scores`、`csv_headers`、`consumption_preferences`|获取 CSV 文件格式的个人档案。|

## 创建 {{site.data.keyword.personalityinsightsshort}} 服务实例
{: #service_instance_insights}

安装包之前，必须创建 {{site.data.keyword.personalityinsightsshort}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.personalityinsightsshort}} 服务实例](https://cloud.ibm.com/catalog/services/personality_insights){: external}。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

## 安装 {{site.data.keyword.personalityinsightsshort}} 包
{: #install_insights}

具有 {{site.data.keyword.personalityinsightsshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.personalityinsightsshort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #personalityinsights_cli}

**开始之前**

[安装 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.personalityinsightsshort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.personalityinsightsshort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.personalityinsightsshort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    根据在其中创建服务实例的区域，可能会以不同方式命名服务实例，因为它是 IAM 服务。如果命令失败，请将以下服务名称用于 bind 命令：
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    **示例输出**
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.personalityinsightsshort}} 服务实例凭证。
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    **示例输出**
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

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #personalityinsights_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用“名称空间”下拉菜单，选择要将包安装到其中的名称空间。 

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Personality Insights** 包。

6. 单击**安装**。

7. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 `personality-insights-v3` 的新包。

8. 要使用 `personality-insights-v3` 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循 CLI 指示信息中的步骤 5 和 6。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。

  对于要使用的每个操作，必须完成以下步骤。
  {: note}

    1. 单击 `personality-insights-v3` 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新参数。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.personalityinsightsshort}} 包
{: #usage_insights}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

所有操作都需要格式为 YYYY-MM-DD 的 version 参数。以向后不兼容方式更改 API 后，会发布一个新版本日期。请在 [API 参考](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning){: external}中查看更多详细信息。

此包的函数使用当前版本的 {{site.data.keyword.personalityinsightsshort}} (2017-10-13)。请试用 `profile` 操作。
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "You can write an excerpt about yourself here, but it will need to be at least 100 words long. This excerpt is just some filler text and probably won't return anything very interesting from the personality insights service. The service uses linguistic analytics to infer individuals' intrinsic personality characteristics, including Big Five, Needs, and Values, from digital communications such as email, text messages, tweets, and forum posts. The service can automatically infer, from potentially noisy social media, portraits of individuals that reflect their personality characteristics. The service can infer consumption preferences based on the results of its analysis and for JSON content that is timestamped, can report temporal behavior."
```
{: pre}



