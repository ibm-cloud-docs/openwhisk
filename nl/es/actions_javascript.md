---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, javascript, node, node.js

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


# Creación de acciones de JavaScript
{: #actions_javascript
{: #creating-and-invoking-javascript-actions}

Las secciones siguientes le guían en la forma de trabajar con acciones en JavaScript. Empiece por crear e invocar una acción simple. A continuación, añada parámetros a la acción e invoque dicha acción con parámetros. Después, establezca parámetros predeterminados e invóquelos. Por último, cree acciones asíncronas.
{: shortdesc}

## Creación e invocación de una acción JavaScript sencilla
{: #single_action_js}
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
  {: shortdesc}

2. Cree una acción denominada `hello` con la función JavaScript.

  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  Salida de ejemplo:
  
  ```
  ok: created action hello
  ```
  {: screen}

  El tipo de acción se determina utilizando la extensión del archivo de origen. Para archivos de origen `.js`, la acción se ejecuta utilizando el entorno de ejecución Node.js. Puede especificar la versión del entorno de ejecución Node.js para su acción JavaScript estableciendo el parámetro `--kind` en `nodejs:10` o `nodejs:8`. Para obtener más información sobre el entorno de ejecución Node.js, consulte [Entornos de ejecución](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).
  {: shortdesc}

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

4. Ejecute la acción en la nube ejecutando una invocación de bloqueo. Las invocaciones de bloqueo utilizan un estilo de solicitud/respuesta y esperan a que el resultado de la activación esté disponible. El período de espera es inferior a 60 segundos o el [valor de límite de tiempo](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits) de la acción.

    ```
    ibmcloud fn action invoke --blocking hello
    ```
    {: pre}

    El mandato genera la siguiente información:
        * El ID de activación (`44794bd6aab74415b4e42a308d880e5b`), que se puede utilizar para recuperar los registros o el resultado de la invocación
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
    
    Para obtener sugerencias sobre las activaciones de supervisión, consulte [Supervisión de la salida de acción](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#monitor-action-output).
    {: tip}

5. Si no necesita el resultado de la acción inmediatamente, puede omitir el distintivo `--blocking` para ejecutar una invocación sin bloqueo.

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
    
### Prototipo de una función Javascript
{: #openwhisk_ref_javascript_fnproto}

Las acciones JavaScript de {{site.data.keyword.openwhisk_short}} se ejecutan en un tiempo de ejecución Node.js.

Las acciones que están escritas en JavaScript se deben confinar a un único archivo. El archivo puede contener varias funciones pero, por convenio, debe existir una función llamada `main` y es la que se llama cuando se invoca la acción. Por ejemplo, el ejemplo siguiente muestra una acción con varias funciones.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

Los parámetros de entrada de la acción se pasan como un objeto JSON como un parámetro a la función
`main`. El resultado de la activación correcta también es un objeto JSON pero se devuelve de forma distinta,
según si la acción es síncrona o asíncrona, según se describe en la sección siguiente.

### Comportamiento síncrono y asíncrono
{: #openwhisk_ref_javascript_synchasynch}

Es frecuente para funciones de JavaScript continuar la ejecución en una función de devolución de llamada incluso después de la devolución. Para ajustarse a este comportamiento, una activación de una acción de JavaScript puede ser *síncrona* o *asíncrona*.

Una activación de una acción de JavaScript es **síncrona** si la función main sale bajo una de las condiciones siguientes:

- La función main sale sin ejecutar una sentencia `return`.
- La función main sale al ejecutar una sentencia `return` que devuelve cualquier valor
*excepto* un Promise.

Consulte el siguiente ejemplo de una acción síncrona:

```javascript
// una acción en la que cada vía tiene como resultado una activación síncrona
function main(params) {
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

La activación de una acción de JavaScript es **asíncrona** si la función main sale devolviendo un Promise. En este caso, el sistema presupone que la acción sigue en ejecución, hasta que se haya cumplimentado o rechazado el Promise.
Empiece por crear una instancia de un nuevo objeto Promise y pasarlo a una función de devolución de llamada. La devolución de llamada tiene dos argumentos, resolve y reject, ambos son funciones. Todo el código asíncrono va dentro de una devolución de llamada.

En el siguiente ejemplo se muestra cómo rellenar un Promise llamando a la función resolve.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Este ejemplo muestra cómo rechazar un Promise llamando a la función reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Es posible que una acción sea síncrona en varias entradas y asíncrona en otras, como se muestra en el siguiente ejemplo.
```javascript
function main(params) {
     if (params.payload) {
        // activación asíncrona
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
       }, 100);
    })
     } else {
        // activación síncrona
         return {done: true};
      }
}
```
{: codeblock}

Independientemente de si la activación es síncrona o asíncrona, la invocación de la acción puede ser o no de bloqueo (blocking o non-blocking).

## Creación de acciones asíncronas
{: #asynchronous_javascript}
{: #openwhisk_asynchrony_js}

Las funciones de JavaScript que se ejecutan de forma asíncrona pueden devolver el resultado de la activación después de que de la función `main` retorne una promesa en la acción.

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

      * La llamada a `resolve()` cumple la promesa e indica que la activación se ha completado con normalidad.
      * Se puede utilizar una llamada a `reject()` para rechazar la promesa e indicar que la activación se ha completado de forma anómala.

2. Cree una acción denominada `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js --kind nodejs:10
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

4. Compruebe cuánto tiempo tardó la activación en completarse obteniendo el registro de activación.

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

## Uso de acciones para invocar una API externa
{: #apicall_action}
{: #openwhisk_apicall_action}

Los ejemplos proporcionados hasta ahora son funciones JavaScript autocontenidas. También puede crear una acción que invoque una API externa.
{: shortdesc}

En el ejemplo siguiente se invoca el servicio Astronomy Picture of the Day (APOD) de la NASA, que proporciona una imagen única de nuestro universo cada día.

1. Guarde el siguiente código en un archivo denominado `apod.js`.

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

2. Se realiza una llamada a la API APOD de la NASA y los se extraen del resultado JSON. Para obtener más información sobre los paquetes de Node.js que se pueden utilizar en sus acciones, consulte [Entornos de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments).

3. Cree una acción denominada `apod`.
    ```
    ibmcloud fn action create apod apod.js --kind nodejs:10
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

## Empaquetado de una acción como un módulo Node.js
{: #packaging_javascript_actions}
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
    * Debe denominar este archivo como `index.js` o especificar el nombre de archivo que prefiera como la propiedad `main` en `package.json`.

3. Instale todas las dependencias localmente.

    ```
    npm install
    ```
    {: pre}

    **Nota**: La mayoría de los paquetes de `npm` instalan orígenes JavaScript en `npm install`, algunos también instalan y compilan artefactos binarios. El archivo de archivado solo da soporte a dependencias de JavaScript. Las invocaciones de acción pueden fallar si el archivo incluye dependencias binarias.

4. Cree un archivo `.zip` que contenga todos los archivos, incluidas todas las dependencias.

    ```
    zip -r action.zip *
    ```
    {: pre}

    El uso de la acción de Windows Explorer para crear el archivo zip da lugar a una estructura incorrecta. Las acciones .zip de {{site.data.keyword.openwhisk_short}} deben tener `package.json` como raíz del zip, mientras que Windows Explorer las coloca dentro de una carpeta anidada. La opción más segura consiste en utilizar el mandato `zip` desde la línea de mandatos.
    {: tip}

5. Cree la acción. Al crear una acción a partir de un archivo `.zip`, debe establecer un valor para el parámetro
`--kind` para especificar la versión del entorno de ejecución Node.js. Elija entre `nodejs:8` y `nodejs:10`.

    ```
    ibmcloud fn action create packageAction action.zip --kind nodejs:10
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

## Empaquetado de una acción como un paquete individual
{: #webpack_javascript}
{: #openwhisk_js_webpack_action}

Si el empaquetamiento de la acción como .zip incluye demasiados archivos innecesarios o si necesita un despliegue más rápido, puede escribir el código mínimo en un archivo `.js` individual que incluya las dependencias.
{: shortdesc}

Puede empaquetar una acción utilizando un empaquetador de módulos de JavaScript como, por ejemplo, [webpack ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://webpack.js.org/concepts/). Cuando `webpack` procesa el código, se crea un gráfico de dependencias recursivamente que incluye todos los módulos que necesita la acción.

1. Guarde el código siguiente en un archivo denominado `package.json`. `webpack` se añade como una dependencia de desarrollo.

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
        ibmcloud fn action update my-action dist/bundle.js --kind nodejs:10
        ```
        {: pre}

    El archivo de paquete que `webpack` crea solo da soporte a dependencias de JavaScript. Las invocaciones a la acción podrían fallar si el paquete depende de dependencias de archivos binarios debido a que estas no se incluyen con el archivo `bundle.js`.
    {: tip}
    



