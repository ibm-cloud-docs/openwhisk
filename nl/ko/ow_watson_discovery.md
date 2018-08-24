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

# {{site.data.keyword.discoveryshort}} 패키지 

{{site.data.keyword.discoveryfull}} 서비스는 패턴, 상태동향 및 액션 가능한 통찰을 식별하여 보다 나은 의사결정을 내릴 수 있도록 애플리케이션에 추가할 수 있는 코그너티브 검색 및 컨텐츠 분석 엔진입니다. 사전 보강된 컨텐츠의 구조화된 및 구조화되지 않은 데이터를 안전하게 통합하고, 단순한 조회 언어의 사용으로 결과를 수동으로 필터링할 필요가 없습니다.
{: shortdesc}

{{site.data.keyword.discoveryshort}} 패키지에는 다음 엔티티가 포함되어 있습니다. 엔티티 이름을 클릭하여 {{site.data.keyword.discoveryshort}} API 참조서의 추가 세부사항을 찾을 수 있습니다. 

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html) |패키지 | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |{{site.data.keyword.discoveryshort}} 서비스 관련 작업을 수행합니다. |
| [create-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, description, size,  |환경을 작성합니다. |
| [delete-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,  |환경을 삭제합니다. |
| [get-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,  |환경에 대한 정보를 가져옵니다. |
| [list-environments](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,  |환경을 나열합니다. |
| [list-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,  |콜렉션 간의 필드를 나열합니다. |
| [update-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description,  |환경을 업데이트합니다. |
| [create-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, conversions, enrichments, normalizations,  |구성을 추가합니다.|
| [delete-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,  |구성을 삭제합니다.|
| [get-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,  |구성 세부사항을 가져옵니다. |
| [list-configurations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     name,  |구성을 나열합니다. |
| [update-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,    name, description, conversions, enrichments, normalizations,  |구성을 업데이트합니다.|
| [test-configuration-in-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration,     step,     configuration_id,     file,     metadata,     file_content_type,  |구성을 테스트합니다.|
| [create-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, configuration_id, language,  |콜렉션을 작성합니다. |
| [delete-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |콜렉션을 삭제합니다.|
| [get-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |콜렉션 세부사항을 가져옵니다. |
| [list-collection-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |콜렉션 필드를 나열합니다. |
| [list-collections](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     name,  |콜렉션을 나열합니다.|
| [update-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    name, description, configuration_id,  |콜렉션을 업데이트합니다.|
| [create-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    expansions,  |확장 목록을 작성하거나 업데이트합니다. |
| [delete-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |확장 목록을 삭제합니다. |
| [list-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |확장 목록을 가져옵니다. |
| [add-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     file,     metadata,     file_content_type,  |문서를 추가합니다.|
| [delete-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,  |문서를 삭제합니다. |
| [get-document-status](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,  |문서 세부사항을 가져옵니다. |
| [update-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,     file,     metadata,     file_content_type,  |문서를 업데이트합니다. |
| [federated-query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  |다중 콜렉션의 문서를 조회합니다. |
| [federated-query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  |다중 콜렉션 시스템 공지를 조회합니다. |
| [query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  |콜렉션을 조회합니다. |
| [query-entities](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    feature, entity, context, count, evidence_count,  |Knowledge Graph 엔티티를 조회합니다. |
| [query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  |시스템 공지를 조회합니다. |
| [query-relations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    entities, context, sort, filter, count, evidence_count,  |Knowledge Graph 관계를 조회합니다. |
| [add-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    natural_language_query, filter, examples,  |훈련 데이터에 조회를 추가합니다. |
| [create-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,    document_id, cross_reference, relevance,  |훈련 데이터 조회에 예제를 추가합니다. |
| [delete-all-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |모든 훈련 데이터를 삭제합니다. |
| [delete-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  |훈련 데이터 조회를 삭제합니다. |
| [delete-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,  |훈련 데이터 조회의 예제를 삭제합니다. |
| [get-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  |조회에 대한 세부사항을 가져옵니다. |
| [get-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,  |훈련 데이터 예제에 대한 세부사항을 가져옵니다. |
| [list-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,  |훈련 데이터를 나열합니다. |
| [list-training-examples](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,  |훈련 데이터 조회에 대한 예제를 나열합니다. |
| [update-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,    cross_reference, relevance,  |예제에 대한 레이블 또는 교차 참조를 변경합니다. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data) |액션 |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  |레이블 지정된 데이터를 삭제합니다. |

## {{site.data.keyword.discoveryshort}} 서비스 인스턴스 작성
{: #service_instance}

패키지를 설치하기 전에 {{site.data.keyword.discoveryshort}} 서비스 인스턴스 및 서비스 신임 정보를 작성해야 합니다. 

1. [{{site.data.keyword.discoveryshort}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](https://console.bluemix.net/catalog/services/discovery).
2. 서비스 인스턴스가 작성되면 자동 생성 서비스 신임 정보도 사용자를 위해 작성됩니다. 

## {{site.data.keyword.discoveryshort}} 패키지 설치
{: #install}

{{site.data.keyword.discoveryshort}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI를 사용하여 네임스페이스에 {{site.data.keyword.discoveryshort}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #discovery_cli}

시작하기 전에 다음을 수행하십시오. 
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](bluemix_cli.html#cloudfunctions_cli).
  2. [`wskdeploy` 명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)을 설치하고 다운로드된 2진을 PATH에 추가하십시오. 

{{site.data.keyword.discoveryshort}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.discoveryshort}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. 패키지를 배치하십시오.
    ```
    wskdeploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
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
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. 작성한 {{site.data.keyword.discoveryshort}} 인스턴스의 신임 정보를 패키지에 바인드하십시오.
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. 패키지가 {{site.data.keyword.discoveryshort}} 서비스 인스턴스 신임 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "discovery": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #discovery_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/create)로 이동하십시오. 

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록에서 {{site.data.keyword.cos_short}} 패키지가 설치될 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다. 

3. **패키지 설치**를 클릭하십시오. 

4. **Watson** 패키지 그룹을 클릭하십시오. 

5. **Discovery** 패키지를 클릭하십시오. 

5. **설치**를 클릭하십시오. 

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **discovery-v1**인 새 패키지를 검색할 수 있습니다. 

7. **discovery-v1** 패키지의 액션을 사용하려면 서비스 신임 정보를 액션에 바인드해야 합니다. 
  * 서비스 신임 정보를 패키지의 모든 액션에 바인드하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.  
  * 서비스 신임 정보를 개별 액션에 바인드하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다. 
    1. 사용할 **discovery-v1** 패키지의 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.  
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.  
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 신임 정보 JSON 오브젝트를 붙여넣으십시오. 

## {{site.data.keyword.discoveryshort}} 패키지 사용
{: #usage}

이 패키지의 액션을 사용하려면 다음 형식으로 명령을 실행하십시오. 

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

모든 액션에서는 YYYY-MM-DD 형식의 버전 매개변수가 필요합니다. API가 백워드 호환 불가 방식으로 변경된 경우에는 새 버전 날짜가 릴리스됩니다. [API 참조서](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning)의 세부사항을 참조하십시오. 

이 패키지의 기능에서는 Discovery의 현재 버전(2018-03-05)을 사용합니다. `list-environments` 액션을 사용해 보십시오. 
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}
