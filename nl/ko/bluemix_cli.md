---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}} CLI 플러그인 설정
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}}는 {{site.data.keyword.openwhisk_short}} 시스템의 완벽한 관리를 허용하는 강력한 {{site.data.keyword.Bluemix_notm}} CLI용 플러그인을 제공합니다. {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하여 액션에서 코드 스니펫을 관리하고, 트리거 및 규칙을 작성하여 이벤트에 응답하는 액션을 사용하도록 하고, 액션을 패키지로 번들링할 수 있습니다.  {:shortdesc}

이제 다음의 {{site.data.keyword.openwhisk_short}} 플러그인 명령에서 별명 `fn`을 사용할 수 있습니다. `ibmcloud fn <command>`
{: tip}

## {{site.data.keyword.Bluemix_notm}} CLI 설정
{: #bluemix_cli_setup}

{{site.data.keyword.Bluemix_notm}} CLI를 다운로드하고 설치한 후 로그인하십시오.
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli)를 다운로드하고 설치하십시오.

2. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. IBM Cloud 영역을 지정하려면 [API 엔드포인트를 포함](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)시키십시오.

  ```
  ibmcloud login
  ```
  {: pre}

  다음 플래그를 사용하여 이에 대한 프롬프트를 건너뛰도록 로그인할 때 조직 및 영역을 지정할 수 있습니다. `ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. 조직 및 영역을 지정하지 않은 경우, 로그인 명령을 따르는 프롬프트를 완료하십시오. 


## {{site.data.keyword.openwhisk_short}} 플러그인 설정
{: #cloudfunctions_plugin_setup}

{{site.data.keyword.openwhisk_short}}에 대한 작업을 수행하려면 CLI 플러그인을 다운로드하고 설치하십시오.
{: shortdesc}

플러그인을 사용하여 다음을 수행할 수 있습니다.

* {{site.data.keyword.openwhisk_short}}에서 코드 스니펫 또는 액션을 실행합니다. [액션 작성 및 호출](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)을 참조하십시오.
* 트리거 및 규칙을 사용하여 액션이 이벤트에 응답할 수 있도록 합니다. [트리거 및 룰 작성](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers)을 참조하십시오.
* 패키지가 액션을 번들링하고 외부 이벤트 소스를 구성하는 방법을 알아봅니다. [패키지 작성 및 사용](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)을 참조하십시오.
* 패키지의 카탈로그를 탐색하고 [{{site.data.keyword.cloudant}} 이벤트 소스](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant) 등의 외부 서비스로 애플리케이션을 개선합니다.

{{site.data.keyword.openwhisk_short}} 플러그인과 함께 수행할 수 있는 모든 작업을 보려면 인수 없이 `ibmcloud fn`을 실행하십시오.
{: tip}

1. {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오.

  ```
    ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. 플러그인이 설치되어 있는지 확인하십시오.

  ```
    ibmcloud plugin list cloud-functions
  ```
  {: pre}

  출력:
  ```
Plugin Name          Version
    Cloud-Functions      1.0.16
  ```
  {: screen}

플러그인이 이미 있지만 업데이트해야 하는 경우 `ibmcloud plugin update cloud-functions`를 실행하십시오.
{:tip}



## 액션에서 서비스 사용
{: #binding_services_cli}

{{site.data.keyword.openwhisk_short}}는 실행 시간에 사용자 코드가 {{site.data.keyword.Bluemix_notm}} 서비스 인증 정보를 사용할 수 있도록 하는 `service bind` 명령을 제공합니다. 그런 다음 `service bind` 명령을 사용하여 {{site.data.keyword.openwhisk_short}}에 정의된 액션에 {{site.data.keyword.Bluemix_notm}} 서비스를 바인딩할 수 있습니다.

액션에서 서비스를 사용하는 방법에 대한 자세한 단계는 [액션에 서비스 바인딩](/docs/openwhisk?topic=cloud-functions-binding_services)를 참조하십시오.


## HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI 구성
{: #cli_https_proxy}

HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI를 설정할 수 있습니다. HTTPS 프록시를 설정하려면 `HTTPS_PROXY`라고 하는 환경 변수를 작성해야 합니다. 변수는 `{PROXY IP}:{PROXY PORT}` 형식을 사용하여 HTTPS 프록시 주소 및 해당 포트로 설정되어야 합니다.



## 여러 지역, 조직 및 영역으로 전환
{: #region_info}

이미 로그인되어 있는 경우에는 {{site.data.keyword.Bluemix_notm}} CLI에서 `ibmcloud target` 명령을 실행하여 지역, 조직 및 영역을 전환할 수 있습니다.


엔티티를 작성 및 관리하려면 네임스페이스를 대상화해야 합니다. 일부 상황에서 밑줄(`_`)로 표시될 수 있는 기본 네임스페이스는 현재 대상화된 Cloud Foundry 기반 네임스페이스에 해당합니다. 

각각 영역을 작성하여 사전-프로덕션(스테이징) 및 프로덕션 배치를 처리하기 위한 영역을 작성할 수 있습니다. 영역을 작성하면 사용자를 위해 정의된 두 개의 서로 다른 네임스페이스를 {{site.data.keyword.openwhisk_short}}에서 보유할 수 있습니다. [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create)를 실행하여 "staging" 및 "production" 등의 추가 영역을 조직 아래에서 작성하십시오.

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}}에는 네임스페이스 이름에 대한 제한사항이 있습니다. 자세한 정보는 [시스템 세부사항 및 한계](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities) 문서를 참조하십시오.
{: tip}

**경고**: 조직 또는 영역의 이름을 변경하면 변경된 이름을 기반으로 새 네임스페이스가 작성됩니다. 이전 네임스페이스의 엔티티는 새 네임스페이스에 표시되지 않으며 삭제될 수 있습니다.


## OpenWhisk CLI에서 {{site.data.keyword.openwhisk_short}} CLI 플러그인으로 마이그레이션
{: #cli_migration}

이제 {{site.data.keyword.openwhisk_short}} 엔티티와 상호작용하도록 {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용할 수 있습니다. 독립형 OpenWhisk CLI를 계속해서 사용할 수 있지만, 여기에는 {{site.data.keyword.openwhisk_short}}에서 지원하는 최신 기능(예: IAM 기반 네임스페이스 및 `서비스 바인드`)이 없습니다.
{: shortdesc}

### 명령 구문
{: #command_syntax}

더 이상 필요하지 않은 `wsk bluemix login` 명령을 제외한 모든 `wsk` 명령은 `ibmcloud fn` 명령을 사용하여 동일한 방법으로 작동됩니다. Cloud Functions CLI 플러그인의 명령에 대한 모든 명령 옵션과 인수는 OpenWhisk 독립형 CLI에 대한 명령과 동일합니다. 자세한 정보는 [Apache OpenWhisk CLI 프로젝트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-cli)의 내용을 참조하십시오.

### API 인증 및 호스트
{: #api_authentication}

OpenWhisk CLI에서는 사용자가 인증 API 키와 API 호스트를 구성해야 합니다. {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 API 키와 API 호스트를 명시적으로 구성할 필요가 없습니다. 대신 `ibmcloud login`을 사용하여 로그인하고 `ibmcloud target` 명령을 사용하여 지역 및 네임스페이스를 대상으로 지정할 수 있습니다. 로그인 이후에 모든 명령은 `ibmcloud fn`으로 시작됩니다.


cURL 또는 Postman 등의 외부 HTTP 클라이언트에서 {{site.data.keyword.openwhisk_short}}에 인증 API 키를 사용해야 하는 경우 다음 명령으로 이를 검색할 수 있습니다.

현재 API 키를 가져오려면 다음을 실행하십시오.
```
ibmcloud fn property get --auth
```
{: pre}

현재 API 호스트를 가져오려면 다음을 실행하십시오.
```
ibmcloud fn property get --apihost
```
{: pre}

API 키는 {{site.data.keyword.openwhisk_short}} CLI 플러그인에서 대상으로 지정한 지역, 조직 및 영역마다 특정합니다.
{: tip}

### API 게이트웨이 인증
{: #apigw_authentication}

OpenWhisk CLI에서는 `wsk api` 명령을 사용하여 API 관리를 위한 API 게이트웨이 권한을 구성할 수 있도록 사용자가 `wsk bluemix login`을 실행해야 합니다. {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 더 이상 `wsk bluemix login`을 실행할 필요가 없습니다. 대신에 `ibmcloud login` 명령을 사용하여 {{site.data.keyword.Bluemix_notm}}에 로그인하면 {{site.data.keyword.openwhisk}} 플러그인은 사용자의 현재 로그인 및 대상 정보를 자동으로 활용합니다. 이제 `ibmcloud fn api` 명령을 사용하여 API를 관리할 수 있습니다.

### 배치 스크립트 마이그레이션
{: #migrating_deploy_scripts}

`wsk` 2진에서 OpenWhisk CLI를 사용하는 스크립트가 있는 경우에는 모든 명령이 `ibmcloud fn` 명령을 사용하여 동일한 방법으로 작동됩니다. {{site.data.keyword.Bluemix_notm}} CLI 플러그인을 사용하도록 스크립트를 수정하거나, `wsk`를 사용하는 현재 명령이 `ibmcloud fn`으로 변환될 수 있도록 별명 또는 랩퍼를 작성할 수 있습니다. {{site.data.keyword.Bluemix_notm}} CLI에서 `ibmcloud login` 및 `ibmcloud target` 명령은 무인 모드로 작동합니다. 무인 모드에서는 {{site.data.keyword.openwhisk_short}} 엔티티를 배치하고 관리하기 위해 `ibmcloud fn` 명령을 실행하기 전에 사용자 환경을 구성할 수 있습니다.





## CLI 버전 히스토리
{: #version_history}

강조표시 및 버그 수정을 표시하는 버전의 히스토리 레코드입니다.

v1.0.30(2019-04-03)
* IAM 및 조직과 영역 기반의 서비스의 `service bind` 처리가 개선되었습니다. 
* API 엔드포인트 https://cloud.ibm.com 처리를 위한 수정사항이 추가되었습니다.

v1.0.29(2019-02-06)
* Manifest 파일을 통해 함수 엔티티의 콜렉션을 배치 또는 배치 취소하기 위한 `deploy` 및 `undeploy` 명령이 추가되었습니다. 자세한 정보는 [배치](/docs/openwhisk?topic=cloud-functions-deploy#deploy) 문서를 참조하십시오. 

v1.0.28(2019-01-21)
* `update|delete|get namespace name`이 여러 번 있을 경우의 오류 메시지가 추가되었습니다. 

v1.0.27(2018-12-11)
* `namespace get` 수정사항이 추가되었습니다. 
* 액션이 블랙박스 액션인 경우 `--save-as`에 대한 수정사항이 추가되었습니다. 
* 액션 작성 및 액션 업데이트 명령에 대한 `--concurrency` 플래그가 추가되었습니다. 

v1.0.26(2018-11-30)
* 새 환경에서 인증 키를 올바르게 리턴하도록 `fn property get --auth`를 사용할 수 있습니다. 

v1.0.25(2018-11-23)
* 오류 메시지 결과 화면이 개선되었습니다.
* 네임스페이스 특성을 올바르게 표시하도록 `fn namespace get` 수정사항이 추가되었습니다. 

1.0.23(2018-10-15)
* ruby(.rb) 액션 코드 인식에 대한 지원이 추가되었습니다. 

1.0.22(2018-08-20)
* 미국 동부 지역 지원이 추가되었습니다. 

1.0.21(2018-08-01)
* 별명 `fn` 및 `functions`를 이제 다음의 {{site.data.keyword.openwhisk_short}} 명령에 사용할 수 있습니다. `ibmcloud fn <command>` 및 `ibmcloud fn <command>`. 또한 다음 명령을 계속 사용할 수도 있습니다. `ibmcloud wsk <command>`.

1.0.19(2018-07-02)
* 사소한 버그 수정사항 및 개선사항.

1.0.18(2018-06-20)
* 사용자 제공 서비스 인스턴스를 바인딩 해제하기 위한 수정사항이 추가되었습니다. 
* 성능 향상.

1.0.17(2018-06-12)
* `ibmcloud cf create-user-provided-service` 명령을 사용하여 작성된 사용자 제공 서비스 인스턴스 바인드(`ibmcloud wsk service bind`) 및 바인드 해제(`ibmcloud wsk service unbind`)에 대한 지원이 추가되었습니다. 

1.0.16(2018-05-24)
* 사소한 버그 수정사항 및 개선사항.

1.0.15(2018-05-21)
* 사소한 버그 수정사항 및 개선사항.

1.0.14(2018-05-17)
* 조직 및 영역 이름에 `&` 문자 지원을 사용할 수 있습니다.

1.0.13(2018-05-07)
* 사소한 버그 수정사항 및 오류 처리 개선사항.

1.0.12(2018-04-30)
* {{site.data.keyword.Bluemix_notm}} SDK는 `bx` CLI 호환성을 유지보수하도록 업데이트됩니다.

1.0.11(2018-04-23)
* 사소한 버그 수정사항 및 개선사항.

1.0.10(2018-04-09)
* 웹 액션 엔드포인트를 보안하기 위해 새 `--web-secure` 옵션이 `ibmcloud wsk action create|update` 명령에 추가되었습니다. 
* 연이은 경로 매개변수 [결함](https://github.com/apache/incubator-openwhisk-cli/issues/237)이 수정되었습니다. 

1.0.9(2018-03-16)
* 패키지 레벨에서 서비스 바인드의 지원을 사용할 수 있습니다.

1.0.8(2018-02-22)
* IAM 서비스 바인드의 지원을 사용할 수 있습니다.

1.0.7(2018-02-02)
* `/api/{id}` 같은 경로 매개변수를 허용하도록 `ibmcloud wsk api`가 업데이트되었습니다. 관련 정보는 [API 게이트웨이](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)를 참조하십시오.
* 프록시 지원이 복원되었습니다. 
* `swift:3`이 제거되었습니다.

1.0.6(2018-01-30)
* 패키지 내 액션에 대한 `ibmcloud wsk service bind` 명령의 버그가 수정되었습니다. 
