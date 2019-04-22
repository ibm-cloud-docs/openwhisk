---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: logging, monitoring, viewing, logs, query, performance, dashboard, metrics, health

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Actividad de registro y supervisión
{: #openwhisk_logs}

El registro y la supervisión se habilitan automáticamente en {{site.data.keyword.openwhisk}} para ayudarle a resolver problemas y mejorar el estado y el rendimiento de las acciones. También puede utilizar el servicio {{site.data.keyword.cloudaccesstraillong}} para realizar un seguimiento de cómo interactúan los usuarios y aplicaciones con el servicio {{site.data.keyword.openwhisk_short}}.

## Visualización de registros
{: #view-logs}

Puede ver los registros de activación directamente desde el panel de control de Supervisión de {{site.data.keyword.openwhisk_short}}. Los registros también se reenvían a [{{site.data.keyword.loganalysisfull}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) donde se indexan, lo que permite realizar búsquedas de texto completo a través de los mensajes generados y realizar consultas con base a campos específicos.
{:shortdesc}

**Nota**: el registro no está disponible para la región de EE.UU. este.

1. Abra la [página de Supervisión de {{site.data.keyword.openwhisk_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/dashboard).

2. Opcional: Para ver registros sólo para una acción específica, limite el resumen de supervisión a dicha acción. En la sección de Opciones de filtrado, seleccione el nombre de la acción en la lista desplegable **Limitar a**.

3. En la navegación de la izquierda, pulse **Registros**. Se abre la página de Kibana de {{site.data.keyword.loganalysisshort_notm}}.

4. Opcional: Para ver los registros más antiguos, cambie el valor del periodo de tiempo predeterminado de 15 minutos pulsando **Últimos 15 minutos** en la esquina superior derecha y seleccionando un periodo de tiempo distinto.

### Consulta de los registros
{: #query-logs}

Podrá encontrar registros de activación específicos en [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) utilizando la sintaxis de consultas de Kibana.

Las consultas de ejemplo siguientes pueden ayudarle a depurar errores:
  * Buscar todos los registros de error:
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Buscar todos los registros de error que "myAction" ha generado:
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Consulta de resultados
{: #query-results}

Además de registrar líneas, [{{site.data.keyword.loganalysislong_notm}}](/docs/services/CloudLogAnalysis/kibana?topic=cloudloganalysis-analyzing_logs_Kibana#analyzing_logs_Kibana) también indexa los resultados, o los registros de activación, que {{site.data.keyword.openwhisk_short}} genera. Los resultados contienen metadatos de activación como, por ejemplo, la duración de la activación o el código de resultado de activación. Los campos de resultados para la creación de consultas le puede ayudar a entender el comportamiento de las acciones de {{site.data.keyword.openwhisk_short}}.

Podrá encontrar registros de activación específicos en utilizando la sintaxis de consultas de Kibana. Las consultas de ejemplo siguientes pueden ayudarle a depurar errores:

* Buscar todas las activaciones con anomalías:
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    En los resultados, un `0` indica una acción que se ha finalizado correctamente y todos los demás valores indican un error.

* Buscar todas las activaciones con anomalías con un error específico:
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}


## Supervisión del rendimiento de las acciones
{: #monitoring_performance}

Obtenga información sobre el rendimiento de las acciones desplegadas con
{{site.data.keyword.openwhisk}}. Las métricas pueden ayudarle a detectar cuellos de botella o predecir posibles problemas de producción basándose en la duración de la acción, los resultados de las activaciones de la acción o cuándo se alcanzan los límites de activación de la acción.
{: shortdesc}

Se recopilan métricas de manera automática para todas las entidades. En función de si las acciones están en un espacio de nombres basado en IAM o basado en Cloud Foundry, las métricas se encontrarán en la cuenta de IBM Cloud o en el espacio. Estas métricas se envían a
{{site.data.keyword.monitoringlong}} y pasan a estar disponibles a través de Grafana, donde puede configurar los paneles de control, crear alertas basadas en los valores de sucesos de las métricas, etc. Para obtener más información sobre las métricas, consulte la
[documentación de {{site.data.keyword.monitoringlong_notm}}](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started-with-ibm-cloud-monitoring).

### Creación de un panel de control
{: #create_dashboard}

Comience creando un panel de control de supervisión de Grafana.

1. Vaya a uno de los URL siguientes.
  <table>
    <thead>
      <tr>
        <th>Región de {{site.data.keyword.openwhisk_short}}</th>
        <th>Dirección de supervisión</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>UE central</td>
        <td>metrics.eu-de.bluemix.net</td>
      </tr>
      <tr>
        <td>Reino Unido sur</td>
        <td>metrics.eu-gb.bluemix.net</td>
      </tr>
      <tr>
        <td>EE.UU. sur</td>
        <td>metrics.ng.bluemix.net</td>
      </tr>
      <tr>
        <td>EE.UU. este</td>
        <td>No disponible</td>
      </tr>
    </tbody>
  </table>

2. Seleccione el dominio de métricas.
    * Espacios de nombres basados en IAM:
        1. Pulse sobre su nombre de usuario.
        2. En la lista desplegable **Dominio**, seleccione **cuenta**.
        3. En la lista desplegable **Cuenta**, seleccione la cuenta de IBM Cloud donde se encuentra el espacio de nombres basado en IAM.
    * Espacios de nombres basados en Cloud Foundry:
        1. Pulse sobre su nombre de usuario.
        2. En la lista desplegable **Dominio**, seleccione **espacio**.
        3. Utilice las listas desplegables **Organización** y **Espacio** para seleccionar su espacio de nombres basado en Cloud Foundry.

3. Cree un panel de control.
    * Para utilizar el panel de control de {{site.data.keyword.openwhisk_short}} preconstruido:
        1. Navegue a **Inicio > Importar**.
        3. Especifique el ID del panel de control de {{site.data.keyword.openwhisk_short}} preconstruido, `8124`, en el campo **Panel de control de Grafana.net**.
        4. Pulse **Importar**.
    * Para crear un panel de control personalizado, vaya a **Inicio > Crear nuevo**.

Después de que se ejecute una acción, se generan nuevas métricas y se pueden buscar en Grafana. Nota: la acción ejecutada puede tardar hasta 10 minutos en aparecer en Grafana.


### Formato de métrica
{: #metric_format}

Las métricas reflejan datos recopilados de las activaciones de acciones que se agregan cada minuto. Se pueden buscar métricas según el rendimiento de acción o el nivel de simultaneidad de acciones.


**Métricas de rendimiento de acción**

Las métricas de rendimiento de acción son valores que se calculan para una acción individual. Las métricas de rendimiento de acción engloban tanto las características de temporización de las ejecuciones como el estado de las activaciones. Nota: si no especifica el nombre de un paquete durante la creación, se utilizará el nombre de paquete predeterminado. Estas métricas tienen el formato siguiente:

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.<package>.<action>.<metric_name>
```
{: codeblock}

Los caracteres siguientes se convierten en guiones (`-`): punto (.), el signo arroba (@), espacio en blanco ( ), ampersand (&), guión bajo (_), dos puntos (:)
{: tip}

Ejemplo, si tiene una acción denominada `hello-world` en el espacio de nombres basado en Cloud Foundry `user@email.com_dev` en la región `us-south`, una métrica de rendimiento de acción tendría un aspecto similar al siguiente:

```
ibmcloud.public.functions.us-south.action.namespace.user-ibm-com-dev.action-performance.default.hello-world.duration
```
{: screen}

</br>

**Métricas de simultaneidad de acciones**

Las métricas de simultaneidad de acciones se calculan en función de los datos de todas las acciones activas en un espacio de nombres. La simultaneidad de acciones incluye el número de invocaciones simultáneas y el estrangulamiento que podría producirse potencialmente en el sistema al superar los límites de simultaneidad. Estas métricas tienen el formato siguiente:

```
ibmcloud.public.functions.<region>.action.namespace.<namespace>.action-performance.<package>.<action>.<metric_name>
```
{: codeblock}

Ejemplo: si tiene un espacio de nombres basado en IAM denominado `myNamespace` en la región
`us-south`, una métrica de simultaneidad de acciones tendría un aspecto similar al siguiente:

```
ibmcloud.public.functions.us-south.action.namespace.all.concurrent-invocations
```
{: screen}

</br>

**Métricas disponibles**

Debido a que es posible que tenga miles o millones de activaciones de acción, los valores de métricas se representan como una agregación de sucesos generados por muchas activaciones. Los valores se agregan de las maneras siguientes:
* Suma: todos los valores de métrica se añaden conjuntamente.
* Promedio: se calcula una media aritmética.
* Promedio con suma: se calcula una media aritmética basada en componentes y en la adición de componentes distintos.

Consulte la tabla siguiente para ver las métricas que tiene disponibles.

<table>
  <thead>
    <tr>
      <th>Nombre de métrica</th>
      <th>Descripción</th>
      <th>Tipo</th>
      <th>Categoría</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>duration</code></td>
      <td>La duración promedio de la acción, tiempo de ejecución de acción facturado.</td>
      <td>Promedio</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>init-time</code></td>
      <td>El tiempo invertido en inicializar el contenedor de acción.</td>
      <td>Promedio</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>wait-time</code></td>
      <td>El tiempo promedio invertido en una cola esperando a que se planifique una activación.</td>
      <td>Promedio</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>activation</code></td>
      <td>El número global de activaciones que se han desencadenado en el sistema.</td>
      <td>Suma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.success</code></td>
      <td>El número de activaciones correctas del código de acción.</td>
      <td>Suma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.application</code></td>
      <td>El número de activaciones incorrectas provocadas por errores de aplicación. Por ejemplo, errores ordenados de las acciones. Para obtener más información sobre cómo se derivan las métricas action-performance (rendimiento de acción), consulte
[Explicación del registro de activación](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#understanding-the-activation-record).</td>
      <td>Suma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.developer</code></td>
      <td>El número de activaciones incorrectas provocadas por el desarrollador. Por ejemplo, la infracción de la
[interfaz de proxy de acción](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions-new.md#action-interface) por excepciones no gestionadas en el código de acción.</td>
      <td>Suma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>status.error.internal</code></td>
      <td>El número de activaciones incorrectas provocadas por errores internos de {{site.data.keyword.openwhisk_short}}.</td>
      <td>Suma</td>
      <td><code>action-performance</code></td>
    </tr>
    <tr>
      <td><code>concurrent-rate-limit</code></td>
      <td>La suma de activaciones que se han regulado debido a que se ha sobrepasado el límite de la tasa de simultaneidad. No se emite ninguna métrica si no se alcanza el límite.</td>
      <td>Suma</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>timed-rate-limit</code></td>
      <td>La suma de activaciones que se han regulado debido a que se ha sobrepasado el límite por minuto. No se emite ninguna métrica si no se alcanza el límite.</td>
      <td>Suma</td>
      <td><code>action-concurrency</code></td>
    </tr>
    <tr>
      <td><code>concurrent-invocations</code></td>
      <td>El número de invocaciones simultáneas en el sistema.</td>
      <td>Promedio con suma</td>
      <td><code>action-concurrency</code></td>
    </tr>
  </tbody>
</table>

Las métricas de acciones que existen como parte de un espacio de nombres predeterminado están disponibles en la categoría predeterminada.
{: tip}



## Supervisión del estado de las acciones
{: #openwhisk_monitoring}

El [Panel de control de {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/dashboard) proporciona un resumen gráfico de la actividad. Utilice el panel de control para determinar el rendimiento y estado de sus acciones de {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Filtre los registros de acción que quiera ver, y seleccione el marco de tiempo de la actividad registrada. Estos filtros se aplican a todas las vistas del panel de control. Pulse **Recargar** en cualquier momento para actualizar el panel de control con los datos de registro de activación más recientes.

### Resumen de actividades
{: #activity_summary}

La vista de **Resumen de actividad** proporciona un resumen de alto nivel de su entorno {{site.data.keyword.openwhisk_short}}. Utilice esta vista para supervisar el rendimiento y estado general de su servicio habilitado para {{site.data.keyword.openwhisk_short}}. En las métricas de esta vista, puede hacer lo siguiente:
* Determine la tasa de uso de las acciones habilitadas para {{site.data.keyword.openwhisk_short}} de su servicio, visualizando las veces que se han invocado.
* Determine la tasa global de fallos en todas las acciones. Si detecta un error, puede aislar los servicios o acciones que tiene errores, abriendo la vista **Histograma de actividad**. Aísle los errores en sí mismos, viendo el **Registro de actividad**.
* Determine el rendimiento de sus acciones, viendo el tiempo de terminación medio asociado a cada acción.

### Línea temporal de la actividad
{: #timeline}

La vista **Línea temporal de actividad** muestra un gráfico de barras verticales para ver la actividad
de acciones pasadas y presentes. En rojo se indican los errores en acciones específicas. Correlacione esta vista con el **Registro de actividad** para ver más detalles sobre los errores.



### Registro de actividades
{: #log}

Esta vista del **Registro de actividad** muestra una versión con formato del registro de activación. Esta vista muestra los detalles de cada activación y sondea una vez por minuto en busca de nuevas activaciones. Pulse en una acción para mostrar un registro detallado.

Para obtener la salida mostrada del registro de actividad usando la CLI, utilice el mandato siguiente:
```
ibmcloud fn activation poll
```
{: pre}
