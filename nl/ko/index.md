---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.openwhisk_short}} 시작하기

{{site.data.keyword.openwhisk}}는 서버리스 컴퓨팅 또는 FaaS(Function as a Service)라고도 하는 분산 및 이벤트 구동 컴퓨팅 서비스입니다. {{site.data.keyword.openwhisk_short}}는 HTTP 상에서 웹 또는 모바일 앱의 직접 호출이나 이벤트에 응답하여 애플리케이션 로직을 실행합니다. 이벤트는 Cloudant 등의 {{site.data.keyword.Bluemix}} 서비스에서 제공되거나 외부 소스에서 제공될 수 있습니다. 개발자는 애플리케이션 로직을 작성하고 요청 시에 실행되는 조치를 작성하는 데만 집중할 수 있습니다.
이 새로운 패러다임의 중요한 이점은 사용자가 명시적으로 서버를 프로비저닝하지 않는다는 점입니다. 따라서 Auto-Scaling, 고가용성, 업데이트, 유지보수, 그리고 서버가 실행되지만 요청을 처리하지 않을 때의 프로세서 시간에 대한 비용을 염려할 필요가 없습니다.
코드는 HTTP 호출, 데이터베이스 상태 변경 또는 코드 실행을 트리거하는 기타 유형의 이벤트가 있을 때 실행됩니다.
사용자는 해당 VM이 유용한 작업을 수행 중인지 여부와 무관하게 VM 활용 시간당으로 비용이 청구되지 않고 실행 시간(밀리초)에 따라 비용이 청구됩니다(최인접 100ms로 반올림됨).
{: shortdesc}

