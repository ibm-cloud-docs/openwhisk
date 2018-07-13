---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI 플러그인 설정
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}}는 {{site.data.keyword.openwhisk_short}} 시스템의 완벽한 관리를 허용하는 강력한 {{site.data.keyword.Bluemix_notm}} CLI용 플러그인을 제공합니다.
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}} CLI 설정
{: #bluemix_cli_setup}

{{site.data.keyword.Bluemix_notm}} CLI를 다운로드하고 설치한 후 로그인하십시오.
{: shortdesc}

1. [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)를 다운로드하고 설치하십시오.

2. {{site.data.keyword.Bluemix_notm}} CLI에 로그인하십시오. {{site.data.keyword.openwhisk_short}}는 미국 남부 및 영국 {{site.data.keyword.Bluemix_notm}} 지역에서 사용 가능합니다. {{site.data.keyword.Bluemix_notm}} API 엔드포인트가 지정되지 않은 경우 `-a` 플래그를 사용하여 지정하십시오.

    * 미국 남부 지역에 로그인하려면 다음을 수행하십시오.
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * 영국 지역에 로그인하려면 다음을 수행하십시오.
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  `ibmcloud api` 명령을 사용하여 명시적으로 {{site.data.keyword.Bluemix_notm}} API 엔드포인트를 설정할 수 있습니다. 현재 API 엔드포인트 설정을 표시하려면 `ibmcloud target` 명령을 사용하십시오.
  {: tip}

3. `ibmcloud login` 명령은 조직, 영역, 비밀번호(지정되지 않은 경우) 등의 정보에 대한 프롬프트를 표시합니다.

  로그인하여 프롬프트를 건너뛸 때 조직 및 영역을 지정할 수 있습니다. 다음 플래그를 사용하십시오. `ibmcloud login -o <ORG> -s <SPACE>`.
  {: tip}

{{site.data.keyword.Bluemix_notm}} API 키를 사용하여 로그인할 수도 있습니다. 이 방법은 `--sso` 플래그로 로그인하도록 요구하는 연합 로그인으로 계정이 구성된 경우에 유용합니다. [API 키 사용](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key)은 CI(Continuous Integration)를 설정하고 무인 파이프라인을 구성하려는 경우에도 유용합니다.

1. 새 API 키를 작성하십시오.
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. API 키의 생성된 값을 사용하여 로그인하십시오.
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
`ibmcloud login` 명령에 대한 자세한 정보는 `ibmcloud login --help`를 사용하거나 [IBM Cloud (bx) 명령](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login) 주제를 검토하십시오.

## {{site.data.keyword.openwhisk_short}} 플러그인 설정
{: #cloudfunctions_plugin_setup}

{{site.data.keyword.openwhisk_short}} 플러그인을 다운로드하고 설치하십시오.
{: shortdesc}

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

3. `echo`의 블로킹(동기) 호출을 수행하십시오. `hello`를 인수로 전달하십시오.
    ```
    ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. `hello` 메시지가 출력에 리턴되었는지 확인하십시오.
    출력 예:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. 다음 명령을 실행하여 {{site.data.keyword.openwhisk_short}} 플러그인을 업그레이드할 수 있습니다.
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

{{site.data.keyword.openwhisk_short}} CLI를 사용하여 다음을 수행할 수 있습니다.

* {{site.data.keyword.openwhisk_short}}에서 코드 스니펫 또는 액션을 실행합니다. [액션 작성 및 호출](./openwhisk_actions.html)을 참조하십시오.
* 트리거 및 규칙을 사용하여 액션이 이벤트에 응답할 수 있도록 합니다. [트리거 및 룰 작성](./openwhisk_triggers_rules.html)을 참조하십시오.
* 패키지가 액션을 번들링하고 외부 이벤트 소스를 구성하는 방법을 알아봅니다. [패키지 작성 및 사용](./openwhisk_packages.html)을 참조하십시오.
* 패키지의 카탈로그를 탐색하고 [{{site.data.keyword.cloudant}} 이벤트 소스](./openwhisk_cloudant.html) 등의 외부 서비스로 애플리케이션을 개선합니다.

{{site.data.keyword.openwhisk_short}} 플러그인의 명령을 나열하려면 인수 없이 `ibmcloud wsk`를 실행하십시오.
{: tip}

## 액션에서 서비스 사용
{: #binding_services}

{{site.data.keyword.openwhisk_short}}는 실행 시간에 사용자 코드가 {{site.data.keyword.Bluemix_notm}} 서비스 신임 정보를 사용할 수 있도록 하는 `service bind` 명령을 제공합니다. 그런 다음 `service bind` 명령을 사용하여 {{site.data.keyword.openwhisk_short}}에 정의된 액션에 {{site.data.keyword.Bluemix_notm}} 서비스를 바인드할 수 있습니다.

액션에서 서비스를 사용하는 방법에 대한 자세한 단계는 [액션에 서비스 바인드](./binding_services.html)를 참조하십시오.

## HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI 구성
{: #cli_https_proxy}

HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI를 설정할 수 있습니다. HTTPS 프록시를 설정하려면 `HTTPS_PROXY`라고 하는 환경 변수를 작성해야 합니다. 변수는 `{PROXY IP}:{PROXY PORT}` 형식을 사용하여 HTTPS 프록시 주소 및 해당 포트로 설정되어야 합니다.

## 여러 지역, 조직 및 영역으로 전환
{: #region_info}

이미 로그인되어 있는 경우에는 {{site.data.keyword.Bluemix_notm}} CLI에서 `ibmcloud target` 명령을 실행하여 지역, 조직 및 영역을 전환할 수 있습니다.

{{site.data.keyword.openwhisk_short}}는 미국 남부 및 영국 {{site.data.keyword.Bluemix_notm}} 지역에서 사용 가능합니다. 지역을 변경하려면 `ibmcloud target` 명령을 사용하십시오. 예를 들어, 영국 지역과 이 지역의 `staging` 영역으로 전환하려면 다음을 실행하십시오.
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

각각 영역을 작성하여 사전-프로덕션(스테이징) 및 프로덕션 배치를 처리하기 위한 영역을 작성할 수 있습니다. 영역을 작성하면 사용자를 위해 정의된 두 개의 서로 다른 네임스페이스를 {{site.data.keyword.openwhisk_short}}에서 보유할 수 있습니다. [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create)를 실행하여 "staging" 및 "production" 등의 추가 영역을 조직 아래에서 작성하십시오.

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}}에는 네임스페이스 이름에 대한 제한사항이 있습니다. 자세한 정보는 [시스템 세부사항 및 한계](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities) 문서를 참조하십시오.
{: tip}

## OpenWhisk CLI에서 {{site.data.keyword.openwhisk_short}} CLI 플러그인으로 마이그레이션
{: #cli_migration}

{{site.data.keyword.openwhisk_short}} CLI 플러그인이 새로 제공되므로 OpenWhisk 독립형 CLI는 더 이상 필요하지 않습니다.

### 명령 구문
{: #command_syntax}

더 이상 필요하지 않은 `wsk bluemix login` 명령을 제외한 모든 `wsk` 명령은 `ibmcloud wsk` 명령을 사용하는 방식과 동일하게 작동합니다. 모든 명령 옵션과 인수는 동일합니다.

### API 인증 및 호스트
{: #api_authentication}

OpenWhisk CLI에서는 사용자가 인증 API 키와 API 호스트를 구성해야 합니다.
{{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 API 키와 API 호스트를 명시적으로 구성할 필요가 없습니다. 대신 `ibmcloud login`을 사용하여 로그인하고 `ibmcloud target` 명령을 사용하여 지역 및 네임스페이스를 대상으로 지정할 수 있습니다. 로그인하면 모든 명령이 `ibmcloud wsk`로 시작됩니다.

cURL 또는 Postman 등의 외부 HTTP 클라이언트에서 {{site.data.keyword.openwhisk_short}}에 인증 API 키를 사용해야 하는 경우 다음 명령으로 이를 검색할 수 있습니다.

현재 API 키를 가져오려면 다음을 실행하십시오.
```
ibmcloud wsk property get --auth
```
{: pre}

현재 API 호스트를 가져오려면 다음을 실행하십시오.
```
ibmcloud wsk property get --apihost
```
{: pre}

API 키는 {{site.data.keyword.openwhisk_short}} CLI 플러그인에서 대상으로 지정한 지역, 조직 및 영역마다 특정합니다.
{: tip}

### API 게이트웨이 인증
{: #apigw_authentication}

OpenWhisk CLI에서는 `wsk api` 명령을 사용하여 API 관리를 위한 API 게이트웨이 권한을 구성할 수 있도록 사용자가 `wsk bluemix login`을 실행해야 합니다. {{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 더 이상 `wsk bluemix login`을 실행할 필요가 없습니다. 대신에 `ibmcloud login` 명령을 사용하여 {{site.data.keyword.Bluemix_notm}}에 로그인하면 {{site.data.keyword.openwhisk}} 플러그인은 사용자의 현재 로그인 및 대상 정보를 자동으로 활용합니다. 이제 `ibmcloud wsk api` 명령을 사용하여 API를 관리할 수 있습니다.

### 배치 스크립트 마이그레이션
{: #migrating_deploy_scripts}

`wsk` 2진이 있는 OpenWhisk CLI를 사용하는 스크립트가 있는 경우 모든 명령이 `ibmcloud wsk` 명령을 사용하여 동일한 방식으로 작동됩니다. {{site.data.keyword.Bluemix_notm}} CLI 플러그인을 사용하도록 스크립트를 수정하거나 `wsk`를 사용하는 현재 명령이 `ibmcloud wsk`로 변환될 수 있도록 별명 또는 랩퍼를 작성할 수 있습니다. {{site.data.keyword.Bluemix_notm}} CLI에서 `ibmcloud login` 및 `ibmcloud target` 명령은 무인 모드로 작동합니다. 무인 모드를 사용하면 {{site.data.keyword.openwhisk_short}} 엔티티를 배치하고 관리하도록 `ibmcloud wsk` 명령 실행 전에 사용자 환경을 구성할 수 있습니다.

## 버전 히스토리
{: #version_history}

강조표시 및 버그 수정을 표시하는 버전의 히스토리 레코드입니다.

1.0.18(2018-06-20)
* 사용자 제공 서비스 인스턴스 바인드 해제를 위한 수정사항.
* 성능 향상.

1.0.17(2018-06-12)
* `ibmcloud cf create-user-provided-service` 명령을 사용하여 작성된 사용자 제공 서비스 인스턴스 바인드(`ibmcloud wsk service bind`) 및 바인드 해제(`ibmcloud wsk service unbind`)에 대한 지원을 추가합니다.

1.0.16(2018-05-24)
* 사소한 버그 수정사항 및 개선사항.

1.0.15(2018-05-21)
* 사소한 버그 수정사항 및 개선사항.

1.0.14(2018-05-17)
* 조직 및 영역 이름에 `&` 문자 지원을 사용합니다.

1.0.13(2018-05-07)
* 사소한 버그 수정사항 및 오류 처리 개선사항.

1.0.12(2018-04-30)
* {{site.data.keyword.Bluemix_notm}} SDK는 `bx` CLI 호환성을 유지보수하도록 업데이트됩니다.

1.0.11(2018-04-23)
* 사소한 버그 수정사항 및 개선사항.

1.0.10(2018-04-09)
* 새 `--web-secure` 옵션을 `ibmcloud wsk action create|update` 명령에 추가하여 웹 액션 엔드포인트를 보안합니다.
* 연이은 경로 매개변수 [결함](https://github.com/apache/incubator-openwhisk-cli/issues/237)을 수정합니다.

1.0.9(2018-03-16)
* 패키지 레벨에서 서비스 바인드의 지원을 사용합니다.

1.0.8(2018-02-22)
* IAM 서비스 바인드의 지원을 사용합니다.

1.0.7(2018-02-02)
* `ibmcloud wsk api`는 이제 `/api/{id}` 등의 경로 매개변수를 허용합니다. 관련 정보는 [API 게이트웨이](./openwhisk_apigateway.html)를 참조하십시오.
* 프록시 지원을 복원합니다.
* `swift:3`을 제거합니다.

1.0.6(2018-01-30)
* 패키지 내 액션에 대한 `ibmcloud wsk service bind` 명령의 버그 수정사항입니다.
