---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications

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


# {{site.data.keyword.cloudaccesstrailshort}} 이벤트 보기
{: #activity_tracker}

{{site.data.keyword.cloudaccesstrailshort}} 서비스를 사용하여 {{site.data.keyword.openwhisk}} 서비스 인스턴스에서 작성한 사용자 시작 활동을 보고, 관리하고, 감사할 수 있습니다.
{: shortdesc}


서비스 작동 방법에 대한 자세한 정보는 [{{site.data.keyword.cloudaccesstrailshort}} 문서](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)를 참조하십시오.


## 이벤트를 볼 수 있는 위치
{: #view}

이벤트는 {{site.data.keyword.Bluemix_notm}} 영역에서 사용 가능한 {{site.data.keyword.cloudaccesstrailshort}} **계정 도메인**에 사용할 수 있습니다. 여기서 {{site.data.keyword.openwhisk_short}} 네임스페이스 리소스가 사용 가능합니다. 자세한 정보는 [계정 이벤트 보기](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events)의 내용을 참조하십시오.

1. {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.
2. 카탈로그에서 {{site.data.keyword.openwhisk}}의 인스턴스와 동일한 계정으로 {{site.data.keyword.cloudaccesstrailshort}} 서비스의 인스턴스를 프로비저닝하십시오.
3. {{site.data.keyword.cloudaccesstrailshort}} 대시보드의 **관리** 탭에서 **Kibana에서 보기**를 클릭하십시오.
4. 로그를 보려는 시간 프레임을 설정하십시오. 기본값은 15분입니다.
5. **사용 가능 필드** 목록에서 **유형**을 클릭하십시오. 로그를 서비스에서 추적되는 항목으로만 제한하려면 **Activity Tracker**의 돋보기 아이콘을 클릭하십시오.
6. 다른 사용 가능 필드를 사용하여 검색 범위를 좁힐 수 있습니다.

계정 소유자 이외의 사용자가 로그를 보려면 프리미엄 계획을 사용해야 합니다. 다른 사용자가 이벤트를 보게 하려면 [계정 이벤트를 볼 수 있는 권한 부여](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)의 내용을 참조하십시오.
{: tip}


## 이벤트 목록
{: #events}

다음 표에서 {{site.data.keyword.cloudaccesstrailshort}}에 전송된 이벤트 목록을 확인하십시오.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>액션</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>IAM 기반 네임스페이스 리소스 작성</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>IAM을 사용할 수 있도록 Cloud Foundry 기반 네임스페이스 마이그레이션</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>표시 이름 또는 설명과 같은 IAM 기반 네임스페이스 리소스 특성 업데이트</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>네임스페이스 리소스 삭제</td>
    </tr>
  </tbody>
</table>
