---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health, functions

subcollection: cloud-functions

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


# Visualización de registros
{: #logs}

El registro está habilitado automáticamente en {{site.data.keyword.openwhisk}} para ayudarle a resolver problemas. También puede utilizar el servicio {{site.data.keyword.cloudaccesstraillong}} para realizar un seguimiento de cómo interactúan los usuarios y aplicaciones con el servicio {{site.data.keyword.openwhisk_short}}.


## Visualización de los registros de acción a medida que se producen
{: #logs_poll}

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

    **Resultado de ejemplo**
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




## Visualización de detalles de activación
{: #activation_details}

Las acciones de {{site.data.keyword.openwhisk_short}} las pueden invocar otros usuarios, en respuesta a varios sucesos, o como parte de una secuencia de acciones. Cuando se invoca una acción, se crea un registro de activación para dicha invocación. Para obtener información sobre el resultado de la invocación de la acción, puede obtener detalles sobre las activaciones.

Puede obtener todos los ID de registros de activación en un espacio de nombres ejecutando el siguiente mandato.
```
ibmcloud fn activation list
```
{: pre}

Puede obtener detalles sobre un registro de activación específico que se ha producido como resultado de una invocación de acción ejecutando el siguiente mandato. Sustituya `<activation_ID>` por el ID de la activación. 
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

**Resultado de ejemplo**
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
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
            "value": "myNamespace/hello"
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
<td><code>end</code></td>
<td>Instante en el que se completó la activación.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Tiempo, en milisegundos, que ha tardado la activación en completarse.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: El estado de salida de la activación.</li>
<li><code>statusCode</code>: El código de estado. Si la acción ha resultado en un error, este valor es el código de error HTTP.</li>
<li><code>success</code>: El resultado de si la acción se ha completado satisfactoriamente.</li>
<li><code>result</code>: El valor de retorno de la activación.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Registros para esta activación.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Anotaciones en esta acción. Para obtener una lista de las posibles anotaciones de activación, consulte el [tema de referencia de anotaciones](/docs/openwhisk?topic=cloud-functions-annotations#annotations_activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>El resultado de si se ha publicado la acción.</td>
</tr>
</tbody></table>



## Visualización de registros en {{site.data.keyword.loganalysisfull_notm}}
{: #logs_view}

Los registros de {{site.data.keyword.loganalysislong_notm}} no están disponibles para los espacios de nombres basados en IAM.
{: note}

Puede ver los registros de activación directamente desde el panel de control de Supervisión de {{site.data.keyword.openwhisk_short}}. Los registros también se reenvían a [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) donde se indexan, lo que permite realizar búsquedas de texto completo a través de los mensajes generados y realizar consultas con base a campos específicos.
{:shortdesc}

El registro no está disponible para la región de EE.UU. este.
{: important}

1. Abra la [página de Supervisión de {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/dashboard){: external}.

2. Opcional: Para ver registros sólo para una acción específica, limite el resumen de supervisión a dicha acción. En la sección de Opciones de filtrado, seleccione el nombre de la acción en la lista desplegable **Limitar a**.

3. En la navegación de la izquierda, pulse **Registros**. Se abre la página de Kibana de {{site.data.keyword.loganalysisshort_notm}}.

4. Opcional: Para ver los registros más antiguos, cambie el valor del periodo de tiempo predeterminado de 15 minutos pulsando **Últimos 15 minutos** y seleccionando un periodo de tiempo distinto.

### Consulta de los registros
{: #logs_query}

Podrá encontrar registros de activación específicos en [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) utilizando la sintaxis de consultas de Kibana.

Las consultas de ejemplo siguientes pueden ayudarle a depurar errores.
  * Buscar todos los registros de error.
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Buscar todos los registros de error que `myAction` ha generado.
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Consulta de resultados
{: #logs_query_results}

Además de registrar líneas, [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) también indexa los resultados, o los registros de activación, que {{site.data.keyword.openwhisk_short}} genera. Los resultados contienen metadatos de activación como, por ejemplo, la duración de la activación o el código de resultado de activación. Los campos de resultados para la creación de consultas le puede ayudar a entender el comportamiento de las acciones de {{site.data.keyword.openwhisk_short}}.

Podrá encontrar registros de activación específicos en utilizando la sintaxis de consultas de Kibana. Las consultas de ejemplo siguientes pueden ayudarle a depurar errores:

* Buscar todas las activaciones fallidas.
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    En los resultados, un `0` indica una acción que se ha finalizado correctamente. Todos los demás valores indican un error.

* Buscar todas las activaciones que han fallado con un error específico.
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}



