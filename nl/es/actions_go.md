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

# Creación de acciones Go
{: #creating-go-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Go y la adición de parámetros a dicha acción. Puede ejecutar acciones Go utilizando Go 1.11. Para utilizar este entorno de ejecución, especifique el parámetro `--kind go:1.11` de la CLI de `ibmcloud fn` al crear o actualizar una acción.
{: shortdesc}

## Creación e invocación de una acción Go
{: #invoking-go-actions}

Una acción Go es simplemente una función pública del paquete `main`. Utilice un archivo individual para fines de desarrollo o pruebas rápidas. En apps de producción, [precompile las acciones Go en un ejecutable](#packaging-go-actions) para obtener un mejor rendimiento o para el soporte de varios archivos de origen, incluyendo bibliotecas de proveedores.
{: shortdesc}

Cree una acción Go.

1. Guarde el código siguiente en un archivo denominado `hello.go`. En el ejemplo, la función se denomina
`Main`, pero puede cambiar el nombre utilizando el distintivo `--main`. Si renombra la función, el nombre debe comenzar por una letra mayúscula, y no puede ser `main`, que es el nombre del paquete Go.

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
    La firma esperada para una función `Main` es:

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. Cree o actualice una acción denominada `helloGo`.

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. Invoque la acción.

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    Salida de ejemplo:
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## Empaquetado de una acción como un ejecutable Go
{: #packaging-go-actions}

Aunque puede crear un binario en cualquier plataforma Go mediante la compilación cruzada con
`GOOS=Linux` y `GOARCH=amd64`, utilice la característica de precompilación incorporada en la imagen del contenedor del entorno de tiempo de ejecución. Puede empaquetar [varios archivos de origen](#multiple-packages-go-actions) o
[bibliotecas de proveedor](#vendor-libs-go-actions).
{: shortdesc}

### Cómo trabajar con varios archivos de origen de paquete
{: #multiple-packages-go-actions}

Para utilizar varios archivos de origen de paquete, utilice un directorio `src` de nivel superior, coloque los archivos de origen que pertenecen al paquete main en la raíz de `src` o dentro de un directorio `main` y cree directorios para otros paquetes. Por ejemplo, el paquete
`hello` pasa a ser el directorio `src/hello`.
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

Con este diseño, puede importar subpaquetes (`import "hello"`) tal como se indica en el ejemplo siguiente.

Puede compilar de forma local estableciendo la variable `GOPATH` en el padre del directorio
`src`. Si utiliza VSCode, necesita establecer el valor de `go.inferGopath` en `true`.
{: note}

Ejemplo de `main/main.go`:

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

Ejemplo de `hello/hello.go`:

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

Puede compilar utilizando el entorno de ejecución. Cree un archivo .zip del contenido del directorio `src`. **No** incluya el directorio de proyecto de nivel superior (`go-action-project/`). Para crear el archivo .zip
`hello-src.zip`:

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

Compile y empaquete el ejecutable Go como `exec` en la raíz del archivo .zip. Compile el archivo
`hello-bin.zip` ejecutando el mandato siguiente. Esto presupone que tiene la CLI de Docker instalada en la estación de trabajo y
`docker` en `PATH`.

```bash
docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

En este ejemplo, la función main es `-compile main`. Para utilizar una función distinta como main, cambie el valor de
`-compile`.
La función main se selecciona en tiempo de compilación. Al precompilar, `ibmcloud fn action [update | create]` ignora el
`--main`.

El contenedor obtiene el contenido del .zip de origen en `stdin`, compila el contenido y crea un nuevo archivo .zip con el ejecutable `exec` en la raíz. El contenido del archivo .zip se transmite a `stdout` que se redirige al archivo
`hello-bin.zip` para su despliegue como acción Go.

Ahora, puede actualizar la acción para producción utilizando la CLI y el nuevo archivo .zip `hello-bin.zip`.

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### Cómo trabajar con bibliotecas de proveedor
{: #vendor-libs-go-actions}

Puede incluir dependencias rellenando un directorio `vendor` dentro del archivo `zip` de origen al compilar la acción Go. El directorio `vendor` no funciona en el nivel superior. Debe ubicar el directorio `vendor` dentro de `src/` y dentro de un directorio de paquetes.
{: shortdesc}

Siguiendo con el ejemplo anterior, utilice el paquete de registro `logrus` en `hello.go`.

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
En este ejemplo, el directorio `vendor` se encuentra en `src/hello/vendor`. Puede añadir bibliotecas de terceros utilizadas por el paquete `hello`. Puede utilizar varias herramientas como
[dep ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://golang.github.io/dep/docs/installation.html) para rellenar y gestionar dependencias.

Puede utilizar `dep` creando un archivo `src/main/Gopkg.toml` que describe la versión y la ubicación de las bibliotecas.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Rellene el directorio `vendor` ejecutando `dep ensure`.

Puede automatizar este proceso utilizando un script o herramienta de despliegue como [Makefile](#makefile).
{: tip}

### Utilización de un Makefile para acciones Go
{: #makefile}

Siguiendo con el ejemplo anterior, el directorio de proyecto será el siguiente.

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

1. Cree el archivo `Makefile` para automatizar el proceso de despliegue.

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

2. Suprima los archivos .zip y el directorio vendor.

```bash
make clean
```
{: pre}

3. Rellene el directorio vendor, cree el .zip de origen, compile el código fuente, archive el exec en un .zip y despliegue la acción Go ejecutando lo siguiente.

```bash
make deploy
```
{: pre}

4. Ahora que se ha creado `go-action-hello`, invoque la acción.

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

Salida de ejemplo:

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
