---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: namespaces, actions, create

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# 네임스페이스 작성
{: #openwhisk_namespaces}

Tokyo 지역에서는 {{site.data.keyword.openwhisk_short}}가 IAM(Identity and Access) 관리 네임스페이스를 사용하여 액션 또는 트리거 등의 엔티티를 함께 그룹화합니다. 그런 다음, 네임스페이스에 대한 액세스 정책을 작성할 수 있습니다.
{: shortdesc}

{{site.data.keyword.openwhisk_short}} 네임스페이스를 작성하면 이는 IAM 서비스 인스턴스로 식별됩니다. IAM 관리 서비스 인스턴스는 [리소스 그룹](/docs/resources?topic=resources-rgs) 내에서
작성되어야 합니다. 자체 리소스 그룹을 작성하거나 기본값을 대상화할 수 있습니다. 계정에 있는 IAM 서비스 인스턴스를 보려면 `ibmcloud resource service-instances`를 실행할 수 있습니다.

다음 아티팩트는 사용자의 네임스페이스와 함께 작성됩니다. 이를 삭제하지 마십시오.

* 아웃바운드 호출을 작성할 때 함수 ID로 사용할 수 있는 서비스 ID가 작성됩니다. 이 네임스페이스에서 작성된 모든 액션은 다른 리소스에 액세스하기 위해 이 서비스 ID를 사용할 수 있습니다. 모든 서비스 ID를 보려면 `ibmcloud iam service-ids`를 실행하십시오.

* API 키는 IAM 토큰을 생성하는 데 사용할 수 있는 위의 서비스 ID용으로 작성됩니다. 그런 다음 토큰을 사용하여 다른 IBM Cloud 서비스로 네임스페이스를 인증할 수 있습니다. API 키는 환경 변수로 액션에 제공됩니다.


## 제한사항
{: #limitations}

[API 게이트웨이로 API 작성](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) 및 [모바일 SDK](/docs/openwhisk?topic=cloud-functions-openwhisk_mobile_sdk) 사용은 이 경우 IAM 관리 네임스페이스에서 지원되지 않습니다.

</br>

도쿄 지역의 {{site.data.keyword.openwhisk_short}} 백엔드 서비스를 대상으로 하려면 `apihost`를 `ibmcloud fn namespace list --apihost jp-tok.functions.cloud.ibm.com` 같은 모든 CLI 호출에 추가해야 합니다. 이는 위치가 `ibmcloud target -r jp-tok`에 의해 대상화될 때까지 일시적입니다.
{: tip}



</br>
</br>


## CLI를 사용하여 네임스페이스 작성
{: #create_iam_cli}

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
          <td>새 IAM 기반 네임스페이스의 표시 이름입니다.</td>
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
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
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
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. 새 네임스페이스에 엔티티를 작성하기 전에 이를 대상화하여 네임스페이스에 대한 CLI 컨텍스트를 설정하십시오.
```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

</br>

## API를 사용하여 네임스페이스 작성
{: #create_iam_api}

리소스 그룹의 일부로 IAM 관리 네임스페이스를 작성하고 네임스페이스가 작성될 때 리소스 그룹을 대상화하여 리소스에 대한 액세스 정책을 관리할 수 있습니다. 네임스페이스에 대한 액세스를 필요로 하는 다른 사용자가 있거나 네임스페이스의 액션에서 다른 리소스에 액세스하려는 경우, 네임스페이스가 작성된 후에 IAM 정책을 설정하도록 하십시오.
{: shortdesc}



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
          <td>IBM Cloud Identity and Access Management(IAM) 토큰입니다. IAM 토큰을 검색하려면 <code>ibmcloud iam oauth-tokens</code>를 실행하십시오.</td>
        </tr>
        <tr>
          <td><code>-n &lt;name&gt;</code></td>
          <td>네임스페이스의 이름입니다. </td>
        </tr>
        <tr>
          <td><code>-n &lt;resource_group_id&gt;</code></td>
          <td>네임스페이스를 작성할 리소스 그룹의 ID입니다. 리소스 그룹 ID를 보려면 <code>ibmcloud resource groups</code>를 실행하십시오.</td>
        </tr>
        <tr>
          <td><code>-n &lt;resource_plan_id&gt;</code></td>
          <td>functions-base-plan 같은 리소스 플랜의 ID입니다. </td>
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


HTTP REST에 대한 작업에 대해 자세히 보려면 [Cloud Functions API 문서](https://cloud.ibm.com/apidocs/functions)의 내용을 참조하십시오.
{: tip}

</br>
</br>


## 다음 단계 
{: #next}

이제 네임스페이스가 작성되었으므로 이를 보호하는 데 도움이 되도록 IAM 액세스 정책을 작성할 수 있습니다. 시작하려면 [액세스 관리](/docs/openwhisk?topic=cloud-functions-iam#iam)의 내용을 확인하십시오. IAM 기반 네임스페이스를 관리하는 방법에 대한 자세한 정보는 [{{site.data.keyword.openwhisk_short}} REST API 참조](https://cloud.ibm.com/apidocs/functions)의 내용을 참조하십시오.
