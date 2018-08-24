---

copyright:
  years: 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}

# Despliegue de plantillas de inicio rápido
{: #serviceauth}

{{site.data.keyword.openwhisk}} ofrece un catálogo de plantillas que le ayudarán a empezar a trabajar rápidamente en su próximo proyecto. Las plantillas son una combinación de acciones, desencadenantes y secuencias, y también pueden incorporar instancias de servicio de {{site.data.keyword.Bluemix}}. Mediante la utilización de plantillas, puede crear rápida y fácilmente un proyecto e iniciar la codificación inmediatamente.

Esta guía de aprendizaje le guía a través del despliegue de las plantillas que están disponibles a través de {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Plantillas de inicio rápido disponibles
{: #available-templates}

| Nombre | Descripción | Tiempos de ejecución soportados |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](./deploy_templates.html#cloudant-template) | Cuando {{site.data.keyword.cloudant}} tiene un documento editado o añadido, registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| [Cargar imagen](./deploy_templates.html#cos-upload-image) | Una acción web que permite cargar una imagen en un depósito de una instancia de {{site.data.keyword.cos_full}} y recuperar una imagen en miniatura de dicha imagen. | Node.js |
| [Get HTTP Resource](./deploy_templates.html#get-http-resource-template) | Una acción web que se invoca en respuesta a un suceso HTTP, y luego capta datos de la API Yahoo Weather. | Node.js, Python |
| [Hello World](./deploy_templates.html#hello-world-template) | Esta acción acepta un parámetro único, que debe ser un objeto JSON. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](./deploy_templates.html#messagehub-events-template) | Cuando se añaden datos nuevos a un tema de {{site.data.keyword.messagehub_full}}, se registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](./deploy_templates.html#slack-reminder-template) | Una acción que se publicará en Slack en función de un desencadenante periódico. | Node.js, Swift, Python, PHP |

## Despliegue de una plantilla de {{site.data.keyword.cloudant_short_notm}} Events
{: #cloudant-template}

La plantilla de {{site.data.keyword.cloudant_short_notm}} crea una secuencia de acciones y un desencadenante que iniciará dicha secuencia. El desencadenante se activa cuando hay un cambio en la base de datos {{site.data.keyword.cloudant_short_notm}} conectada, que debe ser una base de datos de gatos, con un nombre y un color. El elemento de datos previsto es un gato, con un nombre y un color definidos. Cuando se añade un gato nuevo a la base de datos, o se edita un gato actual, los datos se registran en la consola.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) y, a continuación, pulse **Empezar a crear**.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Cloudant Events**.

### Creación de una acción de {{site.data.keyword.cloudant_short_notm}}

1. A continuación, proporcione un nombre para el paquete o utilice el nombre predeterminado proporcionado, `new-cloudant-item`.

2. En el desplegable **Acciones**, seleccione el tiempo de ejecución de las acciones que serán de su propiedad (nodejs, swift, python o php). Para este ejemplo, seleccione **nodejs** y pulse **Siguiente**.

### Creación de un desencadenante de {{site.data.keyword.cloudant_short_notm}}

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de {{site.data.keyword.cloudant_short_notm}}, proporcione al desencadenante la información necesaria de instancia de servicio de {{site.data.keyword.cloudant_short_notm}}.

#### Creación de una instancia de servicio de {{site.data.keyword.cloudant_short_notm}}

Puede elegir entre:
  * **Crear su propia instancia**
  * **Especificar sus propias credenciales**

1. Para este ejemplo, elija **Crear su propia instancia**.

2. Se abrirá una ventana emergente con un nuevo separador con la página de configuración de {{site.data.keyword.cloudant_short_notm}}. Después de crear la instancia de {{site.data.keyword.cloudant_short_notm}}, debe crear un conjunto de credenciales de servicio, y luego cerrar el separador para volver a esta página pulsando **Aceptar**.

3. Ahora, seleccione **Especificar sus propias credenciales** y proporcione la información siguiente:
  * Nombre de usuario: _Su nombre de usuario de {{site.data.keyword.cloudant_short_notm}}_
  * Contraseña: _Su contraseña de {{site.data.keyword.cloudant_short_notm}}_
  * Host: _Suele ser `username.{{site.data.keyword.cloudant_short_notm}}.com`_
  * Base de datos: _El nombre de su base de datos de {{site.data.keyword.cloudant_short_notm}}_

### Despliegue de una plantilla de {{site.data.keyword.cloudant_short_notm}}

Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

## Despliegue de la plantilla Upload Image
{: #cos-upload-image}

La plantilla Upload Image crea una acción web que permite cargar una imagen en un depósito de {{site.data.keyword.cos_short_notm}} con la ayuda de una pequeña interfaz. A continuación, la plantilla recupera la imagen como una miniatura y la muestra en la interfaz de la acción web.

Para desplegar la plantilla:

1. Vaya a la consola de {{site.data.keyword.openwhisk_short}} en [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/).

2. Pulse **Iniciar creación**.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse la plantilla **Upload Image**.

4. Especifique un nombre para su paquete o utilice el nombre predeterminado `upload-image`.

5. Pulse **Siguiente**.

6. La plantilla precisa de credenciales de servicio de una instancia de servicio de {{site.data.keyword.cos_full_notm}}. En la lista de **{{site.data.keyword.cos_short}}**, seleccione una de las siguientes opciones:
  * **Crear una nueva instancia**: Si no tiene una instancia de servicio existente, seleccione esta opción para crear una.
      1. En la página de creación de la instancia de servicio de {{site.data.keyword.cos_full_notm}} que se abre, cree una instancia de servicio.
      2. [Cree un conjunto de credenciales de servicio de HMAC](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).
      3. [Cree al menos un depósito](/docs/services/cloud-object-storage/getting-started.html#create-buckets).
  * **Especificar sus propias credenciales**: Seleccione esta opción para especificar manualmente sus propias credenciales para una instancia de servicio de {{site.data.keyword.cos_short}}. Las credenciales deben tener claves HMAC y la instancia de servicio debe tener al menos un depósito.
  * **Instancias existentes**: Si tiene alguna instancia de {{site.data.keyword.cos_short}} existente, seleccione una de las instancias de la lista. Las credenciales deben tener claves HMAC y la instancia de servicio debe tener al menos un depósito.

7. Pulse **Desplegar**.

8. En la navegación de la izquierda, pulse **Puntos finales**.

9. En la sección de Acción web, copie el enlace sin el sufijo .json y péguelo en la barra de direcciones de su navegador. Se visualiza la interfaz para la acción web de la plantilla.

10. Opcional: Después de desplegar la plantilla, puede ir al panel de control de Acciones para personalizar el código con dos nuevos paquetes:
    * El paquete `cloud-object-storage`, que contiene acciones que funcionan con instancias de {{site.data.keyword.cos_short}}
    * El paquete de plantillas (nombre predeterminado `upload-image`), que contiene la acción `app`

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

## Despliegue de una plantilla de {{site.data.keyword.messagehub}} Events
{: #messagehub-events-template}

La plantilla {{site.data.keyword.messagehub}} Events crea una acción y un desencadenante que activa dicha acción. El desencadenante se activa cuando se añade un elemento nuevo al tema de {{site.data.keyword.messagehub}} seleccionado durante la creación de la plantilla.

1. Para crear una plantilla, vaya a [{{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/openwhisk/) y, a continuación, pulse **Empezar a crear**.

2. Pulse **Plantillas de inicio rápido**.

3. Revise el campo **Nombre de paquete**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `message-hub-events`.

4. Elija el tiempo de ejecución para las acciones que poseerá: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

5. Pulse **Siguiente**.

### Creación de un desencadenante de {{site.data.keyword.messagehub}}

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de {{site.data.keyword.messagehub}}, proporcione al desencadenante la información necesaria de instancia de servicio de {{site.data.keyword.messagehub}}.

Revise el campo **Nombre de desencadenante**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `message-hub-events-trgr`.

### Creación de una instancia de servicio de {{site.data.keyword.messagehub}}

Puede elegir entre:
  * **Crear su propia instancia**
  * **Especificar sus propias credenciales**

1. Para este ejemplo, elija **Crear su propia instancia**.

2. Se abrirá una ventana emergente con un nuevo separador con la página de configuración de {{site.data.keyword.messagehub}}. Después de crear la instancia de {{site.data.keyword.messagehub}}, debe crear un conjunto de credenciales de servicio, y luego cerrar el separador para volver a esta página pulsando **Aceptar**.

3. Ahora, seleccione **Especificar sus propias credenciales** y proporcione la información siguiente:
  * Nombre de usuario: _Su nombre de usuario de {{site.data.keyword.messagehub}}_
  * Contraseña: _Su contraseña de {{site.data.keyword.messagehub}}_
  * kafka_admin_url: _Su URL REST de administrador de {{site.data.keyword.messagehub}}_
  * Base de datos: _El nombre de su base de datos de {{site.data.keyword.messagehub}}_
  * Tema: _Tema a suscribir_

### Despliegue de una plantilla de {{site.data.keyword.messagehub}}

Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

## Despliegue de la plantilla Periodic Slack Reminder
{: #slack-reminder-template}

La plantilla Periodic Slack Reminder publica en Slack a un intervalo especificado por el usuario durante la creación del desencadenante. Antes de crear esta plantilla, vaya a https://api.slack.com/incoming-webhooks para configurar el URL de webhooks entrantes necesario.

1. Revise el campo **Nombre de paquete**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `periodic-slack-reminder`.

2. Elija el tiempo de ejecución para las acciones que poseerá: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

3. En la sección **Parámetros**, escriba el URL del webhook en el campo **Valor de parámetro** y, a continuación, pulse **Siguiente**. (Ejemplo: https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX).

### Creación del desencadenante de Slack Reminder

Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear un desencadenante para la plantilla de Slack Reminder, proporcione al desencadenante la información necesaria de instancia de servicio de {{site.data.keyword.messagehub}}.

1. Revise el campo **Nombre de desencadenante**; puede actualizarlo si es necesario. Automáticamente se establece el valor predeterminado `periodic-slack-reminder-trgr`.

2. A continuación, puede especificar el intervalo en el que se va a activar el desencadenante mediante una expresión de patrón o Cron. Puede seleccionar horas UTC para los días de la semana, las horas y los minutos. Seleccione las opciones de intervalo que desee y estará listo para desplegar la plantilla.

3. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.
