---

copyright:

  years: 2018

lastupdated: "2018-05-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Despliegue de plantillas de inicio rápido
{: #serviceauth}

{{site.data.keyword.openwhisk}} ofrece un catálogo de plantillas que le ayudarán a empezar a trabajar rápidamente en su próximo proyecto. Las plantillas son una combinación de acciones, desencadenantes y secuencias, y también pueden incorporar instancias de servicio de {{site.data.keyword.Bluemix}}. Mediante la utilización de plantillas, puede crear rápida y fácilmente un proyecto e iniciar la codificación inmediatamente.

Esta guía de aprendizaje le mostrará cómo realizar el despliegue de la plantilla de Cloudant.
{: shortdesc}

## Plantillas de inicio rápido disponibles
{: #available-templates}

| Nombre | Descripción | Tiempos de ejecución soportados |
|:-----------------|:-----------------|:-----------------|
| [Cloudant Events](./deploy_templates.html#cloudant-template) | Cuando se edita o se añade un documento en una base de datos Cloudant, se registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Una acción web que se invoca en respuesta a un suceso HTTP, y luego capta datos de la API Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Esta acción acepta un parámetro único, que debe ser un objeto JSON. | Node.js, Swift, Python, PHP |
| [Message Hub Events](./deploy_templates.html#messagehub-events-template) | Cuando se añaden datos nuevos a un tema de Message Hub, se registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | Una acción que se publicará en Slack en función de un desencadenante periódico. | Node.js, Swift, Python, PHP |

## Despliegue de la plantilla Cloudant Events
{: #cloudant-template}

La plantilla de Cloudant crea una secuencia de acciones y un desencadenante que iniciará dicha secuencia. El desencadenante se activa cuando hay un cambio en la base de datos Cloudant conectada, que debe ser una base de datos de gatos, con un nombre y un color. El elemento de datos previsto es un gato, con un nombre y un color definidos. Cuando se añade un gato nuevo a la base de datos, o se edita un gato actual, los datos se registran en la consola.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) y, a continuación, pulse **Empezar a crear**.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Nuevo elemento de Cloudant**.

### Creación de la acción de Cloudant

1. A continuación, proporcione un nombre para el paquete o utilice el nombre predeterminado proporcionado, `new-cloudant-item`.

2. En el desplegable **Acciones**, seleccione el tiempo de ejecución de las acciones que serán de su propiedad (nodejs, swift, python o php). Para este ejemplo, seleccione **nodejs** y pulse **Siguiente**.

### Creación del desencadenante de Cloudant

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de Cloudant, proporcione al desencadenante la información necesaria de instancia de servicio de Cloudant.

#### Crear instancia de servicio de Cloudant

Puede elegir entre:
  * **Crear su propia instancia**
  * **Especificar sus propias credenciales**

1. Para este ejemplo, elija **Crear su propia instancia**.

2. Se abrirá una ventana emergente con un nuevo separador con la página de configuración de Cloudant. Después de crear la instancia de Cloudant, debe crear un conjunto de credenciales de servicio, y luego cerrar el separador para volver a esta página pulsando **Aceptar**.

3. Ahora, seleccione **Especificar sus propias credenciales** y proporcione la información siguiente:
  * Nombre de usuario: _el nombre de usuario de Cloudant_
  * Contraseña: _la contraseña de Cloudant_
  * Host: _suele ser `username.cloudant.com`_
  * Base de datos: _el nombre de la base de datos de Cloudant_

### Desplegar la plantilla de Cloudant

Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

## Despliegue de la plantilla Get HTTP Resource
{: #get-http-resource-template}

La plantilla Obtener recursos HTTP crea una acción para captar un recurso externo, la API de Yahoo Weather y luego devuelve datos. La acción está habilitada como una acción web, lo que permite que se invoque con un URL que está habilitado para CORS y no necesita una clave de autenticación, lo que resulta útil para crear programas de fondo para aplicaciones web. **Nota**: de forma predeterminada, el punto final `get-http-resource` está disponible de forma pública para cualquiera que desee llamarlo.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) y, a continuación, pulse **Empezar a crear**.

2. Pulse **Plantillas de inicio rápido**.

3. Revise el campo **Nombre de paquete**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `get-http-resource`.

4. Elija el tiempo de ejecución para las acciones que poseerá: Node.js 8, Node.js 6 o Python 3.

5. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

## Despliegue de la plantilla Hello World
{: #hello-world-template}

Esta acción acepta un parámetro único, que debe ser un objeto JSON.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) y, a continuación, pulse **Empezar a crear**.

2. Pulse **Plantillas de inicio rápido**.

3. Revise el campo **Nombre de paquete**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `hello-world`.

4. Elija el tiempo de ejecución para las acciones que poseerá: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

5. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

## Despliegue de la plantilla Message Hub Events
{: #messagehub-events-template}

La plantilla Message Hub Events crea una acción y un desencadenante que activa dicha acción. El desencadenante se activa cuando se añade un elemento nuevo al tema Message Hub seleccionado durante la creación de la plantilla.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) y, a continuación, pulse **Empezar a crear**.

2. Pulse **Plantillas de inicio rápido**.

3. Revise el campo **Nombre de paquete**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `message-hub-events`.

4. Elija el tiempo de ejecución para las acciones que poseerá: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

5. Pulse **Siguiente**.

### Creación del desencadenante de Message Hub

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de Message Hub, proporcione al desencadenante la información necesaria de instancia de servicio de Message Hub.

Revise el campo **Nombre de desencadenante**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `message-hub-events-trgr`.

### Creación de la instancia de servicio de Message Hub

Puede elegir entre:
  * **Crear su propia instancia**
  * **Especificar sus propias credenciales**

1. Para este ejemplo, elija **Crear su propia instancia**.

2. Se abrirá una ventana emergente con un nuevo separador con la página de configuración de Message Hub. Después de crear la instancia de Message Hub, debe crear un conjunto de credenciales de servicio, y luego cerrar el separador para volver a esta página pulsando **Aceptar**.

3. Ahora, seleccione **Especificar sus propias credenciales** y proporcione la información siguiente:
  * Nombre de usuario: _el nombre de usuario de Message Hub_
  * Contraseña: _la contraseña de Message Hub_
  * kafka_admin_url: _su URL de REST de administrador de Message Hub_
  * Base de datos: _el nombre de la base de datos de Message Hub_
  * Tema: _Tema a suscribir_

### Desplegar la plantilla de Message Hub

Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

## Despliegue de la plantilla Periodic Slack Reminder
{: #slack-reminder-template}

La plantilla Periodic Slack Reminder publica en Slack a un intervalo especificado por el usuario durante la creación del desencadenante. Antes de crear esta plantilla, vaya a https://api.slack.com/incoming-webhooks para configurar el URL de webhooks entrantes necesario.

1. Revise el campo **Nombre de paquete**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `periodic-slack-reminder`.

2. Elija el tiempo de ejecución para las acciones que poseerá: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

3. En la sección **Parámetros**, escriba el URL del webhook en el campo **Valor de parámetro** y, a continuación, pulse **Siguiente**. (Ejemplo: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Creación del desencadenante de Slack Reminder

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de Slack Reminder, proporcione al desencadenante la información necesaria de instancia de servicio de Message Hub.

1. Revise el campo **Nombre de desencadenante**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `periodic-slack-reminder-trgr`.

2. A continuación, puede especificar el intervalo en el que se va a activar el desencadenante mediante una expresión de patrón o Cron. Puede seleccionar horas UTC para los días de la semana, las horas y los minutos. Seleccione las opciones de intervalo que desee y estará listo para desplegar la plantilla.

3. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.
