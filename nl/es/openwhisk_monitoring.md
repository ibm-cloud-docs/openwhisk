---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Supervisión de la actividad con el Panel de control
{: #openwhisk_monitoring}

El [Panel de control de {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/) proporciona un resumen gráfico de su actividad. Utilice el panel de control para determinar el rendimiento y estado de sus acciones de {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Pulse **Recargar** en cualquier momento para actualizar el panel de control con los datos de registro de activación más recientes.

## Resumen de actividades
{: #summary}

Esta vista proporciona un resumen de alto nivel de su entorno {{site.data.keyword.openwhisk_short}}. Utilice la vista **Resumen de actividad** para supervisar el estado y rendimiento globales de su servicio habilitado para {{site.data.keyword.openwhisk_short}}. En las métricas de esta vista, puede hacer lo siguiente:
* Determine la tasa de uso de las acciones habilitadas para {{site.data.keyword.openwhisk_short}} de su servicio, visualizando las veces que se han invocado.
* Determine la tasa global de fallos en todas las acciones. Si detecta un error, puede aislar los servicios o acciones que tiene errores,
abriendo la vista **Histograma de actividad**. Aísle los errores en sí mismos, viendo el **Registro de actividad**.
* Determine el rendimiento de sus acciones, viendo el tiempo de terminación medio asociado a cada acción.

<!-- For tips on improving performance, see troubleshooting? -->

## Línea temporal de la actividad
{: #timeline}

La vista **Línea temporal de actividad** muestra un gráfico de barras verticales para ver la actividad
de acciones pasadas y presentes. En rojo se indican los errores en acciones específicas. Correlacione esta vista con el **Registro de actividad** para ver más detalles sobre los errores.

## Histograma de actividad
{: #histogram}

La vista **Histograma de actividad** muestra un gráfico de barras horizontales para ver la actividad
de acciones pasadas y presentes. En rojo se indican los errores en acciones específicas. Correlacione esta vista con el **Registro de actividad** para ver más detalles sobre los errores.

## Registro de actividades
{: #log}

Esta vista muestra una versión con formato del registro de activación. Muestra los detalles de cada activación, pero sondea una vez un minuto en busca de nuevas activaciones. Pulse en una acción para mostrar un registro detallado.

Para obtener la salida mostrada del registro de actividad usando la CLI, utilice el mandato siguiente:
```
ibmcloud fn activation poll
```
{: pre}

## Opciones de filtro
{: #filtering}

Seleccione los registros de acción que quiera ver, y seleccione el marco de tiempo de la actividad registrada.

Estos filtros se aplican a todas las vistas del panel de control.
{: tip}
