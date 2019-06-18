---

copyright:
years: 2017, 2019
lastupdated: "2019-05-15"

keywords: composer, openwhisk, compositions, sequence, branch

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

# 작성기
{: #pkg_composer}

{{site.data.keyword.openwhisk}}는 이제 기술 미리보기로 Apache OpenWhisk에 대한 작성기를 지원합니다. 작성기는 추가 조합기([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md), [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md))와 함께 Apache OpenWhisk 시퀀스를 확장하며, 이를 통해 사용자는 조건부 분기, 오류 처리 및 루프가 포함된 보다 복잡한 플로우를 작성할 수 있습니다. 작성기에 대한 전체 문서 및 기술 스펙은 [작성기 Git 저장소](https://github.com/apache/incubator-openwhisk-composer)에서 사용 가능합니다.

또한 오픈 소스 프로젝트 [Kui](https://github.com/ibm/kui)를 사용하여 이를 쉽게 작성하도록 하고 작성기 소스 코드를 배치 및 시각화할 수 있습니다. 작성기와 함께 KUI를 사용하는 방법에 대한 자세한 정보는 [Github의 Kui](https://github.com/ibm/kui)를 참조하십시오.
{: note}

## JavaScript 또는 Python 3의 작성기 라이브러리 설치
{: #install_composer}

임의의 언어로 작성된 액션과 함께 작성기를 사용할 수 있지만 JavaScript 또는 Python 3 중 하나에서 컴포지션을 표현합니다. 설치 후에는 작성기 `compose/pycompose` 및 `deploy/pydeploy` 명령을 사용하여 [컴포지션을 구성 및 실행](#run)할 수 있습니다.
{: shortdesc}

**JavaScript의 경우**:
1. 노드 패키지 관리자를 사용하여 작성기 라이브러리 [Node.js 패키지](https://github.com/apache/incubator-openwhisk-composer)를 설치하십시오.

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  작성기 명령에 대한 도움말을 실행하여 라이브러리를 설치했는지 확인하십시오.

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    출력 예:
    ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Python 3의 경우**:
`pip3`을 사용하여 [Python 3용 작성기](https://github.com/apache/incubator-openwhisk-composer-python)를 설치하십시오.

1.  Python 3 GitHub 저장소용 작성기를 복제하십시오.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  작성기 디렉토리로 이동하십시오.
    ```
    cd composer-python
    ```
    {: pre}
3.  작성기 라이브러리를 설치하십시오. 사용자가 위치해 있는 디렉토리 내에서 명령을 찾을 수 있도록 마침표(`.`)를 포함하십시오.
    ```
    pip3 install -e .
    ```
    {: pre}
4.  작성기 명령에 대한 도움말을 실행하여 라이브러리를 설치했는지 확인하십시오.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    출력 예:
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## IBM Cloud Functions에서 컴포지션 구성 및 실행
{: #run}

JavaScript 또는 Python 3 Composer 라이브러리를 사용하여 {{site.data.keyword.openwhisk}}에서 컴포지션을 작성할 수 있습니다. `compose` 또는 `pycompose`를 사용하여 컴포지션 소스 코드를 컴파일한 후 `deploy` 또는 `pydeploy`를 사용하여 컴포지션을 {{site.data.keyword.openwhisk}}에 배치하십시오. 컴포지션을 구성한 후에 {{site.data.keyword.openwhisk}}에서 이를 실행할 수 있습니다.
{: shortdesc}

**시작하기 전에**:
기본적으로 배치는 `~/.wskprops`에서 설정된 값을 사용합니다. 작성기 `deploy` 또는 `pydeploy` 명령에 대한 입력으로 두 개의 매개변수를 설정하여 기본값을 대체하십시오.

1.  API 호스트를 {{site.data.keyword.openwhisk}} 엔드포인트로 설정하십시오.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  `wsk` CLI 인증 키를 추가하십시오. 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**구성을 실행하려면 다음을 수행하십시오**.

1.  NodeJS 또는 Python 3 라이브러리로 작성기 소스 코드를 작성하십시오. 예를 들어 `demo.js` 파일을 작성하십시오.
2.  작성기 소스 코드를 JSON 파일을 컴파일하십시오.
    *   JavaScript에서:
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   Python 3에서:
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  코드를 {{site.data.keyword.openwhisk}}에 배치하십시오.
    *   JavaScript에서: 이름이 `demo`인 기존 배치를 덮어쓰려면 `-w` 플래그를 포함시키십시오.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   Python 3에서: 이름이 `demo`인 기존 배치를 덮어쓰려면 `-w` 플래그를 포함시키십시오.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  {{site.data.keyword.openwhisk}}에서 [다른 액션을 호출](/docs/openwhisk?topic=cloud-functions-triggers)하는 것과 동일한 방법으로 컴포지션을 실행하십시오.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}}는 특수한 유형의 액션으로 배치한 코드를 실행합니다. 자세한 정보는 [실행기 액션](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md)의 문서를 참조하십시오.

## 작성기를 사용하여 시퀀스 확장
{: #extending}

Apache OpenWhisk를 통해 사용자는 `sequence`에서 함께 함수를 연결할 수 있으며, 여기에서는 한 액션의 출력이 다른 액션에 대한 입력이 됩니다.

### 작성기가 없는 시퀀스
{: #sequences-without-composer}
이름이 `action1` 및 `action2`인 두 개의 함수를 {{site.data.keyword.openwhisk_short}}에서 함께 연결할 수 있습니다.

`ibmcloud fn action create --sequence mysequence action1 action2`.

이 명령의 결과는 이름이 `mysequence`인 함수이며, 이는 `action1` 및 `action2`의 컴포지트입니다.  OpenWhisk의 함수와 동일한 방식으로 `mysequence`를 사용할 수 있습니다.

### 작성기가 있는 시퀀스
{: #sequences-with-composer}
작성기에서 명령행 대신 소스 코드를 사용하여 풍부한 순서를 지정할 수 있습니다.

JavaScript의 경우:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

Python 3의 경우:
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="단순 시퀀스" alt="두 개의 액션이 있는 시퀀스" style="width:250px; border-style: none"/></br>
_그림 1. 두 개의 액션이 있는 시퀀스_

작성기의 함수를 함께 연결하는 것으로 제한되지는 않습니다. 작성기에는 시퀀스의 표현성을 향상시키는 조합기를 기반으로 한 [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 또는 [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md) 제품군이 포함됩니다. 다음 섹션에서 공통 예제를 볼 수 있습니다.

### 오류 처리
{: #error-handling}
`try-catch-finally` 블록을 사용하여 시퀀스에 오류 처리를 추가할 수 있습니다. 이 예에서 한 번의 시도로 시퀀스를 둘러쌉니다. 두 액션 중 하나가 오류를 리턴하는 경우 `handleError` 코드가 실행됩니다.

JavaScript의 경우:
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

Python 3의 경우:
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Try 시퀀스" alt="오류 처리가 있는 시퀀스" style="width:400px; border-style: none"/></br>
_그림 2. 오류 처리가 있는 시퀀스_

### 조건부 분기
{: #conditional-branch}
`if-then-else`를 사용하여 분기된 시퀀스를 작성할 수 있습니다. 이 예에서는 `if-then-else`를 설명합니다. `action1`은 부울을 리턴해야 합니다. `true`인 경우, `action2`가 실행되며, 그렇지 않은 경우 `action3`이 실행됩니다. `action3`은 선택사항이며 `if-then`을 위해 생략될 수 있습니다.

JavaScript의 경우:
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

Python 3의 경우:
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="If 시퀀스" alt="조건부 분기를 사용하는 시퀀스" style="width:250px; border-style: none"/></br>
_그림 3. 조건부 분기를 사용하는 시퀀스_

### 루프
{: #loop}
작성기에서 루프 생성을 작성할 수 있습니다. 이 예에서는 `action1`이 `true`를 리턴함에 따라 `action2`가 실행됩니다. 작성기가 작성된 시퀀스에서 실행할 수 있는 총 단계 수를 제한합니다. 현재 한계는 20입니다.

JavaScript의 경우:
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

Python 3의 경우:
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="While 시퀀스" alt="루프가 있는 시퀀스" style="width:250px; border-style: none"/></br>
_그림 4. `while` 루프가 있는 시퀀스_

### 액션의 인라인 정의
{: #inline-def}
컴포지션 코드 자체 내에서 액션을 정의할 수 있습니다. 이 예에서는 `composer.action()`을 사용하여 이름이 `hello`인 컴포지션으로 액션 정의 인라인을 작성합니다.

JavaScript의 경우:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

Python 3의 경우:
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="While 시퀀스" alt="인라인 액션 정의가 있는 시퀀스" style="width:250px; border-style: none"/></br>
_그림 5. 인라인 액션 정의가 있는 시퀀스_

## 다른 조합기 정의 사용
{: #combinator-def}
조합기 정의의 전체 목록에 대해 Apache OpenWhisk([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) 또는 [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md))의 작성기에 대한 문서를 참조하십시오.

