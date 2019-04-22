---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: nlc, natural language classifier, machine learning

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.nlclassifiershort}} 패키지 

{{site.data.keyword.nlclassifierfull}}에서는 머신 러닝 알고리즘을 사용하여 짧은 텍스트 입력에 대해 최상위 일치의 사전 정의된 클래스를 리턴합니다. 사용자는 사전 정의된 클래스를 예제 텍스트에 연결함으로써 서비스가 해당 클래스를 새 입력에 적용할 수 있도록 분류기를 작성하고 훈련시킵니다.
{: shortdesc}

{{site.data.keyword.nlclassifiershort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.nlclassifiershort}} API 참조서의 추가 세부사항을 찾을 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html) |패키지 | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  |{{site.data.keyword.nlclassifiershort}} 서비스 관련 작업을 수행합니다. |
| [classify](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify) |액션 |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    text,  |구문을 분류합니다. |
| [classify-collection](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection) |액션 |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,    collection,  |다중 구문을 분류합니다. |
| [create-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier) |액션 |  username, password, iam_access_token, iam_apikey, iam_url,   headers, headers[X-Watson-Learning-Opt-Out], url,    metadata,     training_data,  |분류기를 작성합니다. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier) |액션 |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  |분류기를 삭제합니다. |
| [get-classifier](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier) |액션 |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  |분류기에 대한 정보를 가져옵니다. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers) |액션 |  username, password, iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, |분류기를 나열합니다. |

## {{site.data.keyword.nlclassifiershort}} 서비스 인스턴스 작성
{: #service_instance_classifier}

패키지를 설치하기 전에 {{site.data.keyword.nlclassifiershort}} 서비스 인스턴스 및 서비스 인증 정보를 작성해야 합니다.
{: shortdesc}

1. [{{site.data.keyword.nlclassifiershort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](https://cloud.ibm.com/catalog/services/natural_language_classifier).
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 인증 정보도 사용자를 위해 작성됩니다.

## {{site.data.keyword.nlclassifiershort}} 패키지 설치
{: #install_classifier}

{{site.data.keyword.nlclassifiershort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.nlclassifiershort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #nlclassifier_cli}

시작하기 전에 다음을 수행하십시오.
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

{{site.data.keyword.nlclassifiershort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.nlclassifiershort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.nlclassifiershort}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    IAM 서비스이므로, 서비스 인스턴스가 작성된 영역에 따라 서비스 인스턴스에는 서로 다른 이름이 지정될 수 있습니다. 위의 명령이 실패한 경우에는 바인딩 명령에 대해 다음 서비스 이름을 사용하십시오.
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.nlclassifiershort}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get natural-language-classifier-v1 parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #nlclassifier_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/create)로 이동하십시오.

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록을 사용하여 패키지를 설치할 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다.

3. **패키지 설치**를 클릭하십시오.

4. **Watson** 패키지 그룹을 클릭하십시오. 

5. **Natural Language Classifier** 패키지를 클릭하십시오. 

5. **설치**를 클릭하십시오.

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **natural-language-classifier-v1**인 새 패키지를 검색할 수 있습니다. 

7. **natural-language-classifier-v1** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다. 
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다.
    1. 사용할 **natural-language-classifier-v1** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다. 
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오. 
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.

## {{site.data.keyword.nlclassifiershort}} 패키지 사용
{: #usage_classifier}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오.

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

`list-classifiers` 액션을 사용해 보십시오.
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}
