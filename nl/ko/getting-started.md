
---

저작권:
  연도: 2017, 2019
마지막 업데이트 날짜: "2019-05-17"

키워드: getting started, creating actions, invoking actions,

하위 콜렉션: cloud-functions

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

# 시작하기
{: #getting-started}

{{site.data.keyword.openwhisk}}를 통해 원하는 프로그래밍 언어를 사용하여 확장 가능한 방법으로 앱 로직을 실행하는 경량 코드를 기록할 수 있습니다. 애플리케이션에서 HTTP 기반 API 요청으로 코드 On-Demand를 실행하거나 {{site.data.keyword.Bluemix_notm}} 서비스 및 서드파티 이벤트에 대한 응답으로 코드를 실행할 수 있습니다. Faas(Function-as-a-Service) 프로그래밍 플랫폼은 오픈 소스 프로젝트 Apache OpenWhisk를 기반으로 합니다.
{: shortdesc}

## 액션에 대한 작업
{: #gs_actions}

{{site.data.keyword.openwhisk}}를 통해 액션이라는 하나의 특정 태스크를 수행하도록 설정된 Stateless 코드 스니펫을 작성할 수 있습니다. 액션 및 기타 함수 용어에 대해 자세히 알아보려면 [용어](/docs/openwhisk?topic=cloud-functions-about)를 참조하십시오.
{:shortdesc}

시작하려면 옵션을 클릭하십시오.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="{{site.data.keyword.openwhisk_short}}를 신속하게 시작하려면 아이콘을 클릭하십시오." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="액션 작성" title="액션 작성" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="{{site.data.keyword.openwhisk_short}} CLI 플러그인 설정" title="{{site.data.keyword.openwhisk_short}} CLI 플러그인 설정" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="플랫폼 아키텍처 참조" title="플랫폼 아키텍처 참조" shape="rect" coords="326, -10, 448, 218" />
</map>

## GUI에서 액션 작성
{: #gs_hello_world}

{{site.data.keyword.openwhisk_short}}를 시작하려면 HelloWorld 빠른 시작 템플리트 사용을 시도하십시오.

1. [{{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/registration) 계정을 작성하거나 기존 계정에 로그인하십시오. 

2. [{{site.data.keyword.openwhisk_short}} 대시보드 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk)로 이동하십시오.

2. **작성 시작** > **빠른 시작 템플리트**를 클릭하고 **Hello World** 템플리트를 선택하십시오.

3. **패키지 이름** 필드에 고유한 이름을 입력하여 액션에 대한 패키지를 작성하십시오.

4. **액션 helloworld** 섹션의 드롭 다운에서 런타임을 선택하십시오. 템플리트를 배치하기 전에 사용 가능한 각 런타임에서 샘플 액션에 대한 코드를 미리 볼 수 있습니다.

5. **배치**를 클릭하십시오. 액션이 작성되었습니다. 수고하셨습니다!

6. **호출**을 클릭하여 액션을 실행하십시오. 액션을 수동으로 호출하면 액션이 정의하는 앱 로직이 실행됩니다. **활성화** 패널에 액션으로 생성된 "Hello stranger!"라는 인사가 표시됩니다.

7. 선택사항: **입력 변경**을 클릭하여 액션을 변경하거나 직접 시도하십시오.

  a. 다음 코드를 **입력 변경** 상자에 붙여넣은 후 이름 값을 변경하십시오. 
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. 그런 다음 **호출**을 클릭하여 업데이트로 액션을 실행하십시오. 결과는 **활성화** 패널에 표시됩니다. 이 프로세스를 원하는 횟수만큼 반복할 수 있습니다.

잘하셨습니다! 첫 번째 액션을 작성했습니다. 이 액션을 정리하려면 오버플로우 메뉴를 클릭하고 **액션 삭제**를 선택하십시오.

## CLI에서 액션 작성
{: #gs_hello_world_cli}

[HelloWorld JavaScript](/docs/openwhisk?topic=cloud-functions-prep#prep-js) 예제 코드로 빠르게 시작하고 실행합니다. 이 예제에서는 해당 앱 로직을 실행하기 위해 수동으로 호출할 수 있는 기본 `hello` 액션을 작성합니다.

## 다음 단계
{: #gs_next_steps}

첫 번째 템플리트 배치를 완료했습니다. 여기서 어디로 이동하시겠습니까? 다음 중에서 선택할 수 있습니다.

* [용어](/docs/openwhisk?topic=cloud-functions-about#about_technology)를 익힙니다.
* [자체 액션](/docs/openwhisk?topic=cloud-functions-actions)을 시작합니다.
* [패키지](/docs/openwhisk?topic=cloud-functions-pkg_ov)에서 액션 구성 방법을 알아봅니다.
* 고급 옵션: [서버가 없는 REST API](/docs/openwhisk?topic=cloud-functions-apigateway)를 작성하십시오.
