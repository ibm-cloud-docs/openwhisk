---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: package, cognitive, functions

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


# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## 包

|名称|可用性|描述|
| --- | --- | --- | --- |
|[`speech-to-text-v1`](#speech_to_text)|可安装|使用 {{site.data.keyword.speechtotextshort}} V1 服务。|
|[`/whisk.system/watson-speechToText`](#preinstall_speechtotext)|预安装（在东京不可用）|调用 Watson API 以将语音转换为文本|

## {{site.data.keyword.speechtotextshort}} 服务
{: #speech_to_text}

可安装的 {{site.data.keyword.speechtotextfull}} 服务提供了一个 [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external}，该 API 使用 IBM 的语音识别功能来生成语音音频的抄本。
{:shortdesc}

此服务可以转录各种语言和音频格式的语音。除了基本转录外，该服务还可以生成有关音频的许多方面的详细信息。对于大多数语言，此服务支持两个采样率：宽带和窄带。此服务会以 UTF-8 字符集返回所有 JSON 响应内容。有关此服务的更多信息，请参阅 [IBM&reg; Cloud 文档](/docs/services/speech-to-text?topic=speech-to-text-about)。

{{site.data.keyword.speechtotextshort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.speechtotextshort}} API 参考中找到更多信息。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|[`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external}|包|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`|使用 {{site.data.keyword.speechtotextshort}} V1 服务。|
|[`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`model_id`|获取模型。|
|[`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`|列出模型。|
|[`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`audio`、`content_type`、`model`、`customization_id`、`acoustic_customization_id`、`base_model_version`、`customization_weight`、`inactivity_timeout`、`keywords`、`keywords_threshold`、`max_alternatives`、`word_alternatives_threshold`、`word_confidence`、`timestamps`、`profanity_filter`、`smart_formatting`、`speaker_labels`|识别音频（无会话）。|
|[`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`id`|检查一个作业。|
|[`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`|检查多个作业。|
|[`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`audio`、`content_type`、`model`、`callback_url`、`events`、`user_token`、`results_ttl`、`customization_id`、`acoustic_customization_id`、`base_model_version`、`customization_weight`、`inactivity_timeout`、`keywords`、`keywords_threshold`、`max_alternatives`、`word_alternatives_threshold`、`word_confidence`、`timestamps`、`profanity_filter`、`smart_formatting`、`speaker_labels`|创建作业。|
|[`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`id`|删除作业。|
|[`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`callback_url`、`user_secret`|注册回调。|
|[`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`callback_url`|注销回调。|
|[`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`base_model_name`、`dialect`、`description`|创建定制语言模型。|
|[`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|删除定制语言模型。|
|[`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|获取定制语言模型。|
|[`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`language`|列出定制语言模型。|
|[`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|重置定制语言模型。|
|[`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_type_to_add`、`customization_weight`|训练定制语言模型。|
|[`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|升级定制语言模型。|
|[`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`corpus_name`、`corpus_file`、`allow_overwrite`|添加语料库。|
|[`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`corpus_name`|删除语料库。|
|[`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`corpus_name`|获取语料库。|
|[`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|列出语料库。|
|[`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_name`、`word`、`sounds_like`、`display_as`|添加一个定制字。|
|[`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`words`|添加多个定制字。|
|[`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_name`|删除定制字。|
|[`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_name`|获取定制字。|
|[`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`word_type`、`sort`|列出定制字。|
|[`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`name`、`base_model_name`、` description`|创建定制声学模型。|
|[`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|删除定制声学模型。|
|[`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|获取定制声学模型。|
|[`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`language`|列出定制声学模型。|
|[`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|重置定制声学模型。|
|[`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`custom_language_model_id`|训练定制声学模型。|
|[`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`custom_language_model_id`|升级定制声学模型。|
|[`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`audio_name`、`audio_resource`、`content_type`、`contained_content_type`、`allow_overwrite`|添加音频资源。|
|[`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`audio_name`|删除音频资源。|
|[`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`、`audio_name`|获取音频资源。|
|[`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customization_id`|列出音频资源。|
|[`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data){: external}|操作|`username`、`password`、`iam_access_token`、`iam_apikey`、`iam_url`、`headers`、`headers[X-Watson-Learning-Opt-Out]`、`url`、`customer_id`|删除标注的数据。|

### 创建 {{site.data.keyword.speechtotextshort}} 服务实例
{: #service_instance_speechtotext}

安装包之前，必须创建 {{site.data.keyword.speechtotextshort}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.speechtotextshort}} 服务实例](https://cloud.ibm.com/catalog/services/speech_to_text){: external}。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

### 安装 {{site.data.keyword.speechtotextshort}} 包
{: #install_speechtotext}

具有 {{site.data.keyword.speechtotextshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.speechtotextshort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #speechtotext_cli}

**开始之前**

[安装 {{site.data.keyword.cloud_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.speechtotextshort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.speechtotextshort}} 包存储库。
    

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.speechtotextshort}} 实例中的凭证绑定到包。
    

    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    根据在其中创建服务实例的区域，可能会以不同方式命名服务实例，因为它是 IAM 服务。如果命令失败，请将以下服务名称用于 bind 命令：
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    **示例输出**

    ```
Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.speechtotextshort}} 服务实例凭证。
    

    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    **示例输出**

    ```
ok: got package speech-to-text-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #speechtotext_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面](https://cloud.ibm.com/openwhisk/create){: external}。

2. 使用“名称空间”下拉菜单，选择要将包安装到其中的名称空间。

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Speech To Text** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 `speech-to-text-v1` 的新包。

7. 要使用 **speech-to-text-v1** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循 [CLI 指示信息](#speechtotext_cli)中的步骤 4 和 5。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。 
  
  对于要使用的每个操作，必须完成以下步骤。
  {: note}

    1. 单击 **speech-to-text-v1** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

### 使用 {{site.data.keyword.speechtotextshort}} 包
{: #usage_speechtotext}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

请试用 `list-models` 操作。
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson：Speech to Text
{: #preinstall_speechtotext}

此预安装包在东京区域中不可用。请参阅可安装的 [Speech to Text](#install_speechtotext)。
{: tip}

通过 `/whisk.system/watson-speechToText` 包，可以方便地调用要将语音转换为文本的 Watson API。
{: shortdesc}

此包中包含以下操作。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/watson-speechToText`|包|`username`、`password` |用于将语音转换为文本的包。|
|`/whisk.system/watson-speechToText/speechToText`|操作|`payload`、`content_type`、`encoding`、`username`、`password`、`continuous`、`inactivity_timeout`、`interim_results`、`keywords`、`keywords_threshold`、`max_alternatives`、`model`、`timestamps`、`watson-token`、`word_alternatives_threshold`、`word_confidence`、`X-Watson-Learning-Opt-Out`|将音频转换为文本。|

不推荐使用包含 `/whisk.system/watson/speechToText` 操作的 `/whisk.system/watson` 包。请改为查看 [{{site.data.keyword.speechtotextshort}} 包](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud)。
{: deprecated}

### 在 {{site.data.keyword.cloud_notm}} 中设置 Watson Speech to Text 包

如果是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk}}，那么将为 {{site.data.keyword.cloud_notm}} Watson 服务实例自动创建包绑定。

1. 在 {{site.data.keyword.cloud_notm}} [仪表板](https://cloud.ibm.com){: external}中创建 Watson Speech to Text 服务实例。请务必记住服务实例的名称以及您所在的 {{site.data.keyword.cloud_notm}} 组织和空间的名称。

2. 刷新名称空间中的包。刷新操作将自动为已创建的 Watson 服务实例创建包绑定。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **示例输出**

  ```
  created bindings:
  Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. 列出包以查看是否已创建绑定。

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **示例输出**

  ```
  packages
  /myOrg_mySpace/Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### 在 {{site.data.keyword.cloud_notm}} 外部设置 Watson Speech to Text 包

如果不是在 {{site.data.keyword.cloud_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.cloud_notm}} 外部设置 Watson Speech to Text，那么必须为 Watson Speech to Text 服务手动创建包绑定。您需要 Watson Speech to Text 服务用户名和密码。

创建为您的 Watson Speech to Text 服务配置的包绑定。
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### 将语音转换为文本

`/whisk.system/watson-speechToText/speechToText` 操作可将音频语音转换为文本。支持以下参数。

|参数|描述|
| --- | --- |
|`username`|Watson API 用户名。|
|`password`|Watson API 密码。|
|`payload`|要转换为文本的已编码语音二进制数据。|
|`content_type`|音频的 MIME 类型。|
|`encoding`|语音二进制数据的编码。|
|`continuous`|指示是否返回代表长时间停顿所分隔的连续短语的多个最终结果。|
|`inactivity_timeout`|如果在所提交的音频中只检测到沉默，那么在该时间（秒）之后，会关闭连接。|
|`interim_results`|指示服务是否返回中间结果。|
|`keywords`|要在音频中抓住的关键字的列表。|
|`keywords_threshold`|抓住关键字的下限置信度值。|
|`max_alternatives`|要返回的替代抄本的最大数目。|
|`model`|要用于辨识请求的模型的标识。|
|`timestamps`|指示是否针对每一个字返回时间校准。|
|`watson-token`|为服务提供认证令牌，作为提供服务凭证的替代方法。|
|`word_alternatives_threshold`|将假设识别为可能的替代文字的下限置信度值。|
|`word_confidence`|指示是否针对每一个字返回 0 到 1 范围内的置信度度量。|
|`X-Watson-Learning-Opt-Out`|指示是否针对调用选择退出数据收集。|

调用包绑定中的 `speechToText` 操作来转换已编码的音频。

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**示例输出**
```
{
      "data": "Hello Watson"
  }
  ```
{: screen}