이 프로그래밍 모델은 마이크로서비스, 모바일 IoT 및 다수의 기타 앱에 완벽하게 어울립니다. 클러스터, 로드 밸런서, http 플러그인 등을 수동으로 구성하지 않아도 고유의 Auto-Scaling 및 로드 밸런싱을 즉시 이용할 수 있습니다. {{site.data.keyword.openwhisk}}에서 실행 중인 경우에는 모든 하드웨어, 네트워킹 및 소프트웨어를 IBM에서 유지보수하므로 사용자의 별도 관리가 필요하지 않다는 이점도 있습니다. 유일하게 필요한 작업은 실행할 코드를 공급하고 이를 {{site.data.keyword.openwhisk}}에 제공하는 것입니다. 나머지 작업은 마치 “마법처럼” 자동으로 처리됩니다. 서버리스 프로그래밍 모델에 대한 적절한 소개는 [Martin Fowler의 블로그](https://martinfowler.com/articles/serverless.html)에서 사용 가능합니다. 

[Apache OpenWhisk 소스 코드](https://github.com/openwhisk/openwhisk)를 가져와서 시스템을 직접 실행할 수도 있습니다. 

{{site.data.keyword.openwhisk_short}}의 작동 방법에 대한 세부사항은 [{{site.data.keyword.openwhisk_short}} 정보](./openwhisk_about.html)를 참조하십시오. 

브라우저 또는 CLI를 사용하여 {{site.data.keyword.openwhisk_short}} 애플리케이션을 개발할 수 있습니다.
둘 다 애플리케이션을 개발하기 위한 유사한 기능을 제공하지만, CLI는 배치와 운영에 대한 추가적인 제어 기능을 제공합니다. 

## 브라우저에서 개발
{: #openwhisk_start_editor}

[브라우저](https://console.{DomainName}/openwhisk/actions)에서 {{site.data.keyword.openwhisk_short}}를 사용하여 조치를 작성하고, 트리거를 사용하여 조치를 자동화하며 공용 패키지를 탐색해 보십시오. {{site.data.keyword.openwhisk_short}} 사용자 인터페이스를 빠르게 둘러보려면 [자세히 보기](https://console.{DomainName}/openwhisk/learn) 페이지를 방문하십시오. 

## CLI를 사용하여 개발
{: #openwhisk_start_configure_cli}

{{site.data.keyword.openwhisk_short}} 명령 인터페이스(CLI)를 사용하여 네임스페이스 및 권한 부여 키를 설정할 수 있습니다. [CLI 구성](https://console.{DomainName}/openwhisk/cli)으로 이동하여 지시사항에 따라 이를 설치하십시오.


## 개요
{: #openwhisk_start_overview}
- [OpenWhisk의 작동 방식](./openwhisk_about.html)
- [서버리스 애플리케이션에 대한 공통 유스 케이스](./openwhisk_use_cases.html)
- [OpenWhisk CLI 설정 및 사용](./openwhisk_cli.html)
- [iOS 앱에서 OpenWhisk 사용](./openwhisk_mobile_sdk.html)
- [기사, 샘플 및 튜토리얼](https://github.com/openwhisk/openwhisk-external-resources)
- [Apache OpenWhisk FAQ](http://openwhisk.org/faq)
- [가격 책정](https://console.ng.bluemix.net/openwhisk/learn/pricing)

## 프로그래밍 모델
{: #openwhisk_start_programming}
- [시스템 세부사항](./openwhisk_reference.html)
- [OpenWhisk 제공 서비스의 카탈로그](./openwhisk_catalog.html)
- [조치](./openwhisk_actions.html)
- [트리거 및 규칙](./openwhisk_triggers_rules.html)
- [피드](./openwhisk_feeds.html)
- [패키지](./openwhisk_packages.html)
- [어노테이션](./openwhisk_annotations.html)
- [웹 조치](./openwhisk_webactions.html)
- [API 게이트웨이](./openwhisk_apigateway.html)
- [엔티티 이름](./openwhisk_reference.html#openwhisk_entities)
- [조치 시맨틱](./openwhisk_reference.html#openwhisk_semantics)
- [한계](./openwhisk_reference.html#openwhisk_syslimits)

## {{site.data.keyword.openwhisk_short}} Hello World 예제
{: #openwhisk_start_hello_world}
{{site.data.keyword.openwhisk_short}}를 시작하려면 다음 JavaScript 코드 예제를 시도하십시오. 

```javascript
/**
 * Hello world as an OpenWhisk action.
 */
function main(params) {
    var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
}
```
{: codeblock}

이 예제를 사용하려면 다음 단계를 수행하십시오. 

1. 코드를 파일에 저장하십시오. 예: *hello.js*.

2. {{site.data.keyword.openwhisk_short}} CLI 명령행에서 다음 명령을 입력하여 조치를 작성하십시오. 
    ```
    wsk action create hello hello.js
    ```
    {: pre}

3. 그리고 다음 명령을 입력하여 조치를 호출하십시오. 
    ```
    wsk action invoke hello --blocking --result
    ```
    {: pre}  

    이 명령의 출력은 다음과 같습니다. 
    ```json
    {
        "payload": "Hello, World!"
    }
    ```
    
    ```
    wsk action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    이 명령의 출력은 다음과 같습니다. 
    ```json
    {
        "payload": "Hello, Fred!"
    }
    ```

{{site.data.keyword.openwhisk_short}}의 이벤트 구동 기능을 사용하여 이벤트에 대한 응답으로 이 조치를 호출할 수도 있습니다. [알람 서비스 예제](./openwhisk_packages.html#openwhisk_package_trigger)에 따라 주기적 이벤트가 생성될 때마다 `hello` 조치를 호출하도록 이벤트 소스를 구성하십시오. 

OpenWhisk 튜토리얼 및 샘플의 전체 목록은 [여기](https://github.com/openwhisk/openwhisk-external-resources#sample-applications)서 찾을 수 있습니다. 샘플과 더불어, 이 저장소에는 기사, 프리젠테이션, 팟캐스트, 동영상 및 기타 {{site.data.keyword.openwhisk_short}} 관련 리소스에 대한 링크가 포함되어 있습니다. 

## API 참조서
{: #openwhisk_start_api notoc}
* [REST API 문서](./openwhisk_reference.html#openwhisk_ref_restapi)
* [REST API](https://console.{DomainName}/apidocs/98)

## 관련 링크
{: #general notoc}
* [검색: {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
* [IBM developerWorks의 {{site.data.keyword.openwhisk_short}}](https://developer.ibm.com/openwhisk/)
* [Apache {{site.data.keyword.openwhisk_short}} 프로젝트 웹 사이트](http://openwhisk.org)
