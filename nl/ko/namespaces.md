---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# 네임스페이스 관리
{: #namespaces}

{{site.data.keyword.openwhisk}}를 사용하면 IAM(Identity and Access) 관리 네임스페이스를 작성하여 액션 또는 트리거 등의 엔티티를 함께 그룹화할 수 있습니다. 그런 다음, 네임스페이스에 대한 IAM 액세스 정책을 작성할 수 있습니다. IAM의 개요는 [{{site.data.keyword.openwhisk_short}} IAM 인에이블먼트 공지사항 블로그](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}를 참조하십시오.
{: shortdesc}

## 네임스페이스의 개념

네임스페이스는 액션 및 트리거 등의 {{site.data.keyword.openwhisk_short}} 엔티티를 포함하며, 리소스 그룹에 속합니다. 사용자에게 네임스페이스에 대한 액세스 권한을 부여하여 사용자가 엔티티에 액세스할 수 있도록 합니다.

엔티티의 완전한 이름은 `/namespaceName/packageName/entityName`입니다.

### 네임스페이스 작성 시 어떤 일이 발생합니까?

{{site.data.keyword.openwhisk_short}} 내에 작성된 네임스페이스는 IAM 서비스 인스턴스로 식별됩니다.
네임스페이스 작성 중에 서비스 인스턴스를 추가할 [리소스 그룹](/docs/resources?topic=resources-rgs)을 지정할 수 있습니다.

네임스페이스를 작성하는 경우 다음과 같은 컴포넌트가 작성됩니다.

|컴포넌트 |설명 |
| --- | --- | 
|서비스 ID |아웃바운드 호출을 작성할 때 서비스 ID를 함수 ID로 사용할 수 있습니다. 이 네임스페이스에서 작성된 모든 액션은 다른 리소스에 액세스하기 위해 이 서비스 ID를 사용할 수 있습니다. 기능 사용자는 기본적으로 독자 역할을 가져옵니다. 독자 액세스 권한은 네임스페이스 엔티티를 읽고 조치를 호출할 수 있음을 의미합니다. 독자 역할은 액션을 호출하기 위해 트리거에서 사용됩니다. 인바운드 트래픽을 제어하려면 액션을 호출하도록 독자 역할을 지정하는 것과 같은 액세스 권한을 다른 사용자에게 부여할 수 있습니다. |
|API 키 |IAM 토큰을 생성하는 데 사용할 수 있는 서비스 ID용 API 키입니다. 토큰을 사용하여 다른 {{site.data.keyword.cloud_notm}} 서비스로 네임스페이스를 인증할 수 있습니다. API 키는 환경 변수 `__OW_IAM_NAMESPACE_API_KEY`로 액션에 제공됩니다. |

모든 서비스 ID를 봅니다.
```
ibmcloud iam service-ids
```
{: pre}

서비스 ID와 연관된 API 키를 봅니다. 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

API 키를 삭제하지 마십시오.
{: tip}

### 네임스페이스에 대한 제한사항이 있습니까?

[API 게이트웨이로 API 작성](/docs/openwhisk?topic=cloud-functions-apigateway) 및 [모바일 SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk)는 IAM 관리 네임스페이스에서 지원되지 않습니다. 

액션, 트리거, 규칙, 패키지, 네임스페이스를 포함하여 모든 엔티티의 이름은 다음과 같은 형식을 따르는 일련의 문자입니다.
* 첫 번째 문자는 영숫자 문자 또는 밑줄이어야 합니다.
* 후속 문자는 영숫자, 공백 또는 `_`, `@`, `.`, `-` 값일 수 있습니다.
* 마지막 문자는 공백일 수 없습니다.

### Cloud Foundry 기반 네임스페이스가 있는 경우 어떻게 해야 합니까?

Cloud Foundry 기반 네임스페이스가 계속 작동합니다. 그러나 새 기능을 활용하려면 IAM 사용 네임스페이스를 작성해야 합니다.


## UI에서 IAM 기반 네임스페이스 작성
{: #create_iam_ui}

1. [{{site.data.keyword.openwhisk_short}} 콘솔](https://cloud.ibm.com/openwhisk){: external}에서 네임스페이스 드롭 다운 메뉴를 클릭하십시오.

2. **네임스페이스 작성**을 클릭하십시오.

3. 네임스페이스의 표시 이름 및 간략한 설명(예: 조치의 유형 또는 이 네임스페이스에 작성하려는 패키지)을 입력하십시오.

4. 네임스페이스를 작성할 리소스 그룹과 네임스페이스 리소스를 배치할 위치를 선택하십시오.

5. **작성**을 클릭하십시오.

6. 네임스페이스 리소스에 대한 서비스 인스턴스를 확인하려면 [{{site.data.keyword.cloud_notm}} 대시보드](https://cloud.ibm.com/resources){: external}로 이동한 후 **Functions 네임스페이스** 분할창에서 네임스페이스 이름을 찾으십시오.

필요한 경우 {{site.data.keyword.openwhisk_short}} 콘솔의 **네임스페이스 설정** 페이지에서 네임스페이스의 이름 또는 설명을 업데이트할 수 있습니다.

## CLI를 사용하여 IAM 기반 네임스페이스 작성
{: #namespaces_create}

리소스 그룹의 일부로 IAM 관리 네임스페이스를 작성하고 네임스페이스가 작성될 때 리소스 그룹을 대상화하여 리소스에 대한 액세스 정책을 관리할 수 있습니다. 네임스페이스에 대한 액세스를 필요로 하는 다른 사용자가 있거나 네임스페이스의 액션에서 다른 리소스에 액세스하려는 경우, 네임스페이스가 작성된 후에 IAM 정책을 설정하도록 하십시오.
{: shortdesc}

1. 네임스페이스를 작성할 리소스 그룹을 대상화하십시오. [리소스 그룹](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)을 아직 작성하지 않은 경우, `default` 그룹을 대상화할 수 있습니다.

  ```
    ibmcloud target -g default
  ```
  {: pre}

2. IAM 사용 네임스페이스를 작성하십시오. 선택사항: `-n` 또는 `--description` 플래그를 사용하여 네임스페이스에 대한 설명을 포함시키십시오. 설명이 한 단어를 초과하는 경우 따옴표로 묶어야 합니다.

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해하기</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>IAM 기반 네임스페이스의 표시 이름입니다.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>선택사항: 작성할 액션 또는 패키지의 유형 등과 같이 네임스페이스에 설명을 추가하십시오. 설명이 한 단어를 초과하는 경우 따옴표로 묶어야 합니다.</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>선택사항: 작성할 액션 또는 패키지의 유형 등과 같이 네임스페이스에 설명을 추가하십시오. 설명이 한 단어를 초과하는 경우 따옴표로 묶어야 합니다.</td>
      </tr>
    </tbody>
  </table>

  출력 예:

  ```
ok: created namespace myNamespace
  ```
  {: screen}

3. 새 네임스페이스가 작성되었는지 확인하십시오.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  출력 예:

  ```
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  IAM 기반 및 Cloud Foundry 기반 네임스페이스를 포함하여 모든 네임스페이스를 나열할 수도 있습니다.

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 네임스페이스에 엔티티를 작성하기 전에 이를 대상화하여 네임스페이스에 대한 CLI 컨텍스트를 설정해야 합니다.

  ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

`--namespace` 특성과 같은 특성을 설정한 후에는 수동으로 설정 취소할 때까지 해당 특성이 유지됩니다. IAM 네임스페이스 사이에서 전환하거나 Cloud Foundry와 IAM 사이에서 전환하려면 네임스페이스 특성을 설정 취소한 후 재설정해야 합니다. 자세한 정보는 [`ibmcloud fn property set`]를 참조하십시오.
{: note}

## API를 사용하여 네임스페이스 작성
{: #namespaces_create_api}

리소스 그룹의 일부로 IAM 관리 네임스페이스를 작성하고 네임스페이스가 작성될 때 리소스 그룹을 대상화하여 리소스에 대한 액세스 정책을 관리할 수 있습니다. 네임스페이스에 대한 액세스를 필요로 하는 다른 사용자가 있거나 네임스페이스의 액션에서 다른 리소스에 액세스하려는 경우, 네임스페이스가 작성된 후에 IAM 정책을 설정하도록 하십시오.
{: shortdesc}

1. 네임스페이스를 작성할 리소스 그룹을 대상화하십시오. [리소스 그룹](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)을 아직 작성하지 않은 경우, `default` 그룹을 대상화할 수 있습니다.

  ```
    ibmcloud target -g default
  ```
  {: pre}

2. IAM 사용 네임스페이스를 작성하십시오.

  ```
    curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 이 명령의 컴포넌트 이해하기</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>{{site.data.keyword.cloud_notm}} Identity and Access Management(IAM) 토큰입니다. IAM 토큰을 검색하려면 <code>ibmcloud iam oauth-tokens</code>를 실행하십시오.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>네임스페이스의 이름입니다.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>네임스페이스를 작성할 리소스 그룹의 ID입니다. 리소스 그룹 ID를 보려면 <code>ibmcloud resource groups</code>를 실행하십시오.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>functions-base-plan 같은 리소스 플랜의 ID입니다.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>선택사항: 포함할 액션 또는 패키지의 유형 등과 같이 네임스페이스에 설명을 추가하십시오.</td>
      </tr>
    </tbody>
  </table>

  **출력 예**

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. 새 네임스페이스가 작성되었는지 확인하십시오.

  ```
    curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
  ```
  {: pre}

  IAM 기반 및 Cloud Foundry 기반 네임스페이스를 포함하여 모든 네임스페이스를 나열할 수도 있습니다.
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  **출력 예**

  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}

HTTP REST에 대한 작업에 대해 자세히 보려면 [{{site.data.keyword.openwhisk_short}} API 문서](/apidocs/functions)의 내용을 참조하십시오.
{: tip}

### 네임스페이스에서 다른 리소스 액세스
{: #namespace-access}

액션은 일반적으로 적절한 인증이 필요한 기타 {{site.data.keyword.cloud_notm}} 리소스 및 서비스를 호출합니다. 이 서비스는 IAM 사용 서비스이며 IAM 토큰을 허용하는 경우 아웃바운드 통신을 위해 네임스페이스의 함수 ID를 활용할 수 있습니다.
{: shortdesc}

[IAM 액세스 관리](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)에 설명된 대로, 각 네임스페이스마다 네임스페이스를 나타내는 서비스 ID가 작성됩니다. IAM 정책 관리를 사용하여 적절한 역할을 지정함으로써 이 서비스 ID에 대한 기타 서비스 및 리소스에 액세스 권한을 부여할 수 있습니다. 다른 IAM 사용 서비스에 액세스하기 위해 서비스 ID를 작성하는 방법에 대한 자세한 정보는 [서비스 ID 작성 및 관련 작업](/docs/iam?topic=iam-serviceids#serviceids)을 참조하십시오.

런타임 시 {{site.data.keyword.openwhisk_short}}는 네임스페이스 서비스 ID의 API 키를 환경 변수 `__OW_IAM_NAMESPACE_API_KEY`로 액션 코드에 전달합니다. 액션 코드는 이 API 키를 사용하여 IAM 토큰을 생성할 수 있습니다. Cloudant, {{site.data.keyword.watson}} 및 {{site.data.keyword.cos_full_notm}}과 같이 지원되는 대부분의 {{site.data.keyword.openwhisk_short}} SDK는 IAM 키 자체로 인증됩니다. REST API를 사용하는 다른 IAM 관리 서비스 또는 리소스의 경우 IAM 키에서 파생된 토큰으로 인증할 수 있습니다. 자세한 정보는 [사용자 또는 서비스 ID에 대한 IAM 액세스 토큰 작성](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)을 참조하십시오.

API 키 및 토큰을 함께 맞추는 방법이 확실하지 않다면 [IAM 문서](/docs/iam?topic=iam-iamapikeysforservices)에서 자세히 알아보십시오.

## 다음 단계
{: #namespaces_next}

이제 네임스페이스가 작성되었으므로 이를 보호하는 데 도움이 되도록 IAM 액세스 정책을 작성할 수 있습니다. 시작하려면 [액세스 관리](/docs/openwhisk?topic=cloud-functions-iam)의 내용을 확인하십시오. 

IAM 기반 네임스페이스를 관리하는 방법에 대한 자세한 정보는 [{{site.data.keyword.openwhisk_short}} REST API 참조](/apidocs/functions)를 참조하십시오.








