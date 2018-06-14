---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} CLI 플러그인
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}}는 {{site.data.keyword.openwhisk_short}} 시스템의 완벽한 관리를 허용하는 강력한 {{site.data.keyword.Bluemix_notm}} CLI용 플러그인을 제공합니다.
{: shortdesc}

## {{site.data.keyword.Bluemix_notm}} CLI 설정
{: #bluemix_cli_setup}

[{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html)를 다운로드하고 설치하십시오. 

## {{site.data.keyword.openwhisk_short}} 플러그인 설정
{: #cloudfunctions_plugin_setup}

{{site.data.keyword.openwhisk_short}} 플러그인을 설치하려면 다음 명령을 실행하십시오. 
```
ic plugin install cloud-functions
```
{: pre}


{{site.data.keyword.openwhisk_short}} 플러그인 설치에 성공했는지 검증하려면 다음 명령을 실행하십시오. 
```
ic plugin list cloud-functions
```
{: pre}


설치된 {{site.data.keyword.openwhisk_short}} 버전 정보가 출력에 표시됩니다. 
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

다음 명령을 실행하여 {{site.data.keyword.openwhisk_short}} 플러그인을 업그레이드할 수 있습니다. 
```
ic plugin update Cloud-Functions
```
{: pre}


plugin 명령에 대한 자세한 정보를 보려면 `ic plugin --help`를 사용하거나 다음 문서를 확인하십시오. 
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## {{site.data.keyword.Bluemix_notm}} CLI 인증
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}}는 2개의 {{site.data.keyword.Bluemix_notm}} 지역에서 사용 가능합니다. 

{{site.data.keyword.Bluemix_notm}} CLI에 로그인하면 조직 및 영역과 함께 대상 지역에 대한 {{site.data.keyword.Bluemix_notm}} API 엔드포인트를 지정할 수 있습니다. 

미국 남부 지역에 로그인하려면 다음을 수행하십시오. 
```
ic login -a api.ng.bluemix.net
```
{: pre}


영국 지역에 로그인하려면 다음을 수행하십시오. 
```
ic login -a api.eu-gb.bluemix.net
```
{: pre}


`-a` 플래그는 사용할 {{site.data.keyword.Bluemix_notm}} API 엔드포인트를 지정합니다. API 엔드포인트가 지정되어 있으면 `-a` 옵션이 필요하지 않습니다. `ic api` 명령을 사용하여 {{site.data.keyword.Bluemix_notm}} API 엔드포인트를 명시적으로 설정할 수 있습니다. 현재 API 엔드포인트 설정을 표시하려면 `ic target` 명령을 사용하십시오. 

`login` 명령은 조직, 영역, 비밀번호(지정되지 않은 경우) 등의 정보에 대한 프롬프트를 표시합니다. 명령행에서 조직 및 영역을 지정하여 관련된 프롬프트를 표시하지 않을 수 있습니다. 
```
ic login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


{{site.data.keyword.Bluemix_notm}} API 키를 사용하여 로그인할 수도 있습니다. 이 방법은 `--sso` 플래그로 로그인하도록 요구하는 연합 로그인으로 계정이 구성된 경우에 유용합니다. API 키의 사용은 CI(Continue Integration)를 설정하고자 하며 무인 파이프라인을 구성하고자 하는 경우에도 유용합니다. 
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

{{site.data.keyword.Bluemix_notm}} CLI를 사용하여 새 API 키를 작성하려면 다음 명령을 실행하십시오. 
```
ic iam api-key-create MyKey
```
{: pre}


그리고 다음 예제에서와 같이 API 키 생성 값을 사용하여 로그인하십시오. 
```
ic login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


login 명령에 대한 자세한 정보를 보려면 `ic login --help`를 사용하거나 다음 문서를 검토하십시오. 
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## {{site.data.keyword.openwhisk_short}} CLI 플러그인 사용
{: #cloudfunctions_plugin_usage}

설정을 확인하십시오. 다음 예제에서와 같이 인수로서 `hello`를 전달하여 echo의 블로킹 (동기) 호출을 수행하십시오. 
```
ic wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


다음 예제 출력을 참조하십시오. 
```
{
    "message":"hello"
}
```

환경이 구성되면 {{site.data.keyword.openwhisk_short}} CLI를 사용하여 다음 태스크를 수행할 수 있습니다. 

* {{site.data.keyword.openwhisk_short}}에서 코드 스니펫 또는 조치를 실행합니다. [조치 작성 및 호출](./openwhisk_actions.html)을 확인하십시오. 
* 트리거 및 규칙을 사용하여 조치가 이벤트에 응답할 수 있도록 합니다. [트리거 및 규칙 작성](./openwhisk_triggers_rules.html)을 참조하십시오. 
* 패키지가 조치를 번들링하고 외부 이벤트 소스를 구성하는 방법을 알아봅니다. [패키지 작성 및 사용](./openwhisk_packages.html)을 참조하십시오. 
* 패키지의 카탈로그를 탐색하고 [Cloudant 이벤트 소스](./openwhisk_cloudant.html) 등의 외부 서비스로 애플리케이션을 개선합니다. [{{site.data.keyword.openwhisk_short}} 사용 서비스 사용](./openwhisk_catalog.html)을 참조하십시오. 

{{site.data.keyword.openwhisk_short}} 플러그인의 명령 목록를 가져오려면 인수 없이 `ic wsk`를 실행하십시오. 

## 조치에서 서비스 사용
{: #binding_services}

{{site.data.keyword.openwhisk_short}}는 실행 시간에 사용자 코드가 {{site.data.keyword.Bluemix_notm}} 서비스 신임 정보를 사용할 수 있도록 하는 `service bind` 명령을 제공합니다. 그리고 `service bind` 명령을 활용하면 {{site.data.keyword.openwhisk_short}}에서 정의된 조치에 {{site.data.keyword.Bluemix_notm}} 서비스를 바인드할 수 있습니다. 

조치에서 서비스를 사용하는 방법에 대한 세부사항은 [조치에서 서비스 사용](./binding_services.html) 주제를 참조하십시오. 

## HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI 구성
{: #cli_https_proxy}

HTTPS 프록시를 사용하도록 {{site.data.keyword.openwhisk_short}} CLI를 설정할 수 있습니다. HTTPS 프록시를 설정하려면 `HTTPS_PROXY`라고 하는 환경 변수를 작성해야 합니다. 변수는 `{PROXY IP}:{PROXY PORT}` 형식을 사용하여 HTTPS 프록시 주소 및 해당 포트로 설정되어야 합니다. 

## 지역, 조직 및 영역 관련 작업
{: #region_info}

각각 영역을 작성하여 사전-프로덕션(스테이징) 및 프로덕션 배치를 처리하기 위한 영역을 작성할 수 있습니다. 영역을 작성하면 사용자를 위해 정의된 두 개의 서로 다른 네임스페이스를 {{site.data.keyword.openwhisk_short}}에서 보유할 수 있습니다. 

`ic iam space-create`를 사용하여 "staging" 및 "production" 등의 추가 영역을 조직 아래에서 작성할 수 있습니다. 다음 예제를 참조하십시오. 
```
ic iam space-create "staging"
ic iam space-create "production"
```

`ic iam` 명령에 대한 자세한 정보는 해당 문서(https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create)를 참조하십시오. 

{{site.data.keyword.openwhisk_short}}에는 네임스페이스의 이름에 대한 제한사항이 있습니다. 이러한 제한사항에 대한 자세한 정보는 해당 문서(https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities)를 참조하십시오. 

이미 로그인되어 있는 경우에는 {{site.data.keyword.Bluemix_notm}} CLI에서 `ic target` 명령을 실행하여 지역, 조직 및 영역을 전환할 수 있습니다. 

다음 명령을 사용하여 모든 {{site.data.keyword.Bluemix_notm}} 지역을 표시하십시오. 

{{site.data.keyword.openwhisk_short}}는 `us-south` 및 `eu-gb` 지역에서만 지원됩니다.
{: tip}

```
ic regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

`ic target` 명령을 사용하여 지역을 변경하십시오. 예를 들어, 영국 지역과 `staging` 영역으로 전환하려면 다음 명령을 실행하십시오. 
```
ic target -r eu-gb -s staging
```
{: pre}


동일한 지역 내에서 영역을 변경(예: 스테이징에서 프로덕션으로)해야 하는 경우에는 다음 명령을 실행하십시오. 
```
ic target -s production
```
{: pre}


`target` 명령에 대한 자세한 정보를 보려면 `ic target --help`를 사용하거나 다음 문서를 검토하십시오. 
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## OpenWhisk CLI에서 {{site.data.keyword.openwhisk_short}} CLI 플러그인으로 마이그레이션
{: #cli_migration}

{{site.data.keyword.openwhisk_short}} CLI 플러그인이 새로 제공되므로 OpenWhisk CLI는 더 이상 필요하지 않습니다. 

### 명령 구문
{: #command_syntax}

더 이상 필요하지 않은 `wsk bluemix login` 명령을 제외한 모든 `wsk` 명령은 `ic wsk` 명령을 사용하는 방식과 동일하게 작동합니다. 모든 명령 옵션과 인수는 동일합니다. 

### API 인증 및 호스트
{: #api_authentication}

OpenWhisk CLI에서는 사용자가 인증 API 키와 API 호스트를 구성해야 합니다.
{{site.data.keyword.openwhisk_short}} CLI 플러그인을 사용하면 API 키와 API 호스트를 명시적으로 구성할 필요가 없습니다. `ic login`으로 로그인하고 `ic target` 명령을 사용하여 영역 및 네임스페이스를 대상으로 지정해야 합니다. 이후에는 모든 후속 명령이 `ic wsk`로 시작됩니다. 이러한 방식의 인증을 사용하면 CLI 환경을 구성하기 위한 특정 정보를 가져오기 위해 {{site.data.keyword.Bluemix_notm}} 웹 페이지에 액세스하지 않아도 됩니다. 

cURL 또는 Postman 등의 외부 http 클라이언트에서 사용되는 {{site.data.keyword.openwhisk_short}}용 인증 API 키를 가져와야 하는 경우에는 다음 명령으로 이를 검색할 수 있습니다. 

현재 API 키를 가져오려면 다음 명령을 실행하십시오. 
```
ic wsk property get --auth
```
{: pre}


현재 API 호스트를 가져오려면 다음 명령을 실행하십시오. 
```
ic wsk property get --apihost
```
{: pre}


API 키는 {{site.data.keyword.openwhisk_short}} CLI 플러그인에서 대상으로 지정한 지역, 조직 및 영역마다 특정합니다.
{: tip}

### API 게이트웨이 인증
{: #apigw_authentication}

현재 OpenWhisk CLI에서는 `wsk api` 명령을 사용하여 API 관리를 위한 API 게이트웨이 권한을 구성할 수 있도록 사용자가 `wsk bluemix login`을 실행해야 합니다. 

{{site.data.keyword.openwhisk}} CLI 플러그인을 사용하면 더 이상 `wsk bluemix login`을 실행할 필요가 없습니다. 대신에 `ic login` 명령을 사용하여 {{site.data.keyword.Bluemix_notm}}에 로그인하십시오. {{site.data.keyword.openwhisk}} 플러그인은 사용자의 현재 로그인 및 대상 정보를 자동으로 활용합니다. 이제 `ic wsk api` 명령을 사용하여 API를 관리할 수 있습니다. 

### 배치 스크립트 마이그레이션
{: #migrating_deploy_scripts}

`wsk` 2진이 있는 OpenWhisk CLI를 사용하는 스크립트가 있는 경우에는 모든 명령이 `ic wsk`를 사용하여 동일한 방식으로 작동됩니다. {{site.data.keyword.Bluemix_notm}} CLI 플러그인을 사용하도록 스크립트를 수정하거나 `wsk`의 현재 실행이 `ic wsk`로 변환될 수 있도록 별명 또는 랩퍼를 작성할 수 있습니다. {{site.data.keyword.Bluemix_notm}} CLI에서 `ic login` 및 `ic target` 명령은 무인 모드로 작동합니다. 무인 모드를 사용하면 {{site.data.keyword.openwhisk_short}} 엔티티를 배치하고 관리하도록 하는 `ic wsk` 명령 실행 전에 환경을 구성할 수 있습니다. 


## 버전 히스토리
{: #version_history}

강조표시 및 버그 수정을 표시하는 버전의 히스토리 레코드입니다. 

1.0.7(2018-02-02)
* `ic wsk api`에서는 이제 `/api/{id}` 등의 경로 매개변수를 허용합니다. 관련 정보는 [API 게이트웨이](./openwhisk_apigateway.html)를 참조하십시오. 
* 프록시 지원을 복원합니다. 
* `swift:3`을 제거합니다. 

1.0.6(2018-01-30)
* 패키지 내의 조치에 대한 `ic wsk service bind` 명령의 해결된 버그입니다. 
