---

copyright:

  years: 2018

lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Despliegue de plantillas en {{site.data.keyword.openwhisk_short}}
{: #serviceauth}

{{site.data.keyword.openwhisk}} ofrece un catálogo de plantillas que le ayudarán a empezar a trabajar rápidamente en su próximo proyecto. Las plantillas son una combinación de acciones, desencadenantes y secuencias, y también pueden incorporar instancias de servicio de {{site.data.keyword.Bluemix}}. Mediante la utilización de plantillas, puede crear rápida y fácilmente un proyecto e iniciar la codificación inmediatamente. 

Esta guía de aprendizaje le mostrará cómo realizar el despliegue de la plantilla de Cloudant.
{: shortdesc}

## Plantillas disponibles
{: #available-templates}

| Nombre | Descripción | Tiempos de ejecución soportados | 
|:-----------------|:-----------------|:-----------------|
| Cloudant Events | Cuando se edita o se añade un documento en una base de datos Cloudant, se registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| Get HTTP Resource | Una acción web que se invoca en respuesta a un suceso HTTP, y luego capta datos de la API Yahoo Weather. | Node.js, Python |
| Hello World | Esta acción acepta un parámetro único, que debe ser un objeto JSON. | Node.js, Swift, Python, PHP |
| Message Hub Events | Cuando se añaden datos nuevos a un tema de Message Hub, se registra el cambio en la consola. | Node.js, Swift, Python, PHP | 
| Periodic Slack Reminder | Una acción que se publicará en Slack en función de un desencadenante periódico. | Node.js, Swift, Python, PHP |

## Despliegue de la plantilla Cloudant Events
{: #cloudant-template}

La plantilla de Cloudant crea una secuencia de acciones y un desencadenante que iniciará dicha secuencia. El desencadenante se activa cuando hay un cambio en la base de datos Cloudant conectada, que debe ser una base de datos de gatos, con un nombre y un color. El elemento de datos previsto es un gato, con un nombre y un color definidos. Cuando se añade un gato nuevo a la base de datos, o se edita un gato actual, los datos se registran en la consola.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://dev-console.stage1.bluemix.net/openwhisk/) y, a continuación, pulse **Start Creating**. 

2. Pulse **Deploy Template**.

3. Pulse **New Cloudant Item**.

### Creación de la acción de Cloudant

1. A continuación, proporcione un nombre para el paquete o utilice el nombre predeterminado proporcionado, `new-cloudant-item`. 

2. En el desplegable **Actions**, seleccione el tiempo de ejecución de las acciones que serán de su propiedad (nodejs, swift, python o php). Para este ejemplo, seleccione **nodejs** y pulse **Next**.

### Creación del desencadenante de Cloudant

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de Cloudant, proporcione al desencadenante la información necesaria de instancia de servicio de Cloudant.

#### Crear instancia de servicio de Cloudant

Puede elegir entre:
  * **Crear su propia instancia**
  * **Indicar sus propias credenciales** 

1. Para este ejemplo, elija **Create your own instance**.

2. Se abrirá una ventana emergente con un nuevo separador con la página de configuración de Cloudant. Después de crear la instancia de Cloudant, debe crear un conjunto de credenciales de servicio, y luego cerrar el separador para volver a esta página pulsando **Ok**.

3. Ahora, seleccione **Input your own credentials** y proporcione la información siguiente:
  * Username: _el nombre de usuario de Cloudant_
  * Password: _la contraseña de Cloudant_
  * Host: _suele ser `username.cloudant.com`_
  * Database: _el nombre de la base de datos de Cloudant_

### Desplegar la plantilla de Cloudant

1. Pulse **Deploy**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

