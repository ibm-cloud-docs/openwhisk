---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# 액세스 정책 설정
{: #iam}

## {{site.data.keyword.openwhisk_short}}에 IAM 역할 맵핑
{: #user-roles}

{{site.data.keyword.openwhisk_short}}에서는 네임스페이스가 액세스 관리를 위해 IAM 역할 및 정책을 사용할 수 있는 {{site.data.keyword.cloud_notm}} 리소스입니다. 네임스페이스에 대해 설정하는 모든 정책은 네임스페이스가 포함하고 있는 {{site.data.keyword.openwhisk_short}} 엔티티에도 적용됩니다(예: 액션 또는 트리거).
{: shortdesc}

{{site.data.keyword.openwhisk_short}}는 플랫폼 및 서비스 관리 역할을 둘 다 사용합니다. 플랫폼 레벨에서 네임스페이스를 작성할 수 있는 사용자에 대한 정책을 설정하고 네임스페이스 자체와의 상호작용을 관리하기 위해 서비스 역할을 사용할 수 있습니다.

IAM 키 개념에 대해 자세히 알아보려면 [IAM 문서](/docs/iam?topic=iam-iamconcepts#iamconcepts)의 내용을 참조하십시오.
{: tip}

</br>

### 플랫폼 관리 역할

다음 표에서는 플랫폼 관리 역할에 맵핑되는 액션에 대해 설명합니다. 플랫폼 관리 역할을 사용하면 사용자가 플랫폼 레벨에서 서비스 리소스에 대한 태스크를 수행할 수 있습니다. 예를 들어, 서비스에 대한 사용자 액세스를 지정하고, 서비스 ID를 작성 또는 삭제하고, 인스턴스를 작성하며, 인스턴스를 애플리케이션에 바인딩하십시오.
{: shortdesc}

리소스 액세스 정책의 지정, 편집, 검토 또는 삭제 방법에 대한 자세한 정보는 [IAM 액세스 관리](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)를 참조하십시오.
{: tip}

<table>
  <thead>
    <tr>
      <th>플랫폼 역할</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>관리자</td>
      <td>사용자가 네임스페이스를 작성할 수 있습니다. 서비스 작성 중에 `service id` 및 `apikey lock` 조작을 수행하려면 관리자 역할이 필요합니다.</td>
    </tr>
  </tbody>
</table>

서비스는 프로비저닝할 필요가 없기 때문에 서비스 관련 작업에 필요한 플랫폼 역할은 편집자 역할 뿐입니다. 기타 역할에 대한 자세한 정보는 [플랫폼 관리 역할](/docs/iam?topic=iam-userroles)을 확인하십시오.

</br>

### 서비스별 역할
{: #service_specific_roles}

서비스별 역할은 특정 서비스 내의 액세스 정책 범위를 판별합니다. {{site.data.keyword.openwhisk_short}}의 경우, UI 액세스 또는 API 호출 수행 등의 서비스를 사용하기 위해 역할을 사용자 기능에 적용할 수 있습니다.
{: shortdesc}

권한이 서로에 대해 빌드됩니다. 예를 들어 `writer` 역할을 수행할 수 있는 조작은 `manager` 역할도 수행할 수 있습니다. 그러나 `manager` 역할에는 더 많은 권한이 추가됩니다. 각 역할에 대한 일반적인 권한을 보려면 [서비스 액세스 역할](/docs/iam?topic=iam-userroles)을 확인하십시오.

각 조작을 수행하는 데 필요한 역할을 확인하려면 다음 표를 확인하십시오.

<table><caption>어떤 역할이 어떤 오퍼레이션을 수행할 수 있습니까?</caption>
  <tr>
    <th style="width:150px">액션</th>
    <th style="width:2500px">설명</th>
    <th style="width:50px">독자</th>
    <th style="width:50px">작성자</th>
    <th style="width:50px">관리자</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>네임스페이스를 업데이트합니다.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>네임스페이스를 삭제합니다.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>사용 가능한 네임스페이스를 봅니다.</td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>함수 네임스페이스 내에서 액션과 같은 엔티티를 작성합니다.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>함수 네임스페이스 내에서 패키지와 같은 엔티티를 업데이트합니다.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>함수 네임스페이스에서 트리거와 같은 엔티티를 삭제합니다.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>네임스페이스 내에서 규칙과 같은 사용 가능한 엔티티를 봅니다.</td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>네임스페이스 내에서 액션과 같은 엔티티를 활성화합니다.</td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
  </tr>
</table>

</br>

### CLI를 통해 정책 설정
{: #cli-set}

IAM 기반 네임스페이스 액세스에서 IAM 기반 서비스에 조치 등의 리소스를 제공하기 위해 리소스가 속한 네임스페이스에 대한 IAM 액세스 정책을 작성할 수 있습니다.

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>ibmcloud iam service-policy-create</code> 명령 컴포넌트 이해하기</th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>네임스페이스의 서비스 ID입니다. 모든 서비스 ID를 보려면 <code>ibmcloud iam service-ids</code>를 실행하십시오.</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>대상 서비스를 사용하기 위해 조치에서 보유해야 하는 IAM 서비스 액세스 역할의 유형입니다. 다른 서비스에 대해 지원되는 역할을 확인하려면 <code>ibmcloud iam roles --service SERVICE_NAME</code>을 실행하십시오. 자세한 정보는 [IAM 액세스 역할](/docs/iam?topic=iam-userroles#service-access-roles)을 참조하십시오.</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>기타 {{site.data.keyword.cloud_notm}} 서비스 유형의 이름입니다.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>조치에서 액세스 권한을 보유하도록 하려는 기타 서비스 인스턴스의 GUID입니다. 서비스 인스턴스 GUID를 가져오려면 <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>을 실행하십시오.</td>
    </tr>
  </tbody>
</table>

</br>

**다음 단계**
서비스 신임 정보를 관리하는 방법에 대한 자세한 정보는 [서버리스 애플리케이션에 대한 서비스 신임 정보 관리](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} 블로그를 참조하십시오.



