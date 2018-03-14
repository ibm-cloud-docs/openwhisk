---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 트리거 및 규칙 작성
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}} 트리거 및 규칙은 플랫폼에 이벤트 구동 기능을 제공합니다. 외부 또는 내부 이벤트 소스의 이벤트는 트리거를 통해 전달되며, 규칙은 조치가 이러한 이벤트에 반응할 수 있도록 허용합니다.
{: shortdesc}

## 트리거 작성
{: #openwhisk_triggers_create}

트리거는 이벤트의 클래스에 대한 이름 지정된 채널입니다. 다음은 트리거의 예입니다. 
- 위치 업데이트 이벤트의 트리거. 
- 웹 사이트에 문서 업로드의 트리거. 
- 수신 이메일의 트리거. 

트리거는 키-값 쌍의 사전을 사용하여 *실행*(활성화)될 수 있습니다. 이 사전을 *이벤트*라고 부르기도 합니다. 조치에서와 같이, 각 트리거 실행의 결과로 인해 ID가 생성됩니다. 

트리거는 사용자에 의해 명시적으로 실행되거나 외부 이벤트 소스에 의해 사용자 대신 실행될 수 있습니다.
*피드*는 {{site.data.keyword.openwhisk_short}}에서 이용할 수 있는 트리거 이벤트를 실행하도록 외부 이벤트 소스를 구성하는 편리한 방법입니다. 다음 예제 피드를 참조하십시오. 
- 데이터베이스의 문서가 추가되거나 수정될 때마다 트리거 이벤트를 실행하는 Cloudant 데이터 변경 피드. 
- Git 저장소에 대해 커미트 시마다 트리거 이벤트를 실행하는 Git 피드. 

## 규칙 사용
{: #openwhisk_rules_use}

대응하는 조치가 입력으로서 트리거 이벤트로 호출되도록 하는 트리거의 모든 실행으로, 규칙은 하나의 트리거를 하나의 조치와 연관시킵니다. 

적절한 규칙 세트로 단일 트리거 이벤트는 다중 조치를 호출할 수 있으며, 다중 트리거의 이벤트에 대한 응답으로서 조치가 호출될 수 있습니다. 

예를 들어, 다음 조치가 있는 시스템을 고려하십시오. 
- `classifyImage` - 이미지의 오브젝트를 발견하고 이를 분류하는 조치. 
- `thumbnailImage` - 이미지의 작은 그림 버전을 작성하는 조치. 

또한 두 개의 이벤트 소스가 다음 트리거를 실행한다고 가정합니다. 
- `newTweet` - 새 트윗이 게시될 때 실행되는 트리거. 
- `imageUpload` - 이미지가 웹 사이트에 업로드될 때 실행되는 트리거. 

단일 트리거 이벤트가 다중 조치를 호출하고 다중 트리거가 동일한 조치를 호출하도록 규칙을 설정할 수 있습니다. 
- `newTweet -> classifyImage` 규칙. 
- `imageUpload -> classifyImage` 규칙. 
- `imageUpload -> thumbnailImage` 규칙. 

3개의 규칙이 다음 작동을 설정합니다.  
- 두 트윗의 이미지가 모두 분류됩니다. 
- 업로드된 이미지가 분류됩니다. 
- 작은 그림 버전이 생성됩니다. 

## 트리거 작성 및 실행
{: #openwhisk_triggers_fire}

트리거는 특정 이벤트가 발생할 때 실행되거나 수동으로 실행될 수 있습니다. 

예제로서, 사용자 위치 업데이트를 전송하는 트리거를 작성하고 트리거를 수동으로 실행하십시오. 
1. 트리거를 작성하려면 다음 명령을 입력하십시오. 
  ```
  wsk trigger create locationUpdate
  ```
  {: pre}

  ```
  ok: created trigger locationUpdate
  ```

2. 트리거 세트를 나열하여 트리거가 작성되었는지 확인하십시오. 
  ```
  wsk trigger list
  ```
  {: pre}

  ```
  triggers
  /someNamespace/locationUpdate                            private
  ```

  이제 이벤트가 실행될 수 있는 이름 지정된 "채널"이 작성되었습니다. 

3. 그 다음에는 트리거 이름 및 매개변수를 지정하여 트리거 이벤트를 실행하십시오. 
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```

일치되는 동반 규칙 없이 실행된 트리거는 가시적 영향이 없습니다.
트리거는 패키지 내에서 작성될 수 없으며, 네임스페이스 아래에서 직접 작성되어야 합니다. 

## 규칙을 사용하여 트리거 및 조치 연관
{: #openwhisk_rules_assoc}

규칙을 사용하여 트리거를 조치와 연관시킬 수 있습니다. 트리거 이벤트가 실행될 때마다 이벤트 매개변수와 함께 조치가 호출됩니다. 

예제로서, 위치 업데이트가 게시될 때마다 `hello` 조치를 호출하는 규칙을 작성하십시오. 
1. 다음과 같은 조치 코드로 'hello.js' 파일을 작성하십시오. 
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 트리거 및 조치가 존재하는지 확인하십시오. 
  ```
  wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  wsk action update hello hello.js
  ```
  {: pre}

3. 다음 단계는 규칙을 작성하는 것입니다. 규칙은 작성 시에 사용되며, 이는 트리거의 활성화에 대해 즉각적으로 응답할 수 있음을 의미합니다. 3개의 매개변수는 규칙, 트리거 및 조치의 이름입니다. 
  ```
  wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  언제든지 규칙을 사용하지 않도록 선택할 수 있습니다. 
  ```
  wsk rule disable myRule
  ```
  {: pre}

4. `locationUpdate` 트리거를 실행하십시오. 이벤트를 실행할 때마다 `hello` 조치가 이벤트 매개변수와 함께 호출됩니다. 
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```

5. 최신 활성화를 확인하여 조치가 호출되었는지 확인하십시오.
  ```
  wsk activation list --limit 1 hello
  ```
  {: pre}

  ```
  activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  ```
  wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  ```json
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```

  `hello` 조치가 이벤트 페이로드를 수신하고 예상 문자열을 리턴했음을 볼 수 있습니다. 

동일한 트리거를 서로 다른 조치와 연관시키는 다중 규칙을 작성할 수 있습니다.
트리거 및 규칙은 패키지에 속할 수 없습니다. 그러나 규칙은 패키지에 속한 조치와 연관될 수 있습니다. 예를 들어, 다음과 같습니다. 
  ```
  wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

시퀀스의 규칙을 사용할 수도 있습니다. 예를 들어, `anotherRule` 규칙에 의해 활성화된 조치 시퀀스 `recordLocationAndHello`를 작성할 수 있습니다. 
  ```
  wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
