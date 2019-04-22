---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 시작하기
{: #getting_started}

{{site.data.keyword.openwhisk}}를 통해 원하는 프로그래밍 언어를 사용하여 확장 가능한 방법으로 앱 로직을 실행하는 경량 코드를 기록할 수 있습니다. {{site.data.keyword.Bluemix_notm}} 서비스 및 서드파티 이벤트에 대한 응답으로 자동으로 또는 애플리케이션에서 HTTP 기반 API 요청으로 코드 On-Demand를 실행할 수 있습니다. Faas(Function-as-a-Service) 프로그래밍 플랫폼은 오픈 소스 프로젝트 Apache OpenWhisk를 기반으로 합니다.
{: shortdesc}

{{site.data.keyword.openwhisk_short}}에는 서버가 없으므로 사용할 수 있는 언어로 제한되지 않으며 백엔드 인프라를 명시적으로 피로비저닝하느라 시간을 소모할 필요가 없습니다. Auto-Scaling, 고가용성, 업데이트 또는 유지보수에 대해 걱정하는 대신 앱 로직 작성에 집중할 수 있습니다. 구입 즉시 IBM에서 하드웨어, 네트워킹, 소프트웨어 관리, 로드 밸런싱, 플러그인 등을 제공합니다. 사용자는 코드를 가져오기만 하면 됩니다. 

## 액션에 대한 작업
{: #creating_actions}
{: #openwhisk_actions}

{{site.data.keyword.openwhisk}}를 통해 액션이라는 하나의 특정 태스크를 수행하도록 설정된 Stateless 코드 스니펫을 작성할 수 있습니다.
{:shortdesc}

**액션의 개념**

액션은 이벤트에 대한 응답으로 자동으로 실행되도록 호출하거나 설정할 수 있는 작은 코드 조각입니다. 어떤 경우든지 각 실행은 고유 활성화 ID로 식별되는 레코드를 생성합니다. 액션의 입력 및 결과는 키 값 쌍으로 표시될 수 있습니다. 키는 문자열이며 값은 유효한 JSON 값입니다. 액션은 사용자가 선택하는 언어로 기록될 수 있으며, 소스 코드 또는 Docker 이미지 중 하나로 서비스에 제공됩니다. 액션 코드는 Cloud Functions API, CLI 또는 iOS SDK에 의해 직접 호출될 때 실행됩니다. 액션은 IBM Cloud 또는 서드파티 서비스에서 이벤트에 자동으로 응답할 수 있습니다. 

**액션을 사용하는 이유**

액션을 사용하면 코드가 실행되는 시간을 제한할 수 있으며, 이로써 오버헤드 비용을 줄일 수 있습니다. 

예를 들어 액션을 사용하여 이미지에서 얼굴을 감지하거나 데이터베이스의 변경사항에 응답하거나 API 호출 세트를 집계하거나 트윗 게시를 수행할 수 있습니다. 

**한 번에 둘 이상의 액션을 사용할 수 있습니까?**

예. 액션을 사용하여 다른 액션을 호출하거나 문자열 액션을 함께 사용하여 시퀀스를 작성할 수 있습니다. 이 작업을 수행하려면 한 액션의 출력은 다른 액션 등을 트리거하는 데 사용할 수 있는 출력을 제공하는 다른 액션에 대한 입력이 될 수 있습니다. 패키지 형성을 위해 작성하는 액션의 그룹을 번들할 수도 있습니다. 패키지를 사용하면 액션 또는 시퀀스를 다시 구성하는 대신 패키지를 호출하여 공통 액션 또는 시퀀스를 다시 사용할 수 있습니다. 

시작하려면 옵션을 클릭하십시오.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="{{site.data.keyword.openwhisk_short}}를 신속하게 시작하려면 아이콘을 클릭하십시오." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="액션 작성" title="액션 작성" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="{{site.data.keyword.openwhisk_short}} CLI 플러그인 설정" title="{{site.data.keyword.openwhisk_short}} CLI 플러그인 설정" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="플랫폼 아키텍처 참조" title="플랫폼 아키텍처 참조" shape="rect" coords="326, -10, 448, 218" />
</map>

## GUI에서 액션 작성
{: #openwhisk_start_hello_world}

{{site.data.keyword.openwhisk_short}}를 시작하려면 HelloWorld 빠른 시작 템플리트 사용을 시도하십시오.

1.  [{{site.data.keyword.Bluemix_notm}} **카탈로그** ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk)에서 함수 대시보드를 방문하십시오.

2. **작성 시작** > **빠른 시작 템플리트**를 클릭하고 **Hello World** 템플리트를 선택하십시오.

3. **패키지 이름** 필드에 고유한 이름을 입력하여 액션에 대한 패키지를 작성하십시오. 

4. **액션 helloworld** 섹션의 드롭 다운에서 런타임을 선택하십시오. 템플리트를 배치하기 전에 사용 가능한 각 런타임에서 샘플 액션에 대한 코드를 미리 볼 수 있습니다. 

5. **배치**를 클릭하십시오. 액션이 작성되었습니다. 수고하셨습니다!

6. **호출**을 클릭하여 액션을 실행하십시오. 액션을 수동으로 호출하면 액션이 정의하는 앱 로직이 실행됩니다. **활성화** 패널에 액션으로 생성된 "Hello stranger!"라는 인사가 표시됩니다.

7. 선택사항: **입력 변경**을 클릭하여 액션을 변경하거나 직접 시도하십시오. 그런 다음 **호출**을 클릭하여 업데이트로 액션을 실행하십시오. 결과는 **활성화** 패널에 표시됩니다. 이 프로세스를 원하는 횟수만큼 반복할 수 있습니다. 

잘하셨습니다! 첫 번째 액션을 작성했습니다. 이 액션을 정리하려면 오버플로우 메뉴를 클릭하고 **액션 삭제**를 선택하십시오.

## CLI에서 액션 작성
{: #openwhisk_start_hello_world_cli}

HelloWorld JavaScript 예제 코드로 빠르게 시작하고 실행합니다. 이 예제에서는 해당 앱 로직을 실행하기 위해 수동으로 호출할 수 있는 기본 `hello` 액션을 작성합니다.

1. [{{site.data.keyword.openwhisk_short}} CLI 플러그인을 설정](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)하십시오.

2. **hello.js** 파일에 다음 코드를 저장하십시오.

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
function main(params) {
        var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. `hello` 액션을 작성하십시오.

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. 매개변수를 전달하지 않고 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    출력:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. 앱 로직을 테스트하려면 이름 매개변수를 전달하여 액션을 다시 호출하십시오.

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    출력:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

잘하셨습니다! 첫 번째 액션을 작성했습니다. 이 액션을 정리하려면 `ibmcloud fn action delete hello`를 실행하십시오.

## 다음 단계
{: #getting_started_next_steps}

첫 번째 템플리트 배치를 완료했습니다. 여기서 어디로 이동하시겠습니까? 다음 중에서 선택할 수 있습니다. 

* [용어](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology)를 익힙니다.
* [자체 액션](/docs/openwhisk?topic=cloud-functions-openwhisk_actions)을 시작합니다.
* [패키지](/docs/openwhisk?topic=cloud-functions-openwhisk_packages)에서 액션 구성 방법을 알아봅니다.
* 고급 옵션: [서버가 없는 REST API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway)를 작성하십시오.
