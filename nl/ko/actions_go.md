---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: docker, actions, serverless

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

# Go 액션 작성
{: #creating-go-actions}

다음 절에서는 단일 Go 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다. Go 1.11을 사용하여 Go 액션을 실행할 수 있습니다. 이 런타임을 사용하려면 액션 작성 또는 업데이트 시에 `ibmcloud fn` CLI 매개변수
`--kind go:1.11`을 지정하십시오.
{: shortdesc}

## Go 액션 작성 및 호출
{: #invoking-go-actions}

Go 액션은 단순히 `main` 패키지의 공용 함수입니다. 빠른 테스트 또는 개발 목적인 경우 단일 파일을 사용하십시오. 프로덕션 앱의 경우 벤더 라이브러리를 포함하여 보다 나은 성능 또는 다중 소스 파일 지원을 위해 [Go 액션을 실행 파일에 미리 컴파일하십시오](#packaging-go-actions).
{: shortdesc}

Go 액션을 작성하십시오.

1. 이름이 `hello.go`인 파일에 다음 코드를 저장하십시오. 예를 들어 함수의 이름은 `Main`이지만, `--main` 플래그를 사용하여 이름을 변경할 수 있습니다. 함수의 이름을 바꾸는 경우 이름은 대문자로 시작해야 하며 Go 패키지의 이름인 `main` 용어로는 바꿀 수 없습니다. 

    ```go
    package main

    import "fmt"

    // Main is the function implementing the action
    func Main(params map[string]interface{}) map[string]interface{} {
        // parse the input JSON
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")
        // return the output JSON
        return msg
    }
    ```
    {: codeblock}

    </br>
    `Main` 함수의 예상 서명은 다음과 같습니다. 

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. `helloGo`라는 액션을 작성 또는 업데이트하십시오.

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. 액션을 호출하십시오.

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    출력 예:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## 액션을 Go 실행 파일로 패키징
{: #packaging-go-actions}

`GOOS=Linux` 및 `GOARCH=amd64`로 교차 컴파일하여 Go 플랫폼에서 2진을 작성할 수 있더라도 런타임 컨테이너 이미지에 포함된 사전 컴파일 기능을 사용하십시오. [다중 소스 파일](#multiple-packages-go-actions) 또는 [벤더 라이브러리](#vendor-libs-go-actions)를 패키징할 수 있습니다.
{: shortdesc}

### 여러 패키지 소스 파일에 대한 작업
{: #multiple-packages-go-actions}

여러 패키지 소스 파일을 사용하려면 최상위 `src` 디렉토리를 사용하고 기본 패키지에 속한 소스 파일을 `src`의 루트 또는 `main` 디렉토리 내부에 배치하고 다른 패키지용 디렉토리를 작성하십시오. 예를 들어 `hello` 패키지는 `src/hello` 디렉토리가 됩니다.
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
    │   └── hello.go
    └── main
        └── main.go
```
{: screen}

이 레이아웃을 사용하면 다음 예에 표시된 대로 서브패키지(`import "hello"`)를 가져올 수 있습니다. 

`GOPATH`를 `src` 디렉토리의 상위로 설정하여 로컬로 컴파일할 수 있습니다. VSCode를 사용하는 경우, `go.inferGopath` 설정을 `true`로 사용해야 합니다.
{: note}

`main/main.go`의 예:

```go
package main

import (
	"fmt"
	"hello"
)

// Main forwading to Hello
func Main(args map[string]interface{}) map[string]interface{} {
	fmt.Println("This is main.Main")
	greetings := "World"
	name, ok := args["name"].(string)
	if ok {
		greetings = name
	}
	return hello.Hello(greetings)
}
```
{: codeblock}

`hello/hello.go`의 예:

```go
package hello

import "fmt"

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	fmt.Println("This is hello.Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello " + name
	return res
}
```
{: codeblock}

런타임 환경을 사용하여 컴파일할 수 있습니다. `src` 디렉토리의 컨텐츠를 사용하여 .zip 아카이브를 작성하십시오. 최상위 프로젝트 디렉토리 `go-action-project/`는 포함하지 **마십시오**. .zip 아카이브 `hello-src.zip`을 작성하려면 다음을 수행하십시오. 

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

.zip 아카이브의 루트에서 `exec`로 Go 실행 파일을 컴파일 및 패키징하십시오. 다음 명령을 사용하여 `hello-bin.zip` 아카이브를 빌드하십시오. 여기서는 사용자 워크스테이션에 Docker CLI가 설치되어 있고 `PATH`에 `docker`가 있다고 가정합니다. 

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

이 예에서는 기본 함수가 `-compile main`입니다. 다른 함수를 기본으로 사용하려면 `-compile`의 값을 변경하십시오.
기본 함수는 컴파일 시에 선택됩니다. 사전 컴파일하는 경우, `ibmcloud fn action [update | create]`는 `--main`을 무시합니다.

컨테이너는 `stdin`에서 소스 .zip의 컨텐츠를 얻고, 컨텐츠를 컴파일하고 루트에서 `exec` 실행 파일로 새 .zip 아카이브를 작성합니다. .zip 아카이브 컨텐츠는 `stdout`으로 스트림되며, 이렇게 되면 Go 액션으로 배치되도록 `hello-bin.zip` 아카이브로 경로 재지정됩니다. 

이제 CLI 및 새 .zip 아카이브 `hello-bin.zip`을 사용하여 프로덕션용 액션을 업데이트할 수 있습니다.

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### 벤더 라이브러리 관련 작업
{: #vendor-libs-go-actions}

Go 액션을 컴파일할 때 `vendor` 디렉토리를 소스 `zip` 아카이브에 채움으로써 종속 항목을 포함시킬 수 있습니다. `vendor` 디렉토리는 최상위 레벨에 두지 못합니다. `vendor` 디렉토리는 `src/` 내에 패키지 디렉토리 내부에 배치해야 합니다.
{: shortdesc}

이전 예를 계속하여 로그 패키지 `logrus`를 `hello.go`에서 사용하십시오.

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}

</br>
이 예에서는 `vendor` 디렉토리가 `src/hello/vendor`에 위치합니다. `hello` 패키지에 사용되는 서드파티 라이브러리를 추가할 수 있습니다. [dep ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://golang.github.io/dep/docs/installation.html) 등의 다중 도구를 사용하여 종속 항목을 채우고 관리할 수 있습니다.

라이브러리의 버전 및 위치를 설명하는 `src/main/Gopkg.toml` 파일을 작성하여 `dep`를 사용할 수 있습니다. 

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

`vendor` 디렉토리를 채우고 `dep ensure`를 실행하십시오.

[Makefile](#makefile) 같은 배치 도구 또는 스크립트를 사용하여 이 프로세스를 자동화할 수 있습니다.
{: tip}

### Go 액션에 Makefile 사용
{: #makefile}

이전 예를 계속하면 프로젝트 디렉토리는 다음과 같게 됩니다.

```bash
go-action-hello/
├── Makefile
└── src
    ├── hello
    │   ├── Gopkg.toml
    │   ├── hello.go
    │   └── vendor/
    └── main
        └── main.go
```
{: screen}

1. 배치 프로세스를 자동화하려면 `Makefile` 파일을 작성하십시오. 

```Makefile
GO_COMPILER?=openwhisk/actionloop-golang-v1.11
CLI?=ibmcloud fn
MAIN=main
APP=hello
SRCS=$(MAIN)/$(MAIN).go $(APP)/$(APP).go
VENDORS=$(APP)/vendor
NAME=go-action-$(APP)
BINZIP=$(APP)-bin.zip
SRCZIP=$(APP)-src.zip

deploy: $(BINZIP)
	$(CLI) action update $(NAME) $(BINZIP) --main $(MAIN) --kind go:1.11

$(BINZIP): $(SRCZIP)
	docker run -i $(GO_COMPILER) -compile $(MAIN) <$(SRCZIP) >$(BINZIP)

$(SRCZIP): src/$(VENDORS)
	cd src ; zip ../$(SRCZIP) -r $(SRCS) $(VENDORS)

src/%/vendor:
	cd $(@D) ; DEPPROJECTROOT=$(realpath $(@D)/../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}

2. .zip 아카이브 및 벤더 디렉토리를 삭제하십시오. 

```bash
make clean
```
{: pre}

3. 벤더 디렉토리를 채우고 소스 .zip을 작성하고 소스 코드를 컴파일하고 실행 파일을 .zip으로 아카이브하고 실행을 통해 Go 액션을 배치하십시오. 

```bash
make deploy
```
{: pre}

4. `go-action-hello`가 작성되었으므로 액션을 호출하십시오. 

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

출력 예:

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
