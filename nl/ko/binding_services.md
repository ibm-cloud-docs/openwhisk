---

copyright:
  years: 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# 액션에 서비스 바인드
{: #binding_services}

[{{site.data.keyword.openwhisk}} CLI 플러그인](./bluemix_cli.html)을 활용하여 서비스를 액션 또는 패키지에 바인드할 수 있습니다. {{site.data.keyword.openwhisk_short}} `ibmcloud fn service bind` 명령을 사용하면 런타임 시에 {{site.data.keyword.Bluemix_notm}} 서비스 신임 정보를 {{site.data.keyword.openwhisk_short}} 코드에서 사용할 수 있습니다.
{: shortdesc}

`ibmcloud fn service bind` 명령을 Cloud Foundry에서 사용 가능한 `cf bind-service` 명령과 혼동하지 마십시오.
{: tip}

## 액션 또는 패키지에 서비스 바인드
{: #cli_bind}

{{site.data.keyword.openwhisk_short}}에 정의된 액션에 {{site.data.keyword.Bluemix_notm}} 서비스를 바인드하십시오. 서비스를 바인드하면 서비스 인스턴스 신임 정보가 포함된 기존 액션에서 새 매개변수가 작성됩니다.

**참고**: 액션 또는 패키지에 각 유형에 대해 하나의 서비스만 바인드할 수 있습니다. 동일한 유형의 다중 서비스 바인딩은 지원되지 않습니다.

시작하기 전에 바인드할 서비스에 대한 [신임 정보를 정의](/docs/apps/reqnsi.html#accser_external)하십시오.

1. 액션 또는 패키지에 바인드할 서비스 인스턴스의 이름을 가져오십시오.
    ```
    ibmcloud service list
    ```
    {: pre}

    출력 예:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 이전 단계에서 가져온 서비스 인스턴스에 대해 정의된 신임 정보의 이름을 가져오십시오.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    출력 예:
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. 액션에 서비스를 바인드하십시오.
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption><code>ibmcloud fn service bind</code> 명령 컴포넌트 이해하기</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> <code>ibmcloud fn service bind</code> 명령 컴포넌트 이해하기</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>바인드하는 서비스의 유형입니다.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>서비스를 바인드할 액션 또는 패키지의 이름입니다.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>선택사항: 서비스 인스턴스 이름을 지정하십시오. 서비스 인스턴스 이름을 지정하지 않으면 서비스의 첫 번째 인스턴스가 선택됩니다.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>선택사항: 신임 정보 세트 이름을 지정하십시오. 신임 정보 세트 이름을 지정하지 않으면 서비스 인스턴스의 첫 번째 신임 정보가 선택됩니다.</td>
    </tr>
    </tbody></table>

    예를 들어, {{site.data.keyword.ibmwatson}} conversation 서비스를 액션 `hello`에 바인드하려면 다음을 실행하십시오.
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. 신임 정보가 바인드되었는지 확인하십시오. 서비스가 바인드된 액션은 사용자 정의 플래그를 지원하지 않지만 디버그 및 상세 플래그는 지원합니다.
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    출력 예:
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
                    "conversation": {
                        "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    이 예제에서 conversation 서비스에 대한 신임 정보는 다른 서비스 유형에 대한 다른 신임 정보와 함께 이름이 `__bx_creds`로 지정된 매개변수에 속합니다. 액션은 `__bx_creds` 바인드 매개변수를 찾으며 나열된 서비스 유형에 대한 참조를 제거합니다. 해당 서비스 유형이 나열된 유일한 서비스 유형인 경우, 액션은 `__bx_creds` 매개변수의 값을 널로 처리하여 제외시킵니다. 둘 이상의 서비스가 액션에 바인드된 경우, `__bx_creds` 매개변수는 아직 바인드되어 있는 모든 서비스에서 그대로 유지됩니다.

액션 또는 패키지에 매개변수를 전달하는 방법 또는 `update` 오퍼레이션 중에 신임 정보가 받는 영향에 대한 자세한 정보는 [매개변수에 대한 작업](./parameters.html#pass-params-action)을 참조하십시오.


## 액션 또는 패키지에서 서비스 바인드 해제
{: #cli_unbind}

액션 또는 패키지에서 서비스를 바인드 해제하십시오. 서비스를 바인드 해제하면 `service bind` 명령으로 작성된 기존 바인딩이 제거됩니다.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
