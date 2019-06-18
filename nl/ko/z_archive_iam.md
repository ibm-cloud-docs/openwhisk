---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-10"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# 액세스 관리
{: #iam}

{{site.data.keyword.openwhisk}}에서는 IAM(Identity and Access Management)을 지원합니다. 이제 네임스페이스와 같은 리소스에 대해 IAM 정책을 정의할 수 있습니다.
{: shortdesc}

</br>

## {{site.data.keyword.openwhisk_short}}에 IAM 역할 맵핑
{: #user-roles}

{{site.data.keyword.openwhisk_short}}에서는 네임스페이스가 {{site.data.keyword.Bluemix_notm}} 리소스로 간주되며, 이를 통해 사용자는 액세스 관리를 위해 IAM 역할 및 정책을 사용하여 작업할 수 있습니다. 네임스페이스에 대해 설정하는 모든 정책은 네임스페이스가 포함하고 있는 {{site.data.keyword.openwhisk_short}} 엔티티에도 적용됩니다(예: 액션 또는 트리거).
{: shortdesc}

{{site.data.keyword.openwhisk_short}}는 플랫폼 및 서비스 관리 역할을 둘 다 사용합니다. 네임스페이스 자체와의 상호작용을 관리하기 위해 서비스 역할을 사용하는 한편, 플랫폼 레벨에서 네임스페이스를 작성할 수 있는 사용자에 대한 정책을 설정할 수 있습니다.

IAM 키 개념에 대해 자세히 알아보려면 [IAM 문서](/docs/iam?topic=iam-iamconcepts#iamconcepts)의 내용을 참조하십시오.
{: tip}

</br>

### 플랫폼 관리 역할

다음 표에서는 플랫폼 관리 역할에 맵핑되는 액션에 대해 설명합니다. 플랫폼 관리 역할을 사용하면 사용자가 플랫폼 레벨에서 서비스 리소스에 대한 태스크를 수행할 수 있습니다. 예를 들어, 서비스에 대한 사용자 액세스를 지정하고, 서비스 ID를 작성 또는 삭제하고, 인스턴스를 작성하며, 인스턴스를 애플리케이션에 바인딩하십시오.
{: shortdesc}

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
      <td>사용자가 네임스페이스를 작성할 수 있습니다.</td>
    </tr>
  </tbody>
</table>

서비스에 대한 작업을 수행하려면 플랫폼 관리를 위한 관리자 역할이 있어야 합니다. 역할에 대한 자세한 정보는 [플랫폼 관리 역할](/docs/iam?topic=iam-userroles)을 확인하십시오.

</br>

### 서비스별 역할
{: #service_specific_roles}

서비스별 역할은 특정 서비스 내의 액세스 정책 범위를 판별합니다. {{site.data.keyword.openwhisk_short}}의 경우, UI 액세스 또는 API 호출 수행 등의 서비스를 사용하기 위해 역할을 사용자 기능에 적용할 수 있습니다.
{: shortdesc}


권한이 서로에 대해 빌드됨에 유의하는 것이 중요합니다. 예를 들어 `writer` 역할을 수행할 수 있는 조작은 `manager` 역할도 수행할 수 있습니다. 그러나 `manager` 역할에는 더 많은 권한이 추가됩니다. 각 역할에 대한 일반적인 권한을 보려면 [서비스 액세스 역할](/docs/iam?topic=iam-userroles)을 확인하십시오.

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

UI에서 사용자 역할을 지정하는 방법에 대한 자세한 정보는 [IAM 액세스 관리](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)의 내용을 참조하십시오.

</br>


## 네임스페이스에 대한 IAM 액세스 정책 설정
{: #set-iam}

네임스페이스 또는 엔티티를 내부에서 관리하는 경우 위에 나열된 [서비스별 역할](#service_specific_roles)을 사용하여 다른 사용자에게 액세스 권한을 부여할 수 있습니다. 네임스페이스 작성 중에 기능 사용자 ID와 함께 네임스페이스를 나타내는 서비스 ID가 작성됩니다. 기본적으로 기능 사용자 ID에는 독자 역할이 지정됩니다. 독자는 네임스페이스 엔티티를 읽고 액션을 호출할 수 있습니다. 독자 역할은 액션을 호출하기 위해 트리거에서 사용됩니다. 인바운드 트래픽을 제어하려면 액션을 호출하도록 독자 역할을 지정하는 것과 같은 액세스 권한을 다른 사용자에게 부여할 수 있습니다.
{: shortdesc}

리소스 액세스 정책의 지정, 편집, 검토 또는 삭제 방법에 대한 정보는 [IAM 액세스 관리](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)를 참조하십시오.
{: tip}




</br>
</br>

## 네임스페이스에서 다른 리소스 액세스
{: #namespace-access}

액션은 일반적으로 적절한 인증이 필요한 기타 {{site.data.keyword.Bluemix_notm}} 리소스 및 서비스를 호출합니다.
이 서비스는 IAM 사용 서비스이며 IAM 토큰을 허용하는 경우 아웃바운드 통신을 위해 네임스페이스의 함수 ID를 활용할 수 있습니다.
[IAM 액세스 관리](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)에 설명된 대로, 각 네임스페이스마다 네임스페이스를 나타내는 서비스 ID가 작성됩니다. IAM 정책 관리를 사용하여 적절한 역할을 지정함으로써 이 서비스 ID에 대한 기타 서비스 및 리소스에 액세스 권한을 부여할 수 있습니다. 

런타임 시 {{site.data.keyword.openwhisk_short}}는 네임스페이스 서비스 ID의 API 키를 환경 변수 `__OW_IAM_NAMESPACE_API_KEY`의 값으로 액션 코드에 전달합니다. 액션 코드는 이 API 키를 사용하여 IAM 토큰을 생성할 수 있습니다. Cloudant, {{site.data.keyword.watson}} 및 {{site.data.keyword.cos_full_notm}}과 같이 지원되는 대부분의 {{site.data.keyword.openwhisk_short}} SDK는 IAM 키 자체로 인증됩니다. REST API를 사용하는 다른 IAM 관리 서비스 또는 리소스의 경우 IAM 키에서 파생된 토큰으로 인증할 수 있습니다. 자세한 정보는 [사용자 또는 서비스 ID에 대한 IAM 액세스 토큰 작성](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)을 참조하십시오.

API 키 및 토큰을 함께 맞추는 방법이 확실하지 않다면 [IAM 문서](/docs/iam?topic=iam-iamapikeysforservices)에서 자세히 알아보십시오.

