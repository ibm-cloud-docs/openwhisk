---

copyright:
  years: 2017, [{CURRENT_<em>YEAR</em>}]
lastupdated: "2019-05-20"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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



# CLI de {{site.data.keyword.openwhisk_short}}
{: #functions-cli}

Ejecute estos mandatos para gestionar las entidades que componen las funciones.
{: shortdec}



<br />

## Mandatos de acción
{: #cli_action}



### ibmcloud fn action create
{: #cli_action_create}

Crear una acción.

```
ibmcloud fn action create <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>El nombre de la acción. Para incluir la acción en un paquete, especifique el nombre con el formato <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em>. Este valor es obligatorio. </dd>

   <dt><em>APP_FILE</em></dt>
   <dd>La vía de acceso al archivo o paquete de la app a ejecutar como acción. Esta opción es obligatoria.</dd>

   <dt>--concurrency <em>ACTIVATION_LIMIT</em>, -c <em>ACTIVATION_LIMIT</em></dt>
   <dd>El límite (<em>LIMIT</em>) máximo de activación simultánea dentro de un contenedor para la acción. El valor predeterminado es una activación.</dd>

   <dt>--copy</dt>
   <dd>Tratar la acción como el nombre de una acción existente.</dd>

   <dt>--docker <em>DOCKER_HUB_USERNAME</em>/<em>IMAGE_NAME</em></dt>
   <dd>El nombre de usuario de Docker Hub y el nombre de la imagen Docker en Docker Hub para ejecutar la acción. Este distintivo es obligatorio para crear acciones a partir de imágenes Docker.</dd>

   <dt>--kind <em>LANGUAGE</em></dt>
   <dd>El entorno de ejecución de la app. Este distintivo es opcional. Si no se especifica ningún <em>VALUE</em>, se utiliza la versión predeterminada para el entorno de ejecución detectado.
     <em>VALUES</em> posibles para la opción --kind :
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

   <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
   <dd>El tamaño máximo de registro en MB para la acción. El valor predeterminado es 10 MB.</dd>

   <dt>--main <em>ENTRY_METHOD_NAME</em></dt>
   <dd>Si el método de entrada de la acción no es `main`, especifique el nombre personalizado. Este distintivo es obligatorio cuando el método de entrada no es `main`. Para algunos entornos de ejecución, como por ejemplo Java, el nombre debe ser el método completo.</dd>

   <dt>--native</dt>
   <dd>Puede utilizar el argumento `--native` como abreviatura de `--docker openwhisk/dockerskeleton`. Este argumento permite crear y desplegar un ejecutable que se ejecuta dentro del SDK de acción de Docker estándar.
       <ol><li>Cuando se crea una imagen Docker, se crea un ejecutable binario dentro del contenedor ubicado en `/action/exec`. Copie el archivo `/action/exec` al sistema de archivos local y comprímalo en un archivo `exec.zip`.</li>
       <li>Cree una acción de Docker que reciba el ejecutable como datos de inicialización. El argumento `--native` sustituye al argumento `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

   <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
   <dd>Cree una secuencia de acciones e incluya los nombres de las acciones relacionadas separadas por comas.</dd>

   <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
   <dd>El límite (<em>LIMIT</em>) de tiempo de espera en milisegundos. El valor predeterminado es 60000 milisegundos. Cuando se alcanza el tiempo de espera, la acción finaliza.</dd>

   <dt>--web yes|true|raw|no|false</dt>
   <dd>Tratar la acción como una acción web, como una acción web de HTTP sin procesar o como una acción estándar. Especifique <code>yes</code> o <code>true</code> para una acción web, <code>raw</code> para una acción web de HTTP sin procesar, o <code>no</code> o <code>false</code> para una acción estándar. Para proteger la acción web, incluya también la opción `--web-secure`.</dd>

   <dt>--web-secure <em>SECRET</em></dt>
   <dd>Proteja la acción web. El valor (<em>VALUE</em>) de <em>SECRET</em> puede ser <em>true</em>, <em>false</em>, o cualquier serie. Esta opción sólo se puede utilizar con la opción `--web`.</dd>
   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  Salida:
  ```
  ok: created hello
  ```
  {: screen}


<br />

### ibmcloud fn action delete
{: #cli_action_delete}

Puede realizar una limpieza del espacio de nombres suprimiendo acciones que ya no quiera usar.

```
ibmcloud fn action delete <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  Salida:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn action get
{: #cli_action_get}

Obtener metadatos que describen una acción específica.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as <em>FILENAME</em>] [--summary] [--url]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>El nombre de una acción. Este valor es obligatorio.</dd>

   <dt>--save</dt>
   <dd>Puede obtener y guardar de forma local código asociado con una acción existente, excepto para las secuencias y las acciones de Docker. El campo <em>FILENAME</em> corresponde a un nombre de acción existente en el directorio de trabajo actual y la extensión de archivo corresponde al tipo de acción. Por ejemplo, para un código de acción que es un archivo zip, se utiliza una extensión .zip. Este distintivo es opcional.</dd>

  <dt>--save-as <em>FILENAME</em></dt>
  <dd>Guarde el código para las acciones en un archivo con nombre personalizado proporcionando una vía de acceso de archivo, <em>FILENAME</em>, y una extensión. Este distintivo es opcional.</dd>

  <dt>--summary</dt>
  <dd>Obtener un resumen de los detalles de la acción. Los parámetros con el prefijo "*" están enlazados; los parámetros con el prefijo "**" están enlazados y finalizados. Este distintivo es opcional.</dd>

  <dt>--url</dt>
  <dd>Obtener sólo el URL de la acción. Este distintivo es opcional.</dd>
   </dl>

<br /><strong>Ejemplo</strong>:

```
ibmcloud fn action get hello
```
{: pre}

Salida:
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
            "<em>KEY</em>": "exec",
            "<em>VALUE</em>": "nodejs:6"
        }
    ],
    "<em>LIMIT</em>s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />
### ibmcloud fn action invoke
{: #cli_action_invoke}

Ejecutar una acción para probarla.

```
ibmcloud fn action invoke <em>ACTION_NAME</em> [--blocking] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--result]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>El nombre de la acción. Este valor es obligatorio. </dd>

   <dt>--blocking, -b</dt>
   <dd>Las invocaciones de bloqueo utilizan un estilo de solicitud y respuesta para esperar a que el resultado de la activación esté disponible. El período de espera es inferior a 60 segundos o el [valor (<em>VALUE</em>) de límite (<em>LIMIT</em>) de tiempo](/docs/openwhisk?topic=cloud-functions-limits) de la acción. Este distintivo es opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

   <dt>--result, -r</dt>
   <dd>El resultado del código de la app se muestra como la salida del mandato. Si no se especifica esta opción, se muestra el ID de activación. La invocación es con bloqueo cuando se especifica esta opción. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />
### ibmcloud fn action list
{: #cli_action_list}

Enumerar todas las acciones que ha creado o un número específico de acciones.

```
ibmcloud fn action list <em>ACTION_NAME</em> [--limit <em>NUMBER_OF_ACTIONS</em>] [--name-sort] [--skip <em>NUMBER_OF_ACTIONS</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>El nombre de un paquete de acciones. Este valor es opcional. Si no se especifica, se listan todas las acciones.</dd>

   <dt>--limit <em>NUMBER_OF_ACTIONS</em>, -l <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>Listar un número especificado de acciones. El valor predeterminado es 30 acciones.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de acciones devueltas por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>--skip <em>NUMBER_OF_ACTIONS</em>, -s <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>Excluir un número especificado de las acciones creadas más recientemente del resultado.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />
### ibmcloud fn action update
{: #cli_action_update}

Actualizar una acción o la app de dentro de una acción.

```
ibmcloud fn action update <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> VALE. Este distintivo es opcional.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>El nombre de la acción. Para incluir la acción en un paquete, especifique el nombre con el formato <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em>. Este valor es obligatorio. </dd>

  <dt><em>APP_FILE</em></dt>
  <dd>La vía de acceso al archivo o paquete de la app a ejecutar como acción. Esta opción es obligatoria cuando se desea actualizar la app dentro de la acción.</dd>

  <dt>--concurrency <em>ACTIVATION_LIMIT</em>, -c <em>ACTIVATION_LIMIT</em></dt>
  <dd>El límite máximo de activación simultánea dentro de un contenedor para la acción. El valor predeterminado es una activación.</dd>

  <dt>--copy</dt>
  <dd>Tratar la acción como el nombre de una acción existente.</dd>

  <dt>--docker DOCKER_HUB_USERNAME/IMAGE_NAME</dt>
  <dd>El nombre de usuario de Docker Hub y el nombre de la imagen Docker en Docker Hub para ejecutar la acción. Este distintivo es obligatorio para crear acciones a partir de imágenes Docker.</dd>

  <dt>--kind LANGUAGE</dt>
  <dd>El entorno de ejecución de la app. Este distintivo es opcional. Si no se especifica ningún <em>VALUE</em>, se utiliza la versión predeterminada para el entorno de ejecución detectado.
    Valores posibles para la opción --kind:
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

  <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
  <dd>El tamaño máximo de registro en MB para la acción. El valor predeterminado es 10 MB.</dd>

  <dt>--main ENTRY_METHOD_NAME</dt>
  <dd>Si el método de entrada de la acción no es `main`, especifique el nombre personalizado. Este distintivo es obligatorio cuando el método de entrada no es `main`. Para algunos entornos de ejecución, como por ejemplo Java, el nombre debe ser el método completo.</dd>

  <dt>--native</dt>
  <dd>Puede utilizar el argumento `--native` como abreviatura de `--docker openwhisk/dockerskeleton`. Este argumento permite crear y desplegar un ejecutable que se ejecuta dentro del SDK de acción de Docker estándar.
      <ol><li>Cuando se crea una imagen Docker, se crea un ejecutable binario dentro del contenedor ubicado en `/action/exec`. Copie el archivo `/action/exec` al sistema de archivos local y comprímalo en un archivo `exec.zip`.</li>
      <li>Cree una acción de Docker que reciba el ejecutable como datos de inicialización. El argumento `--native` sustituye al argumento `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

  <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
  <dd>Crear una secuencia de acciones especificando el nombre de acciones relacionadas.</dd>

  <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
  <dd>El límite de tiempo de espera en milisegundos. El valor predeterminado es 60000 milisegundos. Cuando se alcanza el tiempo de espera, la acción finaliza.</dd>

  <dt>--web yes|true|raw|no|false</dt>
  <dd>Tratar la acción como una acción web, como una acción web de HTTP sin procesar o como una acción estándar. Especifique <code>yes</code> o <code>true</code> para una acción web, <code>raw</code> para una acción web de HTTP sin procesar, o <code>no</code> o <code>false</code> para una acción estándar. Para proteger la acción web, incluya también la opción `--web-secure`.</dd>

  <dt>--web-secure <em>SECRET</em></dt>
  <dd>Proteja la acción web. El valor (<em>VALUE</em>) de <em>SECRET</em> puede ser <em>true</em>, <em>false</em>, o cualquier serie. Esta opción sólo se puede utilizar con la opción `--web`.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Mandatos de activación
{: #cli_activation}


### ibmcloud fn activation get
{: #cli_activation_get}

Obtener metadatos que describen una activación específica.

```
ibmcloud fn activation get [<em>ACTIVATION_ID</em>] [<em>FIELD_FILTER</em>] [--last] [--summary]
```
{: pre}


<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>El ID de una activación específica. Utilice `ibmcloud fn activation list` para recuperar una lista de ID disponibles. Este valor es obligatorio, a menos que se especifique la opción `--last` o `-l`.</dd>

  <dt><em>FIELD_FILTER</em></dt>
  <dd>El campo de los metadatos del que mostrar información. Por ejemplo, para mostrar el campo registros (logs), ejecute `ibmcloud fn activation get ACTIVATION_ID logs`. Este valor es opcional.</dd>

  <dt>--last, -l</dt>
  <dd>Mostrar los metadatos de la activación más reciente. Este distintivo es opcional.</dd>

  <dt>--summary, -s</dt>
  <dd>Mostrar la respuesta resultado sólo de los detalles de activación. Este distintivo es opcional.</dd>
  </dl>


<br /><strong>Ejemplo</strong>:
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation list
{: #cli_activation_list}

Listar todos los ID de activación de todas las acciones de un paquete.

```
ibmcloud fn activation list [--full] [--limit <em>NUMBER_OF_ACTIVATIONS</em>] [--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>] [--skip <em>NUMBER_OF_ACTIVATIONS</em>] [--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>]
```
{: pre}


<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt>--full, -f</dt>
  <dd>Mostrar la descripción de activación completa</dd>

  <dt>--limit <em>NUMBER_OF_ACTIVATIONS</em>, -l <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>Listar un número especificado de activaciones. El valor predeterminado es 30 activaciones y el máximo es 200 activaciones.</dd>

  <dt>--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>Listar activaciones que se han creado desde la fecha especificada. Se mide en milisegundos. Ejemplo: Jue, 01, ene 1970</dd>

  <dt>--skip <em>NUMBER_OF_ACTIVATIONS</em>, -s <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>Excluir un número especificado de las activaciones más recientes del resultado.</dd>

  <dt>--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>Listar activaciones que se han creado antes de la fecha especificada. Se mide en milisegundos. Ejemplo: Jue, 01, ene 1970</dd>
  </dl>

<br /><strong>Ejemplo</strong>:
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


<br />
### ibmcloud fn activation logs
{: #cli_activation_logs}

Obtener los registros de una activación específica.

```
ibmcloud fn activation logs [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>El ID de una activación específica. Utilice `ibmcloud fn activation list` para recuperar una lista de ID disponibles. Este valor es obligatorio, a menos que se especifique la opción `--last` o `-l`.</dd>

  <dt>--last, -l</dt>
  <dd>Mostrar los registros de la activación más reciente. Este distintivo es opcional.</dd>

  <dt>--strip, -r</dt>
  <dd>Mostrar sólo el mensaje de registro; excluir la indicación de fecha y hora y la información de secuencia. Este distintivo es opcional.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation poll
{: #cli_activation_poll}

Ver una lista activa en modalidad continua de activaciones para una acción o un espacio de nombres. Puede pulsar CTRL+C para salir del sondeo.

```
ibmcloud fn activation poll [/<em>NAMESPACE</em>] [<em>ACTION_NAME</em>] [--exit <em>SECONDS</em>] [--since-days <em>DAYS</em>] [-since-hours <em>HOURS</em>] [--since-minutes <em>MINUTES</em>] [--since-seconds <em>SECONDS</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt>/<em>NAMESPACE</em></dt>
  <dd>Un espacio de nombres que empiece por /. Sondear activaciones para un espacio de nombres, una acción o un espacio. Este valor es opcional. Si no se especifica un espacio de nombres o una acción, se sondea el espacio.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>Sondear activaciones para un espacio de nombres, una acción o un espacio. Este valor es opcional. Si no se especifica un espacio de nombres o una acción, se sondea el espacio.</dd>

  <dt>--exit <em>SECONDS</em>, -e <em>SECONDS</em></dt>
  <dd>Sondear activaciones durante un número especificado de segundos y luego salir. Este distintivo es opcional.</dd>

  <dt>--since-days <em>DAYS</em></dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de días atrás. Este distintivo es opcional.</dd>

  <dt>--since-hours <em>HOURS</em></dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de horas atrás. Este distintivo es opcional.</dd>

  <dt>--since-minutes <em>MINUTES</em></dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de minutos atrás. Este distintivo es opcional.</dd>

  <dt>--since-seconds <em>SECONDS</em></dt>
  <dd>Iniciar el sondeo de activaciones de un número especificado de segundos atrás. Este distintivo es opcional.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn activation poll
```
{: pre}


<br />
### ibmcloud fn activation result
{: #cli_activation_result}

Obtener el resultado de una activación específica.

```
ibmcloud fn activation result [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>El ID de una activación específica. Utilice `ibmcloud fn activation list` para recuperar una lista de ID disponibles. Este valor es obligatorio, a menos que se especifique la opción `--last` o `-l`.</dd>

  <dt>--last, -l</dt>
  <dd>Mostrar el resultado de la activación más reciente. Este distintivo es opcional.</dd>

  </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## Mandatos de API
{: #cli_api}


### ibmcloud fn api create
{: #cli_api_create}

Crear una API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB <em>ACTION_NAME</em>] [--apiname API_NAME] [--config-file <em>FILE</em>] [--response-type TYPE]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>API_NAME</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>API_VERB</dt>
   <dd>El verbo de la API, como por ejemplo `get` o `post`.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>El nombre de la acción.</dd>

   <dt><em>--apiname API_NAME, -n API_NAME</em></dt>
   <dd>El nombre de la API. Este distintivo se ignora cuando se especifica un archivo de configuración. El nombre predeterminado es la vía de acceso base (BASE_PATH). Este distintivo es opcional.</dd>

   <dt>--config-file <em>FILE</em>, -c <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene la configuración de la API de Swagger. Cuando se utiliza este distintivo, se ignora el distintivo de nombre de API. Este distintivo es obligatorio.</dd>

   <dt>--response-type TYPE</dt>
   <dd>Establezca el tipo de respuesta de acción web como `html`, `http`, `json`, `text` o `svg`. El valor predeterminado es `json`. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Salida:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />
### ibmcloud fn api delete
{: #cli_api_delete}

Suprimir una API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>API_NAME</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>API_PATH</dt>
   <dd>La vía de acceso a la API</dd>

   <dt>API_VERB</dt>
   <dd>El verbo de la API, como por ejemplo `get` o `post`.</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>Especifique el tipo de salida de API como `json` o `yaml`. El valor predeterminado es `json`.</dd>

   <dt>--full, -f</dt>
   <dd>Mostrar todos los detalles de configuración de la API.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />
### ibmcloud fn api get
{: #cli_api_get}

Obtener los metadatos para una API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>API_NAME</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>Especifique el tipo de salida de API como `json` o `yaml`. El valor predeterminado es `json`.</dd>

   <dt>--full, -f</dt>
   <dd>Mostrar todos los detalles de configuración de la API.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />
### ibmcloud fn api list
{: #cli_api_list}

Enumerar todas las API que ha creado o un número específico de API. Si no se especifica ningún nombre o vía de acceso base, se listan todas las API.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>La vía de acceso base de la API.</dd>

   <dt>API_NAME</dt>
   <dd>El nombre de la API. El nombre de la API puede ser igual que la vía de acceso base.</dd>

   <dt>API_PATH</dt>
   <dd>La vía de acceso a la API</dd>

   <dt>API_VERB</dt>
   <dd>El verbo de la API, como por ejemplo `get` o `post`.</dd>

   <dt>--full, -f</dt>
   <dd>Mostrar todos los detalles de la API. Este distintivo es opcional. </dd>

   <dt>--limit NUMBER_OF_APIS, -l NUMBER_OF_APIS</dt>
   <dd>Listar un número especificado de API. El valor predeterminado es 30 API. Este distintivo es opcional. </dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de las API devueltas por nombre, si no, la lista se ordena por fecha de creación. Este distintivo es opcional. </dd>

   <dt>--skip NUMBER_OF_APIS, -s NUMBER_OF_APIS</dt>
   <dd>Excluir un número especificado de las API creadas más recientemente del resultado. Este distintivo es opcional. </dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn api list
  ```
  {: pre}




<br /><br />
## Mandatos de despliegue
{: #cli_deploy_cmds}


### ibmcloud fn deploy
{: #cli_deploy}

Utilizar un archivo de manifiesto para desplegar una colección de paquetes, acciones, desencadenantes y reglas.

```
ibmcloud fn deploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>--apihost HOST</dt>
   <dd>El host de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>El archivo de configuración. El valor predeterminado es <code>$HOME/.wskprops</code>.</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>La vía de acceso al archivo de despliegue.</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>La vía de acceso al archivo de manifiesto. Este distintivo es obligatorio si manifest.yaml no se encuentra en el directorio actual.</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>El nombre o el ID de un espacio de nombres.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

   <dt>--preview </dt>
   <dd>Mostrar el plan de despliegue antes de desplegar.</dd>

   <dt>--project PATH</dt>
   <dd>La vía de acceso al proyecto sin servidor. El valor predeterminado es <code>.</code> (directorio actual).</dd>

   <dt>--strict</dt>
   <dd>Permitir una versión de entorno de ejecución definida por el usuario.</dd>

   <dt>--verbose, -v</dt>
   <dd>Ver salida detallada.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />
### ibmcloud fn undeploy
{: #cli_undeploy}

Utilizar un archivo de manifiesto para anular el despliegue de una colección de paquetes, acciones, desencadenantes y reglas.

```
ibmcloud fn undeploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE<em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt>--apihost HOST</dt>
   <dd>El host de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>El archivo de configuración. El valor predeterminado es <code>$HOME/.wskprops</code>.</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>La vía de acceso al archivo de despliegue.</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>La vía de acceso al archivo de manifiesto. Este distintivo es obligatorio si manifest.yaml no se encuentra en el directorio actual.</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>El nombre o el ID de un espacio de nombres.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

   <dt>--preview </dt>
   <dd>Mostrar el plan de anulación de despliegue antes de desplegar.</dd>

   <dt>--project PATH</dt>
   <dd>La vía de acceso al proyecto sin servidor. El valor predeterminado es <code>.</code> (directorio actual).</dd>

   <dt>--strict</dt>
   <dd>Permitir una versión de entorno de ejecución definida por el usuario.</dd>

   <dt>--verbose, -v</dt>
   <dd>Ver salida detallada.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## Mandato List
{: #cli_list_cmd}


### ibmcloud fn list
{: #cli_list}

Ver una lista agrupada de los paquetes, acciones, desencadenantes y reglas en el espacio de nombres.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt>--name-sort, -n</dt>
   <dd>Ordenar cada grupo de entidades devueltas por nombre, si no, cada grupo se ordena por fecha de creación.</dd>
   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Mandatos de espacio de nombres
{: #cli_namespace}


### ibmcloud fn namespace create
{: #cli_namespace_create}

Crear un espacio de nombres IAM.

```
ibmcloud fn namespace create <em>NAMESPACE</em> [--description DESCRIPTION] 
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>El nombre de un espacio de nombres. No incluya guiones (-) en el nombre. Este valor es obligatorio.</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>Escriba su propia descripción exclusiva para ayudarle a identificar el espacio de nombres. Si su descripción tiene más de una palabra, incluya comillas (") alrededor de la descripción. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />
### ibmcloud fn namespace delete
{: #cli_namespace_delete}

Suprimir un espacio de nombres IAM.

```
ibmcloud fn namespace delete <em>NAMESPACE</em>
```
{: pre}


<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />
### ibmcloud fn namespace get
{: #cli_namespace_get}

Obtener las entidades o la información de metadatos de un espacio de nombres Cloud Foundry o IAM.

```
ibmcloud fn namespace list <em>NAMESPACE</em> [--auth <em>KEY</em>] [--name-sort] [--properties] 
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>El nombre o el ID de un espacio de nombres. Este valor es obligatorio.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de espacios de nombres devueltos por nombre, si no, la lista se ordena por fecha de creación. Este distintivo es opcional. </dd>

   <dt>--properties</dt>
   <dd>Mostrar las propiedades del espacio de nombres en lugar de las entidades que contiene. Este distintivo es opcional. </dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  Salida:
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />
### ibmcloud fn namespace list
{: #cli_namespace_list}

Listar los espacios de nombres Cloud Foundry e IAM disponibles.

```
ibmcloud fn namespace list [--auth <em>KEY</em>] [--cf] [--iam] [--limit NUMBER_OF_<em>NAMESPACE</em>S] [--name-sort] [--skip NUMBER_OF_<em>NAMESPACE</em>S] 
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--cf</dt>
   <dd>Mostrar sólo los espacios de nombres Cloud Foundry. Los espacios de nombres IAM no se muestran. Este distintivo es opcional.</dd>

   <dt>--iam</dt>
   <dd>Mostrar sólo los espacios de nombres IAM. Los espacios de nombres Cloud Foundry no se muestran. Este distintivo es opcional.</dd>

   <dt>--limit NUMBER_OF_<em>NAMESPACE</em>S, -l NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>Listar un número especificado de espacios de nombres. El valor predeterminado es 30 espacios de nombres. Este distintivo es opcional. </dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de espacios de nombres devueltos por nombre, si no, la lista se ordena por fecha de creación. Este distintivo es opcional. </dd>

   <dt>--skip NUMBER_OF_<em>NAMESPACE</em>S, -s NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>Excluir un número especificado de los espacios de nombres creados más recientemente del resultado. Este distintivo es opcional. </dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />
### ibmcloud fn namespace update
{: #cli_namespace_update}

Cambiar el nombre o la descripción de un espacio de nombres IAM.

```
ibmcloud fn namespace update <em>NAMESPACE</em> [NEW_<em>NAMESPACE</em>_NAME] [--description DESCRIPTION] 
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>El nombre de un espacio de nombres. No incluya guiones (-) en el nombre. Este valor es obligatorio.</dd>

   <dt>NEW_<em>NAMESPACE</em>_NAME</dt>
   <dd>El nuevo nombre de un espacio de nombres. No incluya guiones (-) en el nombre. Este valor es opcional.</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>Escriba su propia descripción exclusiva para ayudarle a identificar el espacio de nombres. Si su descripción tiene más de una palabra, incluya comillas (") alrededor de la descripción. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Mandatos de paquetes
{: #cli_pkg}


### ibmcloud fn package bind
{: #cli_pkg_bind}

Enlazar parámetros en un paquete. Todas las acciones dentro del paquete heredarán esos parámetros a menos que se especifique lo contrario.

```
ibmcloud fn package bind <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>El nombre del paquete. Este valor es obligatorio. </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> VALE. Este distintivo es opcional.</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />
### ibmcloud fn package create
{: #cli_pkg_create}

Crear un paquete diseñado para contener una o más acciones. Para añadir una acción en el paquete, incluya el nombre del paquete con el nombre de la acción cuando cree o actualice la acción.

```
ibmcloud fn package create <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>El nombre del paquete. Este valor es obligatorio. </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> VALE. Este distintivo es opcional.</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y C. Este distintivo es opcional.</dd>

  <dt>--shared yes|no</dt>
  <dd>Cuando se especifica sin valor o con el valor yes, el paquete se comparte con otros usuarios.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Salida:
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />
### ibmcloud fn package delete
{: #cli_pkg_delete}

Puede realizar una limpieza del espacio de nombres suprimiendo paquetes que ya no quiera usar.

```
ibmcloud fn package delete <em>PACKAGE_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  Salida:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn package get
{: #cli_pkg_get}

Obtener metadatos que describen un paquete específico.

```
ibmcloud fn package get <em>PACKAGE_NAME</em> [--summary]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

  <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>El nombre de un paquete. Este valor es obligatorio.</dd>

   <dt>--summary</dt>
   <dd>Obtener un resumen de los detalles del paquete. Los parámetros con el prefijo "*" están enlazados. Este distintivo es opcional.</dd>
   </dl>

<br /><strong>Ejemplo</strong>:

```
ibmcloud fn package get hello
```
{: pre}


<br />
### ibmcloud fn package list
{: #cli_pkg_list}

Enumerar todos los paquetes que ha creado o un número específico de paquetes.

```
ibmcloud fn package list [<em>NAMESPACE</em>] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>NAMESPACE</em></dt>
   <dd>Listar los paquetes de un espacio de nombres específico. Este valor es opcional. Si no se especifica, se listan todos los paquetes.</dd>

   <dt>--limit NUMBER_OF_PACKAGES, -l NUMBER_OF_PACKAGES</dt>
   <dd>Listar un número especificado de paquetes. El valor predeterminado es 30 paquetes.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de paquetes devueltos por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>--skip NUMBER_OF_PACKAGES, -s NUMBER_OF_PACKAGES</dt>
   <dd>Excluir un número especificado de los paquetes creados más recientemente del resultado.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Ejecute `ibmcloud fn package list /whisk.system` para ver una lista de los paquetes preinstalados.
  {: tip}


<br />
### ibmcloud fn package refresh
{: #cli_pkg_refresh}

Renovar los enlaces de paquete para todos los paquetes dentro de un espacio de nombres específico.

```
ibmcloud fn package refresh /<em>NAMESPACE</em>
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>/<em>NAMESPACE</em></dt>
   <dd>Un espacio de nombres que empiece por /. Este distintivo es obligatorio. Ejecute <code>ibmcloud fn namespace list</code> para obtener una lista de espacios de nombres entre los que elegir.</dd>
   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />
### ibmcloud fn package update
{: #cli_pkg_update}

Actualizar un paquete diseñado para que contenga una o más acciones. Para añadir una acción en el paquete, incluya el nombre del paquete con el nombre de la acción cuando cree o actualice la acción.

```
ibmcloud fn package update <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt><em>PACKAGE_NAME</em></dt>
   <dd>El nombre del paquete. Este valor es obligatorio. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> VALE. Este distintivo es opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

   <dt>--shared yes|no</dt>
   <dd>Cuando se especifica sin valor o con el valor <code>yes</code>, el paquete se comparte con otros usuarios.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Salida:
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## Mandatos de propiedades
{: #cli_prop}

Establezca las propiedades globales para el entorno de CLI o visualice las propiedades sobre la CLI de <code>wsk</code>, que se ejecuta como parte de la CLI de `ibmcloud fn`.

### ibmcloud fn property get
{: #cli_prop_get}

Ver los detalles de los metadatos de una propiedad desde la CLI de <code>wsk</code>.

```
ibmcloud fn property get [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt>--all</dt>
   <dd>Ver todas las propiedades de la CLI de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>---apibuild</dt>
   <dd>La información de compilación de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--apibuildno</dt>
   <dd>El número de compilación de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--apihost <em>HOST</em></dt>
   <dd>El host de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>La versión de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>El certificado de cliente de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--cliversion</dt>
   <dd>La versión de la CLI de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>El cliente de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>Un espacio de nombres de IAM. Este distintivo no se puede establecer para los espacios de nombres de Cloud Foundry. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn property get --cliversion
  ```
  {: pre}


<br />
### ibmcloud fn property set
{: #cli_prop_set}

Establecer una propiedad. Se requiere al menos un distintivo.

```
ibmcloud fn property set [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd>El host de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>La versión de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>El certificado de cliente de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>El cliente de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>Un espacio de nombres de IAM. Este distintivo no se puede establecer para los espacios de nombres de Cloud Foundry. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  Salida:
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### ibmcloud fn property unset
{: #cli_prop_unset}

Desestablecer una propiedad para la CLI de <code>wsk</code>. Se requiere al menos un distintivo.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd>El host de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>La versión de la API de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd>La autorización de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>El certificado de cliente de <code>wsk</code>. Este distintivo es opcional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>El cliente de <code>wsk</code> <em>KEY</em>. Este distintivo es opcional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>Un espacio de nombres de IAM. Este distintivo no se puede establecer para los espacios de nombres de Cloud Foundry. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn property unset --key my<em>KEY</em>
  ```
  {: pre}



<br /><br />
## Mandatos de regla
{: #cli_rule}


### ibmcloud fn rule create
{: #cli_rule_create}

Crear una regla para asociar un desencadenante con una acción. Antes de poder crear una regla, debe crear primero un desencadenante y una acción.

```
ibmcloud fn rule create <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  Salida:
  ```
  ok: created myrule
  ```
  {: screen}


<br />
### ibmcloud fn rule delete
{: #cli_rule_delete}

Para limpiar el espacio de nombres, elimine las reglas que ya no necesita.

```
ibmcloud fn rule delete <em>RULE_NAME</em> [--disable]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>El nombre de una regla. Este valor es obligatorio.</dd>

  <dt>--disable</dt>
  <dd>Inhabilite la regla antes de suprimirla.</dd>
  </dl>


<br /><strong>Ejemplo</strong>:

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />
### ibmcloud fn rule disable
{: #cli_rule_disable}

Cambie el estado de una regla a inactiva y detenga la ejecución de una acción cuando se active un desencadenante.

```
ibmcloud fn rule disable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule enable
{: #cli_rule_enable}

Cambiar el estado de una regla de inactivo a activo. Cuando está activo, se ejecuta una acción cuando se activa un desencadenante.

```
ibmcloud fn rule enable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule get
{: #cli_rule_get}

Obtener metadatos que describen una regla específica.

```
ibmcloud fn rule get <em>RULE_NAME</em> [--summary]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>El nombre de una regla. Este valor es obligatorio.</dd>

  <dt>--summary</dt>
  <dd>Obtener un resumen de los detalles de la regla.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:

```
ibmcloud fn rule get myrule
```
{: pre}


<br />
### ibmcloud fn rule list
{: #cli_rule_list}

Enumerar todas las reglas que ha creado o un número específico de reglas.

```
ibmcloud fn rule list <em>RULE_NAME</em> [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>El nombre de una regla. Este valor es opcional. Si no se especifica, se listan todas las reglas.</dd>

   <dt>--limit NUMBER_OF_RULES, -l NUMBER_OF_RULES</dt>
   <dd>Listar un número especificado de reglas. El valor predeterminado es 30 reglas.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de reglas devueltas por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>--skip NUMBER_OF_RULES, -s NUMBER_OF_RULES</dt>
   <dd>Excluir un número especificado de las reglas creadas más recientemente del resultado.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />
### ibmcloud fn rule status
{: #cli_rule_status}

Ver si una regla está activa o inactiva. Ejecute los mandatos `ibmcloud fn rule disable` o `ibmcloud fn run enable` para cambiar el estado.

```
ibmcloud fn rule status <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />
### ibmcloud fn rule update
{: #cli_rule_update}

Para cambiar los desencadenantes asociados a las reglas, puede actualizar una regla.

```
ibmcloud fn rule update <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## Mandato SDK
{: #cli_sdk}


### ibmcloud fn sdk install
{: #cli_sdk_install}

Instalar un SDK.

```
ibmcloud fn sdk install <em>COMPONENT</em> [--limit <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>COMPONENT</em></dt>
   <dd>El componente SDK, como por ejemplo `docker`, `iOS` y `bashauto`. Este valor es obligatorio.</dd>

   <dt>--stdout, --s</dt>
   <dd>Imprimir los resultados del mandato bash a stdout. Este distintivo es opcional.</dd>


   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Mandatos de servicio
{: #cli_service}


### ibmcloud fn service bind
{: #cli_service_bind}

Enlazar un servicio a una acción o paquete.

```
ibmcloud fn service bind SERVICE PACKAGE_or_<em>ACTION_NAME</em> [--instance SERVICE_INSTANCE] [--keyname SERVICE_<em>KEY</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>SERVICE</dt>
   <dd>El nombre del servicio.</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>El nombre del paquete o la acción al que se deben enlazar las credenciales.</dd>

   <dt>--instance SERVICE_INSTANCE</dt>
   <dd>El nombre de la instancia de servicio.</dd>

   <dt>--keyname SERVICE_<em>KEY</em></dt>
   <dd>El nombre de las credenciales de servicio <em>KEY</em> a enlazar.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />
### ibmcloud fn service unbind
{: #cli_service_unbind}

Desenlazar las credenciales de servicio de una acción o paquete.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_<em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt>SERVICE</dt>
   <dd>El nombre del servicio.</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>El nombre del paquete o la acción al que se deben enlazar las credenciales.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Mandatos de desencadenante
{: #cli_trigger}


### ibmcloud fn trigger create
{: #cli_trigger_create}

Crear un desencadenante.

```
ibmcloud fn trigger create <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--feed <em>ACTION_NAME</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>El nombre del desencadenante. Este valor es obligatorio. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> VALE. Este distintivo es opcional.</dd>

   <dt>--feed <em>ACTION_NAME</em>, -f <em>ACTION_NAME</em></dt>
   <dd>Establece el tipo de desencadenante como canal de información. Este distintivo es opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>


   </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />
### ibmcloud fn trigger delete
{: #cli_trigger_delete}

Suprimir un desencadenante.

```
ibmcloud fn trigger delete <em>TRIGGER_NAME</em>
```
{: pre}

<br /><strong>Ejemplo</strong>:

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />
### ibmcloud fn trigger fire
{: #cli_trigger_fire}

Probar un activador activándolo, en lugar de esperar a que se desencadene automáticamente.

```
ibmcloud fn trigger fire <em>TRIGGER_NAME</em> [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>

   <dt><em>TRIGGER_NAME</em></dt>
   <dd>El nombre del desencadenante. Este valor es obligatorio. </dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parámetro <em>VALUES</em> con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />
### ibmcloud fn trigger get
{: #cli_trigger_get}

Obtener metadatos que describen un desencadenante específico.

```
ibmcloud fn trigger get <em>TRIGGER_NAME</em> [--summary]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>El nombre de un desencadenante. Este valor es obligatorio.</dd>

  <dt>--summary</dt>
  <dd>Obtener un resumen de los detalles del desencadenante.</dd>
  </dl>

<br /><strong>Ejemplo</strong>:

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### ibmcloud fn trigger list
{: #cli_trigger_list}

Enumerar todos los desencadenantes que ha creado o un número específico de desencadenantes.

```
ibmcloud fn trigger list <em>TRIGGER_NAME</em> [--limit <em>NUMBER_OF_TRIGGERS</em>] [--name-sort] [--skip <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>El nombre de un desencadenante. Este valor es opcional. Si no se especifica, se listan todos los desencadenantes.</dd>

   <dt>--limit <em>NUMBER_OF_TRIGGERS</em>, -l <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>Listar un número especificado de desencadenantes. El valor predeterminado es 30 desencadenantes.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Ordenar la lista de desencadenantes devueltos por nombre, si no, la lista se ordena por fecha de creación.</dd>

   <dt>--skip <em>NUMBER_OF_TRIGGERS</em>, -s <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>Excluir un número especificado de los desencadenantes creados más recientemente del resultado.</dd>

   </dl>

<br /><strong>Ejemplo</strong>:

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### ibmcloud fn trigger update
{: #cli_trigger_update}

Actualizar un desencadenante.

```
ibmcloud fn trigger update <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opciones del mandato</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>El nombre del desencadenante. Este valor es obligatorio. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>Las anotaciones se especifican con el formato <em>KEY</em> <em>VALUE</em>. Para incluir más de una anotación, especifique esta opción para cada anotación. Este distintivo es opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene anotación con el formato <em>KEY</em> VALE. Este distintivo es opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Valores del parámetro con el formato <em>KEY</em> <em>VALUE</em>. Este distintivo es opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Un archivo JSON que contiene el parámetro <em>KEYS</em> y <em>VALUES</em>. Este distintivo es opcional.</dd>
   </dl>

<br /><strong>Ejemplo</strong>:
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}



