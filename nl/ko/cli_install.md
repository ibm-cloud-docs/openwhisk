---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# CLI 및 플러그인 설치
{: #cli_install}

{{site.data.keyword.openwhisk}}는 {{site.data.keyword.openwhisk_short}} 시스템의 완벽한 관리를 허용하는 강력한 {{site.data.keyword.cloud_notm}} CLI용 플러그인을 제공합니다. {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하여 액션에서 코드 스니펫을 관리하고, 트리거 및 규칙을 작성하여 이벤트에 응답하는 액션을 사용하도록 하고, 액션을 패키지로 번들링할 수 있습니다.
{:shortdesc}


## {{site.data.keyword.cloud_notm}} CLI 설정
{: #cli_setup}

**시작하기 전에**

[{{site.data.keyword.cloud_notm}} 계정](https://cloud.ibm.com/){: external}을 작성해야 합니다.

{{site.data.keyword.cloud_notm}} CLI를 다운로드하고 설치한 후 로그인하십시오.
{: shortdesc}

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli)를 다운로드하고 설치하십시오.

2. {{site.data.keyword.cloud_notm}} CLI에 로그인하십시오. {{site.data.keyword.cloud_notm}} 영역을 지정하려면 [API 엔드포인트를 포함](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions)시키십시오.

  ```
  ibmcloud login
  ```
  {: pre}

3. 프롬프트에 따라 {{site.data.keyword.cloud_notm}} 계정을 선택하십시오.

4. 리소스 그룹 목록을 가져오십시오. 

```
ibmcloud resource groups
```
{: pre}

**출력 예**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. 선택사항: 다음 명령을 실행하여 기본값 이외의 리소스 그룹을 대상으로 지정하십시오.
```
ibmcloud target -g <resource_group>
```
{: pre}


**출력 예**

```
Targeted resource group <resource_group>
```
{: screen}

## {{site.data.keyword.openwhisk_short}} CLI 플러그인 설정
{: #cli_plugin_setup}

{{site.data.keyword.openwhisk_short}}에 대한 작업을 수행하려면 CLI 플러그인을 다운로드하고 설치하십시오.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하여 다음 태스크를 수행할 수 있습니다.

* {{site.data.keyword.openwhisk_short}}에서 코드 스니펫 또는 액션을 실행합니다. [액션 작성 및 호출](/docs/openwhisk?topic=cloud-functions-actions)을 참조하십시오.
* 트리거 및 규칙을 작성하여 액션이 이벤트에 응답할 수 있도록 합니다. [트리거 및 룰 작성](/docs/openwhisk?topic=cloud-functions-triggers)을 참조하십시오.
* 액션을 번들링하고 외부 이벤트 소스를 구성합니다. [패키지 작성 및 사용](/docs/openwhisk?topic=cloud-functions-pkg_ov)을 참조하십시오.
* 패키지의 카탈로그를 탐색하고 외부 서비스로 애플리케이션을 개선합니다. [{{site.data.keyword.cloud_notm}} 추가](/docs/openwhisk?topic=cloud-functions-services)를 참조하십시오.

다음 단계를 완료하여 {{site.data.keyword.openwhisk_short}} CLI 플러그인을 설치하십시오.

1. {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오.

  ```
    ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. 플러그인이 설치되어 있는지 확인하십시오.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **출력**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. 로그인 이후에 모든 {{site.data.keyword.openwhisk_short}} 명령은 `ibmcloud fn`으로 시작됩니다. {{site.data.keyword.openwhisk_short}} 플러그인과 함께 수행할 수 있는 모든 작업을 보려면 인수 없이 `ibmcloud fn`을 실행하십시오.
  ```
  ibmcloud fn
  ```
  {: pre}




## {{site.data.keyword.openwhisk_short}} 네임스페이스 대상 지정
{: #cli_regions}
기본적으로 {{site.data.keyword.openwhisk_short}}는 [IAM 사용 네임스페이스](/docs/iam?topic=iam-iamoverview){: external}를 사용합니다. Cloud Foundry 기반 네임스페이스를 더 이상 작성할 수 없습니다.
{: important}

### 네임스페이스를 작성하거나 대상 지정하십시오.
{{site.data.keyword.openwhisk_short}} 네임스페이스 목록을 가져오려면 `ibmcloud fn namespace list`를 실행하십시오.

#### IAM 사용 네임스페이스를 작성하십시오.
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**응답**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### IAM 사용 네임스페이스를 대상 지정하십시오. 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**응답**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### Cloud Foundry 기반 네임스페이스를 대상 지정하십시오. 
  
`-o` 및 `-s` 플래그를 사용하여 특정 `org` 및 `space`를 대상 지정하거나 프롬프트의 내용을 수행할 수 있습니다.

* `target` 명령에 `org` 및 `space` 이름을 포함시켜 Cloud Foundy 네임스페이스를 대상 지정하십시오.

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Cloud Foundry를 대상 지정한 후 프롬프트의 내용에 따라 `org` 및 `space`를 선택하십시오.

```
ibmcloud target --cf
```
{: pre}


**응답**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### 선택사항: 스테이징 및 프로덕션 배치를 위한 네임스페이스를 작성하십시오.

각각 영역을 작성하여 사전-프로덕션(스테이징) 및 프로덕션 {{site.data.keyword.openwhisk_short}} 배치를 처리하기 위한 IAM 사용 네임스페이스를 작성할 수 있습니다. [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create)를 실행하여 "staging" 및 "production" 등의 추가 네임스페이스를 조직 아래에서 작성하십시오.

스테이징 네임스페이스를 작성하십시오.
```
ibmcloud fn namespace create staging
```
{: pre}

프로덕션 네임스페이스를 작성하십시오.
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}}에는 네임스페이스 이름에 대한 제한사항이 있습니다. 자세한 정보는 [시스템 세부사항 및 한계](/docs/openwhisk?topic=cloud-functions-limits#limits_entities) 문서를 참조하십시오.
{: tip}


## HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI 구성
{: #cli_proxy}

HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI를 설정할 수 있습니다. HTTPS 프록시를 설정하려면 `HTTPS_PROXY`라고 하는 환경 변수를 작성해야 합니다. 변수는 `{PROXY IP}:{PROXY PORT}` 형식을 사용하여 HTTPS 프록시 주소 및 해당 포트로 설정되어야 합니다.

`org` 또는 `space`의 이름을 변경하면 변경된 이름에 따라 새 네임스페이스가 작성됩니다. 이전 네임스페이스의 엔티티는 새 네임스페이스에 표시되지 않으며 삭제될 수 있습니다.
{: important}


## OpenWhisk CLI에서 {{site.data.keyword.openwhisk_short}} CLI 플러그인으로 마이그레이션
{: #cli_migrate}

이제 {{site.data.keyword.openwhisk_short}} 엔티티와 상호작용하도록 {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용할 수 있습니다. 독립형 OpenWhisk CLI를 계속해서 사용할 수 있지만, 여기에는 {{site.data.keyword.openwhisk_short}}에서 지원하는 최신 기능(예: IAM 기반 네임스페이스 및 `서비스 바인드`)이 없습니다.
{: shortdesc}


### 명령 구문
{: #cli_syntax}

Cloud Functions CLI 플러그인의 명령에 대한 모든 명령 옵션 및 인수는 [OpenWhisk 독립형 CLI](https://github.com/apache/incubator-openwhisk-cli){: external}에 대한 옵션과 동일합니다. 하지만 다음과 같은 차이점에 유의하십시오.

* {{site.data.keyword.openwhisk}} 플러그인은 사용자의 현재 로그인 및 대상 정보를 자동으로 활용합니다.
* `wsk` 명령은 이제 `ibmcloud fn`으로 실행됩니다.
* `wsk ibmcloud login` 명령은 더 이상 필요하지 않습니다. `ibmcloud login`을 사용하여 로그인할 수 있습니다.
* `ibmcloud fn api`를 사용하여 API를 관리할 수 있습니다.

자세한 정보는 [{{site.data.keyword.openwhisk_short}} CLI 참조서](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli)를 참조하십시오.

### API 인증 및 호스트
{: #cli_api_auth}

{{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 API 키와 API 호스트를 명시적으로 구성할 필요가 없습니다. 대신 `ibmcloud login`을 사용하여 로그인할 수 있습니다. `ibmcloud fn property set --namespace <namespace_name>`을 실행하여 IAM 사용 네임스페이스를 대상으로 지정하거나 `ibmcloud target --cf`를 실행하여 Cloud Foundry 기반 네임스페이스를 대상으로 지정할 수 있습니다. 로그인 이후에 모든 명령은 `ibmcloud fn`으로 시작됩니다.


cURL 또는 Postman 등의 외부 HTTP 클라이언트에서 {{site.data.keyword.openwhisk_short}}에 인증 API 키를 사용해야 하는 경우 다음 명령으로 이를 검색할 수 있습니다.

다음 명령을 실행하여 현재 API 키를 가져오십시오.
```
ibmcloud fn property get --auth
```
{: pre}

다음 명령을 실행하여 현재 API 호스트를 가져오십시오.
```
ibmcloud fn property get --apihost
```
{: pre}

API 키는 {{site.data.keyword.openwhisk_short}} CLI 플러그인에서 대상으로 지정한 지역, 조직 및 영역마다 특정합니다.
{: tip}


### API 게이트웨이 인증
{: #cli_apigw_authentication}

OpenWhisk CLI에서는 `wsk api` 명령을 사용하여 API 관리를 위한 API 게이트웨이 권한을 구성할 수 있도록 사용자가 `wsk ibmcloud login`을 실행해야 합니다. {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 더 이상 `wsk ibmcloud login`을 실행할 필요가 없습니다. 대신에 `ibmcloud login` 명령을 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하면 {{site.data.keyword.openwhisk}} 플러그인은 사용자의 현재 로그인 및 대상 정보를 자동으로 활용합니다. 이제 `ibmcloud fn api` 명령을 사용하여 API를 관리할 수 있습니다.


### 배치 스크립트 마이그레이션
{: #cli_migrating_deploy_scripts}

`wsk` 명령에서 OpenWhisk CLI를 사용하는 스크립트가 있는 경우에는 모든 명령이 `ibmcloud fn` 명령을 사용하여 동일한 방법으로 작동됩니다. {{site.data.keyword.cloud_notm}} CLI 플러그인을 사용하도록 스크립트를 수정하거나, `wsk`를 사용하는 현재 명령이 `ibmcloud fn`으로 변환될 수 있도록 별명 또는 랩퍼를 작성할 수 있습니다. {{site.data.keyword.cloud_notm}} CLI에서 `ibmcloud login` 및 `ibmcloud target` 명령은 무인 모드로 작동합니다. 무인 모드에서는 {{site.data.keyword.openwhisk_short}} 엔티티를 배치하고 관리하기 위해 `ibmcloud fn` 명령을 실행하기 전에 사용자 환경을 구성할 수 있습니다.

## CLI 버전 히스토리
{: #cli_versions}

강조표시 및 버그 수정을 표시하는 버전의 히스토리 레코드입니다.

v1.0.30(2019년 4월 3일)
* IAM 및 조직과 영역 기반의 서비스의 `service bind` 처리가 개선되었습니다.
* API 엔드포인트 https://cloud.ibm.com 처리를 위한 수정사항이 추가되었습니다.

v1.0.29(2019년 2월 6일)
* Manifest 파일을 통해 함수 엔티티의 콜렉션을 배치 또는 배치 취소하기 위한 `deploy` 및 `undeploy` 명령이 추가되었습니다. 자세한 정보는 [배치](/docs/openwhisk?topic=cloud-functions-deploy#deploy) 문서를 참조하십시오.

v1.0.28(2019년 1월 21일)
* `update|delete|get namespace name`이 여러 번 있을 경우의 오류 메시지가 추가되었습니다.

v1.0.27(2018년 12월 11일)
* `namespace get` 수정사항이 추가되었습니다.
* 액션이 블랙박스 액션인 경우 `--save-as`에 대한 수정사항이 추가되었습니다.

v1.0.26(2018년 11월 30일)
* 새 환경에서 인증 키를 올바르게 리턴하도록 `fn property get --auth`를 사용할 수 있습니다.

v1.0.25(2018년 11월 23일)
* 오류 메시지 결과 화면이 개선되었습니다.
* 네임스페이스 특성을 올바르게 표시하도록 `fn namespace get` 수정사항이 추가되었습니다.

1.0.23(2018년 10월 15일)
* ruby(`.rb`) 액션 코드 인식에 대한 지원이 추가되었습니다.

1.0.22(2018년 8월 20일)
* 미국 동부 지역 지원이 추가되었습니다.

1.0.21(2018년 8월 1일)
* 별명 `fn` 및 `functions`를 이제 다음의 {{site.data.keyword.openwhisk_short}} 명령에 사용할 수 있습니다. `ibmcloud fn <command>` 및 `ibmcloud fn <command>`. 또한 다음 명령을 계속 사용할 수도 있습니다. `ibmcloud wsk <command>`.

1.0.19(2018년 7월 2일)
* 사소한 버그 수정사항 및 개선사항.

1.0.18(2018년 6월 20일)
* 사용자 제공 서비스 인스턴스를 바인딩 해제하기 위한 수정사항이 추가되었습니다.
* 성능 향상.

1.0.17(2018년 6월 12일)
* `ibmcloud cf create-user-provided-service` 명령을 사용하여 작성된 사용자 제공 서비스 인스턴스 바인드(`ibmcloud wsk service bind`) 및 바인드 해제(`ibmcloud wsk service unbind`)에 대한 지원이 추가되었습니다.

1.0.16(2018년 5월 24일)
* 사소한 버그 수정사항 및 개선사항.

1.0.15(2018년 5월 21일)
* 사소한 버그 수정사항 및 개선사항.

1.0.14(2018년 5월 17일)
* 조직 및 영역 이름에 `&` 문자 지원을 사용할 수 있습니다.

1.0.13(2018년 5월 7일)
* 사소한 버그 수정사항 및 오류 처리 개선사항.

1.0.12(2018년 4월 30일)
* {{site.data.keyword.cloud_notm}} SDK는 `bx` CLI 호환성을 유지보수하도록 업데이트됩니다.

1.0.11(2018년 4월 23일)
* 사소한 버그 수정사항 및 개선사항.

1.0.10(2018년 4월 9일)
* 웹 액션 엔드포인트를 보안하기 위해 새 `--web-secure` 옵션이 `ibmcloud wsk action create|update` 명령에 추가되었습니다.
* 연이은 경로 매개변수 [결함](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external}이 수정되었습니다.

1.0.9(2018년 3월 16일)
* 패키지 레벨에서 서비스 바인드의 지원을 사용할 수 있습니다.

1.0.8(2018년 2월 22일)
* IAM 서비스 바인드의 지원을 사용할 수 있습니다.

1.0.7(2018년 2월 2일)
* `/api/{id}` 같은 경로 매개변수를 허용하도록 `ibmcloud wsk api`가 업데이트되었습니다. 자세한 정보는 [API 게이트웨이](/docs/openwhisk?topic=cloud-functions-apigateway)를 참조하십시오.
* 프록시 지원이 복원되었습니다.
* `swift:3`이 제거되었습니다.

1.0.6(2018년 1월 30일)
* 패키지 내 액션에 대한 `ibmcloud wsk service bind` 명령의 버그가 수정되었습니다.



