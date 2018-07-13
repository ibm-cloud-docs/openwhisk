---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} 독립형 CLI

**2018년 3월 9일 현재, {{site.data.keyword.openwhisk_short}} 독립형 CLI는 더 이상 다운로드가 불가능합니다. 이 날짜 이후 {{site.data.keyword.openwhisk_short}} 엔티티 관리를 계속하려면 {{site.data.keyword.Bluemix}} CLI에 대해 [{{site.data.keyword.openwhisk_short}} CLI 플러그인](./bluemix_cli.html)을 사용하십시오. **

{{site.data.keyword.openwhisk}} 분배 **wsk** 명령 인터페이스를 사용하면 모든 {{site.data.keyword.openwhisk_short}} 엔티티를 관리할 수 있습니다.
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* {{site.data.keyword.openwhisk_short}} 인증 키 구성
  * 인증 키를 가져오기 위해 더 이상 {{site.data.keyword.openwhisk_short}} 콘솔에 액세스할 필요가 없음
  * 지역, 조직 및 영역 간의 전환 시에 자동 인증 키가 생성됨
* 만료된 {{site.data.keyword.openwhisk_short}} 인증 키 새로 고치기
* 후속 버전으로 CLI 업데이트


다음의 관리 태스크에서는 사용자가 플러그인을 사용해야 합니다. 

* API 관리
  * API GW 액세스 토큰의 구성
  * 만료된 API GW 액세스 토큰 새로 고치기

## {{site.data.keyword.openwhisk_short}} CLI 설정 
{: #openwhisk_cli_setup}

{{site.data.keyword.openwhisk_short}} 명령 인터페이스(CLI)를 사용하여 네임스페이스 및 권한 부여 키를 설정할 수 있습니다. [CLI 구성](https://console.bluemix.net/openwhisk/cli?loadWsk=true)으로 이동하여 지시사항에 따라 이를 설치하십시오.


우선 다음의 2개 특성을 구성하십시오. 

1. 사용하고자 하는 {{site.data.keyword.openwhisk_short}} 배치에 대한 **API 호스트**(이름 또는 IP 주소). 
2. {{site.data.keyword.openwhisk_short}} API에 대한 액세스 권한을 부여하는 **권한 부여 키**(사용자 이름 및 비밀번호). 

자체 고유의 API 호스트 및 권한 부여 키가 필요한 2개의 {{site.data.keyword.Bluemix_notm}} 지역을 사용할 수 있습니다. 

* 미국 남부
  * API 호스트: `openwhisk.ng.bluemix.net`

* 영국
  * API 호스트: `openwhisk.eu-gb.bluemix.net`

다음 명령을 실행하여 원하는 {{site.data.keyword.Bluemix_notm}} 지역에 대한 API 호스트를 설정하십시오. 

미국 남부:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

영국:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

권한 부여 키가 지역마다 특정하므로, 지역 전환이 필요한 경우에는 API 호스트 및 권한 부여 키를 둘 다 사용하여 CLI를 재구성해야 합니다.
{: tip}

패키지 및 액션, 규칙 등의 아티팩트는 지역에 특정합니다. 따라서 다중 지역에서 동일 아티팩트를 사용하는 경우에는 각각의 원하는 지역에 이를 배치해야 합니다. 

권한 부여 키를 알고 있으면 이를 사용하도록 CLI를 구성할 수 있습니다.  

다음 명령을 실행하여 권한 부여 키를 설정하십시오. 

```
wsk property set --auth <authorization_key>
```
{: pre}

**팁:** {{site.data.keyword.openwhisk_short}} CLI는 기본적으로 `~/.wskprops`에 특성 세트를 저장합니다. 이 파일의 위치는 `WSK_CONFIG_FILE` 환경 변수를 설정하여 변경될 수 있습니다.  

CLI 설정을 확인하려면 [액션 작성 및 실행](./index.html#openwhisk_start_hello_world)을 시도하십시오. 

## {{site.data.keyword.openwhisk_short}} CLI 사용
{: #using_openwhisk_cli}

일단 환경이 구성되면 {{site.data.keyword.openwhisk_short}} CLI를 사용하여 다음 태스크를 수행할 수 있습니다. 

* {{site.data.keyword.openwhisk_short}}에서 코드 스니펫 또는 액션을 실행합니다. [액션 작성 및 호출](./openwhisk_actions.html)을 참조하십시오. 
* 트리거 및 규칙을 사용하여 액션이 이벤트에 응답할 수 있도록 합니다. [트리거 및 규칙 작성](./openwhisk_triggers_rules.html)을 참조하십시오. 
* 패키지가 액션을 번들링하고 외부 이벤트 소스를 구성하는 방법을 알아봅니다. [패키지 사용 및 작성](./openwhisk_packages.html)을 참조하십시오.
* 패키지의 카탈로그를 탐색하고 [Cloudant 이벤트 소스](./openwhisk_cloudant.html) 등의 외부 서비스로 애플리케이션을 개선합니다. [사전 설치된 {{site.data.keyword.openwhisk_short}} 패키지](./openwhisk_catalog.html)를 참조하십시오. 

## HTTPS 프록시를 사용하도록 CLI 구성
{: #cli_https_proxy}

HTTPS 프록시를 사용하도록 CLI를 설정할 수 있습니다. HTTPS 프록시를 설정하려면 `HTTPS_PROXY`라고 하는 환경 변수를 작성해야 합니다. 변수는 `{PROXY IP}:{PROXY PORT}` 형식을 사용하여 HTTPS 프록시 주소 및 해당 포트로 설정되어야 합니다. 
