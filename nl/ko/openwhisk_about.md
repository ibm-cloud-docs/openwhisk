---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: platform architecture, openwhisk, couchdb, kafka

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 플랫폼 아키텍처
{: #openwhisk_about}

{{site.data.keyword.openwhisk}}는 이벤트 또는 직접 호출에 대한 응답으로 코드를 실행하는 서버리스 컴퓨팅 또는 FaaS(Function as a Service)라고도 하는 이벤트 구동 컴퓨팅 플랫폼입니다.
{: shortdesc}

## {{site.data.keyword.openwhisk_short}} 기술
{: #technology}

{{site.data.keyword.openwhisk_short}}에 대한 기술의 몇 가지 기본 개념에 대해 알아보십시오.

<dl>
  <dt>액션</dt>
    <dd>[액션](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)은 하나의 특정 태스크를 수행하는 코드 조각입니다. 액션은 JavaScript 또는 Swift 코드의 소형 스니펫이거나 Docker 컨테이너에 임베드된 사용자 정의 2진 코드와 같은 선택한 언어로 작성될 수 있습니다. 액션을 소스 코드 또는 Docker 이미지로 Cloud Functions에 제공합니다.
    <br><br>액션은 {{site.data.keyword.openwhisk_short}} API, CLI 또는 iOS SDK를 사용하여 직접 호출되면 작업을 수행합니다. 또한 트리거를 사용하여 {{site.data.keyword.Bluemix_notm}} 서비스 및 서드파티 서비스에서 이벤트에 자동으로 응답할 수 있습니다.</dd>
  <dt>시퀀스</dt>
    <dd>일련의 액션은 코드를 작성하지 않고도 [시퀀스](/docs/openwhisk?topic=cloud-functions-openwhisk_create_action_sequence)에 연결될 수 있습니다. 시퀀스는 순서대로 호출되는 액션의 체인이며, 여기서 하나의 액션의 출력은 다음 액션에 입력으로 전달됩니다. 이를 통해 기존 액션을 결합하여 쉽고 빠르게 다시 사용할 수 있습니다. 그런 다음 시퀀스는 이벤트에 대한 응답으로 REST API를 통해 또는 자동으로 액션처럼 호출될 수 있습니다.
  </dd>
  <dt>이벤트</dt>
    <dd>이벤트의 예에는 데이터베이스 레코드에 대한 변경, 특정 온도를 초과하는 IoT 센서 측정값, GitHub 저장소에 대한 새 코드 커미트 또는 웹이나 모바일 앱의 단순 HTTP 요청 등이 포함됩니다. 외부 및 내부 이벤트 소스의 이벤트는 트리거를 통해 전달되며 룰은 액션이 이러한 이벤트에 대해 반응하도록 합니다.</dd>
  <dt>트리거</dt>
    <dd>[트리거](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_triggers_create)는 이벤트 클래스의 이름 지정된 채널입니다. 트리거는 사용자 또는 이벤트 소스에 의한 특정 유형의 이벤트에 반응하려는 선언입니다.</dd>
  <dt>룰</dt>
    <dd>[룰](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use)은 트리거를 액션과 연관시킵니다. 트리거가 실행될 때마다 룰은 입력으로서 트리거 이벤트를 사용하고 연관된 액션을 호출합니다. 적절한 룰 세트를 사용하면 단일 트리거 이벤트가 다중 액션을 호출하거나 액션이 다중 트리거의 이벤트에 대한 응답으로 호출될 수 있습니다.</dd>
  <dt>피드</dt>
    <dd>[피드](/docs/openwhisk?topic=cloud-functions-openwhisk_feeds#openwhisk_feeds)는 {{site.data.keyword.openwhisk_short}}에서 이용할 수 있는 트리거 이벤트를 실행하기 위한 외부 이벤트 소스를 구성하는 편리한 방법입니다 예를 들어, Git 피드는 Git 저장소에 대한 모든 커미트에 대해 트리거 이벤트를 실행할 수 있습니다.</dd>
  <dt>패키지</dt>
    <dd>서비스 및 이벤트 제공자와의 통합이 패키지에서 추가될 수 있습니다. [패키지](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)는 피드 및 액션의 번들입니다. 피드는 트리거 이벤트를 실행하기 위해 외부 이벤트 소스를 구성하는 코드 조각입니다. 예를 들어, {{site.data.keyword.cloudant}} 변경 피드로 작성된 트리거는 문서가 수정되거나 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 추가될 때마다 트리거를 실행하도록 서비스를 구성합니다. 서비스 제공자가 가용성을 제공할 수 있는 재사용 가능한 로직을 패키지의 액션에서 표시하므로, 개발자는 서비스를 이벤트 소스로서 사용하고 해당 서비스의 API를 호출할 수 있습니다.
    <br><br>패키지의 기존 카탈로그는 유용한 기능으로 애플리케이션을 강화하고 에코시스템의 외부 서비스에 액세스할 수 있는 빠른 방법을 제공합니다. {{site.data.keyword.openwhisk_short}} 패키지가 포함된 외부 서비스의 예에는 {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack 및 GitHub가 있습니다.</dd>
</dl>

## {{site.data.keyword.openwhisk_short}}의 작동 방식
{: #openwhisk_how}

모든 컴포넌트를 자세히 설명하기 위해 {{site.data.keyword.openwhisk_short}} 시스템을 통한 액션의 호출을 추적해 보겠습니다. 호출은 사용자가 시스템에 피드한 코드를 실행하고 이 실행 결과를 리턴합니다. 다음 그림은 상위 레벨 {{site.data.keyword.openwhisk_short}} 아키텍처를 보여줍니다.

![{{site.data.keyword.openwhisk_short}} 아키텍처](./images/OpenWhisk.png)


## OpenWhisk 내부 처리 작동 방식
{: #openwhisk_internal}

OpenWhisk의 백그라운드에선 어떤 일이 발생합니까?

OpenWhisk는 Nginx, Kafka, Docker 및 CouchDB를 포함한 컴포넌트를 결합하여 서버리스 이벤트 기반 프로그래밍 서비스를 구성하는 오픈 소스 프로젝트입니다.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="OpenWhisk에 대한 내부 처리 플로우" style="width:550px; border-style: none"/>

### 시스템 진입: nginx

우선 OpenWhisk의 사용자 대면 API는 완벽하게 HTTP 기반이며 RESTful 디자인을 따릅니다. 결과적으로, CLI를 통해 전송되는 명령은 OpenWhisk 시스템에 대한 HTTP 요청입니다. 특정 명령은 대략 다음으로 변환됩니다.
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

여기서 *$userNamespace* 변수에 유념하십시오. 사용자는 최소한 하나의 네임스페이스에 액세스할 수 있습니다. 단순함을 위해 사용자가 *myAction*이 놓여진 네임스페이스를 소유한다고 가정합니다.

시스템으로의 첫 시작점은 **nginx**를 통한 “HTTP 및 리버스 프록시 서버”입니다. 이는 SSL 종료 및 다음 컴포넌트로 적절한 HTTP 호출을 전달하는 데 사용됩니다.

### 시스템 진입: 제어기

Nginx는 OpenWhisk를 통한 경로의 다음 컴포넌트인 **제어기**로 HTTP 요청을 전달합니다. (**Akka** 및 **Spray**를 기반으로 하는) 실제 REST API의 Scala 기반 구현이므로, 이는 사용자가 수행할 수 있는 모든 작업에 대한 인터페이스로서의 역할을 합니다. OpenWhisk의 엔티티에 대한 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 요청 및 액션의 호출이 포함됩니다.

제어기는 우선 사용자가 시도하는 작업을 명료화합니다. 이는 HTTP 요청에서 사용되는 HTTP 메소드를 기반으로 이를 수행합니다. 위의 변환에 따라 사용자는 기존 액션에 대해 POST 요청을 발행하며, 제어기는 이를 **액션의 호출**로 변환합니다.

제어기의 중심 역할(이에 따라 이름)이 부여된 경우, 다음 단계에서 어느 정도는 이를 모두 포함합니다.

### 인증 및 권한 부여: CouchDB

이제 제어기는 사용자가 누구인지(*인증*)와 해당 엔티티에서 수행할 작업에 대한 권한이 있는지(*권한*) 여부를 확인합니다. 요청에 포함된 인증 정보에 대한 확인은 **CouchDB** 인스턴스의 이른바 **제목** 데이터베이스에 대해 이루어집니다.

이 경우에는 사용자가 OpenWhisk의 데이터베이스에 있는지와 *myAction* 액션(사용자가 소유하는 네임스페이스의 액션으로 가정됨)을 호출할 권한이 있는지가 확인됩니다. 후자는 액션을 호출하는 권한을 효과적으로 사용자에게 제공합니다.

모든 사항에 이상이 없으므로 다음 처리 단계가 시작됩니다.

### 액션 가져오기: CouchDB… (다시)

제어기가 사용자가 허용되고 액션 호출 권한을 갖고 있음을 확인했으므로 CouchDB의 **whisks** 데이터베이스에서 이 액션(이 경우에는 *myAction*)을 로드합니다.

액션의 레코드에는 실제 호출 요청에 포함된 매개변수와 병합된, 액션에 전달할 기본 매개변수 및 실행할 코드가 주로 포함되어 있습니다. 여기에는 실행 시에 부과된 리소스 제한사항(예: 이용 가능한 메모리)도 포함되어 있습니다.

이 특정한 경우에 액션은 매개변수를 사용하지 않습니다(함수의 매개변수 정의가 비어 있는 목록임). 따라서 액션에 대한 특정 매개변수를 포함하여 기본 매개변수가 설정되지 않았다고 가정되며, 이 관점에서 보면 가장 사소한 경우가 됩니다.


### 액션을 호출하는 주체: Load Balancer

제어기의 파트인 Load Balancer는 실행기의 상태를 지속적으로 확인하여 시스템에서 사용 가능한 실행기를 전체적으로 파악합니다. 이러한 실행기를 **호출기**라고 합니다. Load Balancer는 사용 가능한 호출기를 알 수 있으며, 이 중에서 하나를 선택하여 요청된 액션을 호출합니다.

### 행 구성: Kafka

이제부터, 주로 두 가지의 잘못된 일이 사용자가 전송한 호출 요청에 발생할 수 있습니다.

1. 시스템에서 장애가 발생하여 사용자의 호출이 유실됩니다.
2. 시스템에서 매우 과중한 로드가 발생할 수 있으며, 호출은 우선 다른 호출이 완료될 때까지 대기해야 합니다.

두 가지 모두에 대한 해결책은 “높은 처리량의 분산된 발행/구독 메시징 시스템”인 **Kafka**입니다. 제어기와 호출기는 오직 Kafka에 의해 버퍼링되고 지속되는 메시지를 통해서만 통신합니다. Kafka는 제어기와 호출기 모두에서 *OutOfMemoryException*의 위험성을 유발하는 메모리의 버퍼링 부담을 해소합니다. 또한 이와 동시에 시스템에서 장애가 발생하는 경우에 메시지가 유실되지 않도록 보장합니다.

호출된 액션을 가져오기 위해 제어기는 메시지를 Kafka에 공개하며, 여기에는 호출할 액션과 해당 액션에 전달할 매개변수(이 경우에는 없음)가 포함되어 있습니다. 이 메시지는 호출기에 전달되며, 이를 제어기는 Consul에서 가져온 목록에서 선택합니다.

일단 Kafka가 메시지의 수신을 확인하는 경우, 사용자에 대한 HTTP 요청이 **ActivationId**와 함께 응답됩니다. 사용자는 나중에 이를 사용하여 이 특정 호출의 결과에 액세스할 수 있습니다. 이는 시스템이 액션을 호출하는 요청을 일단 허용하면 HTTP 요청이 종료되는 비동기 호출 모델입니다. 동기 모델(블로킹 호출이라고 함)을 사용할 수 있지만, 여기서 다루지는 않습니다.

### 코드 호출: 호출기

**호출기**는 OpenWhisk의 핵심 기능입니다. 호출기의 임무는 액션을 호출하는 것입니다. 이는 Scala 언어로도 구현되어 있습니다. 그러나 여기에는 그 이상이 있습니다. 격리되고 안전한 방식으로 액션을 실행하기 위해 **Docker**를 사용합니다.

Docker를 사용하면 빠르고 격리되고 제어된 방식으로 호출되는 각 액션에 대해 자체 캡슐화된 새 환경(*컨테이너*라고 함)을 설정할 수 있습니다. 액션 호출마다 Docker 컨테이너가 생성되며 액션 코드가 삽입됩니다. 그리고 이 코드는 전달 받은 매개변수를 사용하여 실행되고 해당 결과의 가져오기가 실행된 후에 컨테이너는 영구적으로 삭제됩니다. 오버헤드를 줄이고 응답 시간을 낮출 수 있도록 성능 최적화가 이 단계에서 수행될 수 있습니다.

이 경우에는 *Node.js* 기반 액션을 바로 실행할 수 있으므로 호출기가 Node.js 컨테이너를 시작합니다. 그리고 *myAction*에서 코드를 삽입하고 매개변수 없이 이를 실행하며 해당 결과를 추출하고 로그를 저장한 후에 Node.js 컨테이너를 다시 영구 삭제합니다.

### 결과 저장: CouchDB (다시)

호출기가 결과를 받으면 이는 ActivationId 아래에서 활성화로서 **whisks** 데이터베이스에 저장됩니다. **whisks** 데이터베이스는 **CouchDB**에 상주합니다.

이 특정한 경우에, 호출기는 액션에서 다시 결과 JSON 오브젝트를 가져오고 Docker에 의해 작성된 로그를 가져오며 이 모두를 활성화 레코드에 두고 이를 데이터베이스에 저장합니다. 다음 예제를 참조하십시오.
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

리턴된 결과와 작성된 로그가 둘 다 레코드에 포함되는 방법에 유념하십시오. 여기에는 액션 호출의 시작 및 종료 시간도 포함됩니다. 활성화 레코드에는 추가 필드가 포함되지만, 단순화를 위해 이 예제에서는 이를 제거했습니다.

이제 다시 REST API를 사용하여(다시 1단계에서 시작함) 활성화 및 액션의 결과를 가져올 수 있습니다. 이를 수행하려면 다음 명령을 실행하십시오.

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### 요약

단순한 **ibmcloud fn action invoked myAction**이 {{site.data.keyword.openwhisk_short}} 시스템의 서로 다른 단계를 거치는 방법을 볼 수 있습니다. 시스템 자체는 기본적으로 2개의 사용자 정의 컴포넌트인 **제어기** 및 **호출기**로만 구성되어 있습니다. 기타 모든 컴포넌트는 오픈 소스 커뮤니티의 많은 개발자들에 의해 개발되어 이미 마련되어 있습니다. 

다음 주제에서 {{site.data.keyword.openwhisk_short}}에 대한 추가 정보를 찾을 수 있습니다.

* [엔티티 이름](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities)
* [액션 시맨틱](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_semantics)
* [한계](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)
* [REST API 참조](/apidocs/functions)
