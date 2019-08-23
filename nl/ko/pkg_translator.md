---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: watson, translator, cognitive, translating text, language, functions

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


# 변환기

## {{site.data.keyword.languagetranslatorshort}} 패키지 

{: #pkg_translator}

설치 가능한 {{site.data.keyword.languagetranslatorfull}} 패키지는 한 언어에서 다른 언어로 텍스트를 변환합니다. 이 서비스는 사용자가 고유한 용어와 언어를 기반으로 사용자 정의할 수 있는 다중 IBM 제공 변환 모델을 제공합니다.
{: shortdesc}

{{site.data.keyword.languagetranslatorshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.languagetranslatorshort}} API 참조서의 추가 세부사항을 찾을 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html){: external} |패키지 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` |{{site.data.keyword.languagetranslatorshort}} 서비스 관련 작업을 수행합니다. |
| [`translate`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `model_id`, `source`, `target` |텍스트를 변환합니다. |
| [`identify`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text` |텍스트의 언어를 식별합니다. |
| [`list-identifiable-languages`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` |식별 가능한 언어를 나열합니다. |
| [`create-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `base_model_id`, `name`, `forced_glossary`, `parallel_corpus` |모델을 작성합니다. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` |모델을 삭제합니다. |
| [`get-model`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` |모델 세부사항을 가져옵니다. |
| [`list-models`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models){: external} |액션 |`username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `source`, `target`, `default_models` |모델을 나열합니다. |

## {{site.data.keyword.languagetranslatorshort}} 서비스 인스턴스 작성
{: #service_instance_translator}

패키지를 설치하기 전에 {{site.data.keyword.languagetranslatorshort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.languagetranslatorshort}} 서비스 인스턴스를 작성하십시오](https://cloud.ibm.com/catalog/services/language_translator){: external}.
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

## {{site.data.keyword.languagetranslatorshort}} 패키지 설치
{: #install_translator}

{{site.data.keyword.languagetranslatorshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.languagetranslatorshort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #languagetranslator_cli}

**시작하기 전에**

{{site.data.keyword.cloud_notm}} CLI용 [{{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.languagetranslatorshort}} 패키지를 설치하십시오.

1. {{site.data.keyword.languagetranslatorshort}} 패키지 저장소를 복제하십시오.

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.languagetranslatorshort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.

    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    **출력 예**

    ```
Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.languagetranslatorshort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.

    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    **출력 예**

    ```
ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #languagetranslator_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지](https://cloud.ibm.com/openwhisk/create){: external}로 이동하십시오.

2. 드롭 다운 메뉴에서 네임스페이스를 사용하여 패키지를 설치할 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다.

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오.

5. **Language Translator** 패키지를 클릭하십시오.

6. **설치**를 클릭하십시오.

7. 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 `language-translator-v3`인 새 패키지를 검색할 수 있습니다.

8. `language-translator-v3` 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 [CLI 지시사항의 5 - 6](#languagetranslator_cli)단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오.
  
  사용할 각 액션마다 다음 단계를 완료해야 합니다.
  {: note}

    1. 사용할 `language-translator-v3` 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 `parameter`를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

## {{site.data.keyword.languagetranslatorshort}} 패키지 사용
{: #usage_translator}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

모든 액션에서는 YYYY-MM-DD 형식의 버전 매개변수가 필요합니다. API가 백워드 호환 불가 방식으로 변경된 경우에는 새 버전 날짜가 릴리스됩니다. [API 참조서](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning){: external}의 세부사항을 참조하십시오.

이 패키지의 기능에서는 Language Translator의 현재 버전(`2018-05-01`)을 사용합니다. `identify` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}




