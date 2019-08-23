---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: natural language, understanding, watson knowledge studio, functions

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


# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

설치 가능한 {{site.data.keyword.nlufull}} 서비스는 스케일에 따라 텍스트 컨텐츠의 다양한 기능을 분석하는 데 도움이 됩니다.
{: shortdesc}

텍스트, 원시 HTML 또는 공용 URL을 제공하면 {{site.data.keyword.nlushort}}에서 요청한 기능에 대한 결과를 제공합니다. 서비스가 기본적으로 분석 전에 HTML 컨텐츠를 정리하므로, 결과에서는 대부분의 광고와 기타 원하지 않는 컨텐츠를 무시할 수 있습니다. 사용자는 Natural Language Understanding에서 사용자 정의 엔티티와 관계의 감지에 사용될 수 있는 Watson Knowledge Studio를 사용하여 <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">사용자 정의 모델</a>을 작성할 수 있습니다.

{{site.data.keyword.nlushort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 자세한 정보는 엔티티 이름을 클릭하여 {{site.data.keyword.nlushort}} API 참조를 참조하십시오.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html){: external} |패키지 | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,  |{{site.data.keyword.nlushort}} 서비스 관련 작업을 수행합니다. |
| [`analyze`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze){: external} |액션 |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `features`, `text`, `html`, `url`, `clean`, `xpath`, `fallback_to_raw`, `return_analyzed_text`, `language`, `limit_text_characters`,  |텍스트, HTML 또는 공용 웹 페이지를 분석합니다. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model){: external} |액션 |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id`,  |모델을 삭제합니다. |
| [`list-models`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models){: external} |액션 |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, |모델을 나열합니다. |

## {{site.data.keyword.nlushort}} 서비스 인스턴스 작성
{: #service_instance_understanding}

패키지를 설치하기 전에 {{site.data.keyword.nlushort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.nlushort}} 서비스 인스턴스를 작성하십시오](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}.
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

## {{site.data.keyword.nlushort}} 패키지 설치
{: #install_understanding}

{{site.data.keyword.nlushort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.nlushort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #nlus_cli}

**시작하기 전에**
[{{site.data.keyword.cloud_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.nlushort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.nlushort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.nlushort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    **출력 예**
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.nlushort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    **출력 예**
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #nlus_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지](https://cloud.ibm.com/openwhisk/create){: external}로 이동하십시오.

2. 드롭 다운 메뉴에서 네임스페이스를 사용하여 패키지를 설치할 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다.

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오.

5. **Natural Language Understanding** 패키지를 클릭하십시오.

5. **설치**를 클릭하십시오.

6. 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **natural-language-understanding-v1**인 새 패키지를 검색할 수 있습니다.

7. **natural-language-understanding-v1** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 [CLI 지시사항](#nlus_cli)의 4 - 5단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. 

  사용할 각 액션마다 다음 단계를 완료해야 합니다.
  {: note}

    1. 사용할 **natural-language-understanding-v1** 패키지의 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

## {{site.data.keyword.nlushort}} 패키지 사용
{: #usage_understanding}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

모든 액션에서는 YYYY-MM-DD 형식의 버전 매개변수가 필요합니다. API가 백워드 호환 불가 방식으로 변경된 경우에는 새 버전 날짜가 릴리스됩니다. [API 참조서](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning){: external}의 세부사항을 참조하십시오.

이 패키지의 기능에서는 Natural Language Understanding의 현재 버전(2018-03-16)을 사용합니다. `list-models` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}



