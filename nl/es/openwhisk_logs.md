---

copyright:
  years: 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Visualización de registros de activación en IBM Cloud
{: #openwhisk_logs}

Los registros de activación se pueden ver directamente desde la [página de supervisión de {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Los registros también se reenvían al servicio de [análisis de registros de IBM Cloud](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana), donde se indexan, lo que permite realizar búsquedas de texto completo en todos los mensajes generados y realizar consultas cómodamente en función de campos específicos (como por ejemplo el nivel de registro).
{:shortdesc}

## Consulta de los registros
{: #query-logs}

Cuando se utiliza el servicio de [análisis de registros de IBM Cloud](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) alojado en Kibana, la consulta de registros se puede realizar de forma muy directa. Utilice la sintaxis de consulta de Kibana para encontrar los registros que está buscando.

La IU de {{site.data.keyword.openwhisk_short}} le permite ir directamente a los registros y a los resultados de sus acciones en Kibana. El enlace **Registros** se encuentra en el panel de navegación izquierdo interior de la [página de supervisión de {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/). Cuando accede a la página de detalles de una acción específica, el enlace **Registros** le lleva a los resultados (registros de activación) de dicha acción concreta. El valor predeterminado del intervalo de tiempo para el que se muestran los registros está establecido en 15 minutos. Puede cambiar este valor directamente en Kibana en la esquina superior derecha si desea visualizar registros más antiguos.

A continuación se muestran un par de ejemplos de consultas que resultan útiles para depurar errores.

### Búsqueda de todos los registros de errores:
```
type: user_logs AND stream_str: stderr
```
{: codeblock}

### Búsqueda de todos los registros de errores generados por "myAction":
```
type: user_logs AND stream_str: stderr AND action_str: "*myAction"
```
{: codeblock}

## Consulta de resultados
{: #query-results}

Además de las líneas de registros, el servicio de [análisis de registros de IBM Cloud](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) también indexa los resultados (registros de activación) generados por {{site.data.keyword.openwhisk_short}}. Los resultados contienen metadatos relevantes para activaciones, como por ejemplo su duración y el código de resultado (si se han ejecutado correctamente o ha generado algún error). Todos los campos se pueden consultar, y esto le puede ayudar a entender el comportamiento de las acciones de {{site.data.keyword.openwhisk_short}}.

Utilice la sintaxis de consulta de Kibana para encontrar las activaciones que está buscando. A continuación se muestran un par de ejemplos de consultas que resultan útiles para depurar errores.

### Buscando todas las activaciones fallidas:
```
type: activation_record AND NOT status_str: 0
```
{: codeblock}

Al igual que en los mandatos de Unix, un "`0`" indica una acción que se ha ejecutado correctamente, mientras que todo lo demás se considera un error.

<!--
### Finding all activations that took longer than 30 seconds:

```
type: activation_record AND duration > 30000
```

Duration is in milliseconds.
-->

### Búsqueda de todas las activaciones que han fallado con un error específico:
```
type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
```
{: codeblock}
