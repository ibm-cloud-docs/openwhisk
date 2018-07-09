---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Cómo trabajar con parámetros

Aprenda a definir parámetros en paquetes y acciones para el despliegue y cómo pasar parámetros a acciones durante la invocación. También puede utilizar un archivo para almacenar parámetros y pasar el nombre del archivo a la acción, en lugar de especificar cada parámetro individualmente en la línea de mandatos.
{: shortdesc}

Con acciones sin servidor, los datos se suministran añadiendo parámetros a las acciones, que se declaran como un argumento de la función sin servidor principal. Todos los datos llegan de este modo y los valores se pueden definir de varias formas diferentes. La primera opción consiste en especificar parámetros cuando se crea (o cuando se actualiza) una acción o un paquete. Esta opción resulta útil para los datos que se mantienen igual en cada ejecución, equivalentes a las variables de entorno en otras plataformas, o para los valores predeterminados que se pueden modificar en el momento de la invocación. La segunda opción consiste en especificar los parámetros cuando se invoca la acción, lo que modifica los parámetros definidos anteriormente.

## Paso de parámetros a una acción durante la invocación
{: #pass-params-action}

Los parámetros se pueden pasar a una acción cuando se invoca. Los ejemplos que se proporcionan utilizan JavaScript, pero todos los demás lenguajes funcionan de la misma forma. Para ver ejemplos detallados, consulte los temas sobre [Acciones de Javascript](./openwhisk_actions.html#creating-and-invoking-javascript-actions), [Acciones de Swift](./openwhisk_actions.html#creating-swift-actions), [Acciones de Python](./openwhisk_actions.html#creating-python-actions), [Acciones de Java](./openwhisk_actions.html#creating-java-actions), [Acciones de PHP](./openwhisk_actions.html#creating-php-actions), [Acciones de Docker](./openwhisk_actions.html#creating-docker-actions) o [Acciones de Go](./openwhisk_actions.html#creating-go-actions).

1. Usar parámetros en la acción. Por ejemplo, cree un archivo llamado **hello.js** con el siguiente contenido:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Los parámetros de entrada se pasan como un parámetro de objeto JSON a la función **main**. Fíjese
cómo los parámetros `name` y `place` se recuperan del objeto `params` en este ejemplo.

2. Actualice la acción **hello** para que esté lista para su uso:
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

  Si modifica los parámetros de credenciales que no son de servicio, al ejecutar el mandato `action update` con nuevos parámetros, se eliminarán los parámetros actuales que no se especifiquen en el mandato `action update`. Por ejemplo, si ejecuta `action update -p key1 new-value -p key2 new-value` pero omite otros parámetros definidos, dichos parámetros dejarán de existir después de que se actualice la acción. Los servicios enlazados a la acción también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios a la acción](./binding_services.html).
  {: tip}

3. Los parámetros se puede proporcionar explícitamente utilizando la línea de mandatos o [proporcionando archivo](./parameters.html#using-parameter-files) que contenga los parámetros deseados.

  Para pasar los parámetros directamente a través de la línea de mandatos, especifique un par clave/valor para el distintivo `--param`:
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **Respuesta:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Fíjese en el uso de la opción `--result`: implica una invocación con bloqueo donde la interfaz de línea de mandatos espera que se complete la activación y, a continuación, solo visualiza el resultado. Para su comodidad, esta opción se puede utilizar sin `--blocking` que se infiere de forma automática.

  Además, si los valores de parámetro especificados en una línea de mandatos tienen un formato JSON válido, se analizan y envían con su acción como un objeto estructurado.

  Por ejemplo, actualice la acción **hello** con lo siguiente:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Ahora la acción espera un único parámetro `person` con los campos `name` y `place`.

  A continuación, invoque la acción con un único parámetro `person` con un formato JSON válido, como en el siguiente ejemplo:
  ```
  ibmcloud wsk action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **Respuesta:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  El resultado es el mismo porque la interfaz de línea de mandatos analiza de forma automática el valor del parámetro `person` en un objeto estructurado que la acción ahora espera.

## Configuración de los parámetros predeterminados en una acción
{: #default-params-action}

Las acciones se pueden invocar con varios parámetros con nombre. Recuerde que la acción **hello**
del ejemplo anterior espera dos parámetros: *name* (nombre) de una persona y *place* (lugar) del que es.

En lugar de pasar todos los parámetros a una acción cada vez, puede enlazar determinados parámetros. El ejemplo siguiente enlaza el parámetro *place* para que el valor predeterminado de la acción sea el lugar (place) "Kansas":

1. Actualizar la acción usando la opción `--param` para enlazar valores de parámetros o pasando un archivo que contiene los parámetros a `--param-file`. (Para ver ejemplos que utilizan archivos, consulte la sección sobre [utilización de archivos de parámetros](./parameters.html#using-parameter-files).

  Para especificar parámetros predeterminados explícitamente en la línea de mandatos, especifique un par clave/valor para el distintivo `param`:
  ```
  ibmcloud wsk action update hello --param place Kansas
  ```
  {: pre}

2. Invocar la acción pasando solo el parámetro `name` esta vez.
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Observe que no ha necesitado especificar el parámetro place al invocar la acción. Los parámetros enlazados aún se pueden
sobrescribir especificando el valor de parámetro en el momento de la invocación.

3. Para invocar la acción, pase los valores `name` y `place` y observe la información de salida:

  Invoque la acción usando el distintivo `--param`:
  ```
  ibmcloud wsk action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  Los parámetros definidos sobre una acción cuando se crea o se actualiza siempre se sustituyen por los parámetros suministrados directamente en la invocación.
  {: tip}

## Configuración de los parámetros predeterminados en un paquete
{: #default-params-package}

Los parámetros se pueden definir a nivel de paquete y sirven como parámetros predeterminados para las acciones _a menos que_:

- La propia acción tenga un parámetro predeterminado.
- La acción tenga un parámetro suministrado en el momento de la invocación, que siempre tiene "prioridad" cuando hay más de un parámetro disponible.

En el ejemplo siguiente se define un parámetro predeterminado de `name` en el paquete **MyApp** y se muestra una acción que lo utiliza.

1. Cree un paquete con un parámetro definido:

  ```
  ibmcloud wsk package update MyApp --param name World
  ```
  {: pre}

2. Cree una acción en el paquete **MyApp**:
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Cree la acción:
  ```
  ibmcloud wsk action update MyApp/hello hello.js
  ```
  {: pre}

3. Invoque la acción y observe el parámetro de paquete predeterminado que se utiliza:
  ```
  ibmcloud wsk action invoke --result MyApp/hello
  ```
  {: pre}

  Salida de ejemplo:
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## Utilización de archivos de parámetros
{: #using-parameter-files}

Puede colocar parámetros en un archivo en formato JSON y luego pasar los parámetros especificando el nombre del archivo con el distintivo `--param-file`. Este método se puede utilizar tanto para la creación (o actualización) de paquetes como de acciones y también durante la invocación de la acción.

1. A modo de ejemplo, utilizaremos el ejemplo **hello** anterior y usaremos `hello.js` con el siguiente contenido:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Actualice la acción con el contenido actualizado de `hello.js`:

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. Cree un archivo de parámetros denominado `parameters.json` que contenga parámetros con formato JSON:

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. Utilice el nombre de archivo `parameters.json` al invocar la acción **hello** y observe la información de salida:

  ```
  ibmcloud wsk action invoke --result hello --param-file parameters.json
  ```

  Salida de ejemplo:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
