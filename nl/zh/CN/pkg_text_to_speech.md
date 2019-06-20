---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: text to speech, watson, cognitive, functions, packages

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



# Text to Speech
{: #pkg_text_to_speech}

## 包选项

|包|可用性|描述|
| --- | --- | --- | --- |
|[`/whisk.system/watson-textToSpeech`](#text_to_speech)|预安装（在东京不可用）|用于将文本转换为语音的包|
|[`text-to-speech-v1`](#text_to_speech_ins)|可安装|使用 {{site.data.keyword.texttospeechshort}} 服务。|

## Watson Text to Speech
{: #text_to_speech}

此预安装包在东京区域中不可用。请使用 IAM 认证来查看可安装的 [Text to Speech](#text_to_speech_ins) 包。
{: tip}

通过 `/whisk.system/watson-textToSpeech` 包，可以方便地调用要将文本转换为语音的 Watson API。
{: shortdesc}

此包中包含以下操作。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|`/whisk.system/watson-textToSpeech`|包|username 和 password|用于将文本转换为语音的包|
|`/whisk.system/watson-textToSpeech/textToSpeech`|操作|payload、voice、accept、encoding、username、password|将文本转换为音频|

**注**：不推荐使用包含 `/whisk.system/watson/textToSpeech` 操作的 `/whisk.system/watson` 包。请改为查看[可安装的 {{site.data.keyword.texttospeechshort}} 包](#text_to_speech_ins)。

### 在 {{site.data.keyword.Bluemix_notm}} 中设置 Watson Text to Speech 包

如果是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk}}，那么将为 {{site.data.keyword.Bluemix_notm}} Watson 服务实例自动创建包绑定。

1. 在 {{site.data.keyword.Bluemix_notm}} [仪表板](http://cloud.ibm.com)中创建 Watson Text to Speech 服务实例。

  请务必记住服务实例的名称以及您所在的 {{site.data.keyword.Bluemix_notm}} 组织和空间的名称。

2. 刷新名称空间中的包。刷新操作将自动为已创建的 Watson 服务实例创建包绑定。
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  示例输出：
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  列出包以查看是否已创建包绑定：
  ```
  ibmcloud fn package list
  ```
  {: pre}

  示例输出：
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### 在 {{site.data.keyword.Bluemix_notm}} 外部设置 Watson Text to Speech 包

如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 Watson Text to Speech，那么必须为 Watson Text to Speech 服务手动创建包绑定。您需要 Watson Text to Speech 服务用户名和密码。

创建为您的 Watson Speech to Text 服务配置的包绑定。
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### 将文本转换为语音

`/whisk.system/watson-textToSpeech/textToSpeech` 操作可将某些文本转换为音频语音。参数如下所示：

- `username`：Watson API 用户名。
- `password`：Watson API 密码。
- `payload`：要转换为语音的文本。
- `voice`：讲话者的声音。
- `accept`：语音文件的格式。
- `encoding`：语音二进制数据的编码。

调用包绑定中的 **textToSpeech** 操作来转换文本。
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
  ```
{: pre}

示例输出：
```
{
      "payload": "<base64 encoding of a .wav file>"
  }
  ```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

可安装的 {{site.data.keyword.texttospeechfull}} 服务提供了一个 API，该 API 使用 IBM 的语音合成功能，将文本合成为各种语言、方言和声音形式的比较自然的语音。
{:shortdesc}

对于每种语言，此服务至少支持一种男声或女声（有时两者都支持）。音频将以最短延迟流式传送回客户机。有关此服务的更多信息，请参阅 [IBM Cloud 文档](/docs/services/text-to-speech?topic=text-to-speech-about)。

{{site.data.keyword.texttospeechshort}} 包中包含以下实体。您可以通过单击实体名称在 {{site.data.keyword.texttospeechshort}} API 参考中找到其他详细信息。

|实体|类型|参数|描述|
| --- | --- | --- | --- |
|[`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html)|包|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|使用 {{site.data.keyword.texttospeechshort}} 服务。|
|[get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、voice、customization_id|获取话语。|
|[list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url|列出话语。|
|[synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、accept、voice、customization_id|合成音频。|
|[get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、text、voice、format、customization_id|获取发音。|
|[create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、language、description|创建定制模型。|
|[delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id|删除定制模型。|
|[get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id|获取定制模型。|
|[list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、language|列出定制模型。|
|[update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、name、description、words|更新定制模型。|
|[add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word、translation、part_of_speech|添加一个定制字。|
|[add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、words|添加多个定制字。|
|[delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word|删除定制字。|
|[get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id、word|获取定制字。|
|[list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customization_id|列出定制字。|
|[delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data)|操作|username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id|删除标注的数据。|

### 创建 {{site.data.keyword.texttospeechshort}} 服务实例
{: #service_instance_texttospeech}

安装包之前，必须创建 {{site.data.keyword.texttospeechshort}} 服务实例和服务凭证。
{: shortdesc}

1. [创建 {{site.data.keyword.texttospeechshort}} 服务实例 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/text_to_speech)。
2. 创建服务实例时，还会为您创建自动生成的服务凭证。

### 安装 {{site.data.keyword.texttospeechshort}} 包
{: #install_texttospeech}

具有 {{site.data.keyword.texttospeechshort}} 服务实例后，请使用 {{site.data.keyword.openwhisk}} CLI 将 {{site.data.keyword.texttospeechshort}} 包安装到名称空间中。
{: shortdesc}

### 通过 {{site.data.keyword.openwhisk_short}} CLI 进行安装
{: #texttospeech_cli}

开始之前：
  1. [安装 {{site.data.keyword.Bluemix_notm}} CLI 的 {{site.data.keyword.openwhisk_short}} 插件](/docs/openwhisk?topic=cloud-functions-cli_install)。

要安装 {{site.data.keyword.texttospeechshort}} 包，请执行以下操作：

1. 克隆 {{site.data.keyword.texttospeechshort}} 包存储库。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 部署包。
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. 将所创建的 {{site.data.keyword.texttospeechshort}} 实例中的凭证绑定到包。
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    根据在其中创建服务实例的区域，可能会以不同方式命名服务实例，因为它是 IAM 服务。如果以上命令失败，请将以下服务名称用于 bind 命令：
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
示例输出：
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. 验证该包是否已配置为使用 {{site.data.keyword.texttospeechshort}} 服务实例凭证。
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    示例输出：
      ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
                "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### 通过 {{site.data.keyword.openwhisk_short}} UI 进行安装
{: #texttospeech_ui}

1. 在 {{site.data.keyword.openwhisk_short}} 控制台中，转至[“创建”页面 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/openwhisk/create)。

2. 使用 **Cloud Foundry 组织**和 **Cloud Foundry 空间**列表，选择要将包安装到其中的名称空间。 

3. 单击**安装包**。

4. 单击 **Watson** 包组。

5. 单击 **Text To Speech** 包。

5. 单击**安装**。

6. 安装包后，会将您重定向到“操作”页面，您可以在其中搜索名为 **text-to-speech-v1** 的新包。

7. 要使用 **text-to-speech-v1** 包中的操作，必须将服务凭证绑定到这些操作。
  * 要将服务凭证绑定到包中的所有操作，请遵循上面列出的 CLI 指示信息中的步骤 5 和 6。
  * 要将服务凭证绑定到单个操作，请在 UI 中完成以下步骤。**注**：对于要使用的每个操作，必须完成以下步骤。
    1. 单击 **text-to-speech-v1** 包中要使用的操作。这将打开该操作的详细信息页面。
    2. 在左侧导航中，单击**参数**部分。
    3. 输入新的**参数**。对于键，输入 `__bx_creds`。对于值，请从先前创建的服务实例中粘贴服务凭证 JSON 对象。

## 使用 {{site.data.keyword.texttospeechshort}} 包
{: #usage_texttospeech}

要使用此包中的操作，请运行以下格式的命令：

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

请试用 `list-voices` 操作。
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}

