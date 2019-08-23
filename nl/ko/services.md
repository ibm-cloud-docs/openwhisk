---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"

keywords: services, serverless, functions

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


# {{site.data.keyword.openwhisk_short}} 엔티티에 {{site.data.keyword.cloud_notm}} 서비스 바인딩
{: #services}

{{site.data.keyword.openwhisk_short}} 앱의 IBM Cloud 서비스에서 통합 기능을 사용할 수 있습니다.
{: shortdesc}

**내 앱에 IBM Cloud 서비스를 추가할 수 있는 방법은 무엇입니까?**

1. REST API 호출을 앱에 하드코딩할 수 있습니다. 이 옵션은 IBM Cloud 서비스와 통신할 수 있는 가장 빠른 방법일 수 있습니다.
2. 사전 설치되거나 설치 가능한 패키지를 사용하여 기능을 통합할 수 있습니다. 앱 코드 내의 패키지에 저장된 액션 및 피드를 사용할 수 있습니다. 이 옵션으로 코드가 약간 줄어들 수 있으며 앱이 시스템 한계에 근접한 경우 유용할 수 있습니다.


**내 앱에 액세스되어야 하는 매개변수를 설정하는 방법은 무엇입니까?**

이 매개변수에는 다른 데이터가 포함된 앱을 재사용할 수 있도록 하는 값이 포함되거나 인증 정보와 같은 서비스에 필요한 값이 포함될 수 있습니다. 
1. 매개변수를 앱에 하드코딩할 수 있습니다. 이 옵션은 인증 정보와 같은 기밀 정보를 저장하기 위한 가장 안전한 방법이 아닐 수 있습니다.
2. 매개변수를 액션 또는 패키지에 바인딩하여 매개변수를 앱에 바인딩할 수 있습니다.


## 액션 또는 패키지에 서비스 바인딩
{: #services_bind}

액션에 {{site.data.keyword.cloud_notm}} 서비스를 바인딩하십시오. 서비스를 바인딩하면 서비스 인스턴스 인증 정보가 포함된 기존 액션에서 새 매개변수가 작성됩니다.

동일한 서비스의 다중 인스턴스를 액션 또는 패키지에 바인딩할 수 없습니다. 서비스의 하나의 인스턴스만 바인딩할 수 있습니다.
{: note}

시작하기 전에 [액션을 작성](/docs/openwhisk?topic=cloud-functions-actions)하고 액션에 바인딩할 서비스에 대한 [인증 정보를 정의](/docs/resources?topic=resources-externalapp#externalapp)하십시오.

1. 액션 또는 패키지에 바인딩할 서비스 및 서비스 인스턴스의 이름을 가져오십시오. 출력 예에서 `composer`는 서비스이고 `Composer-qp`는 서비스 인스턴스 이름입니다.
    ```
    ibmcloud service list
    ```
    {: pre}

    **출력 예**
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 서비스 인스턴스에 대해 정의된 인증 정보의 이름을 가져오십시오.
    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    **예**
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    **출력 예**
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. 액션에 서비스를 바인딩하십시오. `ibmcloud fn service bind` 명령을 사용하면 런타임 시에 {{site.data.keyword.cloud_notm}} 서비스 인증 정보를 {{site.data.keyword.openwhisk_short}} 코드에서 사용할 수 있습니다. `ibmcloud fn service bind` 명령의 경우 다음과 같은 매개변수를 사용할 수 있습니다.

    <table>
    <thead>
        <tr>
        <th>매개변수</th>
        <th>설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><code>SERVICE</code></td>
        <td>바인딩하는 서비스 이름입니다.</td>
        </tr>
        <tr>
        <td><code>ACTION_NAME</code></td>
        <td>서비스를 바인딩할 액션 또는 패키지의 이름입니다.</td>
        </tr>
        <tr>
        <td><code>--instance INSTANCE_NAME</code></td>
        <td>(선택사항) 서비스 인스턴스 이름을 지정하십시오. 서비스 인스턴스 이름을 지정하지 않으면 서비스의 첫 번째 인스턴스가 선택됩니다.</td>
        </tr>
        <tr>
        <td><code>--keyname CREDENTIALS_NAME</code></td>
        <td>(선택사항) 인증 정보 이름을 지정하십시오. 인증 정보 이름을 지정하지 않으면 서비스 인스턴스의 첫 번째 인증 정보가 선택됩니다.</td>
        </tr>
    </tbody>
    </table>

    **구문 예제**
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME][--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    예를 들어, {{site.data.keyword.ibmwatson}} 작성기 서비스를 액션 `hello`에 바인딩하려면 다음 명령을 실행하십시오.
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    **출력**
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. 인증 정보가 바인딩되었는지 확인하십시오. 서비스가 바인딩된 액션은 사용자 정의 플래그를 지원하지 않지만 디버그 및 상세 플래그는 지원합니다.

    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    **출력 예**
    ```
    ok: got action Hello World
{
        "parameters": [
        {
                "key": "var1",
            "value": "val1"
            },
            {
                "key": "dog",
            "value": "cat"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    이 예제에서 작성기 서비스에 대한 인증 정보는 다른 서비스 유형에 대한 다른 인증 정보와 함께 이름이 `__bx_creds`로 지정된 매개변수에 속합니다. 액션은 `__bx_creds` 바인딩 매개변수를 찾으며 나열된 서비스 유형에 대한 참조를 제거합니다. 해당 서비스 유형이 나열된 유일한 서비스 유형인 경우, 액션은 `__bx_creds` 매개변수의 값을 널로 처리하여 제외시킵니다. 둘 이상의 서비스가 액션에 바인딩된 경우, `__bx_creds` 매개변수는 아직 바인딩되어 있는 모든 서비스에서 그대로 유지됩니다.

매개변수를 액션 또는 패키지에 전달하는 데 대한 자세한 정보는 [액션에 매개변수 바인딩](/docs/openwhisk?topic=cloud-functions-actions#actions_params)을 참조하십시오.

## 액션에서 서비스 바인딩 해제
{: #services_unbind}

액션 또는 패키지에서 서비스를 바인딩 해제하면 기존 서비스 바인딩이 제거됩니다.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

