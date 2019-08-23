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

## 패키지

|이름 |가용성 |설명 |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | 설치 가능 |{{site.data.keyword.speechtotextshort}} V1 서비스 관련 작업을 수행합니다. |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | 사전 설치됨(도쿄에서 사용할 수 없음) |음성을 텍스트로 변환하기 위해 Watson API를 호출합니다. |

## {{site.data.keyword.speechtotextshort}} 서비스
{: #speech_to_text}

설치 가능한 {{site.data.keyword.speechtotextfull}} 서비스는 IBM의 음성 인식 기능을 사용하여 발화된 오디오의 필사본을 생성하는 [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external}를 제공합니다.
{:shortdesc}

이 서비스는 다양한 언어와 오디오 형식으로부터 음성을 필사할 수 있습니다. 기본 필사 외에도 이 서비스는 오디오의 다양한 측면에 대한 상세 정보를 생성할 수 있습니다. 대부분의 언어에서, 이 서비스는 2개의 샘플링 속도인 광대역 및 협대역을 지원합니다. 이는 UTF-8 문자 세트의 모든 JSON 응답 컨텐츠를 리턴한다. 이 서비스에 대한 자세한 정보는 [IBM&reg; Cloud 문서](/docs/services/speech-to-text?topic=speech-to-text-about)를 참조하십시오.

{{site.data.keyword.speechtotextshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.speechtotextshort}} API 참조서에서 자세한 정보를 찾을 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} |패키지 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` |{{site.data.keyword.speechtotextshort}} V1 서비스 관련 작업을 수행합니다. |
| [`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` |모델을 가져옵니다. |
| [`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` |모델을 나열합니다. |
| [`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` |오디오를 인식합니다(세션리스). |
| [`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id`  |작업을 검사합니다. |
| [`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` |작업을 검사합니다. |
| [`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `callback_url`, `events`, `user_token`, `results_ttl`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` |작업을 작성합니다. |
| [`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id` |작업을 삭제합니다. |
| [`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url`, `user_secret` |콜백을 등록합니다. |
| [`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url` |콜백을 등록 취소합니다. |
| [`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `dialect`, `description` |사용자 정의 언어 모델을 작성합니다. |
| [`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 언어 모델을 삭제합니다. |
| [`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 언어 모델을 가져옵니다. |
| [`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` |사용자 정의 언어 모델을 나열합니다. |
| [`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 언어 모델을 재설정합니다. |
| [`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type_to_add`, `customization_weight` |사용자 정의 언어 모델을 훈련합니다. |
| [`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 언어 모델을 업그레이드합니다. |
| [`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name`, `corpus_file`, `allow_overwrite` |말뭉치를 추가합니다. |
| [`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` |말뭉치를 삭제합니다. |
| [`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` |말뭉치를 가져옵니다. |
| [`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |말뭉치를 나열합니다. |
| [`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name`, `word`, `sounds_like`, `display_as` | 사용자 정의 단어를 추가합니다. |
| [`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words` |사용자 정의 단어를 추가합니다. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | 사용자 정의 단어를 삭제합니다. |
| [`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | 사용자 정의 단어를 가져옵니다. |
| [`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type`, `sort` |사용자 정의 단어를 나열합니다. |
| [`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`,` description` |사용자 정의 음향 모델을 작성합니다. |
| [`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 음향 모델을 삭제합니다. |
| [`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 음향 모델을 가져옵니다. |
| [`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` |사용자 정의 음향 모델을 나열합니다. |
| [`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |사용자 정의 음향 모델을 재설정합니다. |
| [`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` |사용자 정의 음향 모델을 훈련합니다. |
| [`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` |사용자 정의 음향 모델을 업그레이드합니다. |
| [`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name`, `audio_resource`, `content_type`, `contained_content_type`, `allow_overwrite` |오디오 리소스를 추가합니다. |
| [`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` |오디오 리소스를 삭제합니다. |
| [`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` |오디오 리소스를 가져옵니다. |
| [`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` |오디오 리소스를 나열합니다. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` |레이블 지정된 데이터를 삭제합니다. |

### {{site.data.keyword.speechtotextshort}} 서비스 인스턴스 작성
{: #service_instance_speechtotext}

패키지를 설치하기 전에 {{site.data.keyword.speechtotextshort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.speechtotextshort}} 서비스 인스턴스를 작성하십시오](https://cloud.ibm.com/catalog/services/speech_to_text){: external}.
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

### {{site.data.keyword.speechtotextshort}} 패키지 설치
{: #install_speechtotext}

{{site.data.keyword.speechtotextshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.speechtotextshort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #speechtotext_cli}

**시작하기 전에**

[{{site.data.keyword.cloud_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.speechtotextshort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.speechtotextshort}} 패키지 저장소를 복제하십시오.

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
    ```
    {: pre}

3. 패키지가 패키지 목록에 추가되었는지 확인하십시오.

    ```
    ibmcloud fn package list
    ```
    {: pre}

    **출력**

    ```
packages
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.speechtotextshort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.

    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    IAM 서비스이므로, 서비스 인스턴스가 작성된 영역에 따라 서비스 인스턴스에는 서로 다른 이름이 지정될 수 있습니다. 명령이 실패한 경우에는 바인딩 명령에 대해 다음 서비스 이름을 사용하십시오.
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    **출력 예**

    ```
Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.speechtotextshort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.

    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    **출력 예**

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

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #speechtotext_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지](https://cloud.ibm.com/openwhisk/create){: external}로 이동하십시오.

2. 드롭 다운 메뉴에서 네임스페이스를 사용하여 패키지를 설치할 네임스페이스를 선택하십시오.

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오.

5. **Speech To Text** 패키지를 클릭하십시오.

5. **설치**를 클릭하십시오.

6. 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 `speech-to-text-v1`인 새 패키지를 검색할 수 있습니다.

7. **speech-to-text-v1** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 [CLI 지시사항](#speechtotext_cli)의 4 - 5단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. 
  
  사용할 각 액션마다 다음 단계를 완료해야 합니다.
  {: note}

    1. 사용할 **speech-to-text-v1** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

### {{site.data.keyword.speechtotextshort}} 패키지 사용
{: #usage_speechtotext}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

`list-models` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson: Speech to Text
{: #preinstall_speechtotext}

이 사전 설치된 패키지는 도쿄 지역에서는 사용할 수 없습니다. 설치 가능한 [Speech to Text](#install_speechtotext)를 참조하십시오.
{: tip}

`/whisk.system/watson-speechToText` 패키지는 음성을 텍스트로 변환하는 Watson API를 호출하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` |패키지 |`username`, `password` |음성을 텍스트로 변환하기 위한 패키지입니다. |
|`/whisk.system/watson-speechToText/speechToText` |액션 |`payload`, `content_type`, `encoding`, `username`, `password`, `continuous`, `inactivity_timeout`, `interim_results`, `keywords`, `keywords_threshold`, `max_alternatives`, `model`, `timestamps`, `watson-token`, `word_alternatives_threshold`, `word_confidence`, `X-Watson-Learning-Opt-Out` |오디오를 텍스트로 변환합니다. |

`/whisk.system/watson/speechToText` 액션을 포함하여 `/whisk.system/watson` 패키지는 더 이상 사용되지 않습니다. 대신 [{{site.data.keyword.speechtotextshort}} 패키지](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud)를 참조하십시오.
{: deprecated}

### {{site.data.keyword.cloud_notm}}에서 Watson Speech to Text 텍스트 설정

{{site.data.keyword.cloud_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우 패키지 바인딩이 {{site.data.keyword.cloud_notm}} Watson 서비스 인스턴스에 대해 자동으로 작성됩니다.

1. {{site.data.keyword.cloud_notm}} [대시보드](https://cloud.ibm.com){: external}에서 Watson Speech to Text 서비스 인스턴스를 작성하십시오. 자신이 속한 {{site.data.keyword.cloud_notm}} 조직과 영역 및 서비스 인스턴스의 이름을 반드시 기억하십시오.

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 Watson 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **출력 예**

  ```
  created bindings:
  Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. 패키지를 나열하여 바인딩이 작성되었는지 확인하십시오.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **출력 예**

  ```
  packages
  /myOrg_mySpace/Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### {{site.data.keyword.cloud_notm}} 외부에서 Watson Speech to Text 텍스트 패키지 설정

{{site.data.keyword.cloud_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.cloud_notm}} 외부에서 Watson Speech to Text를 설정하려는 경우 Watson Speech to Text 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Watson Speech to Text 서비스 사용자 이름 및 비밀번호가 반드시 필요합니다.

Watson Speech to Text 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### 음성-문자 변환

`/whisk.system/watson-speechToText/speechToText` 액션은 오디오 음성을 텍스트로 변환합니다. 다음 매개변수가 지원됩니다.

|매개변수 |설명 |
| --- | --- |
|`username` |Watson API 사용자 이름입니다. |
|`password` |Watson API 비밀번호입니다. |
|`payload` |텍스트로 변환할 인코딩된 음성 2진 데이터입니다. |
|`content_type` |오디오의 MIME 유형입니다. |
|`encoding` |음성 2진 데이터의 인코딩입니다. |
|`continuous` |오랜 중단으로 구분된 연속적인 구문을 나타내는 다수의 최종 결과가 리턴되는지 여부를 표시합니다. |
|`inactivity_timeout` |제출된 오디오에서 묵음만 감지되는 경우 해당 시간이 지나면 연결이 닫히는 시간(초)입니다. |
|`interim_results` |서비스가 중간 결과를 리턴하는지 여부를 표시합니다. |
|`keywords` |오디오에서 찾을 키워드의 목록입니다. |
|`keywords_threshold` |키워드를 찾기 위한 하한인 신뢰 값입니다. |
|`max_alternatives` |리턴되는 대체 문서의 최대 수입니다. |
|`model` |인식 요청에 사용되는 모델의 ID입니다. |
|`timestamps` |각 단어마다 시간 맞추기가 리턴되는지 여부를 표시합니다. |
|`watson-token` |서비스 인증 정보 제공의 대안으로서 서비스에 대한 인증 토큰을 제공합니다. |
|`word_alternatives_threshold` |가능한 단어 대체로서 가설을 식별하기 위한 하한인 신뢰 값입니다. |
|`word_confidence` |0 - 1 범위의 신뢰 측정치가 각 단어마다 리턴되는지 여부를 표시합니다. |
|`X-Watson-Learning-Opt-Out` |호출에 대한 데이터 콜렉션을 제외하는지 여부를 표시합니다. |

패키지 바인딩에서 `speechToText` 액션을 호출하여 인코딩된 오디오를 변환하십시오.

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**출력 예**
```
{
  "data": "Hello Watson"
}
```
{: screen}


