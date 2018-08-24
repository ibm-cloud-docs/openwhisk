---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Iniciación
{: #index}

{{site.data.keyword.openwhisk}} es una plataforma de programación políglota que funciona como un servicio (FaaS) basada en Apache OpenWhisk. {{site.data.keyword.openwhisk_short}} permite a los desarrolladores escribir acciones de llamada de código ligero que ejecutan de forma escalable la lógica de la aplicación. Puede configurar acciones para que se produzcan según demanda con base a solicitudes de API basadas en HTTP desde apps web o apps para móviles o que se ejecuten automáticamente en respuesta a solicitudes basadas en sucesos procedentes de servicios de {{site.data.keyword.Bluemix_notm}} y sucesos de terceros.
{: shortdesc}

Puesto que {{site.data.keyword.openwhisk_short}} es una plataforma sin servidor, controlada por sucesos, no es necesario que suministre servidores de forma explícita. Los desarrolladores que trabajan con apps de microservicios, para móviles, IoT y muchas otras apps pueden centrarse en escribir la lógica de la app en lugar de preocuparse por el escalado automático, la alta disponibilidad, las actualizaciones y el mantenimiento. El escalado automático y el equilibrio de carga significa que no tiene que configurar manualmente los clústeres, los plugins http, etc. IBM se encarga de todo el hardware, del sistema de red y de la administración de software. Todo lo que tiene que hacer es suministrar el código.

Pulse en una opción para empezar:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Pulse un icono para empezar a trabajar rápidamente con {{site.data.keyword.openswhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#openwhisk_start_hello_world" alt="Crear una acción" title="Crear una acción" shape="rect" coords="-7, -8, 108, 211" />
<area href="bluemix_cli.html" alt="Configurar el plugin de CLI de {{site.data.keyword.openwhisk_short}}" title="Configurar el plugin de CLI de {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="openwhisk_about.html" alt="Ver la arquitectura de la plataforma" title="Ver la arquitectura de la plataforma" shape="rect" coords="326, -10, 448, 218" />
</map>


## Creación de una acción en la GUI
{: #openwhisk_start_hello_world}

Para empezar a utilizar {{site.data.keyword.openwhisk_short}}, intente utilizar la plantilla de inicio rápido HelloWorld.

1.  En la categoría **Funciones** del [**Catálogo** de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/?category=whisk), pulse Funciones.

2. Pulse **Empezar a crear** > **Plantillas de inicio rápido** y seleccione la plantilla **Hello World**.

5. Extraiga el código correspondiente a la acción y, a continuación, cree la acción pulsando **Desplegar**. Ahora tiene una acción denominada `hello`.

6. Para ejecutar la acción, pulse **Invocar**. Al invocar una acción se ejecuta manualmente la lógica de la app que define la acción. En el panel **Activaciones**, puede ver el saludo "Hello stranger!" que genera la acción.

Enhorabuena. Ha creado su primera acción. Para eliminar esta acción, pulse el menú de desbordamiento y seleccione **Suprimir acción**.

## Creación de una acción en la CLI
{: #openwhisk_start_hello_world_cli}

Comience a trabajar rápidamente con el código de ejemplo JavaScript de HelloWorld. Este ejemplo se crea una acción `hello` básica que puede invocar manualmente para ejecutar la lógica de la app.

1. [Configure el plugin de CLI de {{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

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

**Qué hacer a continuación**
* [Pruebe el ejemplo de servicio de alarma para invocar la acción **hello** cada vez que se genere un suceso periódico.](./openwhisk_packages.html#openwhisk_package_trigger)
* [Cree la API REST sin servidor.](openwhisk_apigateway.html)
* [Extraiga los paquetes de acción preinstalados correspondientes a servicios de {{site.data.keyword.Bluemix_notm}} como Cloudant.](cloudant_actions.html)
