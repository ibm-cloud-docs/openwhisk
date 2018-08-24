---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: Text to Speech 패키지
{: #openwhisk_catalog_watson_texttospeech}

`/whisk.system/watson-textToSpeech` 패키지는 Watson API를 호출하여 텍스트를 음성으로 변환하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` |패키지 |username, password |텍스트를 음성으로 변환하기 위한 패키지 |
|`/whisk.system/watson-textToSpeech/textToSpeech` |액션 |payload, voice, accept, encoding, username, password |텍스트를 오디오로 변환 |

**참고**: `/whisk.system/watson` 패키지는 `/whisk.system/watson/textToSpeech` 액션을 포함하여 더 이상 사용되지 않습니다.

## {{site.data.keyword.Bluemix_notm}}에서 Watson Text to Speech 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우 패키지 바인딩이 {{site.data.keyword.Bluemix_notm}} Watson 서비스 인스턴스에 대해 자동으로 작성됩니다.

1. {{site.data.keyword.Bluemix_notm}} [대시보드](http://console.bluemix.net)에서 Watson Text to Speech 서비스 인스턴스를 작성하십시오.

  자신이 속한 {{site.data.keyword.Bluemix_notm}} 조직과 영역 및 서비스 인스턴스의 이름을 반드시 기억하십시오.

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 Watson 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  출력 예:
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  패키지를 나열하여 패키지 바인딩이 작성되었는지 확인하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Text to Speech 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Text to Speech를 설정하려는 경우에는 Watson Text to Speech 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Watson Text to Speech 서비스 사용자 이름 및 비밀번호가 반드시 필요합니다.

Watson Speech to Text 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 일부 텍스트를 음성으로 변환

`/whisk.system/watson-textToSpeech/textToSpeech` 액션은 일부 텍스트를 오디오 음성으로 변환합니다. 매개변수는 다음과 같습니다.

- `username`: Watson API 사용자 이름입니다.
- `password`: Watson API 비밀번호입니다.
- `payload`: 음성으로 변환할 텍스트입니다.
- `voice`: 발표자의 음성입니다.
- `accept`: 음성 파일의 형식입니다.
- `encoding`: 음성 2진 데이터의 인코딩입니다.

패키지 바인딩에서 **textToSpeech** 액션을 호출하여 텍스트를 변환하십시오.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

출력 예:
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}
