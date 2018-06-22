---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# 조치에서 서비스 사용
{: #binding_services}

[{{site.data.keyword.openwhisk}} CLI 플러그인](./bluemix_cli.html)을 활용하여 서비스를 조치에 바인드할 수 있습니다. {{site.data.keyword.openwhisk_short}}는 실행 시간에 Cloud Functions 코드에서 {{site.data.keyword.Bluemix}} 서비스 신임 정보를 사용할 수 있도록 `service bind` 명령을 제공합니다. `service bind` 명령을 Cloud Foundry에서 사용 가능한 `cf bind-service` 명령과 혼동하지 마십시오. 이는 단지 서비스 신임 정보가 포함된 기존 조치에서 새 매개변수를 작성하는 자동화된 방법입니다. {{site.data.keyword.openwhisk_short}} `service bind` 명령은 보다 유연성이 있으며 이를 사용하면 {{site.data.keyword.Bluemix_notm}} 서비스를 {{site.data.keyword.openwhisk_short}}에서 정의된 조치에 바인드할 수 있습니다. 유일한 제한사항은 바인드할 서비스에 대해 정의된 신임 정보가 있어야 한다는 점입니다.
{: shortdesc}

## 서비스를 조치에 바인드하는 방법
{: #cli_bind}

[{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) CLI 플러그인에서 제공하는 `ibmcloud wsk service bind` 명령을 사용하여 서비스를 조치에 바인드하십시오. 추가 정보는 [제한사항](./binding_services.html#limitations) 절에서 찾을 수 있습니다. 

`bind`를 사용한 사용법 구문:
```
ibmcloud wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

`service bind` 명령에는 서비스 유형 및 바인드할 조치 이름이 필요합니다. 예를 들어, Watson 대화 서비스를 `hello`로 이름 지정된 조치에 바인드하려는 경우에 호출은 다음 명령과 유사할 수 있습니다. 
```
ibmcloud wsk service bind conversation hello
```
{: pre}

생성되는 출력은 다음과 같습니다. 
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

이 명령은 현재 영역에서 기존의 Watson 대화 서비스를 검색하고 발견된 최초의 대화 서비스를 가져온 후에 이 서비스에 속하는 모든 신임 정보를 검색합니다. 이는 이 서비스에 속하는 첫 번째 신임 정보 세트를 사용하여 지정된 `hello` 조치에 매개변수로서 해당 신임 정보를 바인드합니다. 출력은 조치가 바인드된 서비스 및 바인드하는 데 사용된 해당 서비스의 신임 정보 세트를 정확하게 보여줍니다. 

신임 정보가 정상적으로 바인드되었는지 확인하려면 다음 명령을 실행하십시오. 
```
ibmcloud wsk action get hello parameters
```
{: pre}

샘플 출력:
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

여기에서 (기타 서비스 유형에 대한 기타 신임 정보와 함께) 이 대화 서비스의 신임 정보가 `__bx_creds`로 이름 지정된 매개변수에 속해 있음을 볼 수 있으며, 이제 이는 바인드된 기타 매개변수가 사용될 수 있는 것처럼 조치 코드 내에서 사용될 수 있습니다. 조치는 해당 서비스에서 정의된 첫 번째 신임 정보 세트가 포함된 최초로 사용 가능한 대화 서비스를 선정합니다.  

조치에 매개변수 전달 및 `action update` 조작 수행 시에 신임 정보가 영향을 받는 방법에 대한 추가 정보는 [조치 작성 및 호출](openwhisk_actions.html#openwhisk_pass_params) 문서를 참조하십시오. 

`wsk service` 명령은 다음의 2개 플래그를 지원합니다. 

<dl>
    <dt>--instance</dt>
    <dd>사용하고자 하는 유형의 특정 서비스의 이름입니다. </dd>
    <dt>--keyname</dt>
    <dd>사용하고자 하는 서비스 내의 특정 신임 정보의 이름입니다. </dd>
</dl>

이러한 플래그의 사용법을 알아보려면 다음 예제를 참조하십시오. 이전의 `ibmcloud wsk service bind` 명령을 사용함으로써 실제로 2개의 대화 서비스가 있었으며 조치 기본값이 결국 올바르지 않은 서비스/신임 정보를 바인딩했다고 가정합니다. `--instance` 및 `--keyname` 플래그로 명령을 다시 실행하여 올바른 서비스를 올바른 조치에 바인드했는지 확인하십시오. 우선은 사용 가능한 서비스와 이에 바인드된 신임 정보를 찾으십시오. 서비스를 나열하면 다음과 같은 출력이 나타나야 합니다. 

```
ibmcloud service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

이 출력에서 사용자는 나열된 2개의 서비스 중에서 **Conversation-qp**가 첫 번째 서비스이며 이는 초기 `ibmcloud wsk service bind conversation hello` 명령이 최종적으로 바인딩되는 서비스입니다. 사용자가 **Conversation-uc** 서비스에 대신 바인드하고자 할 수도 있습니다. 따라서 절대로 확실함을 보장하기 위해서는 **Conversation-uc**에 포함된 신임 정보를 검사하여 올바른 신임 정보 세트를 사용한 바인드가 이루어졌는지 확인해야 합니다. 

```
ibmcloud service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

사용자는 이 서비스에서 "Credentials-2"에 바인드하고자 합니다. 조치가 원하는 작동을 수행하도록 하려면 다음 명령을 실행하십시오. 
```
ibmcloud wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

생성되는 출력은 다음과 같습니다. 
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

출력에서 사용자는 올바른 신임 정보 세트가 조치에 바인드되었음을 볼 수 있습니다. 다시 확인하기 위해 다음의 `ibmcloud wsk action get` 명령을 살펴볼 수 있습니다. 
```
ibmcloud wsk action get hello parameters
```
{: pre}

생성되는 결과는 다음과 같습니다. 
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

일반 디버그 플래그가 지원되며 호출에서 응답 헤더를 출력합니다. 

## 조치에서 서비스를 바인드 해제하는 방법
{: #cli_unbind}

`ibmcloud wsk service unbind`를 사용하여 조치에서 서비스를 바인드 해제합니다. `service unbind` 명령은 `service bind` 명령에 의해 작성된 기존 바인딩을 제거합니다. 

`unbind`를 사용한 사용법 구문:
```
ibmcloud wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## 제한사항
{: #limitations}

`service` 조치에서 사용자 정의 플래그는 지원하지 않지만, 통상적인 디버그 및 상세 플래그는 지원합니다. 조치는 `__bx_creds` 바인드 매개변수를 찾으며 나열된 서비스 유형에 대한 참조를 제거합니다. 해당 서비스 유형이 나열된 유일한 서비스 유형인 경우, 조치는 `__bx_creds` 매개변수의 값을 널값으로 처리하여 제외시킵니다. 둘 이상의 서비스가 조치에 바인드된 경우, `__bx_creds` 매개변수는 아직 바인드되어 있는 모든 서비스에서 그대로 유지됩니다. 

각 유형의 1개 서비스만 조치에 바인드할 수 있습니다. 단일 조치 내에서 동일 유형의 다중 서비스 바인딩은 지원되지 않습니다.
{: tip}

