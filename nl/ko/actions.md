---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, functions, serverless, javascript, node, node.js

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



# 액션 작성
{: #actions}

JSON 오브젝트를 리턴하는 최상위 레벨 함수인 액션을 작성하십시오. 액션의 관리를 간소화하도록 액션을 패키지로 결합할 수 있습니다.
{: shortdesc}

시작하기 전에:
액션을 작성하려면 소스 코드에는 특정 요구사항이 충족되어야 합니다. 예를 들어, 다중 파일에 포함된 코드에서 액션을 작성하려는 경우 액션을 작성하기 전에 단일 파일로 코드를 패키징하십시오. 각 런타임의 요구사항에 대한 자세한 정보는 [서버리스용 앱 코드 준비](/docs/openwhisk?topic=cloud-functions-prep)를 참조하십시오.


## CLI에서 액션 작성
{: #actions_cli}

1. 액션을 작성하십시오.
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  **예**
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  **출력 예**

  ```
  ok: created action hello
  ```
  {: screen}

  팁:
  - 비용을 절약하기 위해 한계를 설정할 수 있습니다.
      - 메모리 사용량에 대한 한계를 설정하려면 작성 명령에 `--memory VALUE`를 포함하십시오. 여기서, 값의 단위는 MB입니다.
      - 제한시간을 설정하려면 작성 명령에 `--timeout VALUE`를 포함하십시오. 여기서, 값의 단위는 밀리초입니다.
  - Docker 이미지로 코드를 패키징한 경우 앱에 대한 로컬 경로 및 --kind 플래그 대신 작성 명령에 `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG`를 포함하십시오. 가능할 때마다 `latest` 태그를 사용하지 않고 이미지를 잘 관리하십시오. `latest` 태그가 사용되는 경우 해당 태그와 함께 이미지가 사용됩니다. 이 이미지는 항상 최근에 작성된 이미지가 아닐 수도 있습니다.

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  
2. 액션이 액션 목록에 있는지 확인하십시오.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  **출력 예**

  ```
  actions
  hello       private
  ```
  {: screen}


## 액션에서 앱 또는 런타임 업데이트
{: #actions_update}

앱에서 코드를 업데이트하거나 런타임의 최신 버전으로 마이그레이션해야 할 때마다 업데이트 명령을 실행할 수 있습니다. 예를 들어 Node.js 버전 8은 유지보수 모드이므로 런타임을 Node.js 10으로 전환할 수 있습니다.

새 런타임 버전으로 마이그레이션하는 경우 새 런타임 버전을 따르도록 앱에서 코드를 변경해야 할 수 있습니다. 대부분의 경우 런타임 버전이 호환 가능합니다.
{: tip}

1. 앱을 로컬로 업데이트하십시오.

2. Docker 이미지로 앱을 패키징한 경우 최신 이미지를 Docker 허브에 업로드하십시오. 그러면 다음 번에 액션에 대한 코드를 실행할 때 시스템이 새 Docker 이미지를 가져올 수 있습니다. 이전 버전의 Docker 이미지를 사용하는 실행 중인 컨테이너가 있는 경우, 새 호출은 해당 이미지를 계속 사용합니다. 새 호출이 새 이미지에서 실행을 시작하도록 업데이트 명령을 실행해야 합니다.

3. 액션을 업데이트하고 앱 또는 Docke 이미지에 대한 로컬 경로를 포함하십시오.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    **예**

    ```
ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    **출력 예**

    ```
    ok: updated action hello
    ```
    {: screen}

    Docker 이미지로 코드를 패키징한 경우 로컬 앱에 대한 경로 및 `--kind` 플래그 대신 작성 명령에 `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG`를 포함하십시오. 가능할 때마다 `latest` 태그를 사용하지 않고 이미지를 잘 관리하십시오. `latest` 태그가 사용되는 경우 해당 태그와 함께 이미지가 사용됩니다. 이 이미지는 항상 최근에 작성된 이미지가 아닐 수도 있습니다. 

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## 액션에 매개변수 바인딩
{: #actions_params}

기본 매개변수를 설정하기 위해 액션에 매개변수를 바인딩할 수 있습니다. 매개변수가 호출 시 제공되는 경우를 제외하고 바인딩된 매개변수는 액션에 대한 기본 매개변수로 제공됩니다.
{: shortdesc}

시작하기 전에 [액션을 작성](#actions_cli)하십시오.

매개변수를 바인딩하려면 다음을 수행하십시오.

1. 액션을 업데이트하고 기본 매개변수를 액션에 바인딩하십시오.

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **예**

    ```
ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    **출력 예**

    ```
ok: updated action MyApp
    ```
    {: screen}

    비서비스 인증 정보 매개변수를 수정하는 경우, 새 매개변수가 포함된 `action update` 명령을 실행하면 현재 존재하지만 `action update` 명령에 지정되지 않은 매개변수가 제거됩니다. 예를 들어, `action update -p key1 new-value -p key2 new-value`를 실행하지만 설정된 다른 매개변수를 생략하는 경우 액션이 업데이트되면 이 매개변수는 더 이상 존재하지 않습니다. 액션에 바인딩되었던 모든 서비스도 제거됩니다. 서비스를 바인딩한 경우 다시 [액션에 서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-services)해야 합니다.
    {: tip}

3. 매개변수가 액션에 바인딩되었는지 확인하십시오.

    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    **출력 예**

    ```
ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

선택사항: 이전에 바인딩된 매개변수를 지우려면 매개변수를 포함하지 않고 액션을 업데이트하십시오.

```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## 액션 시퀀스로 함께 액션 연결
{: #actions_seq}

일련의 액션을 연결하는 액션을 작성할 수 있습니다. 한 액션의 결과는 다음 액션에 대한 인수로 전달됩니다.
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

기본 매개변수를 제외하면 시퀀스의 액션 간에 전달된 매개변수는 명시적입니다. 따라서 액션 시퀀스에 전달된 매개변수는 시퀀스의 첫 번째 액션에만 사용 가능합니다. 시퀀스에서 첫 번째 액션의 결과는 시퀀스에서 두 번째 액션에 대한 입력 JSON 오브젝트가 됩니다(계속 동일하게 반복됨). 첫 번째 액션이 결과에 이를 포함하지 않는 한, 이 오브젝트에는 시퀀스에 원래 전달된 매개변수가 포함되지 않습니다. 액션에 대한 입력 매개변수는 액션의 기본 매개변수와 병합되며, 전자가 우선순위를 갖고 일치하는 기본 매개변수를 대체합니다.

시퀀스에는 시퀀스 내의 각 액션의 제한시간과 별도인 전체 제한시간이 없습니다. 시퀀스가 오퍼레이션의 파이프라인이므로 한 액션의 장애는 파이프라인을 손상시킵니다. 하나의 액션의 제한시간이 초과되면 전체 시퀀스가 해당 장애로 종료됩니다.

그런 다음, 룰을 작성하거나 액션을 호출할 때 시퀀스의 이름을 사용합니다.


## 액션 패키징
{: #actions_pkgs}

{{site.data.keyword.openwhisk}}에서 패키지를 사용하여 일련의 관련된 액션 및 피드를 번들화하고 이를 다른 사용자와 공유할 수 있습니다. 또한 패키지는 패키지의 모든 엔티티 간에 매개변수가 공유되도록 허용합니다.
{: shortdesc}

패키지에는 *액션* 및 *피드*가 포함될 수 있습니다.
- 액션은 {{site.data.keyword.openwhisk_short}}에서 실행되는 코드 조각입니다. 예를 들어, {{site.data.keyword.cloudant}} 패키지에는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 레코드를 읽고 쓰는 액션이 포함되어 있습니다.
- 피드는 트리거 이벤트를 실행하도록 외부 이벤트 소스를 구성하는 데 사용됩니다. 예를 들어, 알람 패키지에는 지정된 빈도로 트리거를 실행할 수 있는 피드가 포함됩니다.


1. 패키지를 작성하십시오.

  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. 패키지의 요약을 가져오십시오. 패키지가 비어 있다는 점에 유념하십시오.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **출력 예**

  ```
  package /myNamespace/custom
  ```
  {: screen}

4. 액션을 작성하고 패키지에 액션을 포함하십시오. 패키지의 액션을 작성할 때 액션 이름 앞에 패키지 이름을 접두부로 지정해야 합니다. 패키지 중첩은 허용되지 않습니다. 패키지는 액션만 포함할 수 있으며 다른 패키지는 포함할 수 없습니다.

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. 패키지의 요약을 가져오십시오.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **출력 예**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## 패키지에 매개변수 바인딩
{: #actions_pkgs_params}

패키지의 모든 액션에 의해 상속되는 package-level 매개변수를 설정하여 패키지의 모든 엔티티에 기본 매개변수를 설정할 수 있습니다.

바인딩된 매개변수는 다음의 경우를 제외하고 패키지에서 액션에 대한 기본 매개변수로 제공됩니다.

- 액션 자체에 기본 매개변수가 있는 경우
- 액션에 호출 시 제공되는 매개변수가 있는 경우

시작하기 전에 최소 하나의 액션을 포함하는 패키지를 작성하십시오.

1. 패키지를 업데이트하고 기본 매개변수를 패키지에 바인딩하십시오.

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **예**

    ```
  ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    **출력 예**

    ```
ok: updated package MyApp
    ```
    {: screen}

    비서비스 인증 정보 매개변수를 수정하는 경우, 새 매개변수가 포함된 `package update` 명령을 실행하면 현재 존재하지만 `package update` 명령에 지정되지 않은 매개변수가 제거됩니다. 예를 들어, `package update -p key1 new-value -p key2 new-value`를 실행하지만 설정된 다른 매개변수를 생략하는 경우 패키지가 업데이트되면 이 매개변수는 더 이상 존재하지 않습니다. 또한 패키지에 바인딩된 모든 서비스가 제거되므로 다른 매개변수를 업데이트한 후 다시 [패키지에 서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-services)해야 합니다.
    {: tip}

3. 매개변수가 패키지에 바인딩되었는지 확인하십시오.

    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    **출력 예**

    ```
ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. 매개변수가 패키지로 상속되었는지 확인하십시오.

    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    **출력 예**

    ```
ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## 액션의 패키지 공유
{: #actions_pkgs_share}

패키지를 구성하는 액션 및 피드가 디버그되고 테스트된 후에 패키지는 모든 {{site.data.keyword.openwhisk_short}} 사용자와 공유될 수 있습니다. 패키지를 공유하면 사용자가 패키지를 바인딩하고 패키지의 액션을 호출하며 {{site.data.keyword.openwhisk_short}} 룰 및 시퀀스 액션을 작성할 수 있습니다. 공유 패키지 내의 액션 및 피드는 _public_입니다. 패키지가 개인용이면 모든 해당 컨텐츠 또한 개인용입니다.
{: shortdesc}

1. 모든 사용자와 패키지를 공유하십시오.

  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. 패키지의 `publish` 특성을 표시하여 현재 true인지 확인하십시오.

  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  **출력 예**

  ```
  ok: got package PACKAGE_NAME, displaying field publish

    true
  ```
  {: screen}

3. 다른 사용자가 패키지를 바인딩하거나 패키지의 액션을 호출할 수 있도록 다른 사용자에게 패키지의 완전한 이름을 제공하려면 패키지의 설명을 가져오십시오. 완전한 이름에는 네임스페이스가 포함되며, 이 예제에서는 `myNamespace` 네임스페이스입니다.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **출력 예**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## 액션을 위한 환경 변수
{: #actions_envvars}

액션 환경에는 실행 중인 액션에 특정한 여러 환경 변수가 포함되어 있습니다. 특성은 지원되는 모든 런타임에 대해 시스템 환경에서 액세스가 가능합니다. 이러한 특성은 액션이 REST API를 통해 프로그래밍 방식으로 자산에 대해 작업하거나 액션이 할당된 시간 예산을 모두 소진하려는 시점에 내부 알람을 설정할 수 있도록 허용합니다.
{: shortdesc}

|특성 |설명 |
| -------- | ----------- |
| `__OW_API_HOST` |이 액션을 실행 중인 배치를 위한 API 호스트입니다. |
| `__OW_API_KEY` |액션을 호출 중인 주제에 대한 API 키입니다. 이 변수는 클래식 CF 기반 네임스페이스에 대해서만 제공됩니다. |
| `__OW_NAMESPACE` |네임스페이스 ID(GUID)입니다. 클래식 CF 기반 네임스페이스의 경우, 이 ID는 조직 및 영역 이름으로 구성됩니다. |
| `__OW_NAMESPACE_CRN` |네임스페이스 클라우드 리소스 이름 [CRN](/docs/overview?topic=overview-crn)입니다. CRN은 IAM 사용 네임스페이스에서만 사용할 수 있습니다.
| `__OW_ACTION_NAME` |실행 중인 액션의 완전한 이름입니다. |
| `__OW_IAM_NAMESPACE_API_KEY` |IAM 사용 네임스페이스에 대한 API 키입니다. 사용법에 대해서는 [액세스 정책 설정](/docs/openwhisk?topic=cloud-functions-namespaces#namespace-access)을 참조하십시오. |
| `__OW_IAM_API_URL` | API 키에서 토큰을 가져오는 작업과 같은 IAM 조작에 사용되는 서비스 엔드포인트입니다. 이 변수는 IAM 사용 네임스페이스에서만 사용할 수 있습니다. |
| `__OW_ACTIVATION_ID` |실행 중인 이 액션 인스턴스에 대한 활성화 ID입니다. |
| `__OW_DEADLINE` |이 액션이 전체 기간 할당량을 소진할 개략적인 시간(에포크 밀리초)입니다. |

### 앱에서 액션 환경 변수 통합
{: #actions_envvars_app}

액션에 대한 값을 보려면 앱 코드에 값의 표시를 포함하고 결과에 값을 출력하십시오.

**Python의 예**
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

액션에서 코드를 업데이트하고 활성화한 후 결과에는 액션에 대한 완전한 이름이 포함됩니다.
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
{: screen}



