---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: trigger rules, triggers, actions, channel events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 트리거 및 룰로 이벤트에 응답
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk}} 트리거 및 룰은 플랫폼에 이벤트 중심 기능을 제공합니다. 외부 및 내부 이벤트 소스의 이벤트는 트리거를 통해 전달되며 룰은 액션이 이러한 이벤트에 대해 반응하도록 합니다.
{: shortdesc}

## 일반 개념
{: #definitions}

### 트리거
{: #openwhisk_triggers_create}

트리거는 이벤트의 클래스에 대한 이름 지정된 채널입니다.
{: shortdesc}

트리거는 사용자 또는 이벤트 소스에 의한 특정 유형의 이벤트에 반응하려는 선언입니다. 다음은 트리거의 예입니다.
- 위치 업데이트 이벤트의 트리거
- 웹 사이트로 문서 업로드의 트리거
- 수신 이메일의 트리거

트리거는 키-값 쌍의 사전을 사용하여 실행되거나 활성화될 수 있습니다. 종종 이 사전을 이벤트라고 합니다. 트리거는 사용자에 의해 명시적으로 실행되거나 외부 이벤트 소스에 의해 사용자 대신 실행될 수 있습니다. 액션에서와 같이, 룰과 연관된 트리거의 개별 실행은 결과적으로 활성화 ID를 생성합니다. 룰과 연관되지 않은 트리거에는 실행 시에 가시적인 영향이 없습니다.

피드는 {{site.data.keyword.openwhisk_short}}에서 이용할 수 있는 트리거 이벤트를 실행하기 위한 외부 이벤트 소스를 구성하는 편리한 방법입니다. 다음은 피드의 예입니다.
- 데이터베이스의 문서가 추가되거나 수정될 때마다 트리거 이벤트를 실행하는 {{site.data.keyword.cloudant}} 데이터 변경 피드
- Git 저장소에 대한 모든 커미트에 대해 트리거 이벤트를 실행하는 Git 피드

### 룰
{: #openwhisk_rules_use}

룰은 트리거를 액션과 연관시킵니다.
{: shortdesc}

트리거가 실행될 때마다 룰은 입력으로서 트리거 이벤트를 사용하고 연관된 액션을 호출합니다. 적절한 룰 세트를 사용하면 단일 트리거 이벤트가 다중 액션을 호출하거나 액션이 다중 트리거의 이벤트에 대한 응답으로 호출될 수 있습니다.

예를 들어, 다음 액션이 있는 시스템을 고려하십시오.
- `classifyImage` - 이미지에서 오브젝트를 발견하고 이를 분류하는 액션.
- `thumbnailImage` - 이미지의 작은 그림 버전을 작성하는 액션.

또한 두 개의 이벤트 소스가 다음 트리거를 실행한다고 가정하십시오.
- `newTweet` - 새 트윗이 게시될 때 실행되는 트리거.
- `imageUpload` - 이미지가 웹 사이트에 업로드될 때 실행되는 트리거.

단일 트리거 이벤트가 다중 액션을 호출하고 다중 트리거가 동일한 액션을 호출하도록 하는 룰을 설정할 수 있습니다.
- `newTweet -> classifyImage` 룰
- `imageUpload -> classifyImage` 룰
- `imageUpload -> thumbnailImage` 룰

3개의 룰은 다음 동작을 설정합니다.
- 두 트윗의 이미지가 모두 분류됩니다.
- 업로드된 이미지가 분류됩니다.
- 작은 그림 버전이 생성됩니다.

## 채널 이벤트에 대한 트리거 작성
{: #openwhisk_triggers_fire}

다음 단계에서는 사용자 위치 업데이트를 전송하는 트리거 예제를 작성하는 방법과 트리거를 수동으로 실행하는 방법을 보여줍니다.

1. 트리거를 작성하십시오. 트리거는 네임스페이스 내에서 직접 작성되어야 하며 패키지 내에서는 작성될 수 없습니다.
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    출력 예:
    ```
      ok: created trigger locationUpdate
    ```
    {: screen}

2. 트리거가 작성되었는지 확인하십시오.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    출력 예:
    ```
      triggers
  /someNamespace/locationUpdate                            private
    ```
    {: screen}
    트리거는 이벤트가 실행될 수 있는 이름 지정된 채널의 역할을 합니다.

3. 트리거 이벤트를 실행하십시오.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    이 트리거와 연관된 룰이 없으므로 전달된 매개변수는 액션에 의해 입력으로 사용되지 않습니다. 출력 예:
    ```
      ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

다음 섹션에서는 룰을 작성하여 트리거를 액션과 연관시킬 수 있습니다.

## 룰을 사용하여 트리거를 액션과 연관시킴
{: #openwhisk_rules_assoc}

룰은 트리거를 액션과 연관시키는 데 사용됩니다. 트리거 이벤트가 실행될 때마다 트리거 이벤트의 매개변수를 사용하여 액션이 호출됩니다.

[`locationUpdate` 트리거](#openwhisk_triggers_fire)를 작성한 이후, 다음 단계는 위치 업데이트가 게시될 때마다 `hello` 액션을 호출하는 예제 룰을 작성하는 방법을 보여줍니다.

1. 다음 액션 코드를 사용하여 'hello.js' 파일을 작성하십시오.
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: pre}

2. `hello` 액션을 작성하십시오.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. `locationUpdate` 트리거를 `hello` 액션과 연관시키는 `myRule` 룰을 작성하십시오. 룰은 네임스페이스 내에서 직접 작성되어야 하며 패키지 내에서는 작성될 수 없습니다.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. `locationUpdate` 트리거를 실행하십시오. 트리거 이벤트가 발생할 때마다 `hello` 액션이 이벤트 매개변수로 호출됩니다.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    출력 예:
    ```
      ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. 가장 최근의 활성화 레코드를 확인하여 `hello` 액션이 호출되었는지 확인하십시오.
    ```
    ibmcloud fn activation list --limit 1 hello
    ```
    {: pre}

    출력 예:
    ```
      activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. 이전 명령 출력에서 활성화 ID에 대한 자세한 정보를 가져오십시오.
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    출력 예:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
`hello` 액션이 이벤트 페이로드를 수신하고 예상 문자열을 리턴했음을 볼 수 있습니다.

7. 룰을 사용하지 않으려면 다음 명령을 실행할 수 있습니다.
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

룰을 사용하여 트리거를 시퀀스와 연관시킬 수도 있습니다. 예를 들어, `anotherRule` 룰에 의해 활성화된 `recordLocationAndHello`라고 하는 액션 시퀀스를 작성할 수 있습니다.
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
