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

# {{site.data.keyword.speechtotextshort}} 패키지 

{{site.data.keyword.speechtotextfull}} 서비스는 IBM의 음성 인식 기능을 사용하여 발화된 오디오의 필사본을 생성하는 API를 제공합니다.
{:shortdesc}

이 서비스는 다양한 언어와 오디오 형식으로부터 음성을 필사할 수 있습니다. 기본 필사 외에도 이 서비스는 오디오의 다양한 측면에 대한 상세 정보를 생성할 수 있습니다. 대부분의 언어에서, 이 서비스는 2개의 샘플링 속도인 광대역 및 협대역을 지원합니다. 이는 UTF-8 문자 세트의 모든 JSON 응답 컨텐츠를 리턴한다. 이 서비스에 대한 자세한 정보는 [IBM&reg; Cloud 문서](https://console.bluemix.net/docs/services/speech-to-text/index.html)를 참조하십시오. 

{{site.data.keyword.speechtotextshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.speechtotextshort}} API 참조서의 추가 세부사항을 찾을 수 있습니다. 

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) |패키지 | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |{{site.data.keyword.speechtotextshort}} V1 서비스 관련 작업을 수행합니다. |
| [get-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  |모델을 가져옵니다. |
| [list-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, |모델을 나열합니다. |
| [recognize-sessionless](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    audio,     content_type,     model,     customization_id,     acoustic_customization_id,     base_model_version,     customization_weight,     inactivity_timeout,     keywords,     keywords_threshold,     max_alternatives,     word_alternatives_threshold,     word_confidence,     timestamps,     profanity_filter,     smart_formatting,     speaker_labels,  |오디오를 인식합니다(세션리스). |
| [check-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    id,  |작업을 검사합니다. |
| [check-jobs](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, |작업을 검사합니다. |
| [create-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    audio,     content_type,     model,     callback_url,     events,     user_token,     results_ttl,     customization_id,     acoustic_customization_id,     base_model_version,     customization_weight,     inactivity_timeout,     keywords,     keywords_threshold,     max_alternatives,     word_alternatives_threshold,     word_confidence,     timestamps,     profanity_filter,     smart_formatting,     speaker_labels,  |작업을 작성합니다. |
| [delete-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    id,  |작업을 삭제합니다. |
| [register-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    callback_url,     user_secret,  |콜백을 등록합니다. |
| [unregister-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    callback_url,  |콜백을 등록 취소합니다. |
| [create-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, base_model_name, dialect, description,  |사용자 정의 언어 모델을 작성합니다. |
| [delete-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 언어 모델을 삭제합니다. |
| [get-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 언어 모델을 가져옵니다. |
| [list-language-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  |사용자 정의 언어 모델을 나열합니다. |
| [reset-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 언어 모델을 재설정합니다. |
| [train-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_type_to_add,     customization_weight,  |사용자 정의 언어 모델을 훈련합니다. |
| [upgrade-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 언어 모델을 업그레이드합니다. |
| [add-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,     corpus_file,     allow_overwrite,  |말뭉치를 추가합니다.|
| [delete-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,  |말뭉치를 삭제합니다.|
| [get-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,  |말뭉치를 가져옵니다. |
| [list-corpora](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |말뭉치를 나열합니다. |
| [add-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,    word, sounds_like, display_as,  | 사용자 정의 단어를 추가합니다. |
| [add-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  |사용자 정의 단어를 추가합니다. |
| [delete-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,  | 사용자 정의 단어를 삭제합니다. |
| [get-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,  | 사용자 정의 단어를 가져옵니다. |
| [list-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_type,     sort,  |사용자 정의 단어를 나열합니다. |
| [create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, base_model_name, description,  |사용자 정의 음향 모델을 작성합니다. |
| [delete-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 음향 모델을 삭제합니다. |
| [get-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 음향 모델을 가져옵니다. |
| [list-acoustic-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  |사용자 정의 음향 모델을 나열합니다. |
| [reset-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |사용자 정의 음향 모델을 재설정합니다. |
| [train-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     custom_language_model_id,  |사용자 정의 음향 모델을 훈련합니다. |
| [upgrade-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     custom_language_model_id,  |사용자 정의 음향 모델을 업그레이드합니다. |
| [add-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,     audio_resource,     content_type,     contained_content_type,     allow_overwrite,  |오디오 리소스를 추가합니다. |
| [delete-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,  |오디오 리소스를 삭제합니다. |
| [get-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,  |오디오 리소스를 가져옵니다. |
| [list-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  |오디오 리소스를 나열합니다. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  |레이블 지정된 데이터를 삭제합니다. |

## {{site.data.keyword.speechtotextshort}} 서비스 인스턴스 작성
{: #service_instance}

패키지를 설치하기 전에 {{site.data.keyword.speechtotextshort}} 서비스 인스턴스 및 서비스 신임 정보를 작성해야 합니다. {: shortdesc}

1. [{{site.data.keyword.speechtotextshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](https://console.bluemix.net/catalog/services/speech_to_text).
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 신임 정보도 사용자를 위해 작성됩니다. 

## {{site.data.keyword.speechtotextshort}} 패키지 설치
{: #install}

{{site.data.keyword.speechtotextshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.speechtotextshort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #speechtotext_cli}

시작하기 전에 다음을 수행하십시오. 
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](bluemix_cli.html#cloudfunctions_cli).
  2. [`wskdeploy` 명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)을 설치하고 다운로드된 2진을 PATH에 추가하십시오. 

{{site.data.keyword.speechtotextshort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.speechtotextshort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    wskdeploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.speechtotextshort}} 인스턴스의 신임 정보를 패키지에 바인드하십시오.
    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    IAM 서비스이므로, 서비스 인스턴스가 작성된 영역에 따라 서비스 인스턴스에는 서로 다른 이름이 지정될 수 있습니다. 위의 명령이 실패한 경우에는 바인드 명령에 대해 다음 서비스 이름을 사용하십시오.
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.speechtotextshort}} 서비스 인스턴스 신임 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    출력 예:
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

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/create)로 이동하십시오. 

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록에서 {{site.data.keyword.cos_short}} 패키지가 설치될 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다. 

3. **패키지 설치**를 클릭하십시오. 

4. **Watson** 패키지 그룹을 클릭하십시오. 

5. **Speech To Text** 패키지를 클릭하십시오. 

5. **설치**를 클릭하십시오. 

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **speech-to-text-v1**인 새 패키지를 검색할 수 있습니다. 

7. **speech-to-text-v1** 패키지의 액션을 사용하려면 서비스 신임 정보를 액션에 바인드해야 합니다. 
  * 서비스 신임 정보를 패키지의 모든 액션에 바인드하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.  
  * 서비스 신임 정보를 개별 액션에 바인드하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다. 
    1. 사용할 **speech-to-text-v1** 패키지의 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.  
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.  
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 신임 정보 JSON 오브젝트를 붙여넣으십시오. 

## {{site.data.keyword.speechtotextshort}} 패키지 사용
{: #usage}

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
