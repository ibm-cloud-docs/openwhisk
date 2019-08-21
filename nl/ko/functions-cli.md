---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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




# {{site.data.keyword.openwhisk_short}} CLI
{: #functions-cli}

기능을 구성하는 엔티티를 관리하려면 이 명령을 실행하십시오.
{: shortdec}

<br />

## 액션 명령
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

액션을 작성하십시오.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>액션의 이름입니다. 패키지의 액션을 포함하려면 `PACKAGE_NAME`/`ACTION_NAME` 형식으로 된 이름을 입력하십시오. 이 값은 필수입니다. </dd>

   <dt>`APP_FILE`</dt>
   <dd>액션으로 실행할 앱 파일 또는 패키지에 대한 경로입니다. 이 옵션은 필수입니다.</dd>
   
   <dt>`--copy`</dt>
   <dd>액션을 기존 액션의 이름으로 처리합니다.</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>액션을 실행할 Docker 허브 사용자 이름 및 Docker 허브에 있는 Docker 이미지 이름입니다. 이 플래그는 Docker 이미지에서 액션을 작성하는 데 필요합니다.</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>앱의 런타임입니다. 이 플래그는 선택사항입니다. `VALUE`가 지정되지 않으면 발견된 런타임의 기본 버전이 사용됩니다.
     `--kind` 옵션의 가능한 `VALUES`입니다.
     <table>
  <tr>
    <th>언어</th>
    <th>유형 ID</th>
  </tr>
  <tr>
    <td> Node.js</td>
    <td> <code>nodejs:10</code>(기본값), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code>(기본값)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>(기본값)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>(기본값)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>(기본값)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>(기본값)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>(기본값)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>(기본값)</td>
  </tr>
  <tr>
    <td>Docker 액션을 사용하여 다른 언어가 지원됩니다.</td>
  </tr>
</table>
{: caption="표 1. 지원되는 런타임" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>액션에 대한 최대 로그 크기(MB)입니다. 기본값은 10MB입니다.</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>액션의 항목 메소드가 `main`이 아닌 경우 사용자 정의 이름을 지정합니다. 이 플래그는 항목 메소드가 `main`이 아닌 경우 필수입니다. Java와 같은 일부 런타임의 경우 이름은 완전한 메소드여야 합니다.</dd>

   <dt>`--native`</dt>
   <dd>`--docker openwhisk/dockerskeleton`에 대한 속기로 `--native` 인수를 사용할 수 있습니다. 이 인수를 사용하여 표준 Docker 액션 SDK 내부에서 실행되는 실행 파일을 작성하고 배치할 수 있습니다.
      <ol><li>Docker 이미지를 작성할 때 `/action/exec`의 컨테이너 내의 실행 파일을 작성합니다. `/action/exec` 파일을 로컬 파일 시스템으로 복사하고 이를 `exec.zip`으로 압축하십시오.</li>
       <li>초기화 데이터로서 실행 파일을 수신하는 Docker 액션을 작성하십시오. `--native` 인수는 `--docker openwhisk/dockerskeleton` 인수를 대체합니다.</li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>액션 시퀀스를 작성하고 관련 액션의 이름을 포함합니다. `ACTION_NAMEs`를 쉼표로 구분하십시오.</dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>제한시간 `LIMIT`(밀리초)입니다. 기본값은 60000밀리초입니다. 제한시간에 도달하면 액션이 종료됩니다.</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>액션을 웹 액션, 원시 HTTP 웹 액션 또는 표준 액션으로 처리합니다. 웹 액션의 경우 `yes` 또는 `true`를 지정하고, 원시 HTTP 웹 액션의 경우 `raw`를 지정하고, 표준 액션의 경우 `no` 또는 `false`를 지정합니다. 웹 액션에 보안을 설정하기 위해 `--web-secure` 옵션도 포함합니다.</dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>웹 액션에 보안을 설정합니다. `SECRET`의 `VALUE`는 `true`, `false` 또는 어떠한 문자열도 될 수 있습니다. 이 옵션은 `--web` 옵션과만 함께 사용할 수 있습니다.</dd>
   </dl>

<br />**예**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **출력**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

더 이상 사용하지 않을 액션을 삭제하여 네임스페이스를 정리할 수 있습니다.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **출력**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

특정 액션에 대해 설명하는 메타데이터를 가져오십시오.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>액션의 이름입니다. 이 값은 필수입니다.</dd>

   <dt>`--save`</dt>
   <dd>기존 액션과 연관된 코드를 가져와서 로컬로 저장할 수 있습니다. 단, 시퀀스 및 Docker 액션은 제외됩니다. `FILENAME`은 현재 작업 중인 디렉토리에 있는 기존 액션 이름과 일치하고 파일 확장자는 액션 유형과 일치합니다. 예를 들어, 아카이브 파일인 액션 코드의 경우에는 .zip 확장자가 사용됩니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>파일 경로인 `FILENAME`을 제공하여 사용자 정의 이름 파일에 액션의 코드를 저장합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--summary`</dt>
  <dd>액션 세부사항의 요약을 가져옵니다. 접두부가 "*"인 매개변수가 바인딩되고, 접두부가 "**"인 매개변수가 바인딩되고 완료됩니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--url`</dt>
  <dd>액션에 대한 URL만 가져옵니다. 이 플래그는 선택사항입니다.</dd>
   </dl>

<br />**예**

```
ibmcloud fn action get hello
```
{: pre}

**출력**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

액션을 테스트하려면 실행하십시오.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>액션의 이름입니다. 이 값은 필수입니다. </dd>

   <dt>`--blocking, -b`</dt>
   <dd>블로킹 호출은 요청 및 응답 스타일을 사용하여 활성화 결과가 사용 가능할 때까지 기다립니다. 대기 시간은 60초 또는 액션의 [시간 `LIMIT` `VALUE`](/docs/openwhisk?topic=cloud-functions-limits) 중에서 더 작은 값입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--result, -r`</dt>
   <dd>앱 코드의 결과가 명령의 출력으로 표시됩니다. 이 옵션이 지정되지 않으면 활성화 ID가 표시됩니다. 이 옵션이 지정되면 호출이 차단됩니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `ibmcloud fn action list`
{: #cli_action_list}

작성한 모든 액션 또는 특정 수의 액션을 나열하십시오.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>액션의 패키지 이름입니다. 이 값은 선택사항입니다. 지정되지 않은 경우 모든 액션이 나열됩니다.</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>지정된 수의 액션을 나열합니다. 기본 액션 수는 30입니다.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 액션의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다.</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>최근에 작성한 지정된 수의 액션을 결과에서 제외합니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

액션 내에서 액션 또는 앱을 업데이트하십시오.

패키지, 조치 또는 트리거에 대한 매개변수를 업데이트하는 경우 이전에 작성된 매개변수를 모두 지정해야 합니다. 그렇지 않을 경우 이전에 작성된 매개변수가 제거됩니다. 패키지의 경우, 패키지에 바인딩된 모든 서비스도 제거되므로 다른 매개변수를 업데이트한 후 다시 패키지에 [서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-services)해야 합니다.
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**명령 옵션**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>액션의 이름입니다. 패키지의 액션을 포함하려면 `PACKAGE_NAME`/`ACTION_NAME` 형식으로 된 이름을 입력하십시오. 이 값은 필수입니다. </dd>

  <dt>`APP_FILE`</dt>
  <dd>액션으로 실행할 앱 파일 또는 패키지에 대한 경로입니다. 이 옵션은 액션 내에서 앱을 업데이트할 경우에 필요합니다.</dd>

  <dt>`--copy`</dt>
  <dd>액션을 기존 액션의 이름으로 처리합니다.</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>액션을 실행할 Docker 허브 사용자 이름 및 Docker 허브에 있는 Docker 이미지 이름입니다. 이 플래그는 Docker 이미지에서 액션을 작성하는 데 필요합니다.</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>앱의 런타임입니다. 이 플래그는 선택사항입니다. VALUE가 지정되지 않으면 발견된 런타임의 기본 버전이 사용됩니다.
    `--kind` 옵션의 가능한 값입니다.
    <table>
  <tr>
    <th>언어</th>
    <th>유형 ID</th>
  </tr>
  <tr>
    <td> Node.js</td>
    <td> <code>nodejs:10</code>(기본값), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code>(기본값)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code>(기본값)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code>(기본값)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code>(기본값)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code>(기본값)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code>(기본값)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code>(기본값)</td>
  </tr>
  <tr>
    <td>Docker 액션을 사용하여 다른 언어가 지원됩니다.</td>
  </tr>
</table>
{: caption="표 1. 지원되는 런타임" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>액션에 대한 최대 로그 크기(MB)입니다. 기본값은 10MB입니다.</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>액션의 항목 메소드가 `main`이 아닌 경우 사용자 정의 이름을 지정합니다. 이 플래그는 항목 메소드가 `main`이 아닌 경우 필수입니다. Java와 같은 일부 런타임의 경우 이름은 완전한 메소드여야 합니다.</dd>

  <dt>`--native`</dt>
  <dd>`--docker openwhisk/dockerskeleton`에 대한 속기로 `--native` 인수를 사용할 수 있습니다. 인수를 사용하여 표준 Docker 액션 SDK 내부에서 실행되는 실행 파일을 작성하고 배치할 수 있습니다.
      <ol><li>Docker 이미지를 작성할 때 `/action/exec`의 컨테이너 내의 실행 파일을 작성합니다. `/action/exec` 파일을 로컬 파일 시스템으로 복사하고 이를 `exec.zip`으로 압축하십시오.</li>
      <li>초기화 데이터로서 실행 파일을 수신하는 Docker 액션을 작성하십시오. `--native` 인수는 `--docker openwhisk/dockerskeleton` 인수를 대체합니다.</li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>관련된 액션의 이름을 지정하여 액션 시퀀스를 작성합니다.</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>제한시간 한계(밀리초)입니다. 기본값은 60000밀리초입니다. 제한시간에 도달하면 액션이 종료됩니다.</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>액션을 웹 액션, 원시 HTTP 웹 액션 또는 표준 액션으로 처리합니다. 웹 액션의 경우 `yes` 또는 `true`를 지정하고, 원시 HTTP 웹 액션의 경우 `raw`를 지정하고, 표준 액션의 경우 `no` 또는 `false`를 지정합니다. 웹 액션에 보안을 설정하기 위해 `--web-secure` 옵션도 포함합니다.</dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>웹 액션에 보안을 설정합니다. `SECRET`의 `VALUE`는 `true`, `false` 또는 어떠한 문자열도 될 수 있습니다. 이 옵션은 `--web` 옵션과만 함께 사용할 수 있습니다.</dd>
  </dl>

<br />**예**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## 활성화 명령
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

특정 활성화에 대해 설명하는 메타데이터를 가져오십시오.

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**명령 옵션**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>특정 활성화에 대한 ID입니다. `ibmcloud fn activation list`를 사용하여 사용 가능한 ID 목록을 검색합니다. `--last` 또는 `-l` 옵션이 지정되지 않은 경우 이 값은 필수입니다.</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>정보를 표시할 메타데이터의 필드입니다. 예를 들어, 로그 필드를 표시하려는 경우 `ibmcloud fn activation get ACTIVATION_ID logs`를 실행합니다. 이 값은 선택사항입니다.</dd>

  <dt>`--last, -l`</dt>
  <dd>가장 최근의 활성화에 대한 메타데이터를 표시합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--summary, -s`</dt>
  <dd>활성화 세부사항으로부터만 결과 응답을 표시합니다. 이 플래그는 선택사항입니다.</dd>
  </dl>


<br />**예**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list`
{: #cli_activation_list}

패키지에 있는 모든 액션에 대한 활성화 ID를 나열하십시오.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**명령 옵션**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>전체 활성화 설명을 표시합니다.</dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>지정된 수의 활성화를 나열합니다. 기본 활성화 수는 30이고, 최대 활성화 수는 200입니다.</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>날짜가 지정된 이후에 작성된 활성화를 나열합니다. 기간은 1970년 1월 1일 이후 밀리초 단위로 측정됩니다. 예를 들어 `1560371263`은 2019년 6월 12일 08:27:43 UTC입니다.</dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>지정된 수의 최근 활성화를 결과에서 제외합니다.</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>날짜가 지정되기 전에 작성된 활성화를 나열합니다. 기간은 1970년 1월 1일 이후 밀리초 단위로 측정됩니다. 예를 들어 `1560371263`은 2019년 6월 12일 08:27:43 UTC입니다.</dd>
  </dl>

<br />**예**
```
ibmcloud fn activation list
```
{: pre}

**출력**
```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

특정 활성화에 대한 로그를 가져오십시오.

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**명령 옵션**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>특정 활성화에 대한 ID입니다. `ibmcloud fn activation list`를 사용하여 사용 가능한 ID 목록을 검색합니다. `--last` 또는 `-l` 옵션이 지정되지 않은 경우 이 값은 필수입니다.</dd>

  <dt>`--last, -l`</dt>
  <dd>가장 최근의 활성화에 대한 로그를 표시합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--strip, -r`</dt>
  <dd>로그 메시지만 표시합니다. 단, 시간소인 및 스트림 정보는 제외됩니다. 이 플래그는 선택사항입니다.</dd>
  </dl>

<br />**예**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

액션 또는 네임스페이스에 대한 활성화 라이브 목록인 스트리밍을 보십시오. `CTRL+C`를 눌러 폴링을 종료할 수 있습니다.

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**명령 옵션**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>/로 시작하는 네임스페이스입니다. 네임스페이스, 액션 또는 영역에 대한 활성화를 폴링합니다. 이 값은 선택사항입니다. 네임스페이스 또는 액션이 지정되지 않은 경우 영역이 폴링됩니다.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>네임스페이스, 액션 또는 영역에 대한 활성화를 폴링합니다. 이 값은 선택사항입니다. 네임스페이스 또는 액션이 지정되지 않은 경우 영역이 폴링됩니다.</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>지정된 시간(초) 동안 활성화를 폴링한 후 종료합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>지정된 시간(일) 전에 활성화를 위한 폴링을 시작합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>지정된 시간(시간) 전에 활성화를 위한 폴링을 시작합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>지정된 시간(분) 전에 활성화를 위한 폴링을 시작합니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>지정된 시간(초) 전에 활성화를 위한 폴링을 시작합니다. 이 플래그는 선택사항입니다.</dd>
  </dl>

<br />**예**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

특정 활성화에서 결과를 가져오십시오.

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**명령 옵션**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>특정 활성화에 대한 ID입니다. `ibmcloud fn activation list`를 사용하여 사용 가능한 ID 목록을 검색합니다. `--last` 또는 `-l` 옵션이 지정되지 않은 경우 이 값은 필수입니다.</dd>

  <dt>`--last, -l`</dt>
  <dd>가장 최근의 활성화에 대한 결과를 표시합니다. 이 플래그는 선택사항입니다.</dd>

  </dl>

<br />**예**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## API 명령
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

API를 작성하십시오.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API에 대한 기본 경로입니다.</dd>

   <dt>`API_NAME`</dt>
   <dd>API의 이름입니다. API 이름은 기본 경로와 동일할 수 있습니다.</dd>

   <dt>`API_VERB`</dt>
   <dd>API의 verb입니다(예: `get` 또는 `post`).</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>액션의 이름입니다.</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>API의 이름입니다. 이 플래그는 구성 파일이 지정될 때 무시됩니다. 기본 이름은 `BASE_PATH`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>Swagger API 구성이 포함된 JSON 파일입니다. 이 플래그가 사용되면 API 이름 플래그가 무시됩니다. 이 플래그는 필수입니다.</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>웹 액션 응답 유형을 `html`, `http`, `json`, `text` 또는 `svg`로 설정합니다. 기본값은 `json`입니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **출력**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

API를 삭제하십시오.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API에 대한 기본 경로입니다.</dd>

   <dt>`API_NAME`</dt>
   <dd>API의 이름입니다. API 이름은 기본 경로와 동일할 수 있습니다.</dd>

   <dt>`API_PATH`</dt>
   <dd>API에 대한 경로입니다.</dd>

   <dt>`API_VERB`</dt>
   <dd>API의 verb입니다(예: `GET` 또는 `POST`).</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>API 출력 유형을 `json` 또는 `yaml`로 지정합니다. 기본값은 `json`입니다.</dd>

   <dt>`--full, -f`</dt>
   <dd>전체 API 구성 세부사항을 표시합니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

API에 대한 메타데이터를 가져옵니다.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API에 대한 기본 경로입니다.</dd>

   <dt>`API_NAME`</dt>
   <dd>API의 이름입니다. API 이름은 기본 경로와 동일할 수 있습니다.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>API 출력 유형을 `json` 또는 `yaml`로 지정합니다. 기본값은 `json`입니다.</dd>

   <dt>`--full, -f`</dt>
   <dd>전체 API 구성 세부사항을 표시합니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `ibmcloud fn api list`
{: #cli_api_list}

작성한 모든 API 또는 특정 수의 API를 나열하십시오. 이름 또는 기본 경로가 지정되지 않으면 모든 API가 나열됩니다.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>API에 대한 기본 경로입니다.</dd>

   <dt>`API_NAME`</dt>
   <dd>API의 이름입니다. API 이름은 기본 경로와 동일할 수 있습니다.</dd>

   <dt>`API_PATH`</dt>
   <dd>API에 대한 경로입니다.</dd>

   <dt>`API_VERB`</dt>
   <dd>API의 verb입니다(예: `GET` 또는 `POST`).</dd>

   <dt>`--full, -f`</dt>
   <dd>전체 API 세부사항을 표시합니다. 이 플래그는 선택사항입니다. </dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>지정된 수의 API를 나열합니다. 기본 API 수는 30입니다. 이 플래그는 선택사항입니다. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 API의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다. 이 플래그는 선택사항입니다. </dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>최근에 작성한 지정된 수의 API를 결과에서 제외합니다. 이 플래그는 선택사항입니다. </dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## 배치 명령
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

Manifest 파일을 사용하여 패키지, 액션, 트리거 및 룰의 콜렉션을 배치하십시오.

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>`wsk` API 호스트입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>구성 파일입니다. 기본값은 `$HOME/.wskprops`입니다.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>배치 파일에 대한 경로입니다.</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>Manifest 파일에 대한 경로입니다. 이 플래그는 manifest.yaml이 현재 디렉토리에 없는 경우 필수입니다.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>네임스페이스의 이름 또는 ID입니다.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--preview` </dt>
   <dd>배치하기 전에 배치 플랜을 표시합니다.</dd>

   <dt>`--project PATH`</dt>
   <dd>서버리스 프로젝트에 대한 경로입니다. 기본값은 <code>.</code>입니다(현재 디렉토리).</dd>

   <dt>`--strict`</dt>
   <dd>사용자 정의 런타임 버전을 허용합니다.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>verbose 출력을 봅니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Manifest 파일을 사용하여 패키지, 액션, 트리거 및 룰의 콜렉션을 배치 해제하십시오.

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>`wsk` API 호스트입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>구성 파일입니다. 기본값은 `$HOME/.wskprops`입니다.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>배치 파일에 대한 경로입니다.</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>Manifest 파일에 대한 경로입니다. 이 플래그는 manifest.yaml이 현재 디렉토리에 없는 경우 필수입니다.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>네임스페이스의 이름 또는 ID입니다.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--preview` </dt>
   <dd>명령을 실행하지 않고 명령의 결과를 표시합니다.</dd>

   <dt>`--project PATH`</dt>
   <dd>서버리스 프로젝트에 대한 경로입니다. 기본값은 `.`입니다(현재 디렉토리).</dd>

   <dt>`--strict`</dt>
   <dd>사용자 정의 런타임 버전을 허용합니다.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>verbose 출력을 봅니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## 나열 명령
{: #cli_list_cmd}


### `ibmcloud fn list`
{: #cli_list}

네임스페이스에서 그룹화된 패키지, 액션, 트리거 및 룰의 목록을 보십시오.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 엔티티의 각 그룹을 이름별로 정렬합니다. 그렇지 않으면 각 그룹이 작성 날짜별로 정렬됩니다.</dd>
   </dl>

<br />**예**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## 네임스페이스 명령
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

IAM 네임스페이스를 작성하십시오.

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>네임스페이스의 이름입니다. 이름에는 하이픈(-)을 포함하지 마십시오. 이 값은 필수입니다.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>네임스페이스를 식별하는 데 도움이 되도록 고유한 설명을 작성합니다. 설명이 둘 이상의 단어로 구성된 경우 설명에 인용 부호(")를 포함합니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

IAM 네임스페이스를 삭제하십시오.

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**예**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Cloud Foundry 또는 IAM 네임스페이스에서 엔티티 또는 메타데이터 정보를 가져오십시오.

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>네임스페이스의 이름 또는 ID입니다. 이 값은 필수입니다.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 네임스페이스의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다. 이 플래그는 선택사항입니다. </dd>

   <dt>`--properties`</dt>
   <dd>네임스페이스 내부에 포함된 엔티티 대신 네임스페이스 특성을 표시합니다. 이 플래그는 선택사항입니다. </dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **출력**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

사용 가능한 Cloud Foundry 및 IAM 네임스페이스를 나열하십시오.

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--cf`</dt>
   <dd>Cloud Foundry 네임스페이스만 표시합니다. IAM 네임스페이스는 표시되지 않습니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--iam`</dt>
   <dd>IAM 네임스페이스만 표시합니다. Cloud Foundry 네임스페이스는 표시되지 않습니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>지정된 수의 네임스페이스를 나열합니다. 기본 네임스페이스 수는 30입니다. 이 플래그는 선택사항입니다. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 네임스페이스의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다. 이 플래그는 선택사항입니다. </dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>최근에 작성한 지정된 수의 네임스페이스를 결과에서 제외합니다. 이 플래그는 선택사항입니다. </dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

IAM 네임스페이스의 이름 또는 설명을 변경하십시오.

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>네임스페이스의 이름입니다. 이름에는 하이픈(-)을 포함하지 마십시오. 이 값은 필수입니다.</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>네임스페이스의 새 이름입니다. 이름에는 하이픈(-)을 포함하지 마십시오. 이 값은 선택사항입니다.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>네임스페이스를 식별하는 데 도움이 되도록 고유한 설명을 작성합니다. 설명이 둘 이상의 단어로 구성된 경우 설명에 인용 부호(")를 포함합니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## 패키지 명령
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

매개변수를 패키지에 바인딩하십시오. 패키지 내의 모든 액션은 별도로 지정되지 않는 한 해당 매개변수를 상속합니다.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**명령 옵션**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>패키지의 이름입니다. 이 값은 필수입니다. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>
  </dl>

<br />**예**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

하나 이상의 액션이 포함되도록 설계된 패키지를 작성하십시오. 패키지의 액션을 추가하려면 액션을 작성하거나 업데이트할 때 패키지 이름을 액션에 포함하십시오.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**명령 옵션**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>패키지의 이름입니다. 이 값은 필수입니다. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>매개변수 `KEYS` `VALUE` 형식이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

  <dt>`--shared yes|no`</dt>
  <dd>값 없이 지정되거나 값이 yes로 지정된 경우 패키지는 다른 사용자와 공유됩니다.</dd>
  </dl>

<br />**예**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **출력**
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

더 이상 사용하지 않을 패키지를 삭제하여 네임스페이스를 정리할 수 있습니다.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **출력**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

특정 패키지에 대해 설명하는 메타데이터를 가져오십시오.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**명령 옵션**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>패키지의 이름입니다. 이 값은 필수입니다.</dd>

   <dt>`--summary`</dt>
   <dd>패키지 세부사항의 요약을 가져옵니다. 접두부가 "*"인 매개변수가 바인딩됩니다. 이 플래그는 선택사항입니다.</dd>
   </dl>

<br />**예**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `ibmcloud fn package list`
{: #cli_pkg_list}

작성한 모든 패키지 또는 특정 수의 패키지를 나열하십시오.

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>특정 네임스페이스의 패키지를 나열합니다. 이 값은 선택사항입니다. 지정되지 않은 경우 모든 패키지가 나열됩니다.</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>지정된 수의 패키지를 나열합니다. 기본 패키지 수는 30입니다.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 패키지의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다.</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>최근에 작성한 지정된 수의 패키지를 결과에서 제외합니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  `ibmcloud fn package list /whisk.system`을 실행하여 사전 설치된 패키지의 목록을 보십시오.
  {: tip}


<br />

### `ibmcloud fn package refresh`
{: #cli_pkg_refresh}

특정 네임스페이스 내의 모든 패키지에 대한 패키지 바인딩을 새로 고치십시오.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>/로 시작하는 네임스페이스입니다. 이 플래그는 필수입니다. `ibmcloud fn namespace list`를 실행하여 선택할 네임스페이스 목록을 가져오십시오.</dd>
   </dl>

<br />**예**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

하나 이상의 액션이 포함되도록 설계된 패키지를 업데이트하십시오. 패키지의 액션을 추가하려면 액션을 작성하거나 업데이트할 때 패키지 이름을 액션에 포함하십시오.

패키지, 조치 또는 트리거에 대한 매개변수를 업데이트하는 경우 이전에 작성된 매개변수를 모두 지정해야 합니다. 그렇지 않을 경우 이전에 작성된 매개변수가 제거됩니다. 패키지의 경우, 패키지에 바인딩된 모든 서비스도 제거되므로 다른 매개변수를 업데이트한 후 다시 패키지에 [서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-services)해야 합니다.
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>패키지의 이름입니다. 이 값은 필수입니다. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--shared yes|no`</dt>
   <dd>값 없이 지정되거나 값이 `yes`로 지정된 경우 패키지는 다른 사용자와 공유됩니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **출력**
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## 특성 명령
{: #cli_prop}

CLI 환경에 맞는 글로벌 특성을 설정하거나 `ibmcloud fn` CLI의 일부로 실행되는 `wsk` CLI에 대한 특성을 보십시오.

### `ibmcloud fn property get`
{: #cli_prop_get}

`wsk` CLI에서 특성에 대한 메타데이터 세부사항을 보십시오.

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`--all`</dt>
   <dd>`wsk` CLI에 대한 모든 특성을 봅니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`---apibuild`</dt>
   <dd>`wsk` API 빌드 정보입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--apibuildno`</dt>
   <dd>`wsk` API 빌드 번호입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>`wsk` API 호스트입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>`wsk` API 버전입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>`wsk` 클라이언트 인증서입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--cliversion`</dt>
   <dd>`wsk` CLI 버전입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`wsk` 클라이언트 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>IAM 네임스페이스입니다. 이 플래그는 Cloud Foundry 네임스페이스에 대해 설정될 수 없습니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
ibmcloud fn property get --auth
  ```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

속성을 설정하십시오. 하나 이상의 플래그가 필요합니다. 특성이 설정되면 워크스테이션의 `<home_dir>/.bluemix/plugins/cloud-functions/config.json`에서 해당 특성이 유지됩니다. 특성을 제거하려면 [`ibmcloud fn property unset --<property>`](#cli_prop_set)을 실행하십시오. 

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>`wsk` API 호스트입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>`wsk` API 버전입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>`wsk` 클라이언트 인증서입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`wsk` 클라이언트 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>IAM 네임스페이스입니다. 이 플래그는 Cloud Foundry 네임스페이스에 대해 설정될 수 없습니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **출력**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

`wsk` CLI에 대한 특성을 설정 해제하십시오. 하나 이상의 플래그가 필요합니다.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>`wsk` API 호스트입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>`wsk` API 버전입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>`wsk` 권한 부여 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>`wsk` 클라이언트 인증서입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`wsk` 클라이언트 `KEY`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>IAM 네임스페이스입니다. 이 플래그는 Cloud Foundry 네임스페이스에 대해 설정될 수 없습니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## 룰 명령
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

트리거를 액션과 연관시키도록 룰을 작성하십시오. 룰을 작성하기 전에 트리거 및 액션을 먼저 작성하십시오.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **출력**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

네임스페이스를 정리하려면 더 이상 필요하지 않은 룰을 제거하십시오.

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>룰의 이름입니다. 이 값은 필수입니다.</dd>

  <dt>`--disable`</dt>
  <dd>룰을 삭제하려면 사용 안함으로 설정하십시오.</dd>
  </dl>


<br />**예**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

룰의 상태를 비활성으로 변경하고 트리거 실행 시 액션이 실행되지 않도록 하십시오.

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

룰의 상태를 비활성에서 활성으로 변경하십시오. 활성인 경우 트리거 실행 시 액션이 실행됩니다.

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

특정 룰에 대해 설명하는 메타데이터를 가져오십시오.

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>룰의 이름입니다. 이 값은 필수입니다.</dd>

  <dt>`--summary`</dt>
  <dd>룰 세부사항의 요약을 가져옵니다.</dd>
  </dl>

<br />**예**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `ibmcloud fn rule list`
{: #cli_rule_list}

작성한 모든 룰 또는 특정 수의 룰을 나열하십시오.

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>룰의 이름입니다. 이 값은 선택사항입니다. 지정되지 않은 경우 모든 룰이 나열됩니다.</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>지정된 수의 룰을 나열합니다. 기본 룰 수는 30입니다.</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>리턴된 룰의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다.</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>최근에 작성한 지정된 수의 룰을 결과에서 제외합니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

룰의 상태가 활성화인지 아니면 비활성인지 확인하십시오. `ibmcloud fn rule disable` 또는 `ibmcloud fn run enable` 명령을 실행하여 상태를 변경하십시오.

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

트리거가 연관되는 룰을 변경하기 위해 룰을 업데이트할 수 있습니다.

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**예**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## SDK 명령
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

SDK를 설치하십시오.

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>SDK 컴포넌트(예: `docker`, `iOS` 및 `bashauto`)입니다.. 이 값은 필수입니다.</dd>

   <dt>`--stdout, --s`</dt>
   <dd>bash 명령 결과를 `STDOUT`로 인쇄합니다. 이 플래그는 선택사항입니다.</dd>


   </dl>

<br />**예**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## 서비스 명령
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

서비스 인증 정보를 액션 또는 패키지로 바인딩하십시오.

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>서비스의 이름입니다.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>인증 정보를 바인딩할 패키지 또는 액션의 이름입니다.</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>서비스 인스턴스 이름입니다.</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>바인딩할 서비스 `KEY` 인증 정보의 이름입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

액션 또는 패키지에서 서비스 인증 정보를 바인드 해제하십시오.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>서비스의 이름입니다.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>인증 정보를 바인딩 해제할 패키지 또는 액션의 이름입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## 트리거 명령
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

트리거를 작성하십시오.

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>트리거의 이름입니다. 이 값은 필수입니다. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>트리거의 유형을 피드로 설정합니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>


   </dl>

<br />**예**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

트리거를 삭제하십시오.

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**예**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

자동으로 트리거될 때까지 기다리지 않고 트리거를 실행하여 테스트하십시오.

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**명령 옵션**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>트리거의 이름입니다. 이 값은 필수입니다. </dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 `VALUES`입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   </dl>

<br />**예**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

특정 트리거에 대해 설명하는 메타데이터를 가져오십시오.

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>트리거의 이름입니다. 이 값은 필수입니다.</dd>

  <dt>`--summary`</dt>
  <dd>트리거 세부사항의 요약을 가져옵니다.</dd>
  </dl>

<br />**예**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `ibmcloud fn trigger list`
{: #cli_trigger_list}

작성한 모든 트리거 또는 특정 수의 트리거를 나열하십시오.

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>트리거의 이름입니다. 이 값은 선택사항입니다. 지정되지 않은 경우 모든 트리거가 나열됩니다.</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>지정된 수의 트리거를 나열합니다. 기본 트리거 수는 30입니다.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>리턴된 트리거의 목록을 이름별로 정렬합니다. 그렇지 않으면 목록을 작성 날짜별로 정렬합니다.</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>최근에 작성한 지정된 수의 트리거를 결과에서 제외합니다.</dd>

   </dl>

<br />**예**

  ```
    ibmcloud fn trigger list
  ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

트리거를 업데이트하십시오.

패키지, 조치 또는 트리거에 대한 매개변수를 업데이트하는 경우 이전에 작성된 매개변수를 모두 지정해야 합니다. 그렇지 않을 경우 이전에 작성된 매개변수가 제거됩니다. 패키지의 경우, 패키지에 바인딩된 모든 서비스도 제거되므로 다른 매개변수를 업데이트한 후 다시 패키지에 [서비스를 바인딩](/docs/openwhisk?topic=cloud-functions-services)해야 합니다.
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**명령 옵션**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>트리거의 이름입니다. 이 값은 필수입니다. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>어노테이션은 `KEY` `VALUE` 형식으로 지정됩니다. 둘 이상의 어노테이션을 포함하려면 각 어노테이션에 이 옵션을 지정하십시오. 이 플래그는 선택사항입니다.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>`KEY` `VALUE` 형식의 어노테이션이 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`KEY` `VALUE` 형식으로 된 매개변수 값입니다. 이 플래그는 선택사항입니다.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>매개변수 `KEYS` 및 `VALUES`가 포함된 JSON 파일입니다. 이 플래그는 선택사항입니다.</dd>
   </dl>

<br />**예**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






