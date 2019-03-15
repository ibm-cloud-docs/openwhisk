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

# Creating Go actions
{: #creating-go-actions}

The following sections guide you through creating and invoking a single Go action and adding parameters to that action. You can execute Go actions by using Go 1.11. To use this runtime, specify the `ibmcloud fn` CLI parameter
`--kind go:1.11` when creating or updating an action.
{: shortdesc}

## Creating and invoking a Go action
{: #invoking-go-actions}

A Go action is simply a public function from the `main` package. Use a single file for quick testing or development purposes. For production apps, [pre-compile your Go actions into an executable](#packaging-go-actions) for better performance or multiple source file support, including vendor libraries.
{: shortdesc}

Create a Go action.

1. Save the following code in a file named `hello.go`. In the example, the function is named `Main`, but you can change the name by using the `--main` flag. If you rename the function, the name must begin with a capital letter, and cannot be the the term `main`, which is the name of the Go package.

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
    The expected signature for a `Main` function is:

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. Create or update an action called `helloGo`.

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. Invoke the action.

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    Example output:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## Packaging an action as a Go executable
{: #packaging-go-actions}

Although you can create a binary on any Go platform by cross-compiling with `GOOS=Linux` and `GOARCH=amd64`, use the pre-compilation feature that is embedded in the runtime container image. You can package [multiple source files](#multiple-packages-go-actions) or [vendor libraries](#vendor-libs-go-actions).
{: shortdesc}

### Working with multiple package source files
{: #multiple-packages-go-actions}

To use multiple package source files, use a top level `src` directory, place the source files that belong to the main package at the root of `src` or inside a `main` directory and create directories for other packages. For example, the `hello` package becomes the `src/hello` directory.
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

With this layout, you can import subpackages (`import "hello"`) as shown in the following example.

You can compile locally by setting your `GOPATH` to parent of the `src` directory. If you use VSCode, you need to enable the `go.inferGopath` setting to `true`.
{: note}

Example of `main/main.go`:

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

Example of `hello/hello.go`:

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

You can compile by using the runtime environment. Create a .zip archive the content of the `src` directory. Do **not** include the top level project directory `go-action-project/`. To create the .zip archive `hello-src.zip`:

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

Compile and package the Go executable as `exec` in the root of the .zip archive. Build the `hello-bin.zip` archive by running the following command. This assumes you have Docker CLI installed in your workstation and `docker` in your `PATH`.

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

In this example, the main function is `-compile main`. To use a different function as main, change the value for `-compile`.
The main function is selected at compilation time. When you pre-compile, `ibmcloud fn action [update | create]` ignores the `--main`.

The container gets the contents of the source .zip in `stdin`, compiles the content, and creates a new .zip archive with the executable `exec` in the root. The .zip archive content streams out to `stdout` which gets redirected to the `hello-bin.zip` archive to be deployed as the Go Action.

Now, you can update your action for production by using the CLI and new .zip archive `hello-bin.zip`.

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### Working with vendor libraries
{: #vendor-libs-go-actions}

You can include dependencies by populating a `vendor` directory inside the source `zip` archive when you compile the Go Action. The `vendor` directory does not work at the top level. You need to place the `vendor` directory within `src/` and inside a package directory.
{: shortdesc}

Continuing with the previous example, use the log package `logrus` in `hello.go`.

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
In this example, the `vendor` directory is located in `src/hello/vendor`. You can add third-party libraries that are used by the `hello` package. You can use multiple tools such as [dep ![External link icon](../icons/launch-glyph.svg "External link icon")](https://golang.github.io/dep/docs/installation.html) to populate and manage dependencies.

You can use `dep` by creating a file `src/main/Gopkg.toml` describing the version and location of the libraries.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Populate the `vendor` directory, run `dep ensure`.

You can automate this process by using a deployment tool or script such as a [Makefile](#makefile).
{: tip}

### Using a Makefile for Go actions
{: #makefile}

Continuing with the previous example, your project directory is as follows.

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

1. Create the file `Makefile` to automate the deployment process.

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

2. Delete the .zip archives and vendor directory.

```bash
make clean
```
{: pre}

3. Populate the vendor directory, create the source .zip, compile the source code, archive the exec into a .zip, and deploy the Go action by running.

```bash
make deploy
```
{: pre}

4. Now that the `go-action-hello` is created, invoke the action.

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

Example output:

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
