---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

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

Las acciones se pueden invocar explícitamente, o ejecutarse como respuesta a un suceso. En cualquier caso, cada ejecución de una acción tiene como resultado un registro de activación que se identifica mediante un ID de activación exclusivo. La entrada a una acción y el resultado de la acción son un diccionario de pares de clave/valor, en el que la clave es una serie y el valor es un valor JSON válido. Las acciones también se pueden componer de llamadas a otras acciones, o de una secuencia definida de acciones.

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
* [Supervisión de salida de acción](#watching-action-output)
* [Soporte para grandes aplicaciones](#large-app-support)
* [Listado de acciones](#listing-actions)
* [Supresión de acciones](#deleting-actions)
* [Acceso a los metadatos de acción dentro del cuerpo de la acción](#accessing-action-metadata-within-the-action-body)


## Creación e invocación de acciones JavaScript
{: #creating-and-invoking-javascript-actions}

Las secciones siguientes le guían en la forma de trabajar con acciones en JavaScript. Comience con la creación e invocación de una acción simple. A continuación, continúe con la adición de parámetros a una acción y la invocación de la acción con parámetros. Después, establezca parámetros predeterminados e invóquelos. A continuación, cree acciones asíncronas y, finalmente, trabaje con secuencias de acciones.


### Creación e invocación de una acción JavaScript sencilla
{: #openwhisk_single_action_js}

Revise los pasos y ejemplos siguientes para crear su primera acción JavaScript.

1. Crear un archivo JavaScript con el contenido siguiente. Para este ejemplo, el nombre de archivo es 'hello.js'.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  El archivo JavaScript podría tener funciones adicionales. No obstante, por convenio, debe existir una función llamada `main` para proporcionar un punto de entrada para la acción.

2. Crear una acción desde la función JavaScript siguiente. En este ejemplo, la acción se llama 'hello'.

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.js`, la acción se ejecuta utilizando el tiempo de ejecución Node.js 6. También puede crear una acción que se ejecute con Node.js 8 especificando de forma explícita el parámetro `--kind nodejs:8`. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_javascript_environments) de Node.js 6 en comparación con 8.
  
3. Mostrar una lista de las acciones que ha creado:

  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  hello       private
  ```

  Puede ver la acción `hello` que ha creado.

4. Tras crear su acción, puede ejecutarla en la nube en OpenWhisk con el mandato 'invoke'. Puede
invocar acciones con una invocación *blocking* (es decir, estilo solicitud/respuesta) o *non-blocking*, especificando un distintivo en el mandato. Una solicitud de invocación de bloqueo _espera_ a que el resultado de la activación esté disponible. El período de espera es inferior a 60 segundos o el [valor de límite de tiempo](./openwhisk_reference.html#openwhisk_syslimits) de la acción. El resultado de la activación se devuelve si está disponible en el período de espera. De no ser así, la activación se sigue procesando en el sistema y se devuelve un ID de activación para poder buscar el resultado más tarde, como en las solicitudes que no son de bloqueo (consulte [aquí](#watching-action-output) para ver consejos sobre la supervisión de activaciones).

  Este ejemplo utiliza el parámetro blocking, `--blocking`:

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result":{
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  La salida del mandato son dos elementos de información importantes:
  * El ID de activación (`44794bd6aab74415b4e42a308d880e5b`)
  * El resultado de la invocación si está disponible en el período de espera previsto.

  El resultado en este caso es la serie `Hello world` devuelta por la función JavaScript. El ID de activación se puede
utilizar para recuperar los registros o el resultado de la invocación a posteriori.  

5. Si no necesita el resultado de la acción inmediatamente, puede omitir el distintivo `--blocking`
para hacer una invocación sin bloqueo. Puede obtener el resultado posteriormente, usando el ID de activación. Consulte el ejemplo siguiente:

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. Si olvida registrar el ID de activación, puede obtener una lista de activaciones ordenadas de la más reciente a la más antigua. Ejecute el mandato siguiente para obtener una lista de sus activaciones:

  ```
  wsk activation list
  ```
  {: pre}

  ```
  activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### Paso de parámetros a una acción
{: #openwhisk_pass_params}

Los parámetros se pueden pasar a la acción cuando se invoca.

1. Usar parámetros en la acción. Por ejemplo, actualice el archivo 'hello.js' con el contenido siguiente:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Los parámetros de entrada se pasan como un parámetro de objeto JSON a la función `main`. Fíjese
cómo los parámetros `name` y `place` se recuperan del objeto `params` en este ejemplo.

2. Actualizar e invocar la acción `hello`, mientras se pasan los valores de los parámetros `name` y `place`. Consulte el ejemplo siguiente:

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  Si necesita modificar los parámetros de credenciales que no son de servicio, tenga en cuenta que, al ejecutar el mandato `action update` con nuevos parámetros, se eliminarán los parámetros actuales que no se especifiquen en el mandato `action update`. Por ejemplo, si hay dos parámetros aparte de `__bx_creds`, con claves denominadas key1 y key2.  Si ejecuta un mandato `action update` con `-p key1 new-value -p key2 new-value` pero omite el parámetro `__bx_creds`, el parámetro `__bx_creds` dejará de existir una vez realizado correctamente el mandato `action update`. A continuación, deberá volver a enlazar las credenciales de servicio. Se trata de una limitación conocida sin solución alternativa.
  {: tip}  

3.  Los parámetros se puede proporcionar explícitamente en la línea de mandatos o por medio de un archivo que contenga los parámetros deseados.

  Para pasar los parámetros directamente a través de la línea de mandatos, especifique un par clave/valor para el distintivo `--param`:
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  Para poder utilizar un archivo que contiene parámetros, cree un archivo que contenga los parámetros en formato JSON. Luego se debe pasar el nombre del archivo al distintivo `param-file`:

  Consulte el siguiente archivo de parámetros de ejemplo denominado `parameters.json`:
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Fíjese en el uso de la opción `--result`: implica una invocación con bloqueo donde la interfaz de línea de mandatos espera que se complete la activación y, a continuación, solo visualiza el resultado. Para su comodidad, esta opción se puede utilizar sin `--blocking` que se infiere de forma automática.

  Además, si los valores de parámetro especificados en una línea de mandatos tienen un formato JSON válido, se analizan y envían con su acción como un objeto estructurado. Por ejemplo, actualice la acción 'hello' con lo siguiente:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Ahora la acción espera un único parámetro `person` con los campos `name` y `place`. A continuación, invoque la acción con un único parámetro `person` con un formato JSON válido, como en el siguiente ejemplo:

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  El resultado es el mismo porque la interfaz de línea de mandatos analiza de forma automática el valor del parámetro `person` en un objeto estructurado que la acción ahora espera:
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### Configuración de los parámetros predeterminados
{: #openwhisk_binding_actions}

Las acciones se pueden invocar con varios parámetros con nombre. Recuerde que la acción `hello`
del ejemplo anterior espera dos parámetros: *name* (nombre) de una persona y *place* (lugar) del que es.

En lugar de pasar todos los parámetros a una acción cada vez, puede enlazar determinados parámetros. El ejemplo siguiente enlaza
el parámetro *place* para que el valor predeterminado de la acción sea el lugar (place) "Vermont":

1. Actualizar la acción usando la opción `--param` para enlazar valores de parámetros o pasando un archivo que contiene los parámetros a `--param-file`

  Para especificar parámetros predeterminados explícitamente en la línea de mandatos, especifique un par clave/valor para el distintivo `param`:

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  Para pasar parámetros desde un archivo hay que crear un archivo con el contenido deseado en formato JSON. Luego se debe pasar el nombre del archivo al distintivo `-param-file`:

  Consulte el siguiente archivo de parámetros de ejemplo denominado `parameters.json`:
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. Invocar la acción, pasando solo el parámetro `name` esta vez.

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Observe que no ha necesitado especificar el parámetro place al invocar la acción. Los parámetros enlazados aún se pueden
sobrescribir especificando el valor de parámetro en el momento de la invocación.

3. Invocar la acción, pasando los valores `name` y `place`. El segundo sobrescribe el
valor enlazado a la acción.

  Utilización del distintivo `--param`:

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  Utilización del distintivo `--param-file`:

  Archivo parameters.json:
  ```json
  {
    "name": "Bernie",
      "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### Obtener un URL de acción

Una acción se puede invocar mediante la interfaz REST mediante una solicitud HTTPS. Para obtener un URL de acción, ejecute el mandato siguiente:

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

Se debe proporcionar autenticación al invocar una acción mediante una solicitud HTTPS. Para obtener más información sobre las invocaciones de acción mediante la interfaz REST, consulte [Usi de API REST con OpenWhisk](./openwhisk_rest_api.html#openwhisk_rest_api).
{: tip}

### Guardar el código de acción

El código asociado con una acción existente se capta y se guarda localmente. El guardado se realiza en todas las acciones, excepto en las secuencias y las acciones de docker. Al guardar el código de acción en un archivo, el código se guarda en el directorio de trabajo actual, y se muestra la vía de acceso del archivo guardado.

1. Guarde el código de acción en un nombre de archivo que corresponda con un nombre de acción existente. Se utiliza una extensión de archivo que corresponde al tipo de acción, o una extensión de tipo `.zip` para el código de acción que es un archivo zip.
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. En lugar de permitir a la CLI que determine el nombre de archivo y la extensión del código guardado, se pueden proporcionar un nombre de archivo y una extensión personalizados mediante el distintivo `--save-as`.
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### Crear acciones asíncronas
{: #openwhisk_asynchrony_js}

Las funciones de JavaScript que se ejecutan de forma asíncrona pueden devolver el resultado de la activación tras el retorno de la función `main`.

1. Guardar el contenido siguiente en un archivo llamado `asyncAction.js`.

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

2. Ejecute los mandatos siguientes para crear la acción e invocarla:

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  wsk action invoke --result asyncAction
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  Tenga en cuenta que ha realizado una invocación de bloqueo de una acción asíncrona.

3. Recupere el registro de activación para ver cuánto tiempo ha tardado la activación en completarse:

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  activations
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  Si se comparan los registros de fecha y hora de `start` y `end` en el registro de activación,
puede ver que esta activación ha tardado poco más de 2 segundos en completarse.

### Uso de acciones para invocar una API externa
{: #openwhisk_apicall_action}

Los ejemplos hasta ahora sonn funciones JavaScript autocontenidas. También puede crear una acción que invoque una API externa.

En este ejemplo se invoca a un servicio Yahoo Weather para obtener la meteorología actual de una ubicación específica.

1. Guardar el contenido siguiente en un archivo llamado `weather.js`.

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
                }
                else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                    var text = condition.text;
                    var temperature = condition.temp;
                    var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                    resolve({msg: output});
                }
          });
      });
  }
  ```
  {: codeblock}

 La acción en este ejemplo utiliza la biblioteca `request` de JavaScript para
realizar una solicitud HTTP a la API Yahoo Weather, y extra campos del resultado JSON. Las
[Referencias](./openwhisk_reference.html#openwhisk_ref_javascript_environments) detallan los paquetes Node.js que
puede utilizar en sus acciones.

  En este ejemplo también se muestra la necesidad de acciones asíncronas. La acción devuelve un Promise para
indicar que el resultado de esta acción no está aún disponible cuando la función retorna. En su lugar, el
resultado está disponible en la devolución de llamada `request` tras completarse la llamada HTTP, y la pasa como
argumento a la función `resolve()`.

2. Ejecute los mandatos siguientes para crear la acción e invocarla:

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Empaquetar una acción como un módulo Node.js
{: #openwhisk_js_packaged_action}

Como alternativa a grabar todo el código de acción en un único archivo de origen JavaScript, puede grabar una acción como un paquete `npm`. Piense como ejemplo un directorio con los archivos siguientes:

En primer lugar, `package.json`:

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

A continuación, `index.js`:

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

Para crear una acción OpenWhisk desde este paquete:

1. Instale en primer lugar todas las dependencias localmente

  ```
  npm install
  ```
  {: pre}

2. Cree un archivo `.zip` que contenga todos los archivos (incluidas todas las dependencias):

  ```
  zip -r action.zip *
  ```
  {: pre}

  El uso de la acción de Windows Explorer para crear el archivo zip da lugar a una estructura incorrecta. Las acciones zip de OpenWhisk deben tener `package.json` como raíz del zip, mientras que Windows Explorer las coloca dentro de una carpeta anidada. La opción más segura consiste en utilizar el mandato `zip` desde la línea de mandatos.
  {: tip}

3. Cree la acción:

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Al crear una acción desde un archivo `.zip` con la herramienta CLI, debe proporcionar explícitamente un valor para el distintivo `--kind` utilizando `nodejs:6` o `nodejs:8`.

4. Puede invocar la acción como cualquier otra:

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

Finalmente, tenga en cuenta que mientras que la mayoría de los paquetes de `npm` instalan orígenes JavaScript en `npm install`, algunos también instalan y compilan artefactos binarios. La carga del archivo de archivado no da soporte en este momento a las dependencias binarias, sino únicamente a las dependencias de JavaScript. Las invocaciones de acción pueden fallar si el archivo incluye dependencias binarias.

### Empaquetar una acción como un paquete individual
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

Si el nombre de la función es `main`, utilice esta sintaxis:
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

Se proporcionan varias acciones de utilidad en un paquete llamado `/whisk.system/utils` que puede utilizar para crear su primera secuencia. Puede obtener más información sobre los paquetes en la sección [Paquetes](./openwhisk_packages.html).

1. Mostrar las acciones del paquete `/whisk.system/utils`.

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```

  En este ejemplo utiliza las acciones `split` y `sort`.

2. Crear una secuencia de acciones para que el resultado de una acción se pase como argumento a la acción siguiente.

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Esta secuencia de acciones convierte algunas líneas de texto en una matriz, y ordena las líneas.

3. Invoque la acción:

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  En el resultado, puede ver que las líneas se ordenan.

**Nota**: los parámetros que se pasan entre acciones de la secuencia son explícitos, salvo los predeterminados.
Por tanto, los parámetros que se pasan a la secuencia de acción solo están disponibles para la primera acción de la secuencia.
El resultado de la primera acción de la secuencia se convierte en el objeto JSON de entrada de la segunda acción de la secuencia (y así sucesivamente).
Este objeto no incluye ninguno de los parámetros que originalmente se han pasado a la secuencia, a menos que la primera acción los incluya explícitamente en el resultado.
Los parámetros de entrada a una acción se fusionan con los parámetros predeterminados de la acción, dando prioridad a los primeros y sustituyendo los parámetros predeterminados coincidentes.
Para obtener más información sobre la invocación de secuencias de acciones con varios parámetros con nombre, consulte [Establecimiento de parámetros predeterminados](./openwhisk_actions.html#openwhisk_binding_actions).

## Creación de acciones Python
{: #creating-python-actions}

El proceso de creación de acciones Python es parecido al de las acciones JavaScript. En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Python, y se añaden parámetros a dicha acción.

### Creación e invocación de una acción Python
{: #openwhisk_actions_python_invoke}

Una acción es sencillamente una función Python de nivel superior. Por ejemplo, cree un archivo denominado `hello.py` con el siguiente código fuente:

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
        return {"greeting": greeting}
```
{: codeblock}

Las acciones Python siempre consumen un diccionario y generan un diccionario. El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar de forma explícita para crear la acción con la CLI `wsk` utilizando `--main`, como con cualquier otro tipo de acción.

Puede crear una acción OpenWhisk denominada `helloPython` a partir de esta función de la siguiente manera:
```
wsk action create helloPython hello.py
```
{: pre}

La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.py`, la acción se ejecuta utilizando el tiempo de ejecución Python 2. También puede crear una acción que se ejecute con Python 3 especificando de forma explícita el parámetro `--kind python:3`. 
Además, hay un tiempo de ejecución Python 3 con el tipo `python-jessie:3` que contiene paquetes adicionales para IBM Cloud Services como IBM Cloudant, IBM DB2, IBM COS e IBM Watson.
Para obtener más información sobre los paquetes incluidos en este tiempo de ejecución de Python 3, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_python_environments) del tiempo de ejecución de Python.

La invocación de la acción es la misma para acciones Python que para acciones JavaScript:
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

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
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Aunque estos pasos se muestran para Python 3 (con el tipo `python:3`), puede hacer lo mismo con otros tipos de Python, como `python:2` o `python-jessie:3`.


### Empaquetamiento de acciones de Python con un entorno virtual en archivos zip
{: #openwhisk_actions_python_virtualenv}

Otra forma de empaquetar dependencias de Python es utilizando un entorno virtual (`virtualenv`), que permite enlazar a paquetes adicionales que, por ejemplo, se pueden instalar a través de [`pip`](https://packaging.python.org/installing/).


Como con el soporte al archivo zip básico, el nombre para el archivo fuente que contiene el punto de entrada principal debe ser `__main__.py`. Es decir, el contenido de `__main__.py` es la función principal, de modo que, para este ejemplo, puede cambiar el nombre de `hello.py` a `__main__.py` de la sección anterior. Además, el directorio virtualenv debe denominarse `virtualenv`. Consulte el siguiente escenario de ejemplo para instalar dependencias, empaquetándolas en un virtualenv, y creando una acción compatible con OpenWhisk.

Para asegurar la compatibilidad con el contenedor de tiempo de ejecución de OpenWhisk, las instalaciones de paquetes dentro de un virtualenv deben realizarse en el entorno de destino utilizando la imagen correspondiente al tipo.
- Para el tipo `python:2`, utilice la imagen de docker `openwhisk/python2action`.
- Para el tipo `python:3`, utilice la imagen de docker `openwhisk/python3action`.
- Para el tipo `python-jessie:3`, utilice la imagen de docker `ibmfunctions/action-python-v3`.

1. Dado un archivo [requirements.txt ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) con las versiones y módulos `pip` a instalar, ejecute lo siguiente para instalar las dependencias y crear un virtualenv utilizando una imagen Docker compatible:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. Archive el directorio virtualenv y todos los archivos Python adicionales:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Cree la acción:
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


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

Las acciones PHP siempre consumen una matriz asociativa y devuelven una matriz asociativa. El método de entrada para la acción es `main` de forma predeterminada, pero se puede especificar de forma explícita al crear la acción con la CLI `wsk` utilizando `--main`, como con cualquier otro tipo de acción.

Puede crear una acción OpenWhisk denominada `helloPHP` a partir de esta función de la siguiente manera:

```
wsk action create helloPHP hello.php
```
{: pre}

La CLI deduce automáticamente el tipo de acción a partir de la extensión del archivo de origen. Para archivos de origen `.php`, la acción se ejecuta utilizando el tiempo de ejecución PHP 7.1. Para obtener más información, consulte la [referencia](./openwhisk_reference.html#openwhisk_ref_php) de PHP.

La invocación de la acción es la misma para acciones PHP que para acciones JavaScript:

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Empaquetamiento de acciones PHP en archivos zip
{: #openwhisk_actions_php_zip}

Puede empaquetar una acción PHP junto con otros archivos y paquetes dependientes en un archivo zip.
El nombre del archivo fuente que contiene el punto de entrada (por ejemplo, `main`) debe ser `index.php.py`.
Por ejemplo, para crear una acción que incluya un segundo archivo denominado `helper.php`, primero cree un archivador que contenga sus archivos fuente:

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

A continuación, cree la acción:

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Creación de acciones Swift
{: #creating-swift-actions}

El proceso de creación de acciones Swift es parecido al de las acciones JavaScript. En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Swift, y se añaden parámetros a dicha acción.

También puede usar el recinto de pruebas [Swift Sandbox](https://swiftlang.ng.bluemix.net) en línea para probar el código Swift sin tener que instalar Xcode en su máquina.

### Creación e invocación de una acción

Una acción es sencillamente una función Swift de nivel superior. Por ejemplo, cree un archivo llamado
`hello.swift` con el contenido siguiente:

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

Las acciones Swift siempre consumen un diccionario y generan un diccionario.

Puede crear una acción de {{site.data.keyword.openwhisk_short}} llamada `helloSwift` desde esta función, según se indica a continuación:

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

Especifique siempre `swift:3.1.1`, ya que las versiones previas de Swift no están admitidas.
{: tip}

La invocación de la acción es la misma para acciones Swift que para acciones JavaScript:

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**Atención:** La ejecución de acciones Swift en un entorno Linux todavía está en desarrollo, y {{site.data.keyword.openwhisk_short}} suele utilizar el release disponible más reciente, que no es necesariamente estable. Además, la versión de Swift que se utiliza con {{site.data.keyword.openwhisk_short}} podría no ser coherente con versiones de Swift de releases estables de XCode en MacOS.

### Empaquetado de una acción como un ejecutable Swift
{: #openwhisk_actions_swift_zip}

Cuando cree una acción Swift de OpenWhisk con un archivo fuente Swift, se tiene que compilar en un binario antes de ejecutar la acción. Una vez hecho esto, las siguientes llamadas a la acción serán mucho más rápidas hasta que se depure el contenedor que alberga la acción. Este retraso se conoce como retraso de inicio en frío.

Para evitar el retraso de inicio en frío, puede compilar el archivo Swift en un binario y luego cargarlo en OpenWhisk en un archivo zip. Como necesita la protección de OpenWhisk, la forma más sencilla de crear el binario consiste en crearlo en el mismo entorno en el que se ejecuta. Consulte los siguientes pasos:

- Ejecute un contenedor utilizando el siguiente mandato:
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- Copie el código fuente y prepárese para compilarlo.
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (Opcional) Cree el archivo `Package.swift` para añadir dependencias.
   ```
   swift import PackageDescription
   
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
   {: pre}

  Este ejemplo añade las dependencias `swift-watson-sdk` y `example-package-deckofplayingcards`.
  Tenga en cuenta que `CCurl`, `Kitura-net` y `SwiftyJSON` se proporcionan en la acción estándar Swift de forma que puede incluirlos en su propio `Package.swift`.

- Copie Package.swift en el directorio spm-build
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- Vaya al directorio spm-build
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Compile la acción Swift.
  ```
  swift build -c release
  ```
  {: pre}

- Cree el archivo zip.
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Salga del contenedor Docker.
  ```
  exit
  ```
  {: pre}

Puede ver que se ha creado el archivo hello.zip en el mismo directorio que hello.swift. 

- Cárguelo en OpenWhisk con el nombre de acción helloSwifty:
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- Para comprobar lo rápido que es, ejecute 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

El tiempo que se ha tardado en ejecutar la acción está en la propiedad "duration" y se compara con el tiempo que se tarda en ejecutar con un paso de compilación en la acción hello.

## Creación de acciones de Java
{: #creating-java-actions}

El proceso de creación de acciones Java es parecido al de las acciones JavaScript y Swift. En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Swift, y se añaden parámetros a dicha acción.

Para compilar, probar y archivar archivos Java, debe tener un [JDK 8](http://openjdk.java.net/install) instalado localmente.

### Creación e invocación de una acción
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

Puede crear una acción OpenWhisk denominada `helloJava` a partir de este archivo JAR tal como se indica a continuación:

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

Cuando se utiliza la línea de mandatos y un archivo fuente `.jar`, no es necesario especificar que está creando una acción Java; la herramienta lo determina a partir de la extensión de archivo.

Tiene que especificar el nombre de la clase principal con `--main`. Una clase principal apta es una que implemente un método `main` estático. Si la clase no está en el paquete predeterminado, utilice el nombre de clase completo de Java, por ejemplo `--main com.example.MyMain`.

Si es necesario, también puede personalizar el nombre de método de la acción Java. Esto se realiza especificando el nombre completo del método de Java de la acción, por ejemplo, `--main com.example.MyMain#methodName`.

La invocación de la acción es la misma para acciones Java que para acciones Swift y JavaScript:

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

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
  wsk sdk install docker
  ```
  {: pre}

  El esqueleto Docker estará ahora instalado en el directorio actual.
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

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

  Tenga en cuenta que parte del archivo `example.c` está compilado como parte del proceso de compilación de imagen de Docker, por lo que no necesita C compilado en su máquina.
  De hecho, salvo cuando compile el binario en una máquina de host compatible, no puede ejecutarse dentro del contenedor porque los formatos no coinciden.

  El contenedor de Docker ahora se puede utilizar como una acción OpenWhisk.


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Observe el uso de `--docker` para crear una acción. Se presupone que todas las imágenes de Docker se alojan en Docker Hub.
  La acción se puede invocar como cualquier otra acción de {{site.data.keyword.openwhisk_short}}. 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  Para actualizar la acción de Docker, ejecute `buildAndPush.sh` para cargar la imagen más reciente en Docker Hub. Esto permite al sistema extraer la nueva imagen de Docker la próxima vez que ejecute el código para la acción. Si no hay contenedores recientes, las nuevas invocaciones utilizan la imagen de Docker nueva. Sin embargo, si hay un contenedor reciente que utiliza una versión anterior de la imagen de Docker, las nuevas invocaciones siguen utilizando esta imagen a no ser que se ejecute `wsk action update`. Esto indica al sistema que, para las nuevas invocaciones, debe ejecutar una extracción de Docker para obtener su nueva imagen de Docker.

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Encontrará más información sobre cómo crear acciones Docker en la sección [Referencias](./openwhisk_reference.html#openwhisk_ref_docker).

  La versión anterior de la CLI daba soporte a `--docker` sin ningún parámetro y el nombre de la imagen ha producido un argumento posicional. Para permitir que las acciones Docker acepten datos de inicialización a través de un archivo (zip), se ha normalizado la experiencia de usuario para acciones Docker para que un argumento posicional, si está presente, debe ser un archivo (por ejemplo, un archivo zip) en su lugar. El nombre de la imagen debe especificarse después de la opción `-- docker`. Gracias a los comentarios de los usuarios, se ha incluido el argumento `-- native` como abreviatura de `-- docker openwhisk/dockerskeleton`, de modo que los ejecutables que se ejecutan dentro de la acción estándar SDK Docker son más adecuados para crear y desplegar.
  
  Por ejemplo, esta guía de aprendizaje crea un ejecutable binario dentro de la ubicación del contenedor en `/action/exec`. Si copia este archivo a su sistema de archivos local y los comprime en `exec.zip`, a continuación, puede utilizar los mandatos siguientes para crear una acción docker que recibe el ejecutable como datos de inicialización. 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  Que es equivalente a invocar el mandato siguiente. 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Creación de acciones utilizando ejecutables arbitrarios
{: #creating-actions-arbitrary}

Mediante `--native`, puede ver que cualquier ejecutable puede ejecutarse como una acción de OpenWhisk. Esto incluye a los scripts `bash`, o a los binarios de compilación cruzada. Para estos últimos, la restricción es que el binario debe ser compatible con la imagen `openwhisk/dockerskeleton`.

## Creación de acciones Go
{: #creating-go-actions}

La opción `--native` permite empaquetar cualquier ejecutable como una acción. Esto funciona para Go, por ejemplo.
Como en las acciones Docker, el ejecutable de Go recibe un único argumento de la línea de mandatos.
Es la serialización de una cadena del objeto JSON que representa los argumentos para la acción.
El programa puede redirigir sus registros a `stdout` o `stderr`.
Por convenio, la última línea de la salida _debe_ ser un objeto JSON en forma de cadena que represente el resultado de la acción.

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

Guarde el código anterior en un archivo `sample.go` y cree con él una compilación cruzada para OpenWhisk. El ejecutable se debe denominar `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

La acción puede ejecutarse como cualquier otra acción.
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Los registros se recuperan también de una forma similar.
```bash
wsk activation logs --last --strip
my first Go action.
```

## Supervisión de la salida de acción
{: #watching-action-output}

Las acciones de {{site.data.keyword.openwhisk_short}} se podrían invocar por otros usuarios, en respuesta a varios sucesos, o como parte de una secuencia de acciones. En tales casos, puede ser útil supervisar las invocaciones.

Puede utilizar la interfaz de línea de mandatos de {{site.data.keyword.openwhisk_short}} para ver el resultado de la salida de las acciones a medida que se invocan.

1. Emita el siguiente mandato desde un shell:
  ```
  wsk activation poll
  ```
  {: pre}

  Este mandato inicia un bucle de sondeo que comprueba continuamente los registros de las activaciones.

2. Cambie a otra ventana e invoque la acción:

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. Observe el registro de activación en la ventana de sondeo:

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  De la misma forma, cuando ejecute el programa de utilidad de sondeo, verá en tiempo real los registros de las acciones que ejecute en nombre de OpenWhisk.

## Soporte para grandes aplicaciones
{: #large-app-support}

El tamaño máximo de código de una acción es de 48 MB. Las aplicaciones que contienen muchos módulos de terceros, bibliotecas nativas o herramientas externas pueden alcanzar este límite.

Si crea una acción de paquete (zip o jar) de más de 48 MB, la solución es ampliar la imagen de tiempo de ejecución con dependencias, y luego utilizar un único archivo de origen o un archivado de menos de 48 MB.

Por ejemplo, mediante la creación de un tiempo de ejecución de Docker personalizado, que incluye bibliotecas compartidas necesarias, no es necesario que estas dependencias estén presentes en el archivo de archivado. Los archivos de origen privado se pueden empaquetar en el archivado e introducirse en el tiempo de ejecución.

Otra ventaja de reducir el tamaño de los archivos de archivado es que también mejoran los tiempos de despliegue.

### Ejemplo Python

En el ejemplo Python siguiente, opencv puede incluir la biblioteca `opencv-python`, y luego instalar el binario de opencv en la imagen del SO. A continuación, puede utilizar `requirements.txt` y ejecutar `pip install requirements.txt` para aumentar la imagen con más bibliotecas Python. Después, puede utilizar `action.py` con la nueva imagen.

### Ejemplo Node.js

En el ejemplo Node.js siguiente, puede instalar paquetes adicionales en la imagen del SO:

Instale opencv mediante `npm`:
```
npm install opencv
```
{: pre}

Del mismo modo, si tiene un `package.json`, instálelo mediante `npm`:
```
npm install package.json
```
{: pre}

A continuación, utilice `action.js` con la nueva imagen.

## Lista de acciones
{: #listing-actions}

Puede generar una lista de todas las acciones creadas utilizando el siguiente mandato:

```
wsk action list
```
{: pre}

A medida que escriba más acciones, esta lista crece y puede resultar útil agrupar las acciones relacionadas en [paquetes](./openwhisk_packages.html). Para filtrar la lista de acciones y limitarla a las contenidas en un determinado paquete, puede utilizar la siguiente sintaxis de mandato: 

```
wsk action list [NOMBRE PAQUETE]
```
{: pre}

## Supresión de acciones
{: #deleting-actions}

Puede realizar una limpieza mediante la supresión de acciones que no quiera usar.

1. Ejecute el mandato siguiente para suprimir una acción:
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. Compruebe que la acción ya no aparece en la lista de acciones.
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## Acceso a los metadatos de acción dentro del cuerpo de la acción
{: #accessing-action-metadata-within-the-action-body}

El entorno de acción contiene varias propiedades que son específicas de la acción que se está ejecutando.
Esto permite que la acción funcione según programa con activos de OpenWhisk mediante la API REST o permite establecer una alarma interna cuando la acción está a punto de alcanzar su presupuesto de tiempo permitido.
Se accede a las propiedades mediante el entorno del sistema para todos los tiempos de ejecución admitidos: Node.js, Python, Swift, Java y acciones Docker cuando se utiliza el esqueleto OpenWhisk Docker.

* `__OW_API_HOST` - Host de API para el despliegue de OpenWhisk ejecutando esta acción.
* `__OW_API_KEY` - Clave de API para quien invoca la acción. Esta clave puede ser una clave de API restringida.
* `__OW_NAMESPACE` - Espacio de nombres para la _activación_ (puede no ser el mismo que el espacio de nombres para la acción).
* `__OW_ACTION_NAME` - Nombre completo calificado de la acción en ejecución.
* `__OW_ACTIVATION_ID` - ID de activación para esta instancia de acción en ejecución.
* `__OW_DEADLINE` - Tiempo aproximado cuando esta acción habrá consumido toda su cuota de duración (medido en milisegundos desde epoch).
