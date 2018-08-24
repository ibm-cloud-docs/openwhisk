---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Actividad de registro y supervisión
{: #openwhisk_logs}

El registro y la supervisión se habilitan automáticamente en {{site.data.keyword.openwhisk_short}} para ayudarle a resolver problemas y mejorar el estado y el rendimiento de las acciones.

## Visualización de registros
{: #view-logs}

Puede ver los registros de activación directamente desde el panel de control de Supervisión de {{site.data.keyword.openwhisk_short}}. Los registros también se reenvían a [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) donde se indexan, lo que permite realizar búsquedas de texto completo a través de los mensajes generados y realizar consultas con base a campos específicos.
{:shortdesc}

1. Abra la página de Supervisión de [{{site.data.keyword.openwhisk_short}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/dashboard/).

2. Opcional: Para ver registros sólo para una acción específica, limite el resumen de supervisión a dicha acción. En la sección de Opciones de filtrado, seleccione el nombre de la acción en la lista desplegable **Limitar a**.

3. En la navegación de la izquierda, pulse **Registros**. Se abre la página de Kibana de {{site.data.keyword.loganalysisshort_notm}}.

4. Opcional: Para ver los registros más antiguos, cambie el valor del periodo de tiempo predeterminado de 15 minutos pulsando **Últimos 15 minutos** en la esquina superior derecha y seleccionando un periodo de tiempo distinto.

### Consulta de los registros
{: #query-logs}

Podrá encontrar registros de activación específicos en [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) utilizando la sintaxis de consultas de Kibana.

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

Además de registrar líneas, [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) también indexa los resultados, o los registros de activación, que {{site.data.keyword.openwhisk_short}} genera. Los resultados contienen metadatos de activación como, por ejemplo, la duración de la activación o el código de resultado de activación. Los campos de resultados para la creación de consultas le puede ayudar a entender el comportamiento de las acciones de {{site.data.keyword.openwhisk_short}}.

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

## Supervisión de la actividad
{: #openwhisk_monitoring}

El [Panel de control de {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/dashboard/) proporciona un resumen gráfico de su actividad. Utilice el panel de control para determinar el rendimiento y estado de sus acciones de {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Filtre los registros de acción que quiera ver, y seleccione el marco de tiempo de la actividad registrada. Estos filtros se aplican a todas las vistas del panel de control. Pulse **Recargar** en cualquier momento para actualizar el panel de control con los datos de registro de activación más recientes.

### Resumen de actividades
{: #summary}

La vista de **Resumen de actividad** proporciona un resumen de alto nivel de su entorno {{site.data.keyword.openwhisk_short}}. Utilice esta vista para supervisar el rendimiento y estado general de su servicio habilitado para {{site.data.keyword.openwhisk_short}}. En las métricas de esta vista, puede hacer lo siguiente:
* Determine la tasa de uso de las acciones habilitadas para {{site.data.keyword.openwhisk_short}} de su servicio, visualizando las veces que se han invocado.
* Determine la tasa global de fallos en todas las acciones. Si detecta un error, puede aislar los servicios o acciones que tiene errores, abriendo la vista **Histograma de actividad**. Aísle los errores en sí mismos, viendo el **Registro de actividad**.
* Determine el rendimiento de sus acciones, viendo el tiempo de terminación medio asociado a cada acción.

### Línea temporal de la actividad
{: #timeline}

La vista **Línea temporal de actividad** muestra un gráfico de barras verticales para ver la actividad
de acciones pasadas y presentes. En rojo se indican los errores en acciones específicas. Correlacione esta vista con el **Registro de actividad** para ver más detalles sobre los errores.

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### Registro de actividades
{: #log}

Esta vista del **Registro de actividad** muestra una versión con formato del registro de activación. Esta vista muestra los detalles de cada activación y sondea una vez por minuto en busca de nuevas activaciones. Pulse en una acción para mostrar un registro detallado.

Para obtener la salida mostrada del registro de actividad usando la CLI, utilice el mandato siguiente:
```
ibmcloud fn activation poll
```
{: pre}
