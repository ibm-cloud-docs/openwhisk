---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

---



{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}




# CLI de {{site.data.keyword.openwhisk_short}}
{: #functions-cli}

Ejecute estos mandatos para gestionar las entidades que componen las funciones.
{: shortdec}

<br />

## Mandatos de acción
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

Crear una acción.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>El nombre de la acción. Para incluir la acción en un paquete, especifique el nombre con el formato `PACKAGE_NAME`/`ACTION_NAME`. Este valor es obligatorio. </dd>

   <dt>`APP_FILE`</dt>
   <dd>La vía de acceso al archivo o paquete de la app a ejecutar como acción. Esta opción es obligatoria.</dd>
   
   <dt>`--copy`</dt>
   <dd>Tratar la acción como el nombre de una acción existente.</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>El nombre de usuario de Docker Hub y el nombre de la imagen Docker en Docker Hub para ejecutar la acción. Este distintivo es obligatorio para crear acciones a partir de imágenes Docker.</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>El entorno de ejecución de la app. Este distintivo es opcional. Si no se especifica ningún valor, se utiliza la versión predeterminada para el entorno de ejecución detectado``.
     `Valores` posibles para la opción `--kind`.
     <table>
  <tr>
    <th>Lenguaje</th>
    <th>Identificador de tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (predeterminado), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Hay soporte para otros lenguajes utilizando acciones de Docker.</td>
  </tr>
</table>
{: caption="Tabla 1. Entornos de ejecución admitidos" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>El tamaño máximo de registro en MB para la acción. El valor predeterminado es 10 MB.</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>Si el método de entrada de la acción no es `main`, especifique el nombre personalizado. Este distintivo es obligatorio cuando el método de entrada no es `main`. Para algunos entornos de ejecución, como por ejemplo Java, el nombre debe ser el método completo.</dd>

   <dt>`--native`</dt>
   <dd>Puede utilizar el argumento `--native` como abreviatura de `--docker openwhisk/dockerskeleton`. Utilizando este argumento, puede crear y desplegar un ejecutable que se ejecuta dentro del SDK de acción de Docker estándar.
       <ol><li>Cuando se crea una imagen Docker, se crea un ejecutable dentro del contenedor en `/action/exec`. Copie el archivo `/action/exec` al sistema de archivos local y comprímalo en un archivo `exec.zip`.</li>
       <li>Cree una acción de Docker que reciba el ejecutable como datos de inicialización. El argumento `--native` sustituye al argumento `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>Cree una secuencia de acciones e incluya los nombres de las acciones relacionadas. Separe los `ACTION_NAME` por comas.</dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>El límite (`LIMIT`) de tiempo de espera en milisegundos. El valor predeterminado es 60000 milisegundos. Cuando se alcanza el tiempo de espera, la acción finaliza.</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>Tratar la acción como una acción web, como una acción web de HTTP sin procesar o como una acción estándar. Especifique `yes` o `true` para una acción web, `raw` para una acción web de HTTP sin procesar, o `no` o `false` para una acción estándar. Para proteger la acción web, incluya también la opción `--web-secure`.</dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>Proteja la acción web. El valor (`VALUE`) de `SECRET` puede ser `true`, `false`, o cualquier serie. Esta opción sólo se puede utilizar con la opción `--web`.</dd>
   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **Resultado**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

Puede realizar una limpieza del espacio de nombres suprimiendo acciones que ya no quiera usar.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **Resultado**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

Obtener metadatos que describen una acción específica.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>El nombre de una acción. Este valor es obligatorio.</dd>

   <dt>`--save`</dt>
   <dd>Puede obtener y guardar de forma local el código asociado con una acción existente, excepto para las secuencias y las acciones de Docker. El campo `FILENAME` corresponde a un nombre de acción existente en el directorio de trabajo actual y la extensión de archivo corresponde al tipo de acción. Para un código de acción que es un archivo de archivado, se utiliza una extensión .zip. Este distintivo es opcional.</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>Guarde el código para las acciones en un archivo con nombre personalizado proporcionando una vía de acceso de archivo, `FILENAME`, y una extensión. Este distintivo es opcional.</dd>

  <dt>`--summary`</dt>
  <dd>Obtener un resumen de los detalles de la acción. Los parámetros con el prefijo "*" están enlazados; los parámetros con el prefijo "**" están enlazados y finalizados. Este distintivo es opcional.</dd>

  <dt>`--url`</dt>
  <dd>Obtener sólo el URL de la acción. Este distintivo es opcional.</dd>
   </dl>

<br />**Ejemplo**

```
ibmcloud fn action get hello
```
{: pre}

**Resultado**
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
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

Ejecutar una acción para probarla.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>El nombre de la acción. Este valor es obligatorio. </dd>

   <dt>`--blocking, -b`</dt>
   <dd>Las invocaciones de bloqueo utilizan un estilo de solicitud y respuesta para esperar a que el resultado de la activación esté disponible. El período de espera es el que sea menor entre 60 segundos o el [valor (`VALUE`) de límite (`LIMIT`) de tiempo](/docs/openwhisk?topic=cloud-functions-limits) de la acción. Este distintivo es opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

   <dt>`--result, -r`</dt>
   <dd>El resultado del código de la app se muestra como la salida del mandato. Si no se especifica esta opción, se muestra el ID de activación. La invocación es con bloqueo cuando se especifica esta opción. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `ibmcloud fn action list`
{: #cli_action_list}

Enumerar todas las acciones que ha creado o un número específico de acciones.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>El nombre de un paquete de acciones. Este valor es opcional. Si no se especifica, se listan todas las acciones.</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>Listar un número especificado de acciones. El valor predeterminado es 30 acciones.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar la lista de acciones devueltas por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>Excluir un número especificado de las acciones creadas más recientemente del resultado.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

Actualizar una acción o la app de dentro de una acción.

Cuando actualiza parámetros para un paquete, acción o desencadenante, debe especificar todos los parámetros creados previamente. De lo contrario, los parámetros creados anteriormente se eliminan. Para los paquetes, los servicios enlazados al paquete también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios](/docs/openwhisk?topic=cloud-functions-services) al paquete.
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Opciones del mandato**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>El nombre de la acción. Para incluir la acción en un paquete, especifique el nombre con el formato `PACKAGE_NAME`/`ACTION_NAME`. Este valor es obligatorio. </dd>

  <dt>`APP_FILE`</dt>
  <dd>La vía de acceso al archivo o paquete de la app a ejecutar como acción. Esta opción es obligatoria cuando se desea actualizar la app dentro de la acción.</dd>

  <dt>`--copy`</dt>
  <dd>Tratar la acción como el nombre de una acción existente.</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>El nombre de usuario de Docker Hub y el nombre de la imagen Docker en Docker Hub para ejecutar la acción. Este distintivo es obligatorio para crear acciones a partir de imágenes Docker.</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>El entorno de ejecución de la app. Este distintivo es opcional. Si no se especifica ningún valor, se utiliza la versión predeterminada para el entorno de ejecución detectado.
    Valores posibles para la opción `--kind`.
    <table>
  <tr>
    <th>Lenguaje</th>
    <th>Identificador de tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (predeterminado), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (predeterminado)</td>
  </tr>
  <tr>
    <td>Hay soporte para otros lenguajes utilizando acciones de Docker.</td>
  </tr>
</table>
{: caption="Tabla 1. Entornos de ejecución admitidos" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>El tamaño máximo de registro en MB para la acción. El valor predeterminado es 10 MB.</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>Si el método de entrada de la acción no es `main`, especifique el nombre personalizado. Este distintivo es obligatorio cuando el método de entrada no es `main`. Para algunos entornos de ejecución, como por ejemplo Java, el nombre debe ser el método completo.</dd>

  <dt>`--native`</dt>
  <dd>Puede utilizar el argumento `--native` como abreviatura de `--docker openwhisk/dockerskeleton`. Utilizando este argumento, puede crear y desplegar un ejecutable que se ejecuta dentro del SDK de acción de Docker estándar.
      <ol><li>Cuando se crea una imagen Docker, se crea un ejecutable dentro del contenedor en `/action/exec`. Copie el archivo `/action/exec` al sistema de archivos local y comprímalo en un archivo `exec.zip`.</li>
      <li>Cree una acción de Docker que reciba el ejecutable como datos de inicialización. El argumento `--native` sustituye al argumento `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Un archivo JSON que contiene los parámetros `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>Crear una secuencia de acciones especificando el nombre de acciones relacionadas.</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>El límite de tiempo de espera en milisegundos. El valor predeterminado es 60000 milisegundos. Cuando se alcanza el tiempo de espera, la acción finaliza.</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>Tratar la acción como una acción web, como una acción web de HTTP sin procesar o como una acción estándar. Especifique `yes` o `true` para una acción web, `raw` para una acción web de HTTP sin procesar, o `no` o `false` para una acción estándar. Para proteger la acción web, incluya también la opción `--web-secure`.</dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>Proteja la acción web. El valor (`VALUE`) de `SECRET` puede ser `true`, `false`, o cualquier serie. Esta opción sólo se puede utilizar con la opción `--web`.</dd>
  </dl>

<br />**Ejemplo**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Mandatos de activación
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

Obtener metadatos que describen una activación específica.

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**Opciones del mandato**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>El ID de una activación específica. Utilice `ibmcloud fn activation list` para recuperar una lista de ID disponibles. Este valor es obligatorio, a menos que se especifique la opción `--last` o `-l`.</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>El campo de los metadatos del que mostrar información. Por ejemplo, para mostrar el campo registros (logs), ejecute `ibmcloud fn activation get ACTIVATION_ID logs`. Este valor es opcional.</dd>

  <dt>`--last, -l`</dt>
  <dd>Mostrar los metadatos de la activación más reciente. Este distintivo es opcional.</dd>

  <dt>`--summary, -s`</dt>
  <dd>Mostrar la respuesta resultado sólo de los detalles de activación. Este distintivo es opcional.</dd>
  </dl>


<br />**Ejemplo**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list`
{: #cli_activation_list}

Listar todos los ID de activación de todas las acciones de un paquete.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**Opciones del mandato**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>Mostrar la descripción de activación completa.</dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Listar un número especificado de activaciones. El valor predeterminado es 30 activaciones y el máximo es 200 activaciones.</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>Listar activaciones que se han creado desde la fecha especificada. Duración se mide en milisegundos desde el 01 de enero de 1970. Ejemplo: `1560371263` es el 12 de junio de 2019 a las 08:27:43 UTC.</dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Excluir un número especificado de las activaciones más recientes del resultado.</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>Listar activaciones que se han creado antes de la fecha especificada. Duración se mide en milisegundos desde el 01 de enero de 1970. Ejemplo: `1560371263` es el 12 de junio de 2019 a las 08:27:43 UTC.</dd>
  </dl>

<br />**Ejemplo**
```
ibmcloud fn activation list
```
{: pre}

**Resultado**
```
activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

Obtener los registros de una activación específica.

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**Opciones del mandato**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>El ID de una activación específica. Utilice `ibmcloud fn activation list` para recuperar una lista de ID disponibles. Este valor es obligatorio, a menos que se especifique la opción `--last` o `-l`.</dd>

  <dt>`--last, -l`</dt>
  <dd>Mostrar los registros de la activación más reciente. Este distintivo es opcional.</dd>

  <dt>`--strip, -r`</dt>
  <dd>Mostrar sólo el mensaje de registro; excluir la indicación de fecha y hora y la información de secuencia. Este distintivo es opcional.</dd>
  </dl>

<br />**Ejemplo**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

Ver una lista activa en modalidad continua de activaciones para una acción o un espacio de nombres. Puede pulsar `CTRL+C` para salir del sondeo.

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**Opciones del mandato**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>Un espacio de nombres que empiece por /. Sondear activaciones para un espacio de nombres, una acción o un espacio. Este valor es opcional. Si no se especifica un espacio de nombres o una acción, se sondea el espacio.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Sondear activaciones para un espacio de nombres, una acción o un espacio. Este valor es opcional. Si no se especifica un espacio de nombres o una acción, se sondea el espacio.</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>Sondear activaciones durante un número especificado de segundos y luego salir. Este distintivo es opcional.</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de días atrás. Este distintivo es opcional.</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de horas atrás. Este distintivo es opcional.</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de minutos atrás. Este distintivo es opcional.</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de segundos atrás. Este distintivo es opcional.</dd>
  </dl>

<br />**Ejemplo**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

Obtener el resultado de una activación específica.

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**Opciones del mandato**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>El ID de una activación específica. Utilice `ibmcloud fn activation list` para recuperar una lista de ID disponibles. Este valor es obligatorio, a menos que se especifique la opción `--last` o `-l`.</dd>

  <dt>`--last, -l`</dt>
  <dd>Mostrar el resultado de la activación más reciente. Este distintivo es opcional.</dd>

  </dl>

<br />**Ejemplo**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## Mandatos de API
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

Crear una API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>`API_NAME`</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>`API_VERB`</dt>
   <dd>El verbo de la API, como por ejemplo `get` o `post`.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>El nombre de la acción.</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>El nombre de la API. Este distintivo se ignora cuando se especifica un archivo de configuración. El nombre predeterminado es la vía de acceso base (`BASE_PATH`). Este distintivo es opcional.</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>Un archivo JSON que contiene la configuración de la API de Swagger. Cuando se utiliza este distintivo, se ignora el distintivo de nombre de API. Este distintivo es obligatorio.</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>Establezca el tipo de respuesta de acción web como `html`, `http`, `json`, `text` o `svg`. El valor predeterminado es `json`. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Resultado**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

Suprimir una API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>`API_NAME`</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>`API_PATH`</dt>
   <dd>La vía de acceso a la API.</dd>

   <dt>`API_VERB`</dt>
   <dd>El verbo de la API, como por ejemplo `GET` o `POST`.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Especifique el tipo de salida de API como `json` o `yaml`. El valor predeterminado es `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Mostrar todos los detalles de configuración de la API.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

Obtener los metadatos para una API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>`API_NAME`</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Especifique el tipo de salida de API como `json` o `yaml`. El valor predeterminado es `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Mostrar todos los detalles de configuración de la API.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `ibmcloud fn api list`
{: #cli_api_list}

Enumerar todas las API que ha creado o un número específico de API. Si no se especifica ningún nombre o vía de acceso base, se listan todas las API.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>`API_NAME`</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>`API_PATH`</dt>
   <dd>La vía de acceso a la API.</dd>

   <dt>`API_VERB`</dt>
   <dd>El verbo de la API, como por ejemplo `GET` o `POST`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Mostrar todos los detalles de la API. Este distintivo es opcional. </dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>Listar un número especificado de API. El valor predeterminado es 30 API. Este distintivo es opcional. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar la lista de las API devueltas por nombre, si no, la lista se ordena por fecha de creación. Este distintivo es opcional. </dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>Excluir un número especificado de las API creadas más recientemente del resultado. Este distintivo es opcional. </dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## Mandatos de despliegue
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

Utilice un archivo de manifiesto para desplegar una colección de paquetes, acciones, desencadenantes y reglas.

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>El host de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>El archivo de configuración. El valor predeterminado es `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>La vía de acceso al archivo de despliegue.</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>La vía de acceso al archivo de manifiesto. Este distintivo es obligatorio si manifest.yaml no se encuentra en el directorio actual.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>El nombre o el ID de un espacio de nombres.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

   <dt>`--preview` </dt>
   <dd>Mostrar el plan de despliegue antes de desplegar.</dd>

   <dt>`--project PATH`</dt>
   <dd>La vía de acceso al proyecto sin servidor. El valor predeterminado es <code>.</code> (directorio actual).</dd>

   <dt>`--strict`</dt>
   <dd>Permitir una versión de entorno de ejecución definida por el usuario.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Ver salida detallada.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Utilizar un archivo de manifiesto para anular el despliegue de una colección de paquetes, acciones, desencadenantes y reglas.

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>El host de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>El archivo de configuración. El valor predeterminado es `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>La vía de acceso al archivo de despliegue.</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>La vía de acceso al archivo de manifiesto. Este distintivo es obligatorio si manifest.yaml no se encuentra en el directorio actual.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>El nombre o el ID de un espacio de nombres.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

   <dt>`--preview` </dt>
   <dd>Mostrar el resultado del mandato sin ejecutar el mandato.</dd>

   <dt>`--project PATH`</dt>
   <dd>La vía de acceso al proyecto sin servidor. El valor predeterminado es `.` (directorio actual).</dd>

   <dt>`--strict`</dt>
   <dd>Permitir una versión de entorno de ejecución definida por el usuario.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Ver salida detallada.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## Mandato List
{: #cli_list_cmd}


### `ibmcloud fn list`
{: #cli_list}

Ver una lista agrupada de los paquetes, acciones, desencadenantes y reglas en el espacio de nombres.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar cada grupo de entidades devueltas por nombre, si no, cada grupo se ordena por fecha de creación.</dd>
   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Mandatos de espacio de nombres
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

Crear un espacio de nombres IAM.

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>El nombre de un espacio de nombres. No incluya guiones (-) en el nombre. Este valor es obligatorio.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Escriba su propia descripción exclusiva para ayudarle a identificar el espacio de nombres. Si su descripción tiene más de una palabra, incluya comillas (") alrededor de la descripción. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

Suprimir un espacio de nombres IAM.

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**Ejemplo**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Obtener las entidades o la información de metadatos de un espacio de nombres Cloud Foundry o IAM.

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>El nombre o el ID de un espacio de nombres. Este valor es obligatorio.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar la lista de espacios de nombres devueltos por nombre, si no, la lista se ordena por fecha de creación. Este distintivo es opcional. </dd>

   <dt>`--properties`</dt>
   <dd>Mostrar las propiedades del espacio de nombres en lugar de las entidades que contiene. Este distintivo es opcional. </dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **Resultado**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

Listar los espacios de nombres Cloud Foundry e IAM disponibles.

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--cf`</dt>
   <dd>Mostrar sólo los espacios de nombres Cloud Foundry. Los espacios de nombres IAM no se muestran. Este distintivo es opcional.</dd>

   <dt>`--iam`</dt>
   <dd>Mostrar sólo los espacios de nombres IAM. Los espacios de nombres Cloud Foundry no se muestran. Este distintivo es opcional.</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Listar un número especificado de espacios de nombres. El valor predeterminado es 30 espacios de nombres. Este distintivo es opcional. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar la lista de espacios de nombres devueltos por nombre, si no, la lista se ordena por fecha de creación. Este distintivo es opcional. </dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Excluir un número especificado de los espacios de nombres creados más recientemente del resultado. Este distintivo es opcional. </dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

Cambiar el nombre o la descripción de un espacio de nombres IAM.

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>El nombre de un espacio de nombres. No incluya guiones (-) en el nombre. Este valor es obligatorio.</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>El nuevo nombre de un espacio de nombres. No incluya guiones (-) en el nombre. Este valor es opcional.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Escriba su propia descripción exclusiva para ayudarle a identificar el espacio de nombres. Si su descripción tiene más de una palabra, incluya comillas (") alrededor de la descripción. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Mandatos de paquetes
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

Enlazar parámetros en un paquete. Todas las acciones dentro del paquete heredarán esos parámetros a menos que se especifique lo contrario.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opciones del mandato**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>El nombre del paquete. Este valor es obligatorio. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>
  </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

Crear un paquete diseñado para contener una o más acciones. Para añadir una acción en el paquete, incluya el nombre del paquete con el nombre de la acción cuando cree o actualice la acción.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opciones del mandato**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>El nombre del paquete. Este valor es obligatorio. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Un archivo JSON que contiene el formato de los parámetros `KEYS` `VALUE`. Este distintivo es opcional.</dd>

  <dt>`--shared yes|no`</dt>
  <dd>Cuando se especifica sin valor o con el valor yes, el paquete se comparte con otros usuarios.</dd>
  </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Resultado**
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

Puede realizar una limpieza del espacio de nombres suprimiendo paquetes que ya no quiera usar.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **Resultado**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

Obtener metadatos que describen un paquete específico.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**Opciones del mandato**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>El nombre de un paquete. Este valor es obligatorio.</dd>

   <dt>`--summary`</dt>
   <dd>Obtener un resumen de los detalles del paquete. Los parámetros con el prefijo "*" están enlazados. Este distintivo es opcional.</dd>
   </dl>

<br />**Ejemplo**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `ibmcloud fn package list`
{: #cli_pkg_list}

Enumerar todos los paquetes que ha creado o un número específico de paquetes.

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>Listar los paquetes de un espacio de nombres específico. Este valor es opcional. Si no se especifica, se listan todos los paquetes.</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>Listar un número especificado de paquetes. El valor predeterminado es 30 paquetes.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar la lista de paquetes devueltos por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>Excluir un número especificado de los paquetes creados más recientemente del resultado.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Ejecute `ibmcloud fn package list /whisk.system` para ver una lista de los paquetes preinstalados.
  {: tip}


<br />

### `ibmcloud fn package refresh`
{: #cli_pkg_refresh}

Renovar los enlaces de paquete para todos los paquetes dentro de un espacio de nombres específico.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>Un espacio de nombres que empiece por /. Este distintivo es obligatorio. Ejecute `ibmcloud fn namespace list` para obtener una lista de espacios de nombres entre los que elegir.</dd>
   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

Actualizar un paquete diseñado para que contenga una o más acciones. Para añadir una acción en el paquete, incluya el nombre del paquete con el nombre de la acción cuando cree o actualice la acción.

Cuando actualiza parámetros para un paquete, acción o desencadenante, debe especificar todos los parámetros creados previamente. De lo contrario, los parámetros creados anteriormente se eliminan. Para los paquetes, los servicios enlazados al paquete también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios](/docs/openwhisk?topic=cloud-functions-services) al paquete.
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>El nombre del paquete. Este valor es obligatorio. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

   <dt>`--shared yes|no`</dt>
   <dd>Cuando se especifica sin valor o con el valor `yes`, el paquete se comparte con otros usuarios.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Resultado**
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## Mandatos de propiedades
{: #cli_prop}

Establezca las propiedades globales para el entorno de CLI o visualice las propiedades sobre la CLI de `wsk`, que se ejecuta como parte de la CLI de `ibmcloud fn`.

### `ibmcloud fn property get`
{: #cli_prop_get}

Ver los detalles de los metadatos de una propiedad desde la CLI de `wsk`.

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`--all`</dt>
   <dd>Ver todas las propiedades de la CLI de `wsk`. Este distintivo es opcional.</dd>

   <dt>`---apibuild`</dt>
   <dd>La información de compilación de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--apibuildno`</dt>
   <dd>El número de compilación de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>El host de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>La versión de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>El certificado de cliente de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--cliversion`</dt>
   <dd>La versión de la CLI de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>El cliente `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Un espacio de nombres de IAM. Este distintivo no se puede establecer para los espacios de nombres de Cloud Foundry. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn property get --auth
  ```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

Establecer una propiedad. Se requiere al menos un distintivo. Una vez que se ha establecido una propiedad, ésta se retiene en la estación de trabajo
en `<home_dir>/.bluemix/plugins/cloud-functions/config.json`. Para eliminar una propiedad, ejecute [`ibmcloud fn property unset --<property>`](#cli_prop_set). 

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>El host de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>La versión de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>El certificado de cliente de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>El cliente `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Un espacio de nombres de IAM. Este distintivo no se puede establecer para los espacios de nombres de Cloud Foundry. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **Resultado**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

Desestablecer una propiedad para la CLI de `wsk`. Se requiere al menos un distintivo.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>El host de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>La versión de la API de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>La autorización `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>El certificado de cliente de `wsk`. Este distintivo es opcional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>El cliente `wsk` de `KEY`. Este distintivo es opcional.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Un espacio de nombres de IAM. Este distintivo no se puede establecer para los espacios de nombres de Cloud Foundry. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## Mandatos de regla
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

Crear una regla para asociar un desencadenante con una acción. Antes de poder crear una regla, debe crear primero un desencadenante y una acción.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **Resultado**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

Para limpiar el espacio de nombres, elimine las reglas que ya no necesita.

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>El nombre de una regla. Este valor es obligatorio.</dd>

  <dt>`--disable`</dt>
  <dd>Inhabilite la regla antes de suprimirla.</dd>
  </dl>


<br />**Ejemplo**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

Cambie el estado de una regla a inactiva y detenga la ejecución de una acción cuando se active un desencadenante.

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

Cambiar el estado de una regla de inactivo a activo. Cuando está activo, se ejecuta una acción cuando se activa un desencadenante.

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

Obtener metadatos que describen una regla específica.

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>El nombre de una regla. Este valor es obligatorio.</dd>

  <dt>`--summary`</dt>
  <dd>Obtener un resumen de los detalles de la regla.</dd>
  </dl>

<br />**Ejemplo**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `ibmcloud fn rule list`
{: #cli_rule_list}

Enumerar todas las reglas que ha creado o un número específico de reglas.

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>El nombre de una regla. Este valor es opcional. Si no se especifica, se listan todas las reglas.</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>Listar un número especificado de reglas. El valor predeterminado es 30 reglas.</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>Ordenar la lista de reglas devueltas por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>Excluir un número especificado de las reglas creadas más recientemente del resultado.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

Ver si una regla está activa o inactiva. Ejecute los mandatos `ibmcloud fn rule disable` o `ibmcloud fn run enable` para cambiar el estado.

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

Para cambiar los desencadenantes asociados a las reglas, puede actualizar una regla.

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Ejemplo**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## Mandato SDK
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

Instalar un SDK.

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>El componente SDK, como por ejemplo `docker`, `iOS` y `bashauto`. Este valor es obligatorio.</dd>

   <dt>`--stdout, --s`</dt>
   <dd>Imprimir los resultados del mandato bash a `STDOUT`. Este distintivo es opcional.</dd>


   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Mandatos de servicio
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

Enlazar un servicio a una acción o paquete.

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>El nombre del servicio.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>El nombre del paquete o la acción al que se deben enlazar las credenciales.</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>El nombre de la instancia de servicio.</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>El nombre de las credenciales de servicio `KEY` a enlazar.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

Desenlazar las credenciales de servicio de una acción o paquete.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>El nombre del servicio.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>El nombre del paquete o acción del que se deben desenlazar las credenciales.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Mandatos de desencadenante
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

Crear un desencadenante.

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>El nombre del desencadenante. Este valor es obligatorio. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>Establece el tipo de desencadenante como canal de información. Este distintivo es opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>


   </dl>

<br />**Ejemplo**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

Suprimir un desencadenante.

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**Ejemplo**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

Probar un activador activándolo, en lugar de esperar a que se desencadene automáticamente.

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>El nombre del desencadenante. Este valor es obligatorio. </dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parámetro `VALUES` con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

Obtener metadatos que describen un desencadenante específico.

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>El nombre de un desencadenante. Este valor es obligatorio.</dd>

  <dt>`--summary`</dt>
  <dd>Obtener un resumen de los detalles del desencadenante.</dd>
  </dl>

<br />**Ejemplo**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `ibmcloud fn trigger list`
{: #cli_trigger_list}

Enumerar todos los desencadenantes que ha creado o un número específico de desencadenantes.

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>El nombre de un desencadenante. Este valor es opcional. Si no se especifica, se listan todos los desencadenantes.</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Listar un número especificado de desencadenantes. El valor predeterminado es 30 desencadenantes.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Ordenar la lista de desencadenantes devueltos por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Excluir un número especificado de los desencadenantes creados más recientemente del resultado.</dd>

   </dl>

<br />**Ejemplo**

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

Actualizar un desencadenante.

Cuando actualiza parámetros para un paquete, acción o desencadenante, debe especificar todos los parámetros creados previamente. De lo contrario, los parámetros creados anteriormente se eliminan. Para los paquetes, los servicios enlazados al paquete también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios](/docs/openwhisk?topic=cloud-functions-services) al paquete.
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opciones del mandato**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>El nombre del desencadenante. Este valor es obligatorio. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Las anotaciones se especifican con el formato `KEY` `VALUE`. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Un archivo JSON que contiene anotación con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valores del parámetro con el formato `KEY` `VALUE`. Este distintivo es opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Un archivo JSON que contiene el parámetro `KEYS` y `VALUES`. Este distintivo es opcional.</dd>
   </dl>

<br />**Ejemplo**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






