---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: actions, serverless, javascript, node, node.js, functions

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
{:external: target="_blank" .external}


# Preparación de apps para acciones
{: #prep}

Tanto si aporta una app como si escribe un script específicamente para responder a un suceso, el código debe cumplir ciertos requisitos para que se pueda crear una acción a partir del mismo.
{: shortdesc}

Cada lenguaje de programación tiene requisitos específicos para ejecutarse, pero la mayoría de ellos tienen los siguientes requisitos generales:
- De forma predeterminada, el nombre esperado para el punto de entrada en el código es `main`. Si el punto de entrada no es `main`, se puede especificar un nombre personalizado al crear la acción; si es el caso, tome nota de ese nombre.
- Los parámetros de entrada a la app y los resultados de salida de la app deben estar formateados con una estructura específica que se puede pasar entre entidades. La estructura depende del lenguaje en que está el código. Por ejemplo, con las apps Python, los parámetros de entrada deben ser un diccionario y el resultado de la app debe estar estructurado en forma de diccionario. Porque también puede pasar parámetros en un objeto estructurado a la acción. En JSON, por ejemplo, puede estructurar el código para que espere un parámetro de entrada con valores JSON de determinados campos, como `name` y `place`.

    **Ejemplo de entrada JSON**
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    **Ejemplo de JavaScript**
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- Si la app contiene varios archivos, deben combinarse en un solo archivo a utilizar en una acción. Puede volver a escribir el código en un solo archivo o bien empaquetar los archivos y dependencias en un solo archivo de archivado. Si el entorno de ejecución no está soportado, puede empaquetar la app en una imagen de Docker.
- Las dependencias también deben estar empaquetadas con la app. Los entornos de ejecución disponibles ofrecen algunos paquetes y extensiones preinstalados. [Revise la información de referencia de su entorno de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes) para ver si ya se ha incluido una dependencia de su app con el entorno de ejecución. Si la dependencia ya está incluida, no es necesario empaquetarla con la app.

    No es necesario compilar el código, pero si su entorno de ejecución lo permite, compilar previamente el código puede mejorar el rendimiento.
    {: tip}

## Preparación de apps en imágenes de Docker
{: #prep_docker}

Con {{site.data.keyword.openwhisk_short}}, puede escribir la app en cualquier lenguaje y empaquetarla como una imagen de Docker.
{: shortdesc}

Sólo puede utilizar imágenes de registros públicos, como una imagen que esté disponible públicamente en Docker Hub. No se admiten registros privados.
{: important}

### Empaquetado de código en imágenes de Docker
{: #prep_docker_pkg}

Su código se compila en un ejecutable y se incluye en una imagen Docker. El ejecutable interactúa con el sistema
aceptando la entrada desde `stdin` y respondiendo por medio de `stdout`.
{: shortdesc}

**Antes de empezar**
- Debe tener una cuenta de Docker Hub. Puede configurar una cuenta y un ID de Docker gratuito en [Docker Hub](https://hub.docker.com){: external}.
- [Instale Docker](https://hub.docker.com/search/?offering=community&type=edition){:external}.
- [Revise los requisitos para el entorno de ejecución de Docker](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Para empaquetar la app, complete los pasos siguientes.

Para empaquetar el código como imagen de Docker, ejecute el mandato siguiente.
1. Descargue e instale el esqueleto de Docker. El esqueleto es una plantilla de contenedor de Docker en la que puede inyectar código en forma de binarios personalizados.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configure el código en el esqueleto de caja negra. El esqueleto incluye un programa en C que puede usar. Parte del archivo `example.c` está compilado como parte del proceso de compilación de imagen de Docker, por lo que no necesita C compilado en su máquina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  **Resultado de ejemplo**
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. (Opcional) Añada más código y dependencias a la imagen de Docker modificando el `Dockerfile` para compilar el ejecutable. Tenga en cuenta los requisitos siguientes:
  * El código debe estar en el contenedor en `/action/exec`.
  * El ejecutable recibe un único argumento de la línea de mandatos. Este argumento la serialización de una cadena del objeto JSON que representa los argumentos para la acción.
  * El programa puede redirigir sus registros a `stdout` o `stderr`.
  * Por convenio, la última línea de la salida debe ser un objeto JSON <ph class="ignoreSpelling">en forma de cadena</ph>, lo que represente el resultado de la acción.
  Para obtener más información sobre cómo construir de Dockerfiles, consulte la publicación [Referencia de Dockerfile](https://docs.docker.com/engine/reference/builder/){: external}.

4. Construya la imagen de Docker y suba dicha imagen mediante un script proporcionado.
    1. Inicie la sesión en Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Vaya al directorio `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Ejecute el script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}




## Preparación de apps JavaScript
{: #prep_js}

Antes de crear una acción, prepare su código JavaScript. Confirme que el código está bien estructurado y decida si necesita ser empaquetado.
{: shortdesc}

### Estructuración de código JavaScript
{: #prep_js_struct}

- El nombre esperado para la función de punto de entrada es `main`. Si la función en el código no es `main`, tome nota del nombre para especificarlo cuando se cree la acción.
- Los parámetros de entrada se pasan como un parámetro de objeto JSON.
- El resultado de una activación correcta también es un objeto JSON pero se devuelve de forma distinta según si la acción es [síncrona](#prep_js_sync) o [asíncrona](#prep_js_async).



**Ejemplo**
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

**Ejemplo con varias funciones**

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### Estructuración de código JavaScript con comportamiento síncrono
{: #prep_js_sync}

La activación de JavaScript es síncrona cuando la función principal sale sin ejecutar una sentencia `return` o bien cuando sale ejecutando una sentencia `return` que devuelve cualquier valor, excepto una promesa.
{: shortdesc}

**Ejemplo de código síncrono.**

```javascript
// cada vía de acceso resulta en una función de activación
síncrona principal (params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}




### Estructuración de código JavaScript con comportamiento asíncrono
{: #prep_js_async}

Las funciones de JavaScript pueden continuar la ejecución en una función de devolución de llamada incluso después de un retorno. La activación de JavaScript es asíncrona si la función principal sale de la función devolviendo una promesa. En este caso, el sistema presupone que la acción sigue en ejecución, hasta que se haya cumplimentado o rechazado la promesa. Las funciones de JavaScript que se ejecutan de forma asíncrona pueden devolver el resultado de la activación después de que de la función `main` retorne una promesa en la acción.
{: shortdesc}

Empiece por crear una instancia de un nuevo objeto promesa y pasar una función de devolución de llamada. La devolución de llamada tiene dos argumentos, resolve y reject, ambos son funciones. Todo el código asíncrono va dentro de una devolución de llamada. El manejador de acciones puede tener cualquier nombre siempre que cumpla con la firma convencional de aceptar un objeto y devolverlo (o una `Promise` de un objeto).

En el ejemplo siguiente, puede ver cómo cumplir una promesa llamando a la función de resolución.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
         }, 2000);
      })
}
```
{: codeblock}

Este ejemplo muestra cómo rechazar una promesa llamando a la función reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 2000);
     })
}
```
{: codeblock}

En los ejemplos, se ejecutan los detalles siguientes.
* La función `main` devuelve una promesa. La promesa indica que la acción no se ha completado todavía, pero que se espera que lo haga en el futuro.
* La función JavaScript `setTimeout()` espera 2 segundos antes de llamar a la función de retorno de llamada de la promesa, que representa el código asíncrono.
* La devolución de llamada de la promesa acepta los argumentos `resolve` y `reject`, que son ambos funciones.
  * La llamada a `resolve()` cumple la promesa e indica que la activación se ha completado con normalidad.
  * Se puede utilizar una llamada a `reject()` para rechazar la promesa e indicar que la activación se ha completado de forma anómala.


### Estructuración de código JavaScript con comportamiento síncrono y asíncrono
{: #prep_js_both}

Una acción puede ser síncrona en algunas entradas y asíncrona en otras, tal como se muestra en el ejemplo siguiente.
{: shortdesc}

```javascript
function main(params) {
     if (params.payload) {
        // activación asíncrona
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
         }, 2000);
      })
     } else {
        // activación síncrona
         return {done: true};
      }
}
```
{: codeblock}





### Ejemplo de llamada a una API externa con JavaScript
{: #prep_js_api}

En el ejemplo siguiente se invoca a la API externa del servicio Astronomy Picture of the Day (APOD) de la NASA, que proporciona una imagen única de nuestro universo cada día.
{: shortdesc}


```javascript
let rp = require('request-promise')

function main(params) {
    const options = {
        uri: "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo",
            json: true
        }
        return rp(options)
    .then(res => {
        return { response: res }
    })
}
```
{: codeblock}

Se realiza una llamada a la API APOD de la NASA y los se extraen del resultado JSON.

A continuación, [cree](/docs/openwhisk?topic=cloud-functions-actions) e [invoque a la acción](/docs/openwhisk?topic=cloud-functions-test) para probarla. Se devuelve el objeto de ejemplo siguiente:

```
{
  "copyright": "Eric Houck",
  "date": "2018-03-28",
  "explanation": "Does an alignment like this occur only once in a blue moon? ...",
  "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
  "media_type": "image",
  "service_version": "v1",
  "title": "Blue Moon Tree",
  "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
}
```
{: screen}






### Empaquetado de código JavaScript con el módulo `webpack`
{: #prep_js_pkg}

Puede empaquetar una app utilizando un empaquetador de módulos de JavaScript como, por ejemplo `[webpack ](https://webpack.js.org/concepts/){: external}`. Cuando `webpack` procesa el código, se crea un gráfico de dependencias recursivamente que incluye todos los módulos que necesita la acción.
{: shortdesc}

Antes de empezar, [revise los paquetes que se incluyen con el entorno de ejecución JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

1. Cree un archivo `package.json`. Añada `webpack` como una dependencia de desarrollo.

    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10"
      },
	"dependencies": {
        "left-pad" : "1.1.3"
      },
  "devDependencies": {
        "webpack": "^3.8.1"
      }
    }
    ```
    {: codeblock}

2. Guarde el siguiente código de configuración de `webpack` en un archivo denominado `webpack.config.js`.

    ```javascript
    var path = require('path');
    module.exports = {
      entry: './index.js',
  output: {
        path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
      },
  target: 'node'
    };
    ```
    {: codeblock}

3. Prepare el código de su app. En este ejemplo, que puede guardar como un archivo denominado `index.js`, la variable `global.main` se establece como la función main de la app.

    **Ejemplo**
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Instale todas las dependencias localmente.

    ```
    npm install
    ```
    {: pre}

5. Cree el paquete de `webpack`.

    ```
    npm run build
    ```
    {: pre}

    El archivo `dist/bundle.js` se crea y despliega como el código de origen de la acción.

6. Cree la acción utilizando el script `npm` o la CLI.

    * Ejecute el siguiente script `npm`.

        ```
        npm run deploy
        ```
        {: pre}

    * Ejecute el siguiente mandato de CLI.

        ```
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    El archivo de paquete que `webpack` crea solo da soporte a dependencias de JavaScript. Las invocaciones a la acción podrían fallar si el paquete tiene otras dependencias porque estas dependencias no se incluyen con el archivo `bundle.js`.
    {: tip}



### Empaquetar código JavaScript como archivos NPM
{: #prep_js_npm}

Como alternativa a grabar todo el código de acción en un único archivo de origen JavaScript, puede empaquetar el código en forma de paquete `npm` en un archivo .zip.
{: shortdesc}

Antes de empezar, [revise los paquetes que se incluyen con el entorno de ejecución JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

1. En el directorio raíz, cree un archivo `package.json`. 

**Ejemplo**

    ```json
    {
      "name": "my-action",
  "main": "index.js",
  "dependencies" : {
        "left-pad" : "1.1.3"
      }
    }
    ```
    {: codeblock}

2. Instale todas las dependencias localmente.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    La mayoría de los paquetes de `npm` instalan orígenes JavaScript en `npm install`, algunos también instalan y compilan artefactos de archivo binario. El archivo de archivado solo da soporte a dependencias de JavaScript. Si el archivo de archivado incluye dependencias de archivo binario, es posible que las invocaciones de acción no tengan éxito.
    {: note}

3. Cree un archivo `.zip` que contenga todos los archivos, incluidas todas las dependencias.

    ```
    zip -r action.zip *
    ```
    {: pre}

    **Usuarios de Windows** El uso de la acción del Explorador de Windows para crear el archivo .zip da como resultado una estructura de archivos incorrecta. Las acciones de .zip de {{site.data.keyword.openwhisk_short}} deben tener `package.json` en la raíz del archivo de archivado, pero el Explorador de Windows lo pone dentro de una carpeta anidada. Utilice el mandato `zip` en su lugar.
    {: tip}





## Preparación de apps Go
{: #prep_go}

Utilice un archivo individual para fines de desarrollo o pruebas rápidas. En apps de producción, precompile las acciones Go en un ejecutable para obtener un mejor rendimiento o para el soporte de varios archivos de origen, incluyendo bibliotecas de proveedores.
{: shortdesc}

Aunque puede crear un archivo comprimido en cualquier plataforma Go mediante la compilación cruzada con
`GOOS=Linux` y `GOARCH=amd64`, utilice la característica de precompilación incorporada en la imagen del contenedor del entorno de tiempo de ejecución. Puede empaquetar [varios archivos de origen](#prep_go_multi) o
[bibliotecas de proveedor](#prep_go_vendor).
{: tip}


### Estructuración de código Go
{: #prep_go_struct}

- El nombre esperado para el paquete de punto de entrada es `main`. Si el paquete en el código no es `main`, tome nota del nombre para especificarlo cuando se cree la acción.
- El paquete debe ser público.

**Ejemplo**
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

### Empaquetado de varios archivos de origen Go
{: #prep_go_multi}

1. Cree un directorio de nivel superior `src`. Coloque los archivos de origen que pertenecen al paquete principal en la raíz de `src` o bien dentro de un directorio `main` y cree subdirectorios para los demás paquetes. Por ejemplo, el paquete
`hello` pasa a ser el directorio `src/hello`.
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. Importe los subpaquetes. Ejemplo de `main/main.go` importando el subpaquete hello.

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

  Ejemplo de `hello/hello.go`.

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

3. Compile el código. Cree un archivo .zip del directorio `src`. No incluya el directorio de proyecto de nivel superior (`go-action-project/`).

  ```bash
  cd src
zip -r ../hello-src.zip *
cd ..
  ```
  {: pre}

  Puede compilar de forma local estableciendo la variable `GOPATH` en el padre del directorio
`src`. Si utiliza VS Code, debe cambiar el valor de `go.inferGopath` a `true`.
  {: note}

4. Compile y empaquete el ejecutable Go como `exec` en la raíz del archivo .zip. Compile el archivo
`hello-bin.zip` ejecutando el mandato siguiente. Debe tener la CLI de Docker instalada en la estación de trabajo y
`docker` en `PATH`.

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  En este ejemplo, la función main es `-compile main`. Para utilizar una función distinta como main, cambie el valor de
`-compile`. La función main se selecciona en tiempo de compilación. Al precompilar, `ibmcloud fn action [update | create]` ignora el
`--main`.

  El contenedor obtiene el contenido del .zip de origen en `stdin`, compila el contenido y crea un nuevo archivo .zip con el ejecutable `exec` en la raíz. El contenido del archivo .zip se transmite a `stdout` que se redirige al archivo
`hello-bin.zip` para su despliegue como acción Go.




### Empaquetado de código Go con bibliotecas de proveedor
{: #prep_go_vendor}

Puede incluir dependencias llenando un directorio `vendor` dentro del archivo `zip` de origen al compilar los archivos Go. El directorio `vendor` no funciona en el nivel superior. Debe ubicar el directorio `vendor` dentro de `src/` y dentro de un directorio de paquetes.
{: shortdesc}

Ejemplo de paquete de registros `logrus` en una app `hello.go`.

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
En este ejemplo, el directorio `vendor` se encuentra en `src/hello/vendor`. Puede añadir bibliotecas de terceros utilizadas por el paquete `hello`. 

Puede utilizar varias herramientas como [<code>dep</code> ](https://golang.github.io/dep/docs/installation.html){: external} para llenar y gestionar dependencias.

Puede utilizar `dep` creando un archivo `src/main/Gopkg.toml` que describe la versión y la ubicación de las bibliotecas.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Rellene el directorio `vendor` ejecutando `dep ensure`.




## Preparación de apps Swift
{: #prep_swift}

Los archivos Swift deben estar compilados para que se pueda ejecutar una acción. Este retraso se conoce como retraso de inicio en frío. Para evitar el retraso de inicio en frío, puede compilar el archivo Swift y luego cargarlo en {{site.data.keyword.openwhisk_short}} en un archivo .zip. El entorno de ejecución de Docker incluye un compilador para ayudar a los usuarios a compilar y empaquetar acciones Swift 4.2. Las subsiguientes llamadas a la acción serán mucho más rápidas hasta que se purgue el contenedor con la acción.

Las acciones Swift se ejecutan en un entorno Linux. Swift en Linux aún está en desarrollo, y {{site.data.keyword.openwhisk_short}} utiliza el release disponible más reciente. Es posible que estos releases no sean estables. La versión de Swift que se utiliza con {{site.data.keyword.openwhisk_short}} podría no ser coherente con versiones de Swift de releases estables de Xcode en macOS.
{: important}


### Estructuración de código Swift
{: #prep_swift_struc}

El nombre esperado para la función de punto de entrada es `main`. Si la función en el código no es `main`, tome nota del nombre para especificarlo cuando se cree la acción.

Además de la firma para la función main, Swift 4 proporciona dos firmas más para sacar partido del tipo [<code>Codable</code> ](https://developer.apple.com/documentation/swift/codable){: external}. Puede obtener más información sobre los tipos de datos que se pueden [codificar y descodificar para la compatibilidad con representaciones externas como JSON ](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types){: external}.

**Ejemplo**
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
        completion(result, nil)
    }
```
{: codeblock}

Este ejemplo toma un parámetro de entrada como `entrada codificable (Codable Input)` con un campo `name` y devuelve una `salida codificable (Codable output)` con un campo `greetings`.


#### Manejo de errores en Swift
{: #prep_swift_error}

Con la utilización del manejador de terminación `Codable`, se pueden pasar errores para indicar una anomalía en su acción. El [manejo de errores en Swift](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html){: external} es parecido al manejo de excepciones en otros lenguajes, con la utilización de las palabras clave `try`, `catch` y `throw`.
{: shortdesc}

En el siguiente fragmento de código se muestra un ejemplo de manejo de un error.

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Devolver error real
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}


### Empaquetado de un archivo Swift 4.2
{: #prep_swift42_single}

Compile un único archivo de origen que no dependa de bibliotecas externas. Utilice el distintivo `-compile` con el nombre del método main.

**Antes de empezar**
- [Instale Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Revise los paquetes que se incluyen con el entorno de ejecución Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

Empaquete la app.

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

El contenedor de Docker lee el contenido del archivo de `stdin` y escribe un archivo .zip con el ejecutable swift compilado en
`stdout`.



### Empaquetar proyectos y dependencias de varios archivos Swift 4.2
{: #prep_swift42_multi}

**Antes de empezar**
- [Instale Docker](https://hub.docker.com/search/?offering=community&type=edition){: external}.
- [Revise los paquetes que se incluyen con el entorno de ejecución Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

Empaquete la app.

1. Para compilar varios archivos e incluir dependencias externas, cree la estructura de directorios siguiente.

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  El directorio `Sources/` contiene un archivo denominado `main.swift`.

  El `Package.swift` debe empezar con un comentario que especifique la versión `4.2` para el conjunto de herramientas Swift:

  ```swift
  // swift-tools-version:4.2
import PackageDescription

  let package = Package(
      name: "Action",
      products: [
      .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
      targets: [
      .target(
        name: "Action",
        dependencies: ["SwiftyRequest"],
        path: "."
    )
      ]
  )
  ```
  {: codeblock}

2. Cree un archivo .zip con el contenido del directorio.

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. Pase el archivo .zip al contenedor de Docker a través de `stdin`. `stdout` es un nuevo archivo .zip con el ejecutable compilado. El contenedor de Docker lee el contenido del archivo .zip de `stdin` y escribe un nuevo archivo .zip con el ejecutable Swift compilado en `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  En un sistema basado en Linux, puede combinar los pasos de `zip` y `docker run` en un único mandato:

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Empaquetado de apps Python
{: #prep_python}


### Estructuración de código Python
{: #prep_python_struct}

- Las apps Python deben consumir un
diccionario y generar un diccionario.
- El nombre esperado para el método de punto de entrada es `main`. Si la función en el código no es `main`, tome nota del nombre para especificarlo cuando se cree la acción.
{: shortdesc}

**Ejemplo**
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
	return {"greeting": greeting}
```

### Estructuración de código Python
{: #prep_python_pkg}

Empaquete el código Python y los módulos dependientes en un archivo .zip. En este ejemplo, el archivo de origen que contiene el punto de entrada es `__main__.py` y los módulos de ayudante se encuentran en un archivo denominado `helper.py`.

Antes de empezar, [revise los paquetes que se incluyen con el entorno de ejecución Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

Para empaquetar la app, ejecute el mandato siguiente.

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Empaquetamiento de código Python con un entorno virtual en archivos zip
{: #prep_python_virtenv}

Puede empaquetar dependencias de Python utilizando un entorno virtual, `virtualenv`. Utilizando el entorno virtual, puede enlazar más paquetes que se pueden instalar utilizando [`pip` ](https://packaging.python.org/tutorials/installing-packages/){: external}.

Antes de empezar, [revise los paquetes que se incluyen con el entorno de ejecución Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

Empaquete la app completando los pasos siguientes.

1. Cree un archivo [requirements.txt ](https://pip.pypa.io/en/latest/user_guide/#requirements-files){: external} que contenga las versiones y los módulos de `pip` a instalar.

  Para mantener `virtualenv` en un tamaño mínimo, añada únicamente los módulos que no son parte del entorno de ejecución seleccionado a `requirements.txt`. Para obtener más información sobre los paquetes que se incluyen en los entornos de ejecución Python, consulte la
[referencia de entorno de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) de Python.
  {: tip}

2. Extraiga una de las siguientes imágenes del tiempo de ejecución. Para asegurar la compatibilidad con el contenedor de entorno de ejecución, las instalaciones empaquetadas dentro de un entorno virtual deben utilizar la imagen que corresponde al entorno de ejecución que se especifica.
    * Para `python:3.7`, utilice la imagen de Docker `ibmfunctions/action-python-v3.7`.
    * Para `python:3.6`, utilice la imagen de Docker `ibmfunctions/action-python-v3.6`.
    * Para `python:2`, utilice la imagen de Docker `openwhisk/python2action`.

   **Ejemplo**
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. Instale las dependencias y cree un entorno virtual. El directorio virtualenv debe denominarse `virtualenv`.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Empaquete el directorio `virtualenv` y todos los archivos Python adicionales. El archivo fuente que contiene el punto de entrada se debe denominar `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}


## Preparación de apps Ruby
{: #prep_ruby}

Antes de crear una acción, prepare su código Ruby.

### Estructuración de código Ruby
{: #prep_ruby_struct}

* Las acciones Ruby siempre consumen un hash (recopilación de tipo diccionario) y devuelven un hash.
* El nombre esperado para la función de punto de entrada es `main`. Si la función en el código no es `main`, tome nota del nombre para especificarlo cuando se cree la acción.


**Ejemplo**

```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
```
{: codeblock}

### Empaquetado de código Ruby
{: #prep_ruby_pkg}

Puede empaquetar una app Ruby y sus paquetes dependientes en un archivo .zip. Por ejemplo, puede empaquetar una acción con un segundo archivo denominado `helper.rb`.

Cree un archivador que contenga los archivos fuente. El archivo de origen que contiene el punto de entrada se debe denominar `main.rb`.

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

Las gemas `mechanize` y `jwt` están disponibles además de las gemas predeterminadas y empaquetadas. Puede utilizar gemas arbitrarias siempre que utilice acciones comprimidas para empaquetar todas las dependencias.



## Preparación de apps PHP
{: #prep_php}

Antes de crear una acción, prepare su código PHP.

### Estructuración de código PHP
{: #prep_php_struct}

- Las acciones PHP siempre consumen una matriz asociativa y devuelven una matriz asociativa.
- El nombre esperado para la función de punto de entrada es `main`. Si la función en el código no es `main`, tome nota del nombre para especificarlo cuando se cree la acción.

**Ejemplo**
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### Empaquetado de código PHP
{: #prep_php_pkg}

Puede empaquetar archivos PHP o paquetes dependientes en un archivo .zip.

Antes de empezar, [revise los paquetes que se incluyen con el entorno de ejecución PHP](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php) para ver si ya se ha incluido una dependencia de la app con el entorno de ejecución. Si su dependencia no está incluida, debe empaquetarla con la app.

Para empaquetar la app, ejecute el mandato siguiente.

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

**Ejemplo**
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

## Preparación de apps Java
{: #prep_java}

Antes de crear una acción, prepare su código Java.

### Estructuración de código Java
{: #prep_java_struct}

Una acción Java es un programa Java con un método llamado `main`. `main` debe tener la firma siguiente.

**Ejemplo**
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

* Debe especificar el nombre de la clase principal con `--main`. Una clase principal apta es una que implemente un método `main` estático. Si la clase no está en el paquete predeterminado, utilice el nombre de clase completo de Java, por ejemplo `--main com.example.MyMain`.
* Puede personalizar el nombre de método de la acción Java especificando el nombre de método completo de la acción, por ejemplo `--main com.example.MyMain#methodName`.

### Empaquetado de código Java
{: #prep_java_pkg}


**Antes de empezar**
Debe tener [JDK 8](http://openjdk.java.net/install/){: external} instalado localmente. En este ejemplo se utiliza [`google-gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/){: external}.

Si trabaja con una versión de JDK distinta de JDK 8, debe especificar `--release 8` al compilar el código con el mandato `javac`.
{: note}

Para crear una acción Java, complete los pasos siguientes.

1. Guarde el siguiente código en un archivo denominado `Hello.java`.

  ```java
  import com.google.gson.JsonObject;
    public class Hello {
      public static JsonObject main(JsonObject args) {
          String name = "stranger";
          if (args.has("name"))
              name = args.getAsJsonPrimitive("name").getAsString();
          JsonObject response = new JsonObject();
          response.addProperty("greeting", "Hello, " + name + "!");
        return response;
    }
  }
  ```
  {: codeblock}

2. Descargue el archivo [`gson-2.8.5.jar`](http://central.maven.org/maven2/com/google/code/gson/gson/2.8.5/).

3. Añada `gson-2.8.5.jar` a la `ClASSPATH`. En este ejemplo se utiliza `gson-2.8.5.jar` que está guardado en una carpeta `test` en el directorio `Desktop`.
  
  ```
  export CLASSPATH=$CLASSPATH:/Users/Desktop/test/gson-2.8.5.jar
  ```
  {: pre}

4. Añada la carpeta `bin` de JDK a la `CLASSPATH`. En este ejemplo se utiliza `openjdk-8`.
  
  ```
  export CLASSPATH=$CLASSPATH:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: pre}

5. Verifique que la carpeta `bin` de JDK y el archivo `gson-2.8.5.jar` estén en la `CLASSPATH`.
  ```
  echo $CLASSPATH
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  /Desktop/test/gson-2.8.5.jar:/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/bin
  ```
  {: screen}

6. Vaya a la carpeta donde está guardado el archivo `Hello.java`. En este ejemplo, el archivo `Hello.java` está guardado en la carpeta `Desktop/test`.
  ```
  cd Desktop/test
  ```
  {: pre}

7. Compile el archivo `Hello.java` en un archivo de clase.
  ```
  javac Hello.java
  ```
  {: pre}

8. Comprima el archivo de clase en un archivo .jar denominado `hello.jar`.

  ```
  jar cvf hello.jar Hello.class
  ```
  {: pre}

**Pasos siguientes**
Puede crear una acción con el archivo `hello.jar`. Como que el archivo de clase que ha creado no utiliza el nombre predeterminado `main`, debe establecer el distintivo `--main` en `Hello` al crear la acción. El distintivo `--main` debe coincidir con la clase (`class`) Java. Para obtener más información, consulte [Creación de acciones](/docs/openwhisk?topic=cloud-functions-actions).
 
Cuando actualice el código Java, debe repetir estos pasos para volver a compilar el código en un nuevo archivo `.jar`.
{: note}

### Empaquetado de código Java con Gradle
{: #prep_java_gradle}

En lugar de compilar desde la línea de mandatos, puede utilizar una herramienta de compilación como por ejemplo [Gradle](https://gradle.org){: external} para captar las bibliotecas de un repositorio como Maven Central. Puede utilizar Gradle para captar y compilar un archivo de archivado .jar final que incluya el código y todas las dependencias.

A continuación se muestra un ejemplo que utiliza Gradle para compilar una acción Java que hace uso de la biblioteca
`com.google.zxing` que proporciona la función de generar una imagen de código QR.

1. Cree un archivo denominado `build.gradle` y especifique las dependencias.

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
  {: codeblock}

2. Ejecute el mandato `gradle jar`, que genera un archivo .jar en el directorio `build/libs/`.

  Para obtener más información, consulte [Declaración de dependencias](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies){: external} en la documentación de Gradle.


## Preparación de apps .NET Core
{: #prep_dotnet}

Antes de crear una acción, prepare su código .NET Core.

### Estructuración de código .NET Core
{: #prep_dotnet_struct}

Una acción .NET Core es una biblioteca de clases de .NET Core con un método que se espera que se denomine `Main`. Si el método en el código no se llama `Main`, tome nota del nombre para especificarlo cuando se cree la acción con el formato: `--main {Assembly}::{Class Full Name}::{Method}`

**Ejemplo**
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### Empaquetado de código .NET Core
{: #prep_dotnet_pkg}

**Antes de empezar**
Para compilar, probar y archivar proyectos .NET Core, debe:
- Instalar [.NET Core SDK](https://dotnet.microsoft.com/download){: external} localmente.
- Establecer la variable de entorno `DOTNET_HOME` en la ubicación donde se puede encontrar el ejecutable `dotnet`.



Para empaquetar el código, ejecute los mandatos siguientes.

  1. Cree un proyecto C# denominado `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. Vaya al directorio `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. Instale el [paquete de <ph class="ignoreSpelling">Newtonsoft.Json NuGet</ph>](https://www.nuget.org/packages/Newtonsoft.Json/){: external}.

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. Guarde el código siguiente en un archivo denominado `Hello.cs`.

      ```csharp
      using System;
    using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
    {
          public class Hello
        {
              public JObject Main(JObject args)
            {
                  string name = "stranger";
                if (args.ContainsKey("name")) {
                      name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
          }
      }
      ```
      {: codeblock}

  5. Compile `Hello.cs` y cualquier otro archivo con salida en el directorio `out`.

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. Vaya al directorio `out`.

      ```bash
      cd out
      ```
      {: pre}

  7. Comprima los archivos publicados.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}



