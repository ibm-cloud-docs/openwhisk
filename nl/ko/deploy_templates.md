---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# {{site.data.keyword.openwhisk_short}}에서 템플리트 배치
{: #serviceauth}

{{site.data.keyword.openwhisk}}는 다음 프로젝트에서 빠른 시작에 도움을 주는 템플리트의 카탈로그를 제공합니다. 템플리트는 조치, 트리거, 시퀀스의 조합이며 {{site.data.keyword.Bluemix}}의 서비스 인스턴스를 통합할 수도 있습니다. 템플리트를 사용하면 빠르고 손쉽게 프로젝트를 작성하고 즉시 코딩을 시작할 수 있습니다.  

이 튜토리얼은 Cloudant 템플리트 배치를 안내합니다.
{: shortdesc}

## 사용 가능한 템플리트
{: #available-templates}

| 이름 | 설명 | 지원되는 런타임 | 
|:-----------------|:-----------------|:-----------------|
| Cloudant 이벤트 | Cloudant DB에 편집되거나 추가된 문서가 있으면 콘솔에서 변경사항을 로깅합니다. | Node.js, Swift, Python, PHP |
| HTTP 리소스 가져오기 | HTTP 이벤트에 대한 응답으로 호출된 후에 Yahoo Weather API에서 데이터를 페치하는 웹 조치입니다. | Node.js, Python |
| Hello World | 이 조치는 단일 매개변수(JSON 오브젝트여야 함)를 허용합니다. | Node.js, Swift, Python, PHP |
| Message Hub 이벤트 | Message Hub 주제에 새 데이터가 추가되면 콘솔에서 변경사항을 로깅합니다. | Node.js, Swift, Python, PHP | 
| 주기적 Slack 리마인더 | 주기적 트리거를 기반으로 Slack에 게시하는 조치입니다. | Node.js, Swift, Python, PHP |

## Cloudant 이벤트 템플리트 배치
{: #cloudant-template}

Cloudant 템플리트는 조치 시퀀스 및 해당 시퀀스를 시작하는 트리거를 작성합니다. 트리거는 이름과 색상이 있는 고양이의 데이터베이스여야 하는 연결된 Cloudant DB에 변경사항이 있을 때 실행됩니다. 예상되는 데이터 항목은 이름과 색상이 정의된 고양이입니다. 새 고양이가 데이터베이스에 추가되거나 현재 고양이가 편집되면 해당 데이터가 콘솔에 로깅됩니다. 

1. 템플리트를 작성하려면 [{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}}](https://dev-console.stage1.bluemix.net/openwhisk/)로 이동한 후에 **작성 시작**을 클릭하십시오.  

2. **템플리트 배치**를 클릭하십시오. 

3. **새 Cloudant 항목**을 클릭하십시오. 

### Cloudant 조치 작성

1. 그 다음에는 패키지의 이름을 제공하거나 제공된 기본 이름 `new-cloudant-item`을 사용하십시오.  

2. **Actions** 드롭 다운 아래에서 소유할 조치에 대한 런타임을 선택하십시오(nodejs, swift, python 또는 php). 이 예제의 경우에는 **nodejs**를 선택하고 **다음**을 클릭하십시오. 

### Cloudant 트리거 작성

트리거는 이벤트 소스에서 이벤트를 수신할 때 조치를 호출합니다. Cloudant 템플리트에 대한 트리거를 작성하려면 필수 Cloudant 서비스 인스턴스 정보와 함께 트리거를 제공하십시오. 

#### Cloudant 서비스 인스턴스 작성

다음 중 하나를 선택할 수 있습니다. 
  * **자체 인스턴스 작성**
  * **자체 신임 정보 입력** 

1. 이 예제의 경우에는 **자체 인스턴스 작성**을 선택하십시오. 

2. Cloudant 설정 페이지의 새 탭으로 안내하기 위해 팝업이 열립니다. 일단 Cloudant 인스턴스가 작성되면 서비스 신임 정보 세트를 작성한 후에 **확인**을 클릭하여 탭을 닫고 이 페이지로 돌아와야 합니다. 

3. 이제 **자체 신임 정보 입력**을 선택하고 다음 정보를 제공하십시오. 
  * 사용자 이름 - _Cloudant 사용자 이름_
  * 비밀번호 - _Cloudant 비밀번호_
  * 호스트 - _일반적으로 `username.cloudant.com`_
  * 데이터베이스 - _Cloudant 데이터베이스의 이름_

### Cloudant 템플리트 배치

1. **배치**를 클릭하십시오. 

템플리트 배치 이후에는 코드를 추가로 편집하여 필요한 대로 이를 사용자 정의하거나 되돌아가서 사용 가능한 템플리트의 카탈로그를 확인할 수 있습니다. 

