---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: deploy, deployment templates, templates, example, quickstart

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Despliegue de plantillas de inicio rápido
{: #serviceauth}

{{site.data.keyword.openwhisk}} ofrece un catálogo de plantillas que le ayudarán a empezar a trabajar rápidamente en su próximo proyecto. Las plantillas son una combinación de acciones, desencadenantes y secuencias. Algunas plantillas también incorporan otras instancias de servicio de {{site.data.keyword.Bluemix_notm}}. Mediante la utilización de plantillas, puede crear rápida y fácilmente un proyecto e iniciar la codificación inmediatamente.
{: shortdesc}

## Plantillas de inicio rápido disponibles
{: #available-templates}

| Nombre | Descripción | Tiempos de ejecución soportados |
|:-----------------|:-----------------|:-----------------|
| [{{site.data.keyword.cloudant_short_notm}} Events](/docs/openwhisk?topic=cloud-functions-serviceauth#cloudant-template) | Cuando se edite o añada un documento en una base de datos de {{site.data.keyword.cloudantfull}}, registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| [Upload Image](/docs/openwhisk?topic=cloud-functions-serviceauth#cos-upload-image) | Utiliza una acción web para cargar una imagen en el grupo de una instancia de {{site.data.keyword.cos_full}} y, a continuación, recupera una miniatura de la imagen. | Node.js |
| [Get HTTP Resource](/docs/openwhisk?topic=cloud-functions-serviceauth#get-http-resource-template) | Invoca una acción web utilizando un suceso HTTP y obtiene datos de la API de Yahoo Weather. | Node.js, Python |
| [Hello World](/docs/openwhisk?topic=cloud-functions-serviceauth#hello-world-template) | Crea una acción básica que acepte un objeto JSON como único parámetro. | Node.js, Swift, Python, PHP |
| [{{site.data.keyword.messagehub}} Events](/docs/openwhisk?topic=cloud-functions-serviceauth#messagehub-events-template) | Cuando se añaden datos nuevos en un tema de {{site.data.keyword.messagehub_full}}, registra el cambio en la consola. | Node.js, Swift, Python, PHP |
| [Periodic Slack Reminder](/docs/openwhisk?topic=cloud-functions-serviceauth#slack-reminder-template) | Utiliza un webhook para Slack basado en un desencadenante periódico. | Node.js, Swift, Python, PHP |

## Despliegue de la plantilla {{site.data.keyword.cloudant_short_notm}} Events
{: #cloudant-template}

La plantilla de {{site.data.keyword.cloudant_short_notm}} crea una secuencia de acciones y un desencadenante que activa dicha secuencia. El desencadenante se activa cuando se realiza un cambio en la base de datos de ejemplo de gatos de {{site.data.keyword.cloudant_short_notm}} conectada. El elemento de datos previsto es un gato, con un nombre y un color definidos. Cuando se añade un gato nuevo a la base de datos o se edita un gato actual, los datos se registran en la consola.

### Despliegue de la plantilla {{site.data.keyword.cloudant_short_notm}} Events desde la interfaz de usuario

1. Vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create) en la consola de {{site.data.keyword.openwhisk_short}}.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Cloudant Events**.

4. Cree la acción de {{site.data.keyword.cloudant_short_notm}}.
    1. Proporcione un nombre para el paquete o utilice el nombre predeterminado proporcionado, `new-cloudant-item`.
    2. En la lista **Acciones**, seleccione `Node.js 6` para el ejemplo de la base de datos de gatos y pulse **Siguiente**.

5. Cree el desencadenante de {{site.data.keyword.cloudant_short_notm}}. Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear el desencadenante, debe proporcionar una instancia de servicio de {{site.data.keyword.messagehub}} y credenciales de servicio.
    1. En la lista **Instancia de Cloudant**, seleccione **Crear su propia instancia**. Se abre la página de configuración de {{site.data.keyword.cloudant_short_notm}}.
    2. Cree la instancia de servicio de {{site.data.keyword.cloudant_short_notm}}.
    3. Cree un conjunto de credenciales de servicio.
    4. Pulse **Aceptar** para cerrar la página de configuración de {{site.data.keyword.cloudant_short_notm}} y volver a la consola de {{site.data.keyword.openwhisk_short}}.
    5. En la lista **Instancia de Cloudant**, ahora puede seleccionar **Especificar sus propias credenciales** y proporcionar la información siguiente:
      * Nombre de usuario: su nombre de usuario de {{site.data.keyword.cloudant_short_notm}}
      * Contraseña: su contraseña de {{site.data.keyword.cloudant_short_notm}}
      * Host: `<username>.cloudant.com`
      * Base de datos: el nombre de su base de datos de {{site.data.keyword.cloudant_short_notm}}

5. Pulse **Desplegar**. Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

### Despliegue de la plantilla {{site.data.keyword.cloudant_short_notm}} Events desde la CLI

1. Clone el repositorio de plantillas.
    ```
    git clone https://github.com/ibm-functions/template-cloudant-trigger.git
    ```
    {: pre}

2. Vaya al directorio del entorno de ejecución de acciones que desee utilizar: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-cloudant-trigger/runtimes/nodejs
    ```
    {: pre}

3. Despliegue la plantilla, utilizando las variables de entorno siguientes.
    ```
    CLOUDANT_HOSTNAME=<host> CLOUDANT_USERNAME=<username> CLOUDANT_PASSWORD=<password> CLOUDANT_DATABASE=<database> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Descripción de las variables de entorno</caption>
    <thead>
    <th colspan=2>Descripción de las variables de entorno</th>
    </thead>
    <tbody>
    <tr><td><code>CLOUDANT_HOSTNAME</code></td><td><code>&lt;username&gt;.cloudant.com</code></td></tr>
    <tr><td><code>CLOUDANT_USERNAME</code></td><td>Su nombre de usuario de {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>CLOUDANT_PASSWORD</code></td><td>Su contraseña de {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>CLOUDANT_DATABASE</code></td><td>El nombre de la base de datos de {{site.data.keyword.cloudant_short_notm}}</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Un nombre personalizado para el paquete</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Un nombre personalizado para la regla</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Un nombre personalizado para el desencadenante</td></tr>
    </tbody></table>

## Despliegue de la plantilla Upload Image
{: #cos-upload-image}

La plantilla Upload Image crea una acción web que permite cargar una imagen en un grupo de {{site.data.keyword.cos_short}} mediante una pequeña interfaz. A continuación, la plantilla recupera la imagen como una miniatura y la muestra en la interfaz de la acción web.

### Despliegue de la plantilla Upload Image desde la interfaz de usuario

1. Vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create) en la consola de {{site.data.keyword.openwhisk_short}}.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Upload Image**.

4. Especifique el nombre del paquete o utilice el nombre predeterminado `upload-image` y pulse **Siguiente**.

6. La plantilla precisa de credenciales de servicio de una instancia de servicio de {{site.data.keyword.cos_full_notm}}. En la lista de **{{site.data.keyword.cos_short}}**, seleccione una de las siguientes opciones:
  * **Crear una nueva instancia**: Si no tiene una instancia de servicio existente, seleccione esta opción para crear una.
      1. En la página de creación de la instancia de servicio de {{site.data.keyword.cos_full_notm}} que se abre, cree una instancia de servicio.
      2. [Cree un conjunto de credenciales de servicio de HMAC](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials).
      3. [Cree al menos un grupo](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets).
  * **Especificar sus propias credenciales**: Seleccione esta opción para especificar manualmente sus propias credenciales para una instancia de servicio de {{site.data.keyword.cos_short}}. Las credenciales deben tener claves HMAC y la instancia de servicio debe tener al menos un grupo.
  * **Instancias existentes**: Si tiene alguna instancia de {{site.data.keyword.cos_short}} existente, seleccione una de las instancias de la lista. Las credenciales deben tener claves HMAC y la instancia de servicio debe tener al menos un grupo.

7. Pulse **Desplegar**.

8. En la navegación de la izquierda, pulse **Puntos finales**.

9. En la sección **Acción web**, copie el enlace sin el sufijo .json y péguelo en la barra de direcciones del navegador. Se visualiza la interfaz para la acción web de la plantilla.

10. Opcional: Después de desplegar la plantilla, puede ir al panel de control **Acciones** para personalizar el código con dos nuevos paquetes:
    * El paquete `cloud-object-storage`, que contiene acciones que funcionan con instancias de {{site.data.keyword.cos_short}}
    * El paquete de plantillas (nombre predeterminado `upload-image`), que contiene la acción `app`

### Despliegue de la plantilla Upload Image desde la CLI

1. [Instale el paquete de {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_cli).

2. Clone el repositorio de plantillas.
    ```
    git clone https://github.com/ibm-functions/template-cloud-object-storage.git
    ```
    {: pre}

3. Vaya al directorio del entorno de ejecución `nodejs`.
    ```
    cd template-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Despliegue la plantilla, utilizando un nombre de paquete personalizado y el nombre del grupo de {{site.data.keyword.cos_short}} como variables de entorno.
    ```
    PACKAGE_NAME=<name> BUCKET_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. En la [página Acciones ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/actions) de la consola de {{site.data.keyword.openwhisk_short}}, pulse en la acción `process-change`.

5. En la navegación de la izquierda, pulse **Puntos finales**.

6. En la sección **Acción web**, copie el enlace sin el sufijo .json y péguelo en la barra de direcciones del navegador. Se visualiza la interfaz para la acción web de la plantilla.

## Despliegue de la plantilla Get HTTP Resource
{: #get-http-resource-template}

La plantilla Get HTTP Resource crea una acción para captar un recurso externo, la API de Yahoo Weather y luego devuelve datos. La acción está habilitada como una acción web, lo que permite que se invoque con un URL que está habilitado para CORS y no necesita una clave de autenticación, lo que resulta útil para crear programas de fondo para aplicaciones web. **Nota**: de forma predeterminada, el punto final `get-http-resource` está disponible públicamente para cualquiera que lo llame.

### Despliegue de la plantilla Get HTTP Resource desde la interfaz de usuario

1. Vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create) en la consola de {{site.data.keyword.openwhisk_short}}.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Get HTTP Resource**.

3. Especifique un nombre para su paquete o utilice el nombre predeterminado `get-http-resource`.

4. Elija el entorno de ejecución para las acciones: Node.js 8, Node.js 6 o Python 3.

5. Pulse **Desplegar**.

6. Invoque la acción utilizando CURL con el URL siguiente: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Por ejemplo:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

### Despliegue de la plantilla Get HTTP Resource desde la CLI

1. Clone el repositorio de plantillas.
    ```
    git clone https://github.com/ibm-functions/template-get-external-resource.git
    ```
    {: pre}

2. Vaya al directorio del entorno de ejecución de acciones que desee utilizar: `nodejs-6`, `nodejs` o `python`.
    ```
    cd template-get-external-resource/runtimes/nodejs
    ```
    {: pre}

3. Despliegue la plantilla, utilizando un nombre de paquete personalizado como variable de entorno.
    ```
    PACKAGE_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Invoque la acción utilizando CURL con el URL siguiente: `https://us-south.functions.cloud.ibm.com/api/v1/web/<namespace>/$PACKAGE_NAME/weather?location=<city>`. Por ejemplo:
    ```
    curl https://us-south.functions.cloud.ibm.com/api/v1/web/myusername@email.com_myspace/Get%20Resource/weather?location=Austin
    ```
    {: pre}

## Despliegue de la plantilla Hello World
{: #hello-world-template}

Puede desplegar esta acción Hello World básica para iniciarse en {{site.data.keyword.openwhisk_short}} o para probar otras entidades que cree, como desencadenantes y reglas.

### Despliegue de la plantilla Hello World desde la interfaz de usuario

1. Vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create) en la consola de {{site.data.keyword.openwhisk_short}}.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Hello World**.

4. Especifique un nombre para su paquete o utilice el nombre predeterminado `hello-world`.

5. Elija el entorno de ejecución para las acciones: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.

6. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

### Despliegue de la plantilla Hello World desde la CLI

1. Clone el repositorio de la plantilla Hello World.
    ```
    git clone https://github.com/ibm-functions/template-hello-world.git
    ```
    {: pre}

2. Vaya al directorio del entorno de ejecución de acciones que desee utilizar: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-hello-world/runtimes/nodejs
    ```
    {: pre}

3. Despliegue la plantilla.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

## Despliegue de la plantilla {{site.data.keyword.messagehub}} Events
{: #messagehub-events-template}

La plantilla {{site.data.keyword.messagehub}} Events crea una acción y un desencadenante que activa dicha acción. El desencadenante se activa cuando se añade un elemento nuevo al tema de {{site.data.keyword.messagehub}} seleccionado durante la creación de la plantilla.

### Despliegue de la plantilla {{site.data.keyword.messagehub}} Events desde la interfaz de usuario

1. Vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create) en la consola de {{site.data.keyword.openwhisk_short}}.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **{{site.data.keyword.messagehub}} Events**.

4. Cree la acción de {{site.data.keyword.messagehub}}.
    1. Proporcione un nombre para el paquete o utilice el nombre predeterminado proporcionado, `message-hub-events`.
    2. Elija el entorno de ejecución para las acciones: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.
    3. Pulse **Siguiente**.

5. Cree el desencadenante de {{site.data.keyword.messagehub}}. Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos. Para crear el desencadenante, debe proporcionar una instancia de servicio de {{site.data.keyword.messagehub}} y credenciales de servicio.
    1. En la lista **Instancia de MessageHub**, seleccione **Crear su propia instancia**. Se abre la página de creación de {{site.data.keyword.messagehub}}.
    2. Cree la instancia de servicio de {{site.data.keyword.messagehub}}.
    3. Cree un conjunto de credenciales de servicio.
    4. Pulse **Aceptar** para cerrar la página de creación de {{site.data.keyword.messagehub}} y volver a la consola de {{site.data.keyword.messagehub}}.
    5. En la lista **Instancia de MessageHub**, ahora puede seleccionar **Especificar sus propias credenciales** y proporcionar la información siguiente:
      * Nombre de usuario: su nombre de usuario de {{site.data.keyword.messagehub}}
      * Contraseña: su contraseña de {{site.data.keyword.messagehub}}
      * kafka_admin_url: su URL REST de administrador de {{site.data.keyword.messagehub}}
      * Base de datos: el nombre de su base de datos de {{site.data.keyword.messagehub}}
      * Tema: el tema al que se va a suscribir

5. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

### Despliegue de la plantilla {{site.data.keyword.messagehub}} Events desde la CLI

1. Clone el repositorio de plantillas.
    ```
    git clone https://github.com/ibm-functions/template-messagehub-trigger.git
    ```
    {: pre}

2. Vaya al directorio del entorno de ejecución de acciones que desee utilizar: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-messagehub-trigger/runtimes/nodejs
    ```
    {: pre}

3. Despliegue la plantilla, utilizando las variables de entorno siguientes.
    ```
    KAFKA_BROKERS=<host> KAFKA_TOPIC=<topic> MESSAGEHUB_USER=<username> MESSAGEHUB_PASS=<password> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Descripción de las variables de entorno</caption>
    <thead>
    <th colspan=2>Descripción de las variables de entorno</th>
    </thead>
    <tbody>
    <tr><td><code>KAFKA_BROKERS</code></td><td>El URL REST de administrador de {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>KAFKA_TOPIC</code></td><td>El tema al que se va a suscribir</td></tr>
    <tr><td><code>MESSAGEHUB_USER</code></td><td>Su nombre de usuario de {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>MESSAGEHUB_PASS</code></td><td>Su contraseña de {{site.data.keyword.messagehub}}</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Un nombre personalizado para el paquete</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Un nombre personalizado para la regla</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Un nombre personalizado para el desencadenante</td></tr>
    </tbody></table>

## Despliegue de la plantilla Periodic Slack Reminder
{: #slack-reminder-template}

La plantilla Periodic Slack Reminder publica en Slack a un intervalo especificado por el usuario durante la creación del desencadenante.

### Despliegue de la plantilla Periodic Slack Reminder desde la interfaz de usuario

1. Vaya a https://api.slack.com/incoming-webhooks para configurar el URL de webhooks de entrada.

1. Vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create) en la consola de {{site.data.keyword.openwhisk_short}}.

2. Pulse **Plantillas de inicio rápido**.

3. Pulse **Periodic Slack Reminder Events**.

4. Cree la acción de Periodic Slack Reminder.
    1. Proporcione un nombre para el paquete o utilice el nombre predeterminado proporcionado, `periodic-slack-reminder`.
    2. Elija el entorno de ejecución para las acciones: Node.js 8, Node.js 6, Python 3, Swift 4 o PHP 7.1.
    3. En la sección Parámetros, especifique el URL de webhook en el campo **Valor de parámetro**, como `https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX`.
    4. Pulse **Siguiente**.

5. Cree el desencadenante de Periodic Slack Reminder. Los desencadenantes invocan acciones cuando reciben sucesos de orígenes de sucesos.
    1. Especifique el intervalo de tiempo de desencadenante utilizando un patrón o una expresión Cron.
        * Patrón: seleccione horas UTC para los días de la semana, horas y minutos.
        * Cron: especifique una secuencia de Cron basada en la <a href="http://crontab.org">sintaxis de crontab de UNIX</a>. Utilice 5 o menos separados por espacios con el formato `X X X X X`.
    2. Añada la carga útil JSON del desencadenante.

6. Pulse **Desplegar**.

Una vez desplegada la plantilla, puede realizar más cambios en el código para personalizarlo según sea necesario, o retroceder y comprobar el catálogo de plantillas disponibles.

### Despliegue de la plantilla Periodic Slack Reminder desde la CLI

1. Vaya a https://api.slack.com/incoming-webhooks para configurar el URL de webhooks de entrada.

1. Clone el repositorio de plantillas.
    ```
    git clone https://github.com/ibm-functions/template-reminder-slack.git
    ```
    {: pre}

2. Vaya al directorio del entorno de ejecución de acciones que desee utilizar: `nodejs-6`, `nodejs`, `php`, `python` o `swift`.
    ```
    cd template-reminder-slack/runtimes/nodejs
    ```
    {: pre}

3. Despliegue la plantilla, utilizando las variables de entorno siguientes.
    ```
    SLACK_WEBHOOK_URL=<url> ALARM_CRON=<cron> PACKAGE_NAME=<name> RULE_NAME=<name> TRIGGER_NAME=<name> ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    <table>
    <caption>Descripción de las variables de entorno</caption>
    <thead>
    <th colspan=2>Descripción de las variables de entorno</th>
    </thead>
    <tbody>
    <tr><td><code>SLACK_WEBHOOK_URL</code></td><td>El URL de webhook, como <code>https://hooks.slack.com/TXXXXX/BXXXXX/XXXXXXXXXX</code></td></tr>
    <tr><td><code>ALARM_CRON</code></td><td>Una secuencia de Cron basada en la <a href="http://crontab.org">sintaxis de crontab de UNIX</a>. Utilice 5 o menos campos separados por espacios con el formato <code>X X X X X</code>.</td></tr>
    <tr><td><code>PACKAGE_NAME</code></td><td>Un nombre personalizado para el paquete</td></tr>
    <tr><td><code>RULE_NAME</code></td><td>Un nombre personalizado para la regla</td></tr>
    <tr><td><code>TRIGGER_NAME</code></td><td>Un nombre personalizado para el desencadenante</td></tr>
    </tbody></table>
