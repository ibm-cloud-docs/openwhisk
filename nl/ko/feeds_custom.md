---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: feeds, serverless

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# 사용자 정의 이벤트 제공자 피드 작성
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}}는 임의의 사용자가 이벤트 생성자 서비스를 패키지의 피드로 노출할 수 있는 오픈 API를 지원합니다.
{: shortdesc}


## 피드 아키텍처
{: #feeds_arch}

**후크**, **폴링** 및 **연결**의 피드 작성을 위한 3개의 아키텍처 패턴이 있습니다.

### 후크

후크 패턴에서 피드는 다른 서비스에 의해 노출된 [웹훅](https://en.wikipedia.org/wiki/Webhook)을 사용하여 설정됩니다. 이 전략에서 웹축은 트리거의 실행을 위해 URL로 직접 POST를 실행하는 외부 서비스에서 구성됩니다. 이 방법은 빈도가 낮은 피드의 구현을 위한 단연코 가장 손쉽고 가장 매력적인 옵션입니다.

예를 들어, [Github 패키지](/docs/openwhisk?topic=cloud-functions-pkg_github) 및 [푸시 알림 패키지](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications)에서는 웹훅을 사용합니다.


### 폴링

폴링 패턴에서 {{site.data.keyword.openwhisk_short}} 액션은 새 데이터의 페치를 위해 주기적으로 엔드포인트를 폴링하도록 배열됩니다. 이 패턴은 상대적으로 빌드가 용이하지만 이벤트의 빈도가 폴링 간격에 의해 제한됩니다.

### 연결

연결 패턴에서는 별도의 서비스가 피드 소스에 대한 지속적 연결을 유지합니다. 연결 기반 구현은 긴 폴링 간격을 사용하거나 푸시 알림을 설정하여 서비스 엔드포인트와 상호작용할 수 있습니다.

예를 들어, [{{site.data.keyword.cloudant}} 패키지](/docs/openwhisk?topic=cloud-functions-pkg_cloudant)는 연결 패턴을 사용합니다. 



##  피드 액션 구현
{: #feeds_actions}

피드 액션은 액션이며 다음 매개변수를 허용합니다. 
* **lifecycleEvent**: 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' 또는 'UNPAUSE'입니다.
* **triggerName**: 이 피드에서 생성된 이벤트가 포함된 트리거의 완전한 이름입니다.
* **authKey**: 트리거를 소유하는 {{site.data.keyword.openwhisk_short}} 사용자의 기본 권한 인증 정보입니다.

피드 액션은 피드 관리에 필요한 기타 매개변수를 허용할 수도 있습니다. 예를 들어, {{site.data.keyword.cloudant}} 변경 피드 액션에서는 `dbname` 및 `username`이 포함된 매개변수의 수신을 예상합니다. 

사용자가 `--feed` 매개변수로 CLI에서 트리거를 작성하는 경우, 피드 액션은 적합한 매개변수로 자동으로 호출됩니다.

예를 들어, 사용자가 바인딩 매개변수로서 사용자 이름과 비밀번호를 사용하여 **cloudant** 패키지에 대한 `mycloudant` 바인딩을 작성합니다. 사용자가 CLI에서 다음 명령을 실행하는 경우:
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

그런 다음, 다음과 동일한 명령이 실행됩니다. 
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

*changes*로 이름 지정된 피드 액션은 이러한 매개변수를 사용하며, {{site.data.keyword.cloudant_short_notm}}에서 이벤트 스트림의 설정에 필요한 모든 액션을 수행할 것으로 예상됩니다. 피드 액션은 트리거를 대상으로 하는 적절한 구성을 사용하여 발생합니다.

{{site.data.keyword.cloudant_short_notm}} *changes* 피드의 경우, 액션은 연결 기반 아키텍처로 구현된 *{{site.data.keyword.cloudant_short_notm}} 트리거* 서비스와 직접 대화합니다.

`ibmcloud fn trigger delete`, `ibmcloud fn trigger update` 및 `ibmcloud fn trigger get`에 대해 유사한 피드 액션 프로토콜이 발생합니다.

## 후크를 사용하여 피드 구현
{: #feeds_hooks}

이벤트 생성자가 웹훅/콜백 기능을 지원하는 경우에는 후크를 사용하여 피드를 설정합니다. 

이 방법을 사용하면 {{site.data.keyword.openwhisk_short}} 외부에 지속적 서비스를 설정할 필요가 없습니다. 모든 피드 관리는 서드파티 웹훅 API와 직접 협상하는 Stateless {{site.data.keyword.openwhisk_short}} *피드 액션*을 통해 자연스럽게 발생합니다.

`CREATE`로 호출하는 경우, 피드 액션은 단지 일부 기타 서비스에 대한 웹훅을 설치하며 {{site.data.keyword.openwhisk_short}}의 적합한 `fireTrigger` URL에 알림을 게시하도록 원격 서비스에 요청합니다.

웹훅은 다음과 같은 URL에 알림을 전송하도록 지시를 받습니다.

`POST /namespaces/{namespace}/triggers/{triggerName}`

POST 요청의 양식은 트리거 이벤트의 매개변수를 정의하는 JSON 문서로서 해석됩니다. {{site.data.keyword.openwhisk_short}} 룰은 이러한 트리거 매개변수를 이벤트의 결과로서 실행되는 임의의 액션에 전달합니다.

## 폴링을 사용하여 피드 구현
{: #feeds_polling}

지속적 연결이나 외부 서비스를 설정하지 않고도 {{site.data.keyword.openwhisk_short}} 내에서 전체 피드 소스를 폴링하도록 액션을 설정할 수 있습니다.

웹훅을 사용할 수 없지만 많은 볼륨이나 낮은 지연 응답 시간이 필요하지 않는 피드의 경우, 폴링을 사용할 수 있습니다. 

폴링 기반 피드를 설정하기 위해 피드 액션은 `CREATE`에 대한 호출 시에 다음 단계를 수행합니다.

1. 피드 액션이 `whisk.system/alarms` 피드를 사용하여 원하는 빈도로 주기적 트리거를 설정합니다.
2. 피드 개발자가 원격 서비스를 폴링하고 새 이벤트를 리턴하는 `pollMyService` 액션을 작성합니다.
3. 피드 액션이 *룰* *T -> pollMyService*를 설정합니다.

이 프로시저에서는 별도의 서비스 없이 {{site.data.keyword.openwhisk_short}} 액션을 사용하여 폴링 기반 트리거를 전체적으로 구현합니다.

## 연결을 사용하여 피드 구현
{: #feeds_connections}

이전의 2개의 아키텍처 선택사항은 단순하고 구현이 용이합니다. 그러나 고성능 피드를 원하는 경우, 지속적 연결과 장기 폴링 또는 이와 유사한 기술에 대한 대안은 없습니다.

{{site.data.keyword.openwhisk_short}} 액션이 단기 실행되어야 하므로, 액션은 서드파티에 대한 지속적 연결을 유지할 수 없습니다. 대신에, 항상 실행되는 {{site.data.keyword.openwhisk_short}} 외부에 *제공자 서비스*라고 하는 별도의 서비스를 설정할 수 있습니다. 제공자 서비스는 장기 폴링 또는 기타 연결 기반 알림을 지원하는 서드파티 이벤트 소스에 대한 연결을 유지할 수 있습니다.

제공자 서비스에는 피드 제어를 위한 {{site.data.keyword.openwhisk_short}} *피드 액션*을 허용하는 REST API가 있습니다. 제공자 서비스는 이벤트 제공자 및 {{site.data.keyword.openwhisk_short}} 간의 프록시 역할을 수행합니다. 서드파티에서 이벤트를 수신하는 경우에는 트리거를 실행하여 이를 {{site.data.keyword.openwhisk_short}}에 전송합니다.

지속적 연결 중인 {{site.data.keyword.cloudant_short_notm}} 알림과 {{site.data.keyword.openwhisk_short}} 트리거 간에 중재하는 `cloudanttrigger` 서비스를 설정하므로, {{site.data.keyword.cloudant_short_notm}} *changes* 피드는 전형적인 예제입니다.


*알람* 피드는 유사한 패턴으로 구현됩니다.

연결 기반 아키텍처는 최고 성능 옵션이지만, 폴링 및 후크 아키텍처에 비교되는 조작에서는 보다 많은 오버헤드가 부과됩니다.


