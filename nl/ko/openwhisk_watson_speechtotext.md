---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: Speech to Text 패키지
{: #openwhisk_catalog_watson_texttospeech}

`/whisk.system/watson-speechToText` 패키지는 음성을 텍스트로 변환하는 Watson API를 호출하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` |패키지 |username, password |음성을 텍스트로 변환하기 위한 패키지 |
|`/whisk.system/watson-speechToText/speechToText` |액션 |payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out |오디오를 텍스트로 변환 |

**참고**: `/whisk.system/watson/speechToText` 액션을 포함하여 `/whisk.system/watson` 패키지는 더 이상 사용되지 않습니다.

## {{site.data.keyword.Bluemix_notm}}에서 Watson Speech to Text 텍스트 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우 패키지 바인딩이 {{site.data.keyword.Bluemix_notm}} Watson 서비스 인스턴스에 대해 자동으로 작성됩니다.

1. {{site.data.keyword.Bluemix_notm}} [대시보드](http://console.ng.Bluemix.net)에서 Watson Speech to Text 서비스 인스턴스를 작성하십시오.

  자신이 속한 {{site.data.keyword.Bluemix_notm}} 조직과 영역 및 서비스 인스턴스의 이름을 반드시 기억하십시오.

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 Watson 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  출력 예:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  패키지를 나열하여 바인딩이 작성되었는지 확인하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Speech to Text 텍스트 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Speech to Text를 설정하려는 경우 Watson Speech to Text 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Watson Speech to Text 서비스 사용자 이름 및 비밀번호가 반드시 필요합니다.

Watson Speech to Text 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## 음성-문자 변환

`/whisk.system/watson-speechToText/speechToText` 액션은 오디오 음성을 텍스트로 변환합니다. 매개변수는 다음과 같습니다.

- `username`: Watson API 사용자 이름입니다.
- `password`: Watson API 비밀번호입니다.
- `payload`: 텍스트로 변환할 인코딩된 음성 2진 데이터입니다.
- `content_type`: 오디오의 MIME 유형입니다.
- `encoding`: 음성 2진 데이터의 인코딩입니다.
- `continuous`: 오랜 중단으로 구분된 연속적인 구문을 나타내는 다수의 최종 결과가 리턴되는지 여부를 표시합니다.
- `inactivity_timeout`: 제출된 오디오에서 묵음만 감지되는 경우 해당 시간이 지나면 연결이 닫히는 시간(초)입니다.
- `interim_results`: 서비스가 중간 결과를 리턴하는지 여부를 표시합니다.
- `keywords`: 오디오에서 찾을 키워드의 목록입니다.
- `keywords_threshold`:키워드를 찾기 위한 하한인 신뢰 값입니다.
- `max_alternatives`: 리턴되는 대체 문서의 최대 수입니다.
- `model`: 인식 요청에 사용되는 모델의 ID입니다.
- `timestamps`: 각 단어마다 시간 맞추기가 리턴되는지 여부를 표시합니다.
- `watson-token`: 서비스 신임 정보 제공의 대안으로서 서비스에 대한 인증 토큰을 제공합니다.
- `word_alternatives_threshold`: 가능한 단어 대체로서 가설을 식별하기 위한 하한인 신뢰 값입니다.
- `word_confidence`: 0 - 1 범위의 신뢰 측정치가 각 단어마다 리턴되는지 여부를 표시합니다.
- `X-Watson-Learning-Opt-Out`: 호출에 대한 데이터 콜렉션을 제외하는지 여부를 표시합니다.

패키지 바인딩에서 **speechToText** 액션을 호출하여 인코딩된 오디오를 변환하십시오.
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

출력 예:
```
{
  "data": "Hello Watson"
  }
```
{: screen}
