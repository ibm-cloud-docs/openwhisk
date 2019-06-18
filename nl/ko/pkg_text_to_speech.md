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



# 문자-음성 변환
{: #pkg_text_to_speech}

## 패키지 옵션

|패키지 |가용성 |설명 |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | 사전 설치됨(도쿄에서 사용할 수 없음) |텍스트를 음성으로 변환하기 위한 패키지 |
| [`text-to-speech-v1`](#text_to_speech_ins) | 설치 가능 |{{site.data.keyword.texttospeechshort}} 서비스 관련 작업을 수행합니다. |

## Watson 문자-음성 변환
{: #text_to_speech}

이 사전 설치된 패키지는 도쿄 지역에서는 사용할 수 없습니다. IAM 인증을 사용하여 설치 가능한 [Text to Speech](#text_to_speech_ins) 패키지를 참조하십시오.
{: tip}

`/whisk.system/watson-textToSpeech` 패키지는 Watson API를 호출하여 텍스트를 음성으로 변환하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` |패키지 |username, password |텍스트를 음성으로 변환하기 위한 패키지 |
|`/whisk.system/watson-textToSpeech/textToSpeech` |액션 |payload, voice, accept, encoding, username, password |텍스트를 오디오로 변환 |

**참고**: `/whisk.system/watson` 패키지는 `/whisk.system/watson/textToSpeech` 액션을 포함하여 더 이상 사용되지 않습니다. 대신 [설치 가능 {{site.data.keyword.texttospeechshort}} 패키지](#text_to_speech_ins)를 참조하십시오.

### {{site.data.keyword.Bluemix_notm}}에서 Watson Text to Speech 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우 패키지 바인딩이 {{site.data.keyword.Bluemix_notm}} Watson 서비스 인스턴스에 대해 자동으로 작성됩니다.

1. {{site.data.keyword.Bluemix_notm}} [대시보드](http://cloud.ibm.com)에서 Watson Text to Speech 서비스 인스턴스를 작성하십시오.

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

### {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Text to Speech 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Text to Speech를 설정하려는 경우에는 Watson Text to Speech 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Watson Text to Speech 서비스 사용자 이름 및 비밀번호가 반드시 필요합니다.

Watson Speech to Text 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### 문자-음성 변환

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


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

설치 가능한 {{site.data.keyword.texttospeechfull}} 서비스는 IBM의 음성 분석 기능을 사용하여 텍스트를 다양한 언어, 사투리와 목소리로 이루어진 자연적 사운드의 음성으로 합성하는 API를 제공합니다.
{:shortdesc}

이 서비스는 각 언어마다 최소한 하나의 남성 또는 여성 목소리를 지원하거나, 종종 둘 모두를 지원합니다. 오디오는 최소한의 지연으로 고객에게 다시 스트리밍됩니다. 이 서비스에 대한 자세한 정보는 [IBM Cloud 문서](/docs/services/text-to-speech?topic=text-to-speech-about)를 참조하십시오.

{{site.data.keyword.texttospeechshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.texttospeechshort}} API 참조서의 추가 세부사항을 찾을 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) |패키지 | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |{{site.data.keyword.texttospeechshort}} 서비스 관련 작업을 수행합니다. |
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    voice,     customization_id,  |음성을 가져옵니다. |
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, |음성을 나열합니다. |
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text,     accept,     voice,     customization_id,  |오디오를 동기화합니다. |
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,     voice,     format,     customization_id,  |발음을 가져옵니다. |
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, language, description,  |사용자 정의 모델을 작성합니다. |
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 모델을 삭제합니다. |
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 모델을 가져옵니다. |
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  |사용자 정의 모델을 나열합니다. |
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    name, description, words,  |사용자 정의 모델을 업데이트합니다. |
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,    translation, part_of_speech,  | 사용자 정의 단어를 추가합니다. |
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  |사용자 정의 단어를 추가합니다. |
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | 사용자 정의 단어를 삭제합니다. |
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | 사용자 정의 단어를 가져옵니다. |
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 단어를 나열합니다. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  |레이블 지정된 데이터를 삭제합니다. |

### {{site.data.keyword.texttospeechshort}} 서비스 인스턴스 작성
{: #service_instance_texttospeech}

패키지를 설치하기 전에 {{site.data.keyword.texttospeechshort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.texttospeechshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](https://cloud.ibm.com/catalog/services/text_to_speech).
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

### {{site.data.keyword.texttospeechshort}} 패키지 설치
{: #install_texttospeech}

{{site.data.keyword.texttospeechshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.texttospeechshort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #texttospeech_cli}

시작하기 전에 다음을 수행하십시오.
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.texttospeechshort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.texttospeechshort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
    ```
    {: pre}

3. 패키지가 패키지 목록에 추가되었는지 확인하십시오.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    출력:
    ```
    packages
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.texttospeechshort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    IAM 서비스이므로, 서비스 인스턴스가 작성된 영역에 따라 서비스 인스턴스에는 서로 다른 이름이 지정될 수 있습니다. 위의 명령이 실패한 경우에는 바인딩 명령에 대해 다음 서비스 이름을 사용하십시오.
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
출력 예:
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.texttospeechshort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    출력 예:
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

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #texttospeech_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/create)로 이동하십시오.

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록을 사용하여 패키지를 설치할 네임스페이스를 선택하십시오. 

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오.

5. **Text To Speech** 패키지를 클릭하십시오.

5. **설치**를 클릭하십시오.

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **text-to-speech-v1**인 새 패키지를 검색할 수 있습니다.

7. **text-to-speech-v1** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다.
    1. 사용할 **text-to-speech-v1** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

## {{site.data.keyword.texttospeechshort}} 패키지 사용
{: #usage_texttospeech}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

`list-voices` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}

