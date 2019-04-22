---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-12"

keywords: faq, runtimes, actions, memory, monitoring

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:faq: data-hd-content-type='faq'}


# Preguntas más frecuentes
{: #faq}

Estas preguntas más frecuentes proporcionan respuestas a preguntas comunes sobre el servicio {{site.data.keyword.openwhisk_short}}.
{: shortdesc}


## ¿Qué entornos de ejecución de lenguaje se admiten?
{: #runtimes}
{: faq}

Hay soporte para los lenguajes siguientes:

<table>
  <tr>
    <th id="language-col">Lenguaje</th>
    <th id="kind-identifier-col">Identificador de tipo</th>
  </tr>
  <tr>
    <td id="language-col-nodejs" headers="language-col">Node.js</td>
    <td headers="kind-identifier-col language-col-nodejs"><code>nodejs:6</code>, <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td id="language-col-python" headers="language-col">Python</td>
    <td headers="kind-identifier-col language-col-python"><code>python:3.7</code>, <code>python:3.6</code></td>
  </tr>
  <tr>
    <td id="language-col-swift" headers="language-col">Swift</td>
    <td headers="kind-identifier-col language-col-swift"><code>swift:4.1</code>, <code>swift:3.1.1</code></td>
  </tr>
  <tr>
    <td id="language-col-php" headers="language-col">PHP</td>
    <td headers="kind-identifier-col language-col-php"><code>php:7.2</code>, <code>php:7.1</code></td>
  </tr>
  <tr>
    <td id="language-col-ruby" headers="language-col">Ruby</td>
    <td headers="kind-identifier-col language-col-ruby"><code>ruby:2.5</code></td>
  </tr>
  <tr>
    <td id="language-col-java" headers="language-col">Java</td>
    <td headers="kind-identifier-col language-col-java"><code>java (JDK 8)</code></td>
  </tr>
  <tr>
    <td headers="language-col" colspan="2">Hay soporte para otros lenguajes utilizando acciones de Docker.</td>
  </tr>
</table>
{: caption="Tabla 1. Entornos de ejecución admitidos" caption-side="top"}


## ¿Cuál es el tiempo máximo que se puede ejecutar mi función?
{: #max-runtime}
{: faq}

El tiempo de espera máximo es de 10 minutos. El valor predeterminado está establecido en 1 minuto, pero puede cambiarlo a través de la CLI especificando un valor nuevo en milisegundos utilizando el distintivo `--timeout`. También puede cambiar el valor a través de la GUI en la sección de detalles de acción.


## ¿Cuál es la memoria máxima que puede utilizar mi función?
{: #max-memory}
{: faq}

Puede utilizar hasta 2048 MB de memoria para cada función. El valor predeterminado está establecido en 256 MB, pero puede cambiarlo utilizando el distintivo `--memory` o a través de la GUI en la sección de detalles de acción.


## ¿Cuál es la diferencia entre una acción y una acción web?
{: #difference}
{: faq}

La diferencia principal entre una acción y una acción web es el objeto de salida de respuesta. En las
[acciones web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#openwhisk_webactions), el resultado representa una respuesta HTTP, en la que la salida JSON debe tener un campo `body` como mínimo. De manera opcional, también puede contener un statusCode (código de estado) y cabeceras.

## ¿Cómo puedo ver mis registros de acciones?
{: #logs}
{: faq}

Una vez que se hayan recopilado las métricas, puede ver los registros utilizando el
[servicio {{site.data.keyword.loganalysislong_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_logs#view-logs).


## ¿Cómo funciona la supervisión?
{: #monitor}
{: faq}

Puede obtener información sobre el rendimiento de las acciones desplegadas con
{{site.data.keyword.openwhisk_short}} utilizando {{site.data.keyword.monitoringlong}}. También puede supervisar el estado y el rendimiento de las acciones utilizando el panel de control para ver un resumen gráfico de la actividad.


