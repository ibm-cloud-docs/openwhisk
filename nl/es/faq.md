---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: faq, runtimes, actions, memory, monitoring, functions

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
{:faq: data-hd-content-type='faq'}



# Preguntas más frecuentes
{: #faq}

Estas preguntas más frecuentes proporcionan respuestas a preguntas comunes sobre el servicio {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## ¿Qué entornos de ejecución de lenguaje se admiten?
{: #supported-runtimes}

Hay soporte para los lenguajes siguientes:

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


## ¿Cuál es el tiempo máximo que se puede ejecutar mi función?
{: #max-runtime}

El tiempo de espera máximo es de 10 minutos. El valor predeterminado está establecido en 1 minuto, pero puede cambiarlo a través de la CLI especificando un valor nuevo en milisegundos utilizando el distintivo `--timeout`. También puede cambiar el valor a través de la GUI en la sección de detalles de acción.

## ¿Cuál es la memoria máxima que puede utilizar mi función?
{: #max-memory}

Puede utilizar hasta 2048 MB de memoria para cada función. El valor predeterminado está establecido en 256 MB, pero puede cambiarlo utilizando el distintivo `--memory` o a través de la GUI en la sección de detalles de acción.

## ¿Cuál es la diferencia entre una acción y una acción web?
{: #difference}

La diferencia principal entre una acción y una acción web es el objeto de salida de respuesta. En las
[acciones web](/docs/openwhisk?topic=cloud-functions-actions_web), el resultado representa una respuesta HTTP, en la que la salida JSON contiene un campo `body` como mínimo. De forma opcional, también puede contener un `statusCode` y `headers`.

## ¿Cómo puedo ver mis registros de acciones?
{: #logs_faq}

Una vez que se han recopilado las métricas, puede ver los registros utilizando el [servicio {{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-logs).

## ¿Cómo funciona la supervisión?
{: #monitor_faq}

Puede obtener información sobre el rendimiento de las acciones desplegadas con
{{site.data.keyword.openwhisk_short}} utilizando {{site.data.keyword.monitoringlong}}. También puede supervisar el estado y el rendimiento de las acciones utilizando el panel de control para ver un resumen gráfico de la actividad.










