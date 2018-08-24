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

# {{site.data.keyword.visualrecognitionshort}} 패키지 

{{site.data.keyword.visualrecognitionfull}} 서비스는 딥 러닝 알고리즘을 사용하여 서비스로 업로드된 이미지에서 장면, 물체 및 얼굴을 식별합니다. 사용자는 자신의 요구사항에 맞는 주제를 식별하도록 사용자 정의 분류기를 작성하고 훈련시킬 수 있습니다.
{:shortdesc}

{{site.data.keyword.visualrecognitionshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.visualrecognitionshort}} API 참조서의 추가 세부사항을 찾을 수 있습니다. 

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html) |패키지 | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |{{site.data.keyword.visualrecognitionshort}} 서비스 관련 작업을 수행합니다. |
| [classify](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    images_file,     accept_language,     url,     threshold,     owners,     classifier_ids,     images_file_content_type,  |이미지를 분류합니다. |
| [detect-faces](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    images_file,     url,     images_file_content_type,  |이미지에서 얼굴을 감지합니다. |
| [create-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,     classname_positive_examples,     negative_examples,  |분류기를 작성합니다. |
| [delete-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  |분류기를 삭제합니다. |
| [get-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  |분류기 세부사항을 검색합니다. |
| [list-classifiers](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    verbose,  |분류기의 목록을 검색합니다. |
| [update-classifier](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,     classname_positive_examples,     negative_examples,  |분류기를 업데이트합니다. |
| [get-core-ml-model](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    classifier_id,  |분류기의 코어 ML 모델을 검색합니다. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  |레이블 지정된 데이터를 삭제합니다. |

## {{site.data.keyword.}} 서비스 인스턴스 작성
{: #service_instance}

패키지를 설치하기 전에 {{site.data.keyword.}} 서비스 인스턴스 및 서비스 신임 정보를 작성해야 합니다. {: shortdesc}

1. [{{site.data.keyword.}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](https://console.bluemix.net/catalog/services/watson_vision_combined).
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 신임 정보도 사용자를 위해 작성됩니다. 

## {{site.data.keyword.}} 패키지 설치
{: #install}

{{site.data.keyword.}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #visualrecognition_cli}

시작하기 전에 다음을 수행하십시오. 
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](bluemix_cli.html#cloudfunctions_cli).
  2. [`wskdeploy` 명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)을 설치하고 다운로드된 2진을 PATH에 추가하십시오. 

{{site.data.keyword.}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    wskdeploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
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
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.}} 인스턴스의 신임 정보를 패키지에 바인드하십시오.
    ```
    ibmcloud fn service bind watson-vision-combined visual-recognition-v3
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.}} 서비스 인스턴스 신임 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get visual-recognition-v3 parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "watson-vision-combined": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #visualrecognition_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/create)로 이동하십시오. 

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록에서 {{site.data.keyword.cos_short}} 패키지가 설치될 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다. 

3. **패키지 설치**를 클릭하십시오. 

4. **Watson** 패키지 그룹을 클릭하십시오. 

5. **Visual Recognition** 패키지를 클릭하십시오. 

5. **설치**를 클릭하십시오. 

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **visual-recognition-v3**인 새 패키지를 검색할 수 있습니다. 

7. **visual-recognition-v3** 패키지의 액션을 사용하려면 서비스 신임 정보를 액션에 바인드해야 합니다. 
  * 서비스 신임 정보를 패키지의 모든 액션에 바인드하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.  
  * 서비스 신임 정보를 개별 액션에 바인드하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다. 
    1. 사용할 **visual-recognition-v3** 패키지의 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.  
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.  
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 신임 정보 JSON 오브젝트를 붙여넣으십시오. 

## {{site.data.keyword.visualrecognitionshort}} 패키지 사용
{: #usage}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오. 

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

모든 액션에서는 YYYY-MM-DD 형식의 버전 매개변수가 필요합니다. API가 백워드 호환 불가 방식으로 변경된 경우에는 새 버전 날짜가 릴리스됩니다. [API 참조서](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning)의 세부사항을 참조하십시오. 

이 패키지의 기능에서는 Visual Recognition의 현재 버전(2018-03-19)을 사용합니다. `list-classifiers` 액션을 사용해 보십시오. 
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}
