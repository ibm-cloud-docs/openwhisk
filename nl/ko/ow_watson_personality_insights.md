---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: personality insights, watson, cognitive, serverless, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.personalityinsightsshort}} 패키지 

{{site.data.keyword.personalityinsightsfull}} 서비스로 애플리케이션은 소셜 매체, 엔터프라이즈 데이터 또는 기타 디지털 통신으로부터 인사이트를 이끌어낼 수 있습니다. 이 서비스는 언어 분석을 사용하여 이메일, 텍스트 메시지, 트윗 및 포럼 게시물 등의 디지털 통신으로부터 빅파이브, 욕구 및 가치를 포함한 개인의 고유한 성격 특성을 추론합니다.
{: shortdesc}

이 서비스는 잠재적으로 노이즈가 있는 소셜 매체로부터 해당 성격 특성을 반영하는 개인의 모습을 자동으로 추론할 수 있습니다. 이 서비스는 분석 결과를 기반으로 소비 성향을 추론할 수 있으며, 시간소인이 있는 JSON 컨텐츠의 경우에는 시간에 따른 행동을 보고할 수 있습니다.
* 성격 특성을 기술하기 위해 서비스가 사용하는 모델의 의미에 대한 정보는 [성격 모델](https://cloud.ibm.com/docs/services/personality-insights/models.html)을 참조하십시오.
* 소비 성향의 의미에 대한 정보는 [소비 성향](https://cloud.ibm.com/docs/services/personality-insights/preferences.html)을 참조하십시오.

**참고:** 요청 로깅은 {{site.data.keyword.personalityinsightsshort}} 서비스에 대해 사용되지 않습니다. 이 서비스는 `X-Watson-Learning-Opt-Out` 요청 헤더가 설정되었는지 여부와 무관하게 요청 및 응답의 데이터를 로깅하거나 보관하지 않습니다.

{{site.data.keyword.personalityinsightsshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.personalityinsightsshort}} API 참조서의 추가 세부사항을 찾을 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) |패키지 | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |{{site.data.keyword.personalityinsightsshort}} V3 서비스 관련 작업을 수행합니다. |
| [profile](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  |프로파일을 가져옵니다. |
| [profile-as-csv](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  |CSV 파일로서 프로파일을 가져옵니다. |

## {{site.data.keyword.personalityinsightsshort}} 서비스 인스턴스 작성
{: #service_instance_insights}

패키지를 설치하기 전에 {{site.data.keyword.personalityinsightsshort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.personalityinsightsshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](https://cloud.ibm.com/catalog/services/personality_insights).
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

## {{site.data.keyword.personalityinsightsshort}} 패키지 설치
{: #install_insights}

{{site.data.keyword.personalityinsightsshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.personalityinsightsshort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #personalityinsights_cli}

시작하기 전에 다음을 수행하십시오.
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

{{site.data.keyword.personalityinsightsshort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.personalityinsightsshort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.personalityinsightsshort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    IAM 서비스이므로, 서비스 인스턴스가 작성된 영역에 따라 서비스 인스턴스에는 서로 다른 이름이 지정될 수 있습니다. 위의 명령이 실패한 경우에는 바인딩 명령에 대해 다음 서비스 이름을 사용하십시오.
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.personalityinsightsshort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #personalityinsights_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/create)로 이동하십시오.

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록을 사용하여 패키지를 설치할 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다.

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오. 

5. **Personality Insights** 패키지를 클릭하십시오. 

5. **설치**를 클릭하십시오.

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **personality-insights-v3**인 새 패키지를 검색할 수 있습니다. 

7. **personality-insights-v3** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다. 
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다.
    1. 사용할 **personality-insights-v3** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다. 
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오. 
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

## {{site.data.keyword.personalityinsightsshort}} 패키지 사용
{: #usage_insights}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

모든 액션에서는 YYYY-MM-DD 형식의 버전 매개변수가 필요합니다. API가 백워드 호환 불가 방식으로 변경된 경우에는 새 버전 날짜가 릴리스됩니다. [API 참조서](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning)의 세부사항을 참조하십시오.

이 패키지의 기능에서는 Personality Insights의 현재 버전(2017-10-13)을 사용합니다. `profile` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "여기서 자신에 대한 인용을 기술할 수 있지만, 길이는 최소한 100자가 넘어야 합니다. 이 인용은 단지 일부 채우기 텍스트에 불과하며 Personality Insights 서비스에서 제법 흥미로운 어떤 것도 리턴하지 않을 가능성이 높습니다. 이 서비스는 언어 분석을 사용하여 이메일, 텍스트 메시지, 트윗 및 포럼 게시물 등의 디지털 통신으로부터 빅파이브, 욕구 및 가치를 포함한 개인의 고유한 성격 특성을 추론합니다. 이 서비스는 잠재적으로 노이즈가 있는 소셜 매체로부터 해당 성격 특성을 반영하는 개인의 모습을 자동으로 추론할 수 있습니다. 이 서비스는 분석 결과를 기반으로 소비 성향을 추론할 수 있으며, 시간소인이 있는 JSON 컨텐츠의 경우에는 시간에 따른 행동을 보고할 수 있습니다."
```
{: pre}
