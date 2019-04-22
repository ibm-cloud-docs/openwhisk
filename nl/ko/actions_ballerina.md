---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

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

# Ballerina 액션 작성
{: #ballerina-actions}

다음 절에서는 단일 Ballerina 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다. 액션은 JSON 오브젝트를 승인하고 리턴하는 최상위 레벨의 Ballerina 함수입니다.  

Ballerina 액션은 Ballerina [0.990.2](https://ballerina.io/downloads)에서 실행됩니다. 실행 파일을 생성하기 위해 로컬로 사용할 수 있는 컴파일러의 호환 가능 버전이 필요합니다. Ballerina 컴파일러가 없으면 액션을 작성할 수 없습니다. 

## Ballerina 액션 작성 및 호출

**시작하기 전에:** 다음 소스 코드를 사용하여 `hello.bal` 파일을 작성하십시오. 

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

액션에 대한 항목 메소드는 기본적으로 `main`입니다. `--main`을 사용하여 `wsk` CLI로 액션을 작성할 때 이 변수를 지정할 수 있습니다. 

**참고:** Ballerina 컴파일러는 실행 파일을 생성하기 위해 `main`이라는 함수가 있는 것으로 예상하므로 사용자의 소스 파일에는 `main`이라는 플레이스홀더가 있어야 합니다.

`hello`라는 액션을 작성하려면 다음 단계를 완료하십시오. 

1. .balx 파일을 생성하십시오.
  ```
  ballerina build hello.bal
  ```
{: pre}

2. .balx 파일을 사용하여 액션을 작성하십시오. 
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. CLI는 아직 소스 파일 확장자에서 액션의 유형을 판별하지 않습니다. 유형을 명시적으로 지정해야 합니다. `.balx` 소스 파일의 경우, 액션은 Ballerina 0.990.2 런타임을 사용하여 실행됩니다. 
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

예제 출력. 
```json
{
  "greeting": "Hello World!"
}
```
{: screen}
