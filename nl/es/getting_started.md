---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Iniciación
{: #getting_started}

Con {{site.data.keyword.openwhisk}}, puede utilizar su lenguaje de programación favorito para escribir código reducido que ejecute la lógica de la app de manera escalable. Puede ejecutar código a demanda con solicitudes de API basadas en HTTP o automáticamente en respuesta a servicios
{{site.data.keyword.Bluemix_notm}} y sucesos de terceros. La plataforma de programación de función como servicio (Faas) se basa en el proyecto de código abierto Apache OpenWhisk.
{: shortdesc}

Debido a que {{site.data.keyword.openwhisk_short}} no tiene servidor, no tiene limitaciones en los lenguajes que puede utilizar y no tiene que invertir tiempo explícitamente en proporcionar infraestructura de fondo. Puede centrarse en escribir la lógica de la app en lugar de preocuparse por el escalado automático, la alta disponibilidad, las actualizaciones o el mantenimiento. De forma predeterminada, IBM proporciona el hardware, la red, la administración de software, el equilibrio de carga, los plugins, etc. Solo tiene que poner el código.

## Cómo trabajar con acciones
{: #creating_actions}
{: #openwhisk_actions}

Con {{site.data.keyword.openwhisk}}, puede crear fragmentos de código sin estado definidos para realizar una tarea específica conocida como acción.
{:shortdesc}

**¿Qué es una acción?**

Una acción es un fragmento de código pequeño que se puede invocar o definir para que se ejecute automáticamente en respuesta a un suceso. En cualquier caso, cada ejecución da como resultado un registro que se identifica mediante un ID de activación exclusivo. La entrada y el resultado de una acción se pueden ver como pares de clave-valor. La clave es una serie y el valor es un valor JSON válido. Una acción se puede escribir en el lenguaje que prefiera y se proporciona al servicio como código fuente o como una imagen de Docker. El código de acción se ejecuta cuando se invoca directamente mediante la API de Cloud Functions, la CLI o el SDK de iOS. Una acción puede responder automáticamente a sucesos de servicios de IBM Cloud o de terceros.

**¿Cuáles son las ventajas de utilizar una acción?**

Al utilizar acciones, puede limitar la cantidad de tiempo que se ejecuta el código, lo que reduce el coste de gastos generales.

Por ejemplo, puede utilizar acciones para detectar caras en una imagen, responder a cambios en una base de datos, agregar un conjunto de llamadas de API o incluso publicar un tweet.

**¿Puedo utilizar más de una acción al mismo tiempo?**

Sí. Puede utilizar acciones para llamar a otras acciones, o puede serializar acciones para crear secuencias. Para realizar este trabajo, la salida de una acción debe ser la entrada de otra acción, lo que proporciona una salida que se puede utilizar para desencadenar otra acción, y así sucesivamente. Puede incluso empaquetar el grupo de acciones que cree para formar un paquete. Con un paquete, puede reutilizar acciones o secuencias comunes llamando al paquete en lugar de configurar la acción o la secuencia de nuevo.

Pulse en una opción para empezar:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Pulse un icono para empezar rápidamente con {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="Crear una acción" title="Crear una acción" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="Configurar el plugin de CLI de {{site.data.keyword.openwhisk_short}}" title="Configurar el plugin de CLI de {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="Ver la arquitectura de la plataforma" title="Ver la arquitectura de la plataforma" shape="rect" coords="326, -10, 448, 218" />
</map>

## Creación de una acción en la GUI
{: #openwhisk_start_hello_world}

Para empezar a utilizar {{site.data.keyword.openwhisk_short}}, intente utilizar la plantilla de inicio rápido HelloWorld.

1.  Vaya al panel de control de Functions en el **Catálogo** de
[{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk).

2. Pulse **Empezar a crear** > **Plantillas de inicio rápido** y seleccione la plantilla **Hello World**.

3. Cree un paquete para las acciones especificando un nombre exclusivo en el campo **Nombre de paquete**.

4. Seleccione un entorno de ejecución en la lista desplegable de la sección **Acción helloworld**. Puede obtener una vista previa del código de la acción de ejemplo en cada entorno de ejecución disponible antes de desplegar la plantilla.

5. Pulse **Desplegar**. Ha creado una acción. ¡Buen trabajo!

6. Para ejecutar la acción, pulse **Invocar**. Al invocar una acción se ejecuta manualmente la lógica de la app que define la acción. En el panel **Activaciones**, puede ver el saludo "Hello stranger!" que genera la acción.

7. Opcional: pulse **Cambiar entrada** para cambiar la acción o probar una propia. A continuación, pulse
**Invocar** para ejecutar la acción con sus actualizaciones. El resultado aparece en el panel
**Activaciones**. Puede repetir este proceso tantas veces como quiera.

Enhorabuena. Ha creado su primera acción. Para eliminar esta acción, pulse el menú de desbordamiento y seleccione **Suprimir acción**.

## Creación de una acción en la CLI
{: #openwhisk_start_hello_world_cli}

Comience a trabajar rápidamente con el código de ejemplo JavaScript 'HelloWorld'. En este ejemplo se crea una acción `hello` básica, que se puede invocar manualmente para ejecutar la lógica de la app.

1. [Configure el plugin de CLI de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli).

2. Guarde el código siguiente en un archivo denominado **hello.js**.

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
function main(params) {
        var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. Cree la acción `hello`.

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. Invoque la acción sin pasar ningún parámetro.

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    Salida:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Vuelva a invocar la acción, pasando el parámetro name, para probar la lógica de la app.

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Salida:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Enhorabuena. Ha creado su primera acción. Para suprimir esta acción, ejecute `ibmcloud fn action delete hello`.

## ¿Qué hacer a continuación?
{: #getting_started_next_steps}

Ahora que ha completado su primer despliegue de plantilla, ¿qué puede hacer a partir de aquí? Puede:

* Familiarizarse con la [terminología](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology).
* Empezar con [sus propias acciones](/docs/openwhisk?topic=cloud-functions-openwhisk_actions).
* Aprender a organizar acciones en [paquetes](/docs/openwhisk?topic=cloud-functions-openwhisk_packages).
* Opción avanzada: crear una [API REST sin servidor](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway).
