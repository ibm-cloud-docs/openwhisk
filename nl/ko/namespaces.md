---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

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



# 네임스페이스 관리
{: #namespaces}

{{site.data.keyword.openwhisk}}를 사용하면 IAM(Identity and Access) 관리 네임스페이스를 작성하여 액션 또는 트리거 등의 엔티티를 함께 그룹화할 수 있습니다. 그런 다음, 네임스페이스에 대한 IAM 액세스 정책을 작성할 수 있습니다.
{: shortdesc}


**네임스페이스의 개념**

네임스페이스는 액션 및 트리거 등의 {{site.data.keyword.openwhisk_short}} 엔티티를 포함하며, 리소스 그룹에 속합니다. 사용자에게 네임스페이스에 대한 액세스 권한을 부여하여 사용자가 엔티티에 액세스할 수 있도록 합니다. 

엔티티의 완전한 이름은
`/namespaceName/[packageName]/entityName`입니다.


**네임스페이스 작성 시 어떤 일이 발생합니까?**

{{site.data.keyword.openwhisk_short}} 내에 작성된 네임스페이스는 IAM 서비스 인스턴스로 식별됩니다.
네임스페이스 작성 중에 서비스 인스턴스를 추가할 [리소스 그룹](/docs/resources?topic=resources-rgs)을 지정할 수 있습니다. 

네임스페이스 작성 시 다음 아티팩트가 동시에 작성됩니다. 

* 아웃바운드 호출을 작성할 때 함수 ID로 사용할 수 있는 서비스 ID입니다. 이 네임스페이스에서 작성된 모든 액션은 다른 리소스에 액세스하기 위해 이 서비스 ID를 사용할 수 있습니다. 모든 서비스 ID를 보려면 `ibmcloud iam service-ids`를 실행하십시오.

* IAM 토큰을 생성하는 데 사용할 수 있는 서비스 ID용 API 키입니다. 그런 다음 토큰을 사용하여 다른 {{site.data.keyword.Bluemix_notm}} 서비스로 네임스페이스를 인증할 수 있습니다. API 키는 환경 변수로 액션에 제공됩니다.

    API 키를 삭제하지 마십시오.
    {: tip}

**네임스페이스에 대한 제한사항이 있습니까?**

[API 게이트웨이로 API 작성](/docs/openwhisk?topic=cloud-functions-apigateway) 및 [모바일 SDK](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) 사용은 IAM 관리 네임스페이스에서 지원되지 않습니다.

{{site.data.keyword.openwhisk_short}}에는 네임스페이스 이름에 대한 제한사항이 있습니다. 자세한 정보는 [시스템 세부사항 및 한계](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov) 문서를 참조하십시오.
{: tip}



**Cloud Foundry 기반 네임스페이스가 있는 경우 어떻게 해야 합니까?**

Cloud Foundry 기반 네임스페이스를 계속 사용할 수 있습니다. 그러나 새 기능을 활용하려면 [네임스페이스를 IAM으로 마이그레이션](/docs/resources?topic=resources-migrate)해야 합니다.

</br>


## CLI를 사용하여 네임스페이스 작성
{: #namespaces_create}

리소스 그룹의 일부로 IAM 관리 네임스페이스를 작성하고 네임스페이스가 작성될 때 리소스 그룹을 대상화하여 리소스에 대한 액세스 정책을 관리할 수 있습니다. 네임스페이스에 대한 액세스를 필요로 하는 다른 사용자가 있거나 네임스페이스의 액션에서 다른 리소스에 액세스하려는 경우, 네임스페이스가 작성된 후에 IAM 정책을 설정하도록 하십시오.
{: shortdesc}

1. 네임스페이스를 작성할 리소스 그룹을 대상화하십시오. [리소스 그룹](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create)을 아직 작성하지 않은 경우, `default` 그룹을 대상화할 수 있습니다.

  ```
    ibmcloud target -g default
  ```
  {: pre}

2. IAM 사용 네임스페이스를 작성하십시오.

  ```
    ibmcloud fn namespace create <namespace_name> [-n <description>]
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
        <td>선택사항: 포함할 액션 또는 패키지의 유형 등과 같이 네임스페이스에 설명을 추가하십시오.</td>
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
    Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  IAM 기반 및 Cloud Foundry 기반 네임스페이스를 포함하여 모든 네임스페이스를 나열할 수도 있습니다.

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. 네임스페이스에 엔티티를 작성하기 전에 이를 대상화하여 네임스페이스에 대한 CLI 컨텍스트를 설정하십시오.

  ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

</br>

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
        <td>{{site.data.keyword.Bluemix_notm}} Identity and Access Management(IAM) 토큰입니다. IAM 토큰을 검색하려면 <code>ibmcloud iam oauth-tokens</code>를 실행하십시오.</td>
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

  출력 예:

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

  출력 예:
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



## 다음 단계
{: #namespaces_next}

이제 네임스페이스가 작성되었으므로 이를 보호하는 데 도움이 되도록 IAM 액세스 정책을 작성할 수 있습니다. 시작하려면 [액세스 관리](/docs/openwhisk?topic=cloud-functions-iam)의 내용을 확인하십시오. IAM 기반 네임스페이스를 관리하는 방법에 대한 자세한 정보는 [{{site.data.keyword.openwhisk_short}} REST API 참조](/apidocs/functions)의 내용을 참조하십시오.


