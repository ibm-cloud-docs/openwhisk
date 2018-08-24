---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Gestión de acciones
{: #openwhisk_managing}

Gestione acciones supervisando su salida, obteniendo información específica sobre una acción o suprimiendo acciones.
{: shortdec}

## Obtención de acciones
{: #getting-actions}

Una vez que haya creado una acción, puede obtener más información sobre los detalles de la acción. También puede listar las acciones en el espacio de nombres.
{: shortdesc}

Para listar todas las acciones que ha creado:
```
ibmcloud fn action list
```
{: pre}

A medida que cree más acciones, puede resultar útil agrupar las acciones relacionadas en [paquetes](./openwhisk_packages.html). Para filtrar la lista de acciones para limitarla a las contenidas en un paquete concreto:
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

Para obtener los metadatos que describen acciones específicas:

```
ibmcloud fn action get hello
```
{: pre}

Salida de ejemplo:
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

<table>
<caption>Visión general de la salida del mandato <code>action get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de la salida del mandato <code>action get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>El espacio de nombres en el que se encuentra esta acción.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>El nombre de la acción.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>La versión semántica de la acción.</td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code>: El tipo de la acción. Los valores posibles son nodejs:6, nodejs:8, php:7.1, python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox y sequence.</li>
<li><code>code</code>: Código Javascript o Swift a ejecutar cuando el tipo es nodejs o swift.</li>
<li><code>components</code>: Las acciones de la secuencia cuando el tipo es sequence. Las acciones se listan en orden.</li>
<li><code>image</code>: Imagen de contenedor cuando el tipo es blackbox.</li>
<li><code>init</code>: Referencia de archivo zip opcional cuando el tipo es nodejs.</li>
<li><code>binary</code>: Indica si la acción es compilada en un ejecutable binario.</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Anotaciones en esta acción. Para obtener una lista de posibles anotaciones, consulte los temas de referencia [anotaciones de acción](openwhisk_annotations.html#action) y [anotaciones de acción web](openwhisk_annotations.html#annotations-specific-to-web-actions).</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>: El tiempo de espera, en milisegundos, establecido para la acción, después del cual, termina la acción. Predeterminado: 6000</li>
<li><code>memory</code>: El límite máximo de memoria, en MB, establecido para la acción. Predeterminado: 256</li>
<li><code>logs</code>: El límite máximo de tamaño de registro, en MB, establecido para la acción. Predeterminado: 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indica si la acción se publica de forma pública.</td>
</tr>
</tbody></table>

## Visualización de detalles de activación
{: #activation}

Las acciones de {{site.data.keyword.openwhisk_short}} las pueden invocar otros usuarios, en respuesta a varios sucesos, o como parte de una secuencia de acciones. Siempre que se invoca una acción, se crea un registro de activación para dicha invocación. Para obtener información sobre el resultado de la invocación de la acción, puede obtener detalles sobre las activaciones.

Para obtener todos los ID de registros de activación en un espacio de nombres:
```
ibmcloud fn activation list
```
{: pre}

Para obtener detalles sobre un registro de activación específico que se ha producido como resultado de una invocación de acción:
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Salida de ejemplo:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "BobsOrg_dev",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "BobsOrg_dev/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Visión general de la salida del mandato <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de la salida del mandato <code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>El espacio de nombres en el que se encuentra esta activación. Podría ser distinto del espacio de nombres en el que se encuentra la acción.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>El nombre de la acción.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>La versión semántica de la acción.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>La cuenta de usuario que ha activado el elemento.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID de este registro de activación.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>Instante en el que comenzó la activación.</td>
</tr>
<tr>
<td><code>end
</code></td>
<td>Instante en el que se completó la activación.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Tiempo, en milisegundos, que ha tardado la activación en completarse.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: El estado de salida de la activación.</li>
<li><code>statusCode</code>: El código de estado. Si se ha producido un error en la acción, el código de error HTTP.</li>
<li><code>success</code>: Indica si la acción se completó de forma satisfactoria.</li>
<li><code>result</code>: El valor de retorno de la activación.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Registros para esta activación.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Anotaciones en esta acción. Para obtener una lista de las posibles anotaciones de activación, consulte el [tema de referencia de anotaciones](openwhisk_annotations.html#activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Indica si la acción se publica de forma pública.</td>
</tr>
</tbody></table>

## Acceso a metadatos de acción dentro del cuerpo de la acción
{: #accessing-action-metadata-within-the-action-body}

El entorno de acción contiene varias propiedades que son específicas de la acción que se está ejecutando. Estas propiedades permiten que la acción funcione mediante programación con activos de OpenWhisk a través de la API REST o permiten establecer una alarma interna cuando la acción está a punto de alcanzar su presupuesto de tiempo permitido. Las propiedades están accesibles en el entorno del sistema para todos los tiempos de ejecución admitidos: Node.js, Python, Swift, Java y Docker cuando se utiliza el esqueleto Docker de OpenWhisk.

| Propiedad | Descripción |
| -------- | ----------- |
| `__OW_API_HOST` | El host de API para el despliegue de OpenWhisk ejecutando esta acción. |
| `__OW_API_KEY` | La clave de API para quien invoca la acción. Esta clave puede ser una clave de API restringida. |
| `__OW_NAMESPACE` | El espacio de nombres para la activación. Este espacio de nombres podría no ser el mismo que el espacio de nombres para la acción. |
| `__OW_ACTION_NAME` | El nombre completo calificado de la acción en ejecución. |
| `__OW_ACTIVATION_ID` | El ID de activación para esta instancia de acción en ejecución. |
| `__OW_DEADLINE` | El tiempo aproximado, en milisegundos de epoch, en el que esta acción consumirá toda su cuota de duración. |

## Obtención de un URL de acción
{: #get-action-url}

Una acción se puede invocar utilizando la interfaz REST mediante una solicitud HTTPS.
{: shortdesc}

Para obtener un URL de acción:
```
ibmcloud fn action get actionName --url
```
{: pre}

Salida de ejemplo para acciones estándar:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Ejemplo de salida para [acciones web](./openwhisk_webactions.html):
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Nota:** para las acciones estándares, la autenticación se debe proporcionar cuando se invoca mediante una solicitud HTTPS. Para obtener más información sobre las invocaciones de acciones mediante la interfaz REST, vea la [documentación de consulta de API REST](https://console.bluemix.net/apidocs/openwhisk).

## Guardado del código de acción
{: #save-action}

Puede obtener y guardar de forma local código asociado con una acción existente. Puede guardar código de todas las acciones excepto para secuencias y acciones de Docker.
{: shortdesc}

Guarde el código de acción en un nombre de archivo que corresponda con un nombre de acción existente en el directorio de trabajo actual.
```
ibmcloud fn action get actionName --save
```
{: pre}

Se utiliza una extensión de archivo que corresponde al tipo de acción. Para un código de acción que es un archivo zip, se utiliza una extensión .zip. Salida de ejemplo:
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

En su lugar puede proporcionar una vía de acceso de archivos, un nombre de archivo y una extensión personalizadas utilizando el distintivo `--save-as`.
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

Salida de ejemplo:
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## Supervisión de registros de acción
{: #monitor-action-output}

Las acciones de {{site.data.keyword.openwhisk_short}} las pueden invocar otros usuarios, en respuesta a varios sucesos, o como parte de una secuencia de acciones. Para obtener información acerca de cuándo se han invocado acciones y cuál era la salida, puede ser útil supervisar los registros de las acciones.

Puede utilizar la interfaz de línea de mandatos de {{site.data.keyword.openwhisk_short}} para ver el resultado de la salida de las acciones a medida que se invocan.

1. Inicie un bucle de sondeo que compruebe continuamente los registros de las activaciones.
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Conmute a otra ventana para invocar una acción.
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. En la ventana de sondeo, puede ver el registro de activación.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    También podría ver los registros para las acciones que se ejecutan en nombre de {{site.data.keyword.openwhisk_short}} en tiempo real.

## Supresión de acciones
{: #deleting-actions}

Puede realizar una limpieza mediante la supresión de acciones que no quiera usar.

1. Suprima una acción.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: deleted hello
    ```
    {: screen}

2. Compruebe que la acción ya no aparece en la lista de acciones.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    actions
    ```
    {: screen}
