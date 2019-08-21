---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: functions, serverless, watson

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


# {{site.data.keyword.toneanalyzershort}}
{: #pkg_tone_analyzer}

설치 가능한 {{site.data.keyword.toneanalyzerfull}} 서비스는 언어 분석을 사용하여 쓰여진 텍스트에서 감정과 언어의 어조를 감지합니다.
{:shortdesc}

이 서비스는 문서 레벨과 문장 레벨 모두에서 어조를 분석할 수 있습니다. 이 서비스를 사용하여 쓰여진 전언이 인지되는 방법을 파악한 후에 전언의 어조를 개선할 수 있습니다. 업체에서는 이 서비스를 사용하여 자사 고객의 전언에 대한 어조를 알아보고 각 고객에게 이에 응대하거나 자사 고객의 대화를 이해하고 이를 개선할 수 있습니다.

요청 로깅은 Tone Analyzer 서비스에 대해 사용되지 않습니다. 이 서비스는 `X-Watson-Learning-Opt-Out` 요청 헤더가 설정되었는지 여부와 무관하게 요청 및 응답의 데이터를 로깅하거나 보관하지 않습니다.
{: note}

{{site.data.keyword.toneanalyzershort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.toneanalyzershort}} API 참조서의 추가 세부사항을 찾을 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html){: external} |패키지 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` |{{site.data.keyword.toneanalyzershort}} 서비스 관련 작업을 수행합니다. |
| [`tone`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`,`iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,    `tone_input`, `content_type`, `sentences`, `tones`, `content_language`, `accept_language` |일반 어조를 분석합니다. |
| [`tone-chat`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat){: external} |액션 |`username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `utterances`, `content_language`, `accept_language` |고객 관여 어조를 분석합니다. |


## {{site.data.keyword.toneanalyzershort}} 서비스 인스턴스 작성
{: #service_instance_tone}

패키지를 설치하기 전에 {{site.data.keyword.toneanalyzershort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.toneanalyzershort}} 서비스 인스턴스를 작성하십시오](https://cloud.ibm.com/catalog/services/tone_analyzer){: external}.
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

## {{site.data.keyword.toneanalyzershort}} 패키지 설치
{: #install_tone}

{{site.data.keyword.toneanalyzershort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.toneanalyzershort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #toneanalyzer_cli}

**시작하기 전에**
[{{site.data.keyword.cloud_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.toneanalyzershort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.toneanalyzershort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.toneanalyzershort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    IAM 서비스이므로, 서비스 인스턴스가 작성된 영역에 따라 서비스 인스턴스에는 서로 다른 이름이 지정될 수 있습니다. 명령이 실패한 경우에는 바인딩 명령에 대해 다음 서비스 이름을 사용하십시오.
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}

    **출력 예**
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.toneanalyzershort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    **출력 예**
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #toneanalyzer_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지](https://cloud.ibm.com/openwhisk/create){: external}로 이동하십시오.

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록을 사용하여 패키지를 설치할 네임스페이스를 선택하십시오. 

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오.

5. **Tone Analyzer** 패키지를 클릭하십시오.

5. **설치**를 클릭하십시오.

6. 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **`tone-analyzer-v3`**인 새 패키지를 검색할 수 있습니다.

7. **tone-analyzer-v3** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 [CLI 지시사항](#toneanalyzer_cli)의 4 - 5단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. 
  
  사용할 각 액션마다 다음 단계를 완료해야 합니다.
  {: note}
  
    1. 사용할 **tone-analyzer-v3** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

## {{site.data.keyword.toneanalyzershort}} 패키지 사용
{: #usage_tone}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

모든 액션에서는 YYYY-MM-DD 형식의 버전 매개변수가 필요합니다. API가 백워드 호환 불가 방식으로 변경된 경우에는 새 버전 날짜가 릴리스됩니다. [API 참조서](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning){: external}의 세부사항을 참조하십시오.

이 패키지의 기능에서는 Tone Analyzer의 현재 버전(2017-09-21)을 사용합니다. `tone` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "즐거운 하루가 되시길 기원합니다."
```
{: pre}

