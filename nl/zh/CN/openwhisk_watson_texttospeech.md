---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson：Text to Speech 包
{: #openwhisk_catalog_watson_texttospeech}

通过 `/whisk.system/watson-textToSpeech` 包，可以方便地调用要将文本转换为语音的 Watson API。
{: shortdesc}

此包中包含以下操作。

|实体|类型|参数|描述
|
| --- | --- | --- | --- |
|`/whisk.system/watson-textToSpeech`|包|username 和 password|用于将文本转换为语音的包|
|`/whisk.system/watson-textToSpeech/textToSpeech`|操作|payload、voice、accept、encoding、username、password|将文本转换为音频|

**注**：不推荐使用包含 `/whisk.system/watson/textToSpeech` 操作的 `/whisk.system/watson` 包。

## 在 {{site.data.keyword.Bluemix_notm}} 中设置 Watson Text to Speech 包

如果是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk}}，那么将为 {{site.data.keyword.Bluemix_notm}} Watson 服务实例自动创建包绑定。

1. 在 {{site.data.keyword.Bluemix_notm}} [仪表板](http://console.bluemix.net)中创建 Watson Text to Speech 服务实例。

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

## 在 {{site.data.keywrod.Bluemix_notm}} 外部设置 Watson Text to Speech 包

如果不是在 {{site.data.keyword.Bluemix_notm}} 中使用 {{site.data.keyword.openwhisk_short}}，或者如果要在 {{site.data.keyword.Bluemix_notm}} 外部设置 Watson Text to Speech，那么必须为 Watson Text to Speech 服务手动创建包绑定。您需要 Watson Text to Speech 服务用户名和密码。

创建为您的 Watson Speech to Text 服务配置的包绑定。
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 将某些文本转换为语音

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
