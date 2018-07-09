---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creación e invocación de acciones
{: #openwhisk_actions}

Las acciones son fragmentos de código sin estado que se ejecutan en la plataforma {{site.data.keyword.openwhisk}}. Por ejemplo, una acción se puede utilizar para detectar las caras de una imagen, responder a un cambio en la base de datos, agregar un conjunto de llamadas de API o publicar un Tweet. Una acción se puede escribir como JavaScript, Swift, Python, función PHP, método Java, o bien como un ejecutable binario personalizado, incluidos los programas Go y ejecutables personalizados empaquetado en un contenedor Docker.
{:shortdesc}

Las acciones se pueden invocar explícitamente, o ejecutarse como respuesta a un suceso. En cualquier caso, cada ejecución de una
acción tiene como resultado un registro de activación que se identifica mediante un ID de activación exclusivo. La entrada a una
acción y el resultado de la acción son un diccionario de pares de clave/valor, en el que la clave es una serie y
el valor es un valor JSON válido. Las acciones también se pueden componer de llamadas a otras acciones, o de una secuencia definida de acciones.

Aprenda a crear, invocar y depurar acciones en el entorno de desarrollo que elija:
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [Ejecutables arbitrarios](#creating-actions-arbitrary)

Además, obtendrá información acerca de:
* [Supervisión de la salida de acción](#monitor-action-output)
* [Obtención de acciones](#getting-actions)
* [Listado de acciones](#listing-actions)
* [Supresión de acciones](#deleting-actions)
* [Soporte para grandes aplicaciones](#large-app-support)
* [Acceso a metadatos de acción dentro del cuerpo de la acción](#accessing-action-metadata-within-the-action-body)

## Creación e invocación de acciones de JavaScript
{: #creating-and-invoking-javascript-actions}

Las secciones siguientes le guían en la forma de trabajar con acciones en JavaScript. Comience con la creación e invocación de una acción simple. A continuación, continúe con la adición de parámetros a una acción y la invocación de la acción con parámetros. Después, establezca parámetros predeterminados e invóquelos. A continuación, cree acciones asíncronas y, finalmente, trabaje con secuencias de acciones.

### Creación e invocación de una acción JavaScript sencilla
{: #openwhisk_single_action_js}

Revise los pasos y ejemplos siguientes para crear su primera acción JavaScript.

1. Crear un archivo JavaScript con el contenido siguiente. Para este ejemplo, llame al archivo **hello.js**.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  El archivo JavaScript podría tener funciones adicionales. No obstante, por convenio, debe existir una función llamada **main** para proporcionar un punto de entrada para la acción.

2. Crear una acción desde la función JavaScript siguiente. En este ejemplo, la acción se llama **hello**.
  ```
  ibmcloud wsk action create hello hello.js
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created action hello
  ```
  {: screen}

  La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.js`, la acción se ejecuta utilizando el tiempo de ejecución Node.js 6. También puede crear una acción que se ejecute con Node.js 8 especificando de forma explícita el parámetro `--kind nodejs:8`. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_javascript_environments) de Node.js 6 en comparación con 8.

3. Mostrar una lista de las acciones que ha creado:
  ```
  ibmcloud wsk action list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  actions
  hello       private
  ```
  {: screen}

  Puede ver la acción **hello** que ha creado.

4. Tras crear su acción, puede ejecutarla en la nube con el mandato **invoke**. Puede invocar acciones con una invocación *blocking* (es decir, estilo solicitud/respuesta) o *non-blocking*, especificando un distintivo en el mandato. Una solicitud de invocación de bloqueo _espera_ a que el resultado de la activación esté disponible. El período de espera es inferior a 60 segundos o el [valor de límite de tiempo](./openwhisk_reference.html#openwhisk_syslimits) de la acción. El resultado de la activación se devuelve si está disponible en el período de espera. De no ser así, la activación se sigue procesando en el sistema y se devuelve un ID de activación para poder buscar el resultado más tarde, como en las solicitudes que no son de bloqueo (consulte [aquí](#monitor-action-output) para ver consejos sobre la supervisión de activaciones).

  Este ejemplo utiliza el parámetro blocking, `--blocking`:
  ```
  ibmcloud wsk action invoke --blocking hello
  ```
  {: pre}

  La salida del mandato son dos elementos de información importantes:
  * El ID de activación (`44794bd6aab74415b4e42a308d880e5b`)
  * El resultado de la invocación si está disponible en el período de espera previsto.

  **La salida muestra el ID de activación:**
  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```
  {: screen}

  **Resultado de la invocación:**
  ```
  {
      "result":{
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

  El resultado en este caso es la serie `Hello world` devuelta por la función JavaScript. El ID de activación se puede
utilizar para recuperar los registros o el resultado de la invocación a posteriori.

5. Si no necesita el resultado de la acción inmediatamente, puede omitir el distintivo `--blocking`
para hacer una invocación sin bloqueo. Puede obtener el resultado posteriormente, usando el ID de activación.

  Consulte los ejemplos siguientes:
  ```
  ibmcloud wsk action invoke hello
  ```
  {: pre}

  **Salida del mandato:**
  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: screen}

  Ahora que conoce el ID de activación, puede especificarlo para obtener el resultado de la acción:
  ```
  ibmcloud wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  **Resultado de la acción:**
  ```
  {
      "payload": "Hello world"
  }
  ```
  {: screen}

6. Si olvida registrar el ID de activación, puede obtener una lista de activaciones ordenadas de la más reciente a la más antigua. Ejecute el mandato siguiente para obtener una lista de sus activaciones:

  **Listar activaciones:**
  ```
  ibmcloud wsk activation list
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

1. Guardar el contenido siguiente en un archivo llamado **asyncAction.js**.
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

  Observe que la función `main` devuelve un Promise, que indica que la activación aún no se ha completado, pero que se espera que lo haga en el futuro.

  La función `setTimeout()` de JavaScript en este caso espera 2 segundos antes de invocar la función de devolución de llamada, que representa el código asíncrono y va dentro de la función de devolución de llamada del Promise.

  La devolución de llamada del Promise tiene dos argumentos, resolve y reject, ambos son funciones.  La llamada a `resolve()` rellena el Promise e indica que la activación se ha completado con normalidad.

  Una llamada a `reject()` se puede utilizar para rechazar el Promise e indicar que la activación se ha completado de forma anómala.

2. Ejecute los mandatos siguientes para crear la acción e invocarla.

  Cree una acción denominada **asyncAction**:
  ```
  ibmcloud wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  Invoque la acción:
  ```
  ibmcloud wsk action invoke --result asyncAction
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "done": true
  }
  ```
  {: screen}

  Tenga en cuenta que ha realizado una invocación de bloqueo de una acción asíncrona.

3. Recupere el registro de activación para ver cuánto tiempo ha tardado la activación en completarse.

  Para ello, primero obtenga una lista con la acción para ver el ID de activación:
  ```
  ibmcloud wsk activation list --limit 1 asyncAction
  ```
  {: pre}

  Salida de ejemplo:
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```
  {: screen}

  Ahora obtenga la información de registro de activación utilizando el ID de activación:
  ```
  ibmcloud wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}

  ```
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```
  {: screen}

  Si se comparan los registros de fecha y hora de `start` y `end` en el registro de activación,
puede ver que esta activación ha tardado poco más de 2 segundos en completarse.

### Uso de acciones para invocar una API externa
{: #openwhisk_apicall_action}

Los ejemplos proporcionados hasta ahora son funciones JavaScript autocontenidas. También puede crear una acción que invoque una API externa.

En el ejemplo siguiente se invoca el servicio Astronomy Picture of the Day (APOD) de la NASA, que proporciona una imagen única de nuestro universo cada día.

1. Guarde el contenido siguiente en un archivo llamado **apod.js**.
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

  Se realiza una llamada a la API de APOD de la NASA y se extraen campos del resultado JSON. El tema [Referencias](./openwhisk_reference.html#openwhisk_ref_javascript_environments) detalla los paquetes Node.js que puede utilizar en sus acciones.

2. Ejecute los mandatos siguientes para crear la acción e invocarla.

  Cree la acción denominada **apod**:
  ```
  ibmcloud wsk action create apod apod.js
  ```
  {: pre}

  Invoque la acción **apod**:
  ```
  ibmcloud wsk action invoke --result apod
  ```
  {: pre}

  **Objeto de retorno:**
  ```
  {
    "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? No, although it was during a blue moon that this single-shot image was taken.  During a full moon that happened to be the second of the month -- the situation that defines a blue moon -- the photographer created the juxtaposition in late January by quickly moving around to find just the right spot to get the background Moon superposed behind the arc of a foreground tree.  Unfortunately, in this case, there seemed no other way than getting bogged down in mud and resting the camera on a barbed-wire fence.  The arc in the oak tree was previously created by hungry cows in Knight's Ferry, California, USA.  Quirky Moon-tree juxtapositions like this can be created during any full moon though, given enough planning and time.  Another opportunity will arise this weekend, coincidently during another blue moon. Then, the second blue moon in 2018 will occur, meaning that for the second month this year, two full moons will appear during a single month (moon-th).  Double blue-moon years are relatively rare, with the last occurring in 1999, and the next in 2037.",
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

Como alternativa a grabar todo el código de acción en un único archivo de origen JavaScript, puede grabar una acción como un paquete `npm`. Piense como ejemplo un directorio con los archivos siguientes:

**package.json:**
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

**index.js:**
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

La acción se expone a través de `exports.main`. El manejador de acciones en sí puede tener cualquier nombre, siempre que cumpla con la firma habitual de aceptar un objeto y devolverlo (o una `Promesa` de un objeto). Por convención Node.js, puede poner un nombre a este archivo `index.js` o especificar el nombre de archivo que prefiera como la propiedad `main` en package.json.

Para crear una acción de {{site.data.keyword.openwhisk_short}} desde este paquete:

1. Instale todas las dependencias localmente:
  ```
  npm install
  ```
  {: pre}

2. Cree un archivo `.zip` que contenga todos los archivos (incluidas todas las dependencias):
  ```
  zip -r action.zip *
  ```
  {: pre}

  El uso de la acción de Windows Explorer para crear el archivo zip da lugar a una estructura incorrecta. Las acciones zip de {{site.data.keyword.openwhisk_short}} deben tener `package.json` como raíz del zip, mientras que Windows Explorer las coloca dentro de una carpeta anidada. La opción más segura consiste en utilizar el mandato `zip` desde la línea de mandatos.
  {: tip}

3. Cree la acción:
  ```
  ibmcloud wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Al crear una acción desde un archivo `.zip` con la herramienta CLI, debe proporcionar explícitamente un valor para el distintivo `--kind` utilizando `nodejs:6` o `nodejs:8`.

4. Puede invocar la acción como cualquier otra:
  ```
  ibmcloud wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
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

Finalmente, tenga en cuenta que mientras que la mayoría de los paquetes de `npm` instalan orígenes JavaScript en `npm install`, algunos también instalan y compilan artefactos binarios. La carga del archivo de archivado no da soporte en este momento a las dependencias binarias, sino únicamente a las dependencias de JavaScript. Las invocaciones de acción pueden fallar si el archivo incluye dependencias binarias.

### Empaquetado de una acción como un paquete individual
{: #openwhisk_js_webpack_action}

Es conveniente incluir sólo el código mínimo en un solo archivo `.js` que incluya dependencias. Este enfoque permite agilizar los despliegues, y en algunas circunstancias en las que empaquetar la acción como un zip podría generar un archivo demasiado por incluir archivos innecesarios.

Puede utilizar un empaquetador de módulos JavaScript, como [webpack](https://webpack.js.org/concepts/). Cuando webpack procesa el código, se crea un gráfico de dependencias recursivamente que incluye todos los módulos que necesita la acción.

Aquí se muestra un ejemplo rápido utilizando webpack:

A partir del `package.json` del ejemplo anterior, añada `webpack` como dependencia de desarrollo y añada algunos mandatos de script npm.
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "ibmcloud wsk action update my-action dist/bundle.js --kind nodejs:8"
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

Cree el archivo de configuración de webpack `webpack.config.js`.
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

Establezca la variable `global.main` en la función principal de la acción.

Del ejemplo anterior:
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

Si el nombre de la función es `main`, utilice la siguiente sintaxis:
```javascript
global.main = main;
```
{: codeblock}

Para crear y desplegar una acción de OpenWhisk utilizando `npm` y `webpack`:

1. Primero, instale las dependencias localmente:
  ```
  npm install
  ```
  {: pre}

2. Cree el paquete de webpack:
  ```
  npm run build
  ```
  {: pre}

  Se creará el archivo `dist/bundle.js` y se utilizará para desplegarlo como código fuente de la acción.

3. Cree la acción utilizando el script `npm` o la CLI.

  Mediante el script `npm`:
  ```
  npm run deploy
  ```
  {: pre}

  Utilizando la CLI:
  ```
  ibmcloud wsk action update my-action dist/bundle.js
  ```
  {: pre}

Por último, el archivo de paquete que crea `webpack` no admite dependencias binarias, sino dependencias de JavaScript. Así, las invocaciones de acción fallarán si el paquete depende de las dependencias binarias, porque no se incluyen en el archivo `bundle.js`.

## Creación de secuencias de acciones
{: #openwhisk_create_action_sequence}

Puede crear una acción que encadene juntas una secuencia de acciones.

Se proporcionan varias acciones de utilidad en un paquete llamado `/whisk.system/utils` que puede utilizar para crear su primera secuencia. Puede obtener más información sobre los paquetes en la sección [paquetes](./openwhisk_packages.html).

1. Mostrar las acciones del paquete `/whisk.system/utils`.
  ```
  ibmcloud wsk package get --summary /whisk.system/utils
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

  En este ejemplo utilizará las acciones `split` y `sort`.

2. Crear una secuencia de acciones para que el resultado de una acción se pase como argumento a la acción siguiente.
  ```
  ibmcloud wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Esta secuencia de acciones convierte algunas líneas de texto en una matriz, y ordena las líneas.

3. Invoque la acción:
  ```
  ibmcloud wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  Salida de ejemplo:
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

  En el resultado, puede ver que las líneas se ordenan.

**Nota**: los parámetros que se pasan entre acciones de la secuencia son explícitos, salvo los predeterminados.
Por tanto, los parámetros que se pasan a la secuencia de acción solo están disponibles para la primera acción de la secuencia. El resultado de la primera acción de la secuencia se convierte en el objeto JSON de entrada de la segunda acción de la secuencia (y así sucesivamente). Este objeto no incluye ninguno de los parámetros que originalmente se han pasado a la secuencia, a menos que la primera acción los incluya explícitamente en el resultado. Los parámetros de entrada a una acción se fusionan con los parámetros predeterminados de la acción, dando prioridad a los primeros y sustituyendo los parámetros predeterminados coincidentes. Para obtener más información sobre la invocación de secuencias de acciones con varios parámetros con nombre, consulte [Establecimiento de parámetros predeterminados en una acción](./parameters.html#default-params-action).

## Creación de acciones Python
{: #creating-python-actions}

El proceso de creación de acciones Python es parecido al de las acciones JavaScript. En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Python, y se añaden parámetros a dicha acción.

### Creación e invocación de una acción Python
{: #openwhisk_actions_python_invoke}

Una acción es sencillamente una función Python de nivel superior. Por ejemplo, cree un archivo denominado **hello.py** con el siguiente código fuente:
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
        return {"greeting": greeting}
```
{: codeblock}

Las acciones Python siempre consumen un
diccionario y generan un diccionario. El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar de forma explícita para crear la acción con la CLI `wsk` utilizando `--main`, como con cualquier otro tipo de acción.

Puede crear una acción de {{site.data.keyword.openwhisk_short}} denominada **helloPython** a partir de esta función de la siguiente manera:
```
ibmcloud wsk action create helloPython hello.py
```
{: pre}

La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.py`, la acción se ejecuta utilizando el tiempo de ejecución Python 2. También puede crear una acción que se ejecute con Python 3 especificando de forma explícita el parámetro `--kind python:3`. Además, hay un tiempo de ejecución Python 3 con el tipo `python-jessie:3` que contiene paquetes adicionales para IBM Cloud Services como IBM Cloudant, IBM DB2, IBM COS e IBM Watson.
Para obtener más información sobre los paquetes incluidos en este tiempo de ejecución de Python 3, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_python_environments) del tiempo de ejecución de Python.

La invocación de la acción es la misma para acciones Python que para acciones JavaScript:
```
ibmcloud wsk action invoke --result helloPython --param name World
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

Puede empaquetar una acción Python y los módulos dependientes en un archivo zip.
El nombre del archivo fuente que contiene el punto de entrada (por ejemplo, `main`) debe ser `__main__.py`.
Por ejemplo, para crear una acción con un módulo de ayudante denominado `helper.py`, primero cree un archivador con sus archivos fuente:
```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

A continuación, cree la acción:
```bash
ibmcloud wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Aunque estos pasos se muestran para Python 3 (con el tipo `python:3`), puede hacer lo mismo con otros tipos de Python, como `python:2` o `python-jessie:3`.

### Empaquetamiento de acciones de Python con un entorno virtual en archivos zip
{: #openwhisk_actions_python_virtualenv}

Otra forma de empaquetar dependencias de Python es utilizando un entorno virtual (`virtualenv`), que permite enlazar a paquetes adicionales que, por ejemplo, se pueden instalar utilizando [`pip`](https://packaging.python.org/installing/).

Como con el soporte al archivo zip básico, el nombre para el archivo fuente que contiene el punto de entrada principal debe ser `__main__.py`. Es decir, el contenido de `__main__.py` es la función principal, de modo que, para este ejemplo, puede cambiar el nombre de `hello.py` a `__main__.py` de la sección anterior. Además, el directorio virtualenv debe denominarse `virtualenv`. Consulte el siguiente escenario de ejemplo para instalar dependencias, empaquetándolas en un virtualenv, y creando una acción compatible con OpenWhisk.

Para asegurar la compatibilidad con el contenedor de tiempo de ejecución de OpenWhisk, las instalaciones de paquetes dentro de un virtualenv deben realizarse en el entorno de destino utilizando la imagen correspondiente al tipo.
- Para el tipo `python:2`, utilice la imagen de docker `openwhisk/python2action`.
- Para el tipo `python:3`, utilice la imagen de docker `openwhisk/python3action`.
- Para el tipo `python-jessie:3`, utilice la imagen de docker `ibmfunctions/action-python-v3`.

1. Dado un archivo [requirements.txt ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) con las versiones y módulos `pip` a instalar, ejecute lo siguiente para instalar las dependencias y crear un virtualenv utilizando una imagen Docker compatible:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

2. Archive el directorio virtualenv y todos los archivos Python adicionales:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Cree la acción **helloPython**:
    ```
    ibmcloud wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Añada `requirements.txt` únicamente los módulos que no forman parte del entorno de tiempo de ejecución seleccionado. Esto ayuda a mantener un tamaño mínimo del entorno virtual.
{: tip}

## Creación de acciones PHP
{: #creating-php-actions}

El proceso de creación de acciones PHP es parecido al de las acciones JavaScript. En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción PHP, y se añaden parámetros a dicha acción.

### Creación e invocación de una acción PHP
{: #openwhisk_actions_php_invoke}

Una acción es sencillamente una función PHP de nivel superior. Por ejemplo, cree un archivo denominado `hello.php` con el siguiente código fuente:

```php
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

Las acciones PHP siempre consumen una matriz asociativa y devuelven una matriz asociativa. El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar de forma explícita al crear la acción con la CLI `ibmcloud wsk` utilizando `--main`, como con cualquier otro tipo de acción.

Puede crear una acción de {{site.data.keyword.openwhisk_short}} llamada **helloPHP** desde esta función, según se indica a continuación:
```
ibmcloud wsk action create helloPHP hello.php
```
{: pre}

La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.php`, la acción se ejecuta utilizando el tiempo de ejecución PHP 7.1. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_php) de PHP.

La invocación de la acción es la misma para acciones PHP que para acciones JavaScript:
```
ibmcloud wsk action invoke --result helloPHP --param name World
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

Puede empaquetar una acción PHP junto con otros archivos y paquetes dependientes en un archivo zip.
El nombre del archivo fuente que contiene el punto de entrada (por ejemplo, `main`) debe ser `index.php.py`.

Por ejemplo, para crear una acción que incluya un segundo archivo denominado `helper.php`, primero cree un archivador que contenga sus archivos fuente:
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

A continuación, cree la acción **helloPHP**:
```bash
ibmcloud wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Creación de acciones Swift
{: #creating-swift-actions}

El proceso de creación de acciones Swift es parecido al de las acciones JavaScript. Las siguientes secciones le guían a través de la creación e invocación de una única acción de Swift y por el empaquetado de una acción en un archivo zip.

También puede usar [Online Swift Playground](http://online.swiftplayground.run) en línea para probar el código Swift sin tener que instalar Xcode en su máquina.

**Atención:** Acciones Swift se ejecutan en un entorno Linux. Swift en Linux aún está en desarrollo, y OpenWhisk generalmente utiliza el release disponible más reciente, que no es necesariamente estable. Además, la versión de Swift que se utiliza con OpenWhisk podría no ser coherente con versiones de Swift de releases estables de Xcode en MacOS.

### Creación e invocación de una acción

#### Swift 3
Una acción es sencillamente una función Swift de nivel superior. Por ejemplo, cree un archivo llamado
**hello.swift** con el contenido siguiente:

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

Puede crear una acción OpenWhisk denominada **helloSwift** a partir de esta función de la siguiente manera:
```
ibmcloud wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}

#### Swift 4

Como novedad de Swift 4, además de la firma de la función principal anterior, se han añadido dos firmas más listas para ser utilizadas a fin de aprovechar el tipo [Codable](https://developer.apple.com/documentation/swift/codable). [Aquí](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types) encontrará más información sobre los tipos de datos que se pueden codificar y descodificar con representaciones externas como JSON.

El ejemplo siguiente toma un parámetro de entrada como **entrada codificable (Codable Input)** con un campo `name` y devuelve una **salida codificable (Codable output)** con un campo `greetings`.
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

En este ejemplo, la acción de Swift consume un código codificable y genera un tipo codificable.
No es necesario que gestione la entrada; puede utilizar la firma de la función que no necesita ninguna entrada, solo una salida codificable.
```swift
struct Output: Codable {
    let greeting: String
}
func main(completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello OpenWhisk!")
    completion(result, nil)
}
```
{: codeblock}

Puede crear una acción OpenWhisk denominada `helloSwift` a partir de esta función de la siguiente manera:
```
ibmcloud wsk action create helloSwift hello.swift --kind swift:4.1
```
{: pre}

Consulte la documentación de [referencia](./openwhisk_reference.html#swift-actions) de Swift para obtener más información sobre el tiempo de ejecución de Swift.

La invocación de la acción es la misma para acciones Swift que para acciones JavaScript:
```
ibmcloud wsk action invoke --result helloSwift --param name World
```
{: pre}

Salida de ejemplo:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

Obtenga más información sobre los parámetros en el tema [Cómo trabajar con parámetros](./parameters.html).

### Empaquetado de una acción como un ejecutable Swift
{: #packaging-an-action-as-a-swift-executable}

Cuando cree una acción Swift de OpenWhisk con un archivo fuente Swift, se tiene que compilar en un binario antes de ejecutar la acción. Una vez hecho esto, las siguientes llamadas a la acción serán mucho más rápidas hasta que se depure el contenedor que alberga la acción. Este retraso se conoce como retraso de inicio en frío.

Para evitar el retraso de inicio en frío, puede compilar el archivo Swift en un binario y luego cargarlo en OpenWhisk en un archivo zip. Como necesita la protección de OpenWhisk, la forma más sencilla de crear el binario consiste en crearlo en el mismo entorno en el que se va a ejecutar.

### Utilización de un script para crear una acción empaquetada de Swift

Puede utilizar un script para automatizar el empaquetado de la acción. Cree un archivo de script denominado `compile.sh` con el código de ejemplo siguiente.
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

# Solo para Swift4
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

En el script se presupone que tiene un directorio denominado `actions`, en el que cada directorio de nivel superior representa una acción.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

- Cree el archivo `Package.swift` para añadir dependencias. **Nota:** la sintaxis difiere entre las herramientas de Swift 3 y de Swift 4.

  Sintaxis de ejemplo de Swift 3:
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

  Sintaxis de ejemplo de Swift 4:
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

  Como puede ver, este ejemplo añade `example-package-deckofplayingcards` como una dependencia. Tenga en cuenta que `CCurl`, `Kitura-net` y `SwiftyJSON` se proporcionan en la acción estándar Swift; por eso debe incluirlos en su propio `Package.swift`solo para acciones Swift 3.

- Para crear la acción, ejecute el siguiente mandato para una acción de Swift 3:
  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Para compilar para Swift 4, utilice `swift:4.1` en lugar de `swift:3.1.1`:
  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Esto ha creado `hello.zip` en `build`.

- Cárguelo en OpenWhisk con el nombre de acción **helloSwifty**. Para Swift 3, utilice el tipo `swift:3.1.1`
  ```
  ibmcloud wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Para Swift 4, utilice el tipo `swift:3.1.1`:
  ```
  ibmcloud wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

- Para comprobar lo rápido que es, ejecute el siguiente mandato:
  ```
  ibmcloud wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  El tiempo que se ha tardado en ejecutar la acción está en la propiedad "duration" y se compara con el tiempo que se tarda en ejecutar con un paso de compilación en la acción **hello**.

### Manejo de errores en Swift 4

Con el nuevo manejador de terminación codificable, puede pasar un error a fin de indicar que la acción ha fallado.
El [manejo de errores en Swift](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) se parece al manejo de excepciones en otros lenguajes, con el uso de las palabras clave `try, catch` y `throw`.

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

El proceso de creación de acciones Java es parecido al de las acciones JavaScript y Swift. En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Swift, y se añaden parámetros a dicha acción.

Para compilar, probar y archivar archivos Java, debe tener un [JDK 8](http://openjdk.java.net/install) instalado localmente.

### Creación e invocación de una acción Java
{: #openwhisk_actions_java_invoke}

Una acción Java es un programa Java con un método llamado `main` que tiene la firma exacta que se indica a continuación:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Por ejemplo, cree un archivo Java denominado `Hello.java` con el contenido siguiente:

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

A continuación, compile `Hello.java` en un archivo JAR `hello.jar` tal como se indica a continuación:
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

[google-gson](https://github.com/google/gson) debe existir en CLASSPATH de Java para compilar el archivo Java.
{: tip}

Puede crear una acción de {{site.data.keyword.openwhisk_short}} denominada **helloJava** a partir de este archivo JAR tal como se indica a continuación:
```
ibmcloud wsk action create helloJava hello.jar --main Hello
```
{: pre}

Cuando se utiliza la línea de mandatos y un archivo fuente `.jar`, no es necesario especificar que está creando una acción Java; la herramienta lo determina a partir de la extensión de archivo.

Tiene que especificar el nombre de la clase principal con `--main`. Una clase principal apta es una que implemente un método `main` estático. Si la clase no está en el paquete predeterminado, utilice el nombre de clase completo de Java, por ejemplo `--main com.example.MyMain`.

Si es necesario, también puede personalizar el nombre de método de la acción Java. Esto se realiza especificando el nombre completo del método de Java de la acción, por ejemplo, `--main com.example.MyMain#methodName`.

La invocación de la acción es la misma para acciones Java que para acciones Swift y JavaScript:
```
ibmcloud wsk action invoke --result helloJava --param name World
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

Su código se compila en un binario ejecutable y se incluye en una imagen Docker. El programa binario interactúa con el sistema
aceptando la entrada desde `stdin` y respondiendo por medio de `stdout`.

Como requisito previo, debe tener una cuenta de Docker Hub.  Para configurar un ID y cuenta gratuitos de Docker, acceda a
[Docker Hub](https://hub.docker.com).

Para las instrucciones siguientes, supondremos que el ID de usuario es `janesmith` y la contraseña es `janes_password`.  Suponiendo que la CLI está configurada, quedan tres pasos para configurar un binario personalizado para que lo use {{site.data.keyword.openwhisk_short}}. Tras hacerlo, la imagen de Docker subida se podrá utilizar como una acción.

1. Descargar el esqueleto de Docker. Puede descargarlo e instalarlo usando la CLI de la siguiente manera:
  ```
  ibmcloud wsk sdk install docker
  ```
  {: pre}

  El esqueleto Docker estará ahora instalado en el directorio actual.
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  Salida de ejemplo:
  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```
  {: screen}

  El esqueleto es una plantilla de contenedor de Docker en la que puede inyectar código en forma de binarios personalizados.

2. Configure su binario personalizado en el esqueleto blackbox. El esqueleto ya incluye un programa en C que puede usar.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  Puede modificar este valor cuando sea necesario o añadir código y dependencias adicionales a la imagen de docker.
  En el último caso, puede modificar el `Dockerfile` según convenga para crear el ejecutable.
  El binario debe estar en el contenedor en `/action/exec`.

  El ejecutable recibe un único argumento de la línea de mandatos. Es la serialización de una cadena del objeto JSON que representa los argumentos para la acción. El programa puede redirigir sus registros a `stdout` o `stderr`.
  Por convenio, la última línea de la salida _debe_ ser un objeto JSON en forma de cadena que represente el resultado de la acción.

3. Construya la imagen de Docker y suba dicha imagen mediante un script proporcionado. Antes debe ejecutar `docker login` para la autenticación y, a continuación, ejecute el script con el nombre de imagen elegido.
  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  Tenga en cuenta que parte del archivo `example.c` está compilado como parte del proceso de compilación de imagen de Docker, por lo que no necesita C compilado en su máquina. De hecho, salvo cuando compile el binario en una máquina de host compatible, no puede ejecutarse dentro del contenedor porque los formatos no coinciden.

  El contenedor de Docker ahora se puede utilizar como una acción de {{site.data.keyword.openwhisk_short}}:
  ```
  ibmcloud wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Observe el uso de `--docker` para crear una acción. Se presupone que todas las imágenes de Docker se alojan en Docker Hub. La acción se puede invocar como cualquier otra acción de {{site.data.keyword.openwhisk_short}}.
  ```
  ibmcloud wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  **Salida de la invocación:**
  ```
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
  {: screen}

  Para actualizar la acción de Docker, ejecute `buildAndPush.sh` para cargar la imagen más reciente en Docker Hub. Esto permite al sistema extraer la nueva imagen de Docker la próxima vez que ejecute el código para la acción. Si no hay contenedores recientes, las nuevas invocaciones utilizan la imagen de Docker nueva. Sin embargo, si hay un contenedor reciente que utiliza una versión anterior de la imagen de Docker, las nuevas invocaciones siguen utilizando esta imagen a no ser que se ejecute `ibmcloud wsk action update`. Esto indica al sistema que, para las nuevas invocaciones, debe ejecutar una extracción de Docker para obtener su nueva imagen de Docker.

  **Cargue la imagen más reciente en Docker Hub:**
  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  **Actualice la acción para que las nuevas invocaciones empiecen a utilizar la nueva imagen:***
  ```
  ibmcloud wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Podrá obtener más información sobre la creación de acciones de Docker en la sección
[Referencias](./openwhisk_reference.html#openwhisk_ref_docker).

  La versión anterior de la CLI daba soporte a `--docker` sin ningún parámetro y el nombre de la imagen ha producido un argumento posicional. Para permitir que las acciones Docker acepten datos de inicialización utilizando un archivo (zip), se ha normalizado la experiencia de usuario para acciones Docker para que un argumento posicional, si está presente, debe ser un archivo (por ejemplo, un archivo zip) en su lugar. El nombre de la imagen debe especificarse después de la opción `-- docker`. Gracias a los comentarios de los usuarios, se ha incluido el argumento `-- native` como abreviatura de `-- docker openwhisk/dockerskeleton`, de modo que los ejecutables que se ejecutan dentro de la acción estándar SDK Docker son más adecuados para crear y desplegar.

  Por ejemplo, esta guía de aprendizaje crea un ejecutable binario dentro de la ubicación del contenedor en `/action/exec`. Si copia este archivo a su sistema de archivos local y los comprime en `exec.zip`, a continuación, puede utilizar los mandatos siguientes para crear una acción Docker que recibe el ejecutable como datos de inicialización.

  **Cree una acción a partir del archivo zip:**
  ```
  ibmcloud wsk action create example exec.zip --native
  ```
  {: pre}

  Que es equivalente a invocar el mandato siguiente.
  ```
  ibmcloud wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Creación de acciones Go
{: #creating-go-actions}

La opción `--native` permite empaquetar cualquier ejecutable como una acción. Esto funciona para Go, por ejemplo. Como en las acciones Docker, el ejecutable de Go recibe un único argumento de la línea de mandatos. Es la serialización de una cadena del objeto JSON que representa los argumentos para la acción. El programa puede redirigir sus registros a `stdout` o `stderr`. Por convenio, la última línea de la salida _debe_ ser un objeto JSON en forma de cadena que represente el resultado de la acción.

Aquí se muestra una acción Go de ejemplo.
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

Guarde el código anterior en un archivo `sample.go` y cree con él una compilación cruzada para OpenWhisk. El ejecutable se debe denominar `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
ibmcloud wsk action create helloGo --native exec.zip
```
{: codeblock}

La acción puede ejecutarse como cualquier otra acción.
```bash
ibmcloud wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Los registros se recuperan también de una forma similar.
```bash
ibmcloud wsk activation logs --last --strip
my first Go action.
```

## Creación de acciones utilizando ejecutables arbitrarios
{: #creating-actions-arbitrary}

Mediante `--native`, puede ver que _cualquier_ ejecutable puede ejecutarse como una acción de OpenWhisk. Esto incluye a los scripts `bash`, o a los binarios de compilación cruzada. Para estos últimos, la restricción es que el binario debe ser compatible con la imagen `openwhisk/dockerskeleton`.

## Supervisión de la salida de acción
{: #monitor-action-output}

Las acciones de {{site.data.keyword.openwhisk_short}} se podrían invocar por otros usuarios, en respuesta a varios sucesos, o como parte de una secuencia de acciones. En tales casos, puede ser útil supervisar las invocaciones.

Puede utilizar la interfaz de línea de mandatos de {{site.data.keyword.openwhisk_short}} para ver el resultado de la salida de las acciones a medida que se invocan.

1. Emita el siguiente mandato desde un shell:
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

  Este mandato inicia un bucle de sondeo que comprueba continuamente los registros de las activaciones.

2. Cambie a otra ventana e invoque la acción:
  ```
  ibmcloud wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```
  {: screen}

3. Observe el registro de activación en la ventana de sondeo:
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
  {: screen}

  De la misma forma, cuando ejecute el programa de utilidad de sondeo, verá en tiempo real los registros de las acciones que ejecute en nombre de OpenWhisk.

## Obtención de acciones
{: #getting-actions}

Los metadatos que describen las acciones existentes se pueden recuperar mediante el mandato get `ibmcloud wsk action`.

**Mandato:**
```
ibmcloud wsk action get hello
```

***Resultado:**
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
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

### Obtención de un URL de acción
{: #get-action-url}

Una acción se puede invocar utilizando la interfaz REST mediante una solicitud HTTPS. Para obtener un URL de acción, ejecute el mandato siguiente:
```
ibmcloud wsk action get actionName --url
```
{: pre}

Se devuelve un URL con el formato siguiente para las acciones estándares:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Para las [acciones web](./openwhisk_webactions.html), se devuelve un URL con el formato siguiente:
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Nota:** para las acciones estándares, la autenticación se debe proporcionar cuando se invoca mediante una solicitud HTTPS. Para obtener más información sobre las invocaciones de acciones mediante la interfaz REST, vea la [documentación de consulta de API REST](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

### Guardado del código de acción
{: #save-action}

El código asociado con una acción existente puede recuperarse y guardarse localmente. El guardado puede realizarse en todas las acciones, excepto en las secuencias y las acciones de Docker.

1. Guarde el código de acción en un nombre de archivo que corresponda con un nombre de acción existente en el directorio de trabajo actual. Se utiliza una extensión de archivo que corresponde al tipo de acción, o una extensión de tipo .zip para el código de acción que es un archivo zip.
  ```
  ibmcloud wsk action get actionName --save
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. En lugar de permitir que la CLI determine el destino del código que se va a guardar, se puede proporcionar una vía de acceso de archivo, un nombre de archivo y una extensión personalizados mediante el distintivo `--save-as`.
  ```
  ibmcloud wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## Listado de acciones
{: #listing-actions}

Puede generar una lista de todas las acciones creadas utilizando el siguiente mandato:
```
ibmcloud wsk action list
```
{: pre}

A medida que escriba más acciones, esta lista crece y puede resultar útil agrupar las acciones relacionadas en [paquetes](./openwhisk_packages.html). Para filtrar la lista de acciones y limitarla a las contenidas en un determinado paquete, puede utilizar la siguiente sintaxis de mandato:
```
ibmcloud wsk action list [NOMBRE PAQUETE]
```
{: pre}

## Supresión de acciones
{: #deleting-actions}

Puede realizar una limpieza mediante la supresión de acciones que no quiera usar.

1. Ejecute el mandato siguiente para suprimir una acción:
  ```
  ibmcloud wsk action delete hello
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: deleted hello
  ```
  {: screen}

2. Compruebe que la acción ya no aparece en la lista de acciones.
  ```
  ibmcloud wsk action list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  actions
  ```
  {: screen}

## Soporte para grandes aplicaciones
{: #large-app-support}

El tamaño máximo de código de una acción es de 48 MB. Las aplicaciones que contienen muchos módulos de terceros, bibliotecas nativas o herramientas externas pueden alcanzar este límite.

Si crea una acción de paquete (zip o jar) de más de 48 MB, la solución es ampliar la imagen de tiempo de ejecución con dependencias, y luego utilizar un único archivo de origen o un archivado de menos de 48 MB.

Por ejemplo, mediante la creación de un tiempo de ejecución de Docker personalizado, que incluye bibliotecas compartidas necesarias, no es necesario que estas dependencias estén presentes en el archivo de archivado. Los archivos de origen privado se pueden empaquetar en el archivado e introducirse en el tiempo de ejecución.

Otra ventaja de reducir el tamaño de los archivos de archivado es que también mejoran los tiempos de despliegue. Se proporcionan dos ejemplos de tiempo de ejecución en las secciones siguientes para demostrar cómo se pueden reducir los tamaños de las aplicaciones con esta técnica.

### Ejemplo de Python

Para una aplicación Python, consulte los pasos siguientes para reducir el tamaño del código.

1. Coloque la biblioteca `opencv-python` en opencv.
2. A continuación, instale el binario opencv en la imagen del sistema operativo.
3. A continuación, puede utilizar `requirements.txt` y ejecutar `pip install requirements.txt` para aumentar la imagen con más bibliotecas Python.
4. Después, puede utilizar `action.py` con la nueva imagen.

### Ejemplo de Node.js

A fin de reducir el tamaño de la aplicación Node.js, consulte los pasos siguientes para instalar paquetes adicionales en la imagen del sistema operativo:

1. Instale opencv mediante `npm`:
   ```
   npm install opencv
   ```
   {: pre}

2. Del mismo modo, si tiene un `package.json`, instálelo mediante `npm`:
   ```
   npm install package.json
   ```
   {: pre}

3. A continuación, utilice `action.js` con la nueva imagen.

## Acceso a metadatos de acción dentro del cuerpo de la acción
{: #accessing-action-metadata-within-the-action-body}

El entorno de acción contiene varias propiedades que son específicas de la acción que se está ejecutando. Estas propiedades permiten que la acción funcione mediante programación con activos de OpenWhisk a través de la API REST o permiten establecer una alarma interna cuando la acción está a punto de alcanzar su presupuesto de tiempo permitido. Las propiedades están accesibles en el entorno del sistema para todos los tiempos de ejecución admitidos: Node.js, Python, Swift, Java y acciones Docker cuando se utiliza el esqueleto OpenWhisk Docker.

* `__OW_API_HOST` - Host de API para el despliegue de OpenWhisk ejecutando esta acción.
* `__OW_API_KEY` - Clave de API para quien invoca la acción. Esta clave puede ser una clave de API restringida.
* `__OW_NAMESPACE` - Espacio de nombres para la _activación_ (puede no ser el mismo que el espacio de nombres para la acción).
* `__OW_ACTION_NAME` - Nombre completo calificado de la acción en ejecución.
* `__OW_ACTIVATION_ID` - ID de activación para esta instancia de acción en ejecución.
* `__OW_DEADLINE` - Tiempo aproximado cuando esta acción habrá consumido toda su cuota de duración (medido en milisegundos desde epoch).
