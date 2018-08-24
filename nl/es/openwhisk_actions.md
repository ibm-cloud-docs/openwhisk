---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creación e invocación de acciones
{: #openwhisk_actions}

Las acciones son fragmentos de código sin estado que se ejecutan en la plataforma {{site.data.keyword.openwhisk}}. Por ejemplo, una acción se puede utilizar para detectar las caras de una imagen, responder a un cambio en la base de datos, agregar un conjunto de llamadas de API o publicar un tweet.
{:shortdesc}

Las acciones se pueden invocar explícitamente, o ejecutarse como respuesta a un suceso. En cualquier caso, cada ejecución de una
acción tiene como resultado un registro de activación que se identifica mediante un ID de activación exclusivo. La entrada a una acción y el resultado de la acción son un diccionario de pares de clave/valor, en el que la clave es una serie y el valor es un valor JSON válido. Las acciones también se pueden componer de llamadas a otras acciones, o de una secuencia definida de acciones.

Una acción se puede escribir como una función JavaScript, Swift, Python, PHP, como un método Java, o como un ejecutable binario personalizado, como los programas Go, y como ejecutables personalizados empaquetados en un contenedor Docker. Aprenda a crear, invocar y depurar acciones en el entorno de desarrollo que elija.

## Creación de acciones de JavaScript
{: #creating-and-invoking-javascript-actions}

Las secciones siguientes le guían en la forma de trabajar con acciones en JavaScript. Empiece por crear e invocar una acción simple. A continuación, añada parámetros a la acción e invoque dicha acción con parámetros. Después, establezca parámetros predeterminados e invóquelos. Por último, cree acciones asíncronas.

### Creación e invocación de una acción JavaScript sencilla
{: #openwhisk_single_action_js}

Revise los pasos y ejemplos siguientes para crear su primera acción JavaScript.

1. Guarde el código siguiente en un archivo denominado `hello.js`.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  El archivo JavaScript podría tener funciones adicionales. No obstante, por convenio, debe existir una función llamada `main` para proporcionar un punto de entrada para la acción.

2. Cree una acción denominada `hello` con la función JavaScript.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created action hello
  ```
  {: screen}

  La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.js`, la acción se ejecuta utilizando el tiempo de ejecución Node.js 6. También puede crear una acción que se ejecute con Node.js 8 especificando de forma explícita el parámetro `--kind nodejs:8`. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_javascript_environments) de Node.js 6 en comparación con 8.

3. Verifique que su acción `hello` se encuentra en su lista de acciones.
  ```
  ibmcloud fn action list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  actions
  hello       private
  ```
  {: screen}

4. Ejecute la acción en la nube ejecutando una invocación de bloqueo. Las invocaciones de bloqueo utilizan un estilo de solicitud/respuesta y esperan a que el resultado de la activación esté disponible. El período de espera es inferior a 60 segundos o el [valor de límite de tiempo](./openwhisk_reference.html#openwhisk_syslimits) de la acción.
    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    El mandato genera la siguiente información:
        * El ID de activación (` 44794bd6aab74415b4e42a308d880e5b`), que se puede utilizar para recuperar los registros o el resultado de la invocación
        * El resultado de la invocación si está disponible en el período de espera previsto.
    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result":{
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    Para obtener sugerencias sobre las activaciones de supervisión, consulte [Supervisión de la salida de acción](openwhisk_managing.html#monitor-action-output).
    {: tip}

5. Si no necesita el resultado de la acción inmediatamente, puede omitir el distintivo `--blocking`para ejecutar una invocación sin bloqueo.
    1. Ejecute una acción sin bloqueo.
        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Salida de ejemplo:
        ```
        ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Utilice el ID de activación para obtener el resultado de la acción.
        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        Se devuelve el resultado de la acción:
        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. Si olvida registrar el ID de activación, puede obtener una lista de activaciones ordenadas de más recientes a menos recientes.
    ```
    ibmcloud fn activation list
    ```
    {: pre}

    Salida:
    ```
    activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
    ```
    {: screen}

### Creación de acciones asíncronas
{: #openwhisk_asynchrony_js}

Las funciones de JavaScript que se ejecutan de forma asíncrona pueden devolver el resultado de la activación tras el retorno de la función `main`.

1. Guarde el código siguiente en un archivo denominado `asyncAction.js`.
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

    * La función `main` devuelve una Promise. Promise indica que la acción no se ha completado todavía, pero que se espera que lo haga en el futuro.
    * La función JavaScript `setTimeout()` espera 2 segundos antes de llamar a la función de retorno de llamada de Promise, que representa el código asíncrono.
    * La devolución de llamada de Promise acepta los argumentos `resolve` y `reject`, que son ambos funciones.
      * La llamada a `resolve()` rellena el Promise e indica que la activación se ha completado con normalidad.
      * Una llamada a `reject()` se puede utilizar para rechazar el Promise e indicar que la activación se ha completado de forma anómala.

2. Cree una acción denominada `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js
    ```
    {: pre}

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result asyncAction
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "done": true
  }
    ```
    {: screen}

3. Compruebe cuánto tiempo tardó la activación en completarse obteniendo el registro de activación.

  1. Obtenga el ID de activación.
      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Salida de ejemplo:
      ```
      activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Obtenga el registro para el ID de activación.
      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      El valor de `duration` muestra que la activación tardó algo más de 2 segundos en completarse:
      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

### Uso de acciones para invocar una API externa
{: #openwhisk_apicall_action}

Los ejemplos proporcionados hasta ahora son funciones JavaScript autocontenidas. También puede crear una acción que invoque una API externa.
{: shortdesc}

En el ejemplo siguiente se invoca el servicio Astronomy Picture of the Day (APOD) de la NASA, que proporciona una imagen única de nuestro universo cada día.

1. Guarde el siguiente código en un archivo denominado `apod.js`.
    ```javascript
    var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

    $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
        $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
        $("#copyright").text("Image Credits: " + "Public Domain");
      }

      if(result.media_type == "video") {
        $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
        $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
      $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
    });
    ```
    {: codeblock}

    Se realiza una llamada a la API APOD de la NASA y los se extraen del resultado JSON. Para obtener más información sobre los paquetes de Node.js que se pueden utilizar en sus acciones, consulte [Límites y detalles del sistema](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

2. Cree una acción denominada `apod`.
    ```
    ibmcloud fn action create apod apod.js
    ```
    {: pre}

3. Invoque la acción `apod`.
    ```
    ibmcloud fn action invoke --result apod
    ```
    {: pre}

    Se devuelve el objeto de ejemplo siguiente:
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

### Empaquetado de una acción como un módulo Node.js
{: #openwhisk_js_packaged_action}

Como alternativa a grabar todo el código de acción en un único archivo de origen JavaScript, puede grabar una acción como un paquete `npm`.

Por ejemplo, considere un directorio con los archivos siguientes:

1. Guarde el código siguiente en un archivo denominado `package.json`.
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

2. Guarde el código siguiente en un archivo denominado `index.js`.
    ```javascript
    function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}
    * La acción se expone a través de `exports.main`.
    * El manejador de acciones puede tener cualquier nombre siempre que cumpla con la firma convencional de aceptar un objeto y devolverlo (o una `Promise` de un objeto).
    * Debe denominar este archivo como **index.js** o especificar el nombre de archivo que prefiera como la propiedad `main` en **package.json**.

3. Instale todas las dependencias localmente.
    ```
    npm install
    ```
    {: pre}
    **Nota**: La mayoría de los paquetes de `npm` instalan orígenes JavaScript en `npm install`, algunos también instalan y compilan artefactos binarios. El archivo de archivado actualmente solo da soporte a dependencias de JavaScript. Las invocaciones de acción pueden fallar si el archivo incluye dependencias binarias.

4. Cree un archivo `.zip` que contenga todos los archivos, incluidas todas las dependencias.
    ```
    zip -r action.zip *
    ```
    {: pre}

    El uso de la acción de Windows Explorer para crear el archivo zip da lugar a una estructura incorrecta. Las acciones zip de {{site.data.keyword.openwhisk_short}} deben tener `package.json` como raíz del zip, mientras que Windows Explorer las coloca dentro de una carpeta anidada. La opción más segura consiste en utilizar el mandato `zip` desde la línea de mandatos.
    {: tip}

5. Cree la acción. Al crear una acción desde un archivo `.zip`, debe proporcionar explícitamente un valor para el distintivo `--kind` utilizando `nodejs:6` o `nodejs:8`.
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
    ```
    {: pre}

6. Invoque la acción.
    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "padded": [
            ".......................and now",
          "......for something completely",
          ".....................different"
      ]
    }
    ```
    {: screen}

### Empaquetado de una acción como un paquete individual
{: #openwhisk_js_webpack_action}

Si el empaquetamiento de la acción como zip incluye demasiados archivos innecesarios o si necesita un despliegue más rápido, puede escribir el código mínimo en un archivo `.js` individual que incluya las dependencias.
{: shortdesc}

Puede empaquetar una acción utilizando un empaquetador de módulos de JavaScript como, por ejemplo, [webpack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") ](https://webpack.js.org/concepts/). Cuando `webpack` procesa el código, se crea un gráfico de dependencias recursivamente que incluye todos los módulos que necesita la acción.

1. Guarde el código siguiente en un archivo denominado `package.json`. `webpack` se añade como una dependencia de desarrollo.
    ```json
    {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

2. Guarde el siguiente código de configuración de webpack en un archivo denominado `webpack.config.js`.
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

3. Guarde el código siguiente en un archivo denominado `index.js`. La variable `global.main` se establece en la función main de la acción.
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

5. Cree el paquete de webpack.
    ```
    npm run build
    ```
    {: pre}

    El archivo `dist/bundle.js` se crea y despliega como el código de origen de la acción.

6. Cree la acción utilizando el script `npm` o la CLI.
    * Utilización del script `npm`:
        ```
        npm run deploy
        ```
        {: pre}

    * Utilizando la CLI:
        ```
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **Nota**: El archivo de paquete que `webpack` crea solo da soporte a dependencias de JavaScript. Las invocaciones a la acción podrían fallar si el paquete depende de dependencias binarias porque estas no se incluyen con el archivo `bundle.js`.

## Creación de acciones Python
{: #creating-python-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Python y la adición de parámetros a dicha acción.

### Creación e invocación de una acción Python
{: #openwhisk_actions_python_invoke}

Una acción es sencillamente una función Python de nivel superior. Para crear una acción de Python:

1. Guarde el código siguiente en un archivo denominado `hello.py`.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
        return {"greeting": greeting}
    ```
    {: codeblock}

  * Las acciones Python siempre consumen un
diccionario y generan un diccionario.
  * El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar explícitamente para crear la acción con la CLI de `wsk` utilizando el distintivo `--main`.

2. Creación de una acción `helloPython`.
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: created action helloPython
    ```
    {: screen}

    La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.py`, la acción se ejecuta utilizando el tiempo de ejecución Python 2. También puede crear una acción que se ejecute con Python 3 especificando de forma explícita el parámetro `--kind python:3`. También puede utilizar el entorno de tiempo de ejecución de Python 3 con el tipo `python-jessie:3`, que contiene paquetes adicionales para IBM Cloud Services como, por ejemplo, {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} y {{site.data.keyword.ibmwatson_notm}}. Para obtener más información sobre los paquetes incluidos en este tiempo de ejecución de Python 3, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_python_environments) del tiempo de ejecución de Python.

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Salida de ejemplo:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Empaquetamiento de acciones de Python en archivos zip
{: #openwhisk_actions_python_zip}

Puede empaquetar una acción Python y los módulos dependientes en un archivo zip. Por ejemplo, para crear una acción con un módulo de ayudante denominado `helper.py`:

1. Cree un archivador que contenga los archivos fuente. **Nota**: El archivo fuente que contiene el punto de entrada se debe denominar `__main__.py`.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Cree la acción.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### Empaquetamiento de acciones de Python con un entorno virtual en archivos zip
{: #openwhisk_actions_python_virtualenv}

Puede empaquetar dependencias de Python utilizando un entorno virtual, `virtualenv`. El entorno virtual permite enlazar paquetes adicionales que se pueden instalar utilizando [`pip` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://packaging.python.org/installing/), por ejemplo.

Para instalar dependencias, empaquételas en un entorno virtual y cree una acción de OpenWhisk compatible:

1. Cree un archivo [requirements.txt ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) que contenga las versiones y los módulos de `pip` a instalar.

2. Instale las dependencias y cree un entorno virtual. El directorio virtualenv debe denominarse `virtualenv`. Para asegurar la compatibilidad con el contenedor de tiempo de ejecución de OpenWhisk, las instalaciones de paquetes dentro de un entorno virtual deben utilizar la imagen que corresponde al tipo.
    - Para el tipo `python:2`, utilice la imagen de docker `openwhisk/python2action`.
    - Para el tipo `python:3`, utilice la imagen de docker `openwhisk/python3action`.
    - Para el tipo `python-jessie:3`, utilice la imagen de docker `ibmfunctions/action-python-v3`.
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. Empaquete el directorio `virtualenv` y todos los archivos Python adicionales. El archivo fuente que contiene el punto de entrada se debe denominar `__main__.py`.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Cree la acción `helloPython`.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Añada únicamente los módulos que no son parte del entorno de tiempo de ejecución seleccionado para `requirements.txt`. Esto ayuda a mantener un tamaño mínimo de `virtualenv`.
{: tip}

## Creación de acciones PHP
{: #creating-php-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción PHP y la adición de parámetros a dicha acción.

### Creación e invocación de una acción PHP
{: #openwhisk_actions_php_invoke}

Una acción es sencillamente una función PHP de nivel superior. Para crear una acción PHP:

1. Guarde el código siguiente en un archivo llamado `hello.php`.
    ```
    <?php
function main(array $args) : array
{
        $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
    ```
    {: codeblock}

    * Las acciones PHP siempre consumen una matriz asociativa y devuelven una matriz asociativa.
    * El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar explícitamente cuando crea la acción con la CLI de `ibmcloud` utilizando el distintivo `--main`.

2. Cree una acción denominada `helloPHP`.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.php`, la acción se ejecuta utilizando el tiempo de ejecución PHP 7.1. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_php) de PHP.

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
    ```
    {: pre}

    Salida de ejemplo:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Empaquetamiento de acciones PHP en archivos zip
{: #openwhisk_actions_php_zip}

Puede empaquetar una acción PHP y otros archivos o paquetes dependientes en un archivo zip. Por ejemplo, para empaquetar una acción con un segundo archivo denominado `helper.php`:

1. Cree un archivador que contenga los archivos fuente. **Nota**: El archivo fuente que contiene el punto de entrada se debe denominar `index.php`.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Cree la acción.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Creación de acciones Swift
{: #creating-swift-actions}

Las siguientes secciones le guían a través de la creación e invocación de una única acción de Swift y por el empaquetado de una acción en un archivo zip.

**Nota:** Acciones Swift se ejecutan en un entorno Linux. Swift en Linux aún está en desarrollo, y {{site.data.keyword.openwhisk_short}} utiliza el release disponible más reciente. Es posible que estos releases no sean estables. La versión de Swift que se utiliza con {{site.data.keyword.openwhisk_short}} podría no ser coherente con versiones de Swift de releases estables de Xcode en MacOS.

Para obtener más información sobre el entorno de tiempo de ejecución, consulte [reference](./openwhisk_reference.html#swift-actions) de Swift.
{: tip}

### Creación e invocación de una acción
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

Una acción es sencillamente una función Swift de nivel superior. Para crear una acción de Swift 3:

1. Guarde el siguiente código en un archivo denominado `hello.swift`.
    ```swift
    func main(args: [String:Any]) -> [String:Any] {
        if let name = args["name"] as? String {
            return [ "greeting" : "Hello \(name)!" ]
        } else {
            return [ "greeting" : "Hello stranger!" ]
        }
    }
    ```
    {: codeblock}

    En este ejemplo, la acción Swift consume un diccionario y genera un diccionario.

2. Cree una acción denominada `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.php`, la acción se ejecuta utilizando el tiempo de ejecución PHP 7.1. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_php) de PHP.

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Salida de ejemplo:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

Además de la firma para la función main, Swift 4 proporciona dos firmas más para sacar partido del tipo [Codable ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/documentation/swift/codable). Obtenga [aquí ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) más información sobre los tipos de datos que se pueden codificar y descodificar para su compatibilidad con representaciones externas como por ejemplo JSON.

1. Guarde el siguiente código en un archivo denominado `hello.swift`.
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

2. Cree una acción denominada `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Salida de ejemplo:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Empaquetado de una acción como un ejecutable Swift
{: #packaging-an-action-as-a-swift-executable}

Cuando cree una acción Swift de {{site.data.keyword.openwhisk_short}} con un archivo fuente Swift, el archivo se debe compilar en un código binario antes de poder ejecutar la acción. Este retraso se conoce como retraso de inicio en frío. Una vez creado el binario, las siguientes llamadas a la acción serán mucho más rápidas hasta que se purgue el contenedor que alberga la acción. Para evitar el retraso de inicio en frío, puede compilar el archivo Swift en un binario y luego cargar dicho binario en {{site.data.keyword.openwhisk_short}} en un archivo zip.

Puede utilizar un script para automatizar el empaquetado de la acción.

**Requisito previo**: En el script utilizado en los siguientes pasos se presupone que tiene un directorio denominado `actions`, en el que cada directorio de nivel superior representa una acción.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. Guarde el código siguiente en un archivo de script denominado `compile.sh`.
    ```bash
    #!/bin/bash
    set -ex

    if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
        exit 2
    fi
    OUTPUT_DIR="build"
    if [ ${2} == "swift:3.1.1" ]; then
      BASE_PATH="/swift3Action"
      DEST_SOURCE="$BASE_PATH/spm-build"
      RUNTIME="openwhisk/action-swift-v3.1.1"
    elif [ ${2} == "swift:4.1" ]; then
      RUNTIME="ibmfunctions/action-swift-v4.1"
      BASE_PATH="/swift4Action"
      DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
    else
      echo "Error: Kind $2 not recognize"
      exit 3
    fi
    DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

    BUILD_FLAGS=""
    if [ -n "$3" ] ; then
        BUILD_FLAGS=${3}
    fi

    echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

    if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

    echo 'Setting up build...'
    cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

    # El archivo de acción puede ser {nombre_acción}.swift o main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Añadir las partes específicas de OW
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

    # Only for Swift4
    if [ ${2} != "swift:3.1.1" ]; then
      echo 'Adding wait to deal with escaping'
      echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
    fi

    echo \"Compiling $1...\"
    cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# Tenemos nuestro propio paquete Package.swift, realizar una compilación completa
swift build ${BUILD_FLAGS} -c release

    echo 'Creating archive $1.zip...'
    #.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
    ```
    {: codeblock}

2. Para añadir dependencias, cree el archivo `Package.swift`. El siguiente ejemplo añade `example-package-deckofplayingcards` como una dependencia. `CCurl`, `Kitura-net` y `SwiftyJSON` se proporcionan en la acción estándar Swift; por eso debe incluirlos en su propio `Package.swift`solo para acciones Swift 3.
    * Sintaxis de ejemplo de Swift 3:
        ```swift
        import PackageDescription

        let package = Package(
     name: "Action",
         dependencies: [
                  .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
                  .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
                  .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
                  .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
                  .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
        )
        ```
        {: codeblock}

    * Sintaxis de ejemplo de Swift 4:
        ```swift
        // swift-tools-version:4.0
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
              .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
            ],
      targets: [
              .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
            ]
        )
        ```
        {: codeblock}

3. Para crear un `hello.zip` en `build`, compile la acción.
    * Swift 3:
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. Cargue el archivo zip en {{site.data.keyword.openwhisk_short}} con el nombre de acción `helloSwiftly`.
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. Invoque la acción.
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    El tiempo que se ha tardado en ejecutar la acción está en la propiedad `duration1.

6. Puede comparar la duración de la invocación de la acción precompilada con la duración de una invocación de mandato con un paso de compilación. Invoque la acción de la sección anterior:
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Manejo de errores en Swift 4
{: #error-handling-swift4}

Con la utilización del manejador de terminación Codable, se pueden pasar errores para indicar una anomalía en su acción. El [Manejo de errores en Swift ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) es parecido al manejo de excepciones en otros lenguajes, con la utilización de las palabras clave `try`, `catch` y `throw`.

En el siguiente fragmento de código se muestra un ejemplo de manejo de un error:
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

## Creación de acciones de Java
{: #creating-java-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Java y la adición de parámetros a dicha acción.

Para poder compilar, probar y archivar archivos Java, debe tener [JDK 8 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo") ](http://openjdk.java.net/install) instalado localmente.

### Creación e invocación de una acción Java
{: #openwhisk_actions_java_invoke}

Una acción Java es un programa Java con un método llamado `main`. `main` debe tener la siguiente firma exacta:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Para crear una acción de Java:

1. Guarde el siguiente código en un archivo denominado `Hello.java`.
    ```java
    import com.google.gson.JsonObject;
    public class Hello {
        public static JsonObject main(JsonObject args) {
            String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
    }
    ```
    {: codeblock}

2. Compile `Hello.java` en un archivo JAR denominado `hello.jar`. **Nota**: [google-gson ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/google/gson) debe existir en su CLASSPATH de Java.
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Cree una acción.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * Debe especificar el nombre de la clase principal con `--main`. Una clase principal apta es una que implemente un método `main` estático. Si la clase no está en el paquete predeterminado, utilice el nombre de clase completo de Java, por ejemplo `--main com.example.MyMain`.
  * Puede personalizar el nombre de método de la acción Java. Esto se realiza especificando el nombre completo del método de la acción, por ejemplo, `--main com.example.MyMain#methodName`.
  * La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen.

4. Invoque la acción.
    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Salida de ejemplo:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Creación de acciones Docker
{: #creating-docker-actions}

Con acciones Docker de {{site.data.keyword.openwhisk_short}}, puede escribir sus acciones en cualquier lenguaje.
{: shortdesc}

Su código se compila en un binario ejecutable y se incluye en una imagen Docker. El programa binario interactúa con el sistema
aceptando la entrada desde `stdin` y respondiendo por medio de `stdout`.   Podrá obtener más información sobre la creación de acciones de Docker en la sección
[Referencias](./openwhisk_reference.html#openwhisk_ref_docker).

Requisito previo: Debe tener una cuenta de Docker Hub. Configure una cuenta y un ID de Docker gratuito en [Docker Hub ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://hub.docker.com).

Para configurar un binario personalizado y utilizar la imagen de Docker cargada como una acción:

1. Descargue e instale el esqueleto de Docker. El esqueleto es una plantilla de contenedor de Docker en la que puede inyectar código en forma de binarios personalizados.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configure su binario personalizado en el esqueleto blackbox. El esqueleto incluye un programa en C que puede usar. Parte del archivo `example.c` está compilado como parte del proceso de compilación de imagen de Docker, por lo que no necesita C compilado en su máquina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Salida de ejemplo:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Opcional: Añada código y dependencias adicionales a la imagen de Docker modificando el `Dockerfile` para compilar el ejecutable. Tenga en cuenta los siguientes requisitos y límites:
  * El binario debe estar en el contenedor en `/action/exec`.
  * El ejecutable recibe un único argumento de la línea de mandatos. Este argumento la serialización de una cadena del objeto JSON que representa los argumentos para la acción.
  * El programa puede redirigir sus registros a `stdout` o `stderr`.
  * Por convenio, la última línea de la salida debe ser un objeto JSON en forma de cadena que represente el resultado de la acción.

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

5. Utilice el contendedor Docker para crear una acción.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Invoque la acción.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Para actualizar la acción de Docker, cargue la imagen más reciente en Docker Hub. Esto permite al sistema extraer la nueva imagen de Docker la próxima vez que ejecute el código para la acción.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. Si hay un contenedor reciente que utiliza una versión anterior de la imagen de Docker, las nuevas invocaciones siguen utilizando esta imagen. Actualice la acción para que las nuevas invocaciones empiecen a utilizar la nueva imagen.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Opcional: Puede utilizar el argumento `--native` como abreviatura de `--docker openwhisk/dockerskeleton`. Este argumento hace que sea más fácil crear y desplegar ejecutables que se ejecutan dentro del SDK de acción de Docker estándar.
    1. Los pasos anteriores crean un ejecutable binario dentro del contenedor ubicado en `/action/exec`. Copie el archivo `/action/exec` en su sistema de archivos locales y empaquételo como zip en `exec.zip`.
    2. Cree una acción de Docker que reciba el ejecutable como datos de inicialización. El argumento `--native` sustituye al argumento `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Creación de acciones Go
{: #creating-go-actions}

El argumento `--native` permite empaquetar los ejecutables de Go como una acción.

Tenga en cuenta los siguientes requisitos y límites.
  * Los ejecutables de Go reciben un único argumento de la línea de mandatos. El argumento es la serialización de una cadena del objeto JSON que representa los argumentos para la acción.
  * El programa puede redirigir sus registros a `stdout` o `stderr`.
  * Por convenio, la última línea de la salida debe ser un objeto JSON en forma de cadena que represente el resultado de la acción.

Para crear una acción Go:

1. Guarde el siguiente código en un archivo denominado `sample.go`.
    ```go
    package main

    import "encoding/json"
    import "fmt"
    import "os"

    func main() {
        //program receives one argument: the JSON object as a string
    arg := os.Args[1]

        // unmarshal the string to a JSON object
        var obj map[string]interface{}
        json.Unmarshal([]byte(arg), &obj)

        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")

        name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

        // last line of stdout is the result JSON object as a string
        msg := map[string]string{"msg": ("Hello, " + name + "!")}
        res, _ := json.Marshal(msg)
        fmt.Println(string(res))
    }
    ```
    {: codeblock}

2. Realice una compilación cruzada de `sample.go` para {{site.data.keyword.openwhisk_short}}. El ejecutable se debe denominar `exec`.
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. Invoque la acción.
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## Creación de secuencias de acciones
{: #openwhisk_create_action_sequence}

Puede crear una acción que encadene juntas una secuencia de acciones. El resultado de una acción se pase como argumento a la acción siguiente.
{: shortdesc}

Se proporcionan varias acciones de utilidad en el paquete `/whisk.system/utils` que puede utilizar para crear su primera secuencia.

1. Lista las acciones en el paquete `/whisk.system/utils`.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Salida de ejemplo:
    ```
    package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
    ```
    {: screen}

2. Utilizando las acciones `split` y `sort`, cree una secuencia de acciones de forma que el resultado de `split` se pase como argumento a `sort`. Esta secuencia de acciones convierte algunas líneas de texto en una matriz, y ordena las líneas.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    En la salida, las líneas de split se clasifican alfabéticamente.
    ```
    {
        "length": 3,
      "lines": [
            "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
    }
    ```
    {: screen}

**Nota**:
* Los parámetros que se pasan entre acciones de la secuencia son explícitos, salvo los predeterminados. Por tanto, los parámetros que se pasan a la secuencia de acción solo están disponibles para la primera acción de la secuencia. El resultado de la primera acción de la secuencia se convierte en el objeto JSON de entrada de la segunda acción de la secuencia, y así sucesivamente. Este objeto no incluye ninguno de los parámetros que originalmente se han pasado a la secuencia, a menos que la primera acción los incluya explícitamente en el resultado. Los parámetros de entrada a una acción se fusionan con los parámetros predeterminados de la acción, dando prioridad a los primeros y sustituyendo los parámetros predeterminados coincidentes. Para obtener más información sobre la invocación de secuencias de acciones con varios parámetros con nombre, consulte [Establecimiento de parámetros predeterminados en una acción](./parameters.html#default-params-action).
* Una secuencia no tiene un tiempo de espera excedido global distinto de los tiempos de espera de cada acción dentro de la secuencia. Debido a que una secuencia se trata como un conducto de operaciones, un error en una acción interrumpe el conducto. Si una acción excede el tiempo de espera, toda la secuencia finaliza con ese error.

## Gestión de acciones grandes
{: #large-app-support}

El tamaño máximo de código de una acción es de 48 MB. Las aplicaciones que contienen muchos módulos de terceros, bibliotecas nativas o herramientas externas pueden alcanzar este límite. Si crea una acción de paquete .zip o .jar mayor de 48 MB, debe ampliar la imagen del entorno de tiempo de ejecución con dependencias y, a continuación, utilizar un archivo de origen individual o un archivador más pequeño de 48 MB.

Por ejemplo, mediante la creación de un tiempo de ejecución de Docker personalizado, que incluye bibliotecas compartidas necesarias, no es necesario que estas dependencias estén presentes en el archivo de archivado. Los archivos de origen privado se pueden empaquetar en el archivado e introducirse en el tiempo de ejecución.

### Reducción del tamaño de las acciones
{: #large-app-reduce}

Para reducir el tamaño del código de una app de Python:

1. Coloque la biblioteca `opencv-python` en `opencv`.
2. Instale el binario opencv en la imagen del sistema operativo.
3. Aumente la imagen con más bibliotecas de Python ejecutando `pip install requirements.txt`.
4. Utilice `action.py` con la nueva imagen.

Para reducir el tamaño del código de una app de Node.js:

1. Instale `opencv`.
   ```
   npm install opencv
   ```
   {: pre}

2. Instale `package.json`.
   ```
   npm install package.json
   ```
   {: pre}

3. Utilice `action.js` con la nueva imagen.
