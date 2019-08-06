---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions, ibm cloud functions, cloud functions, getting started, creating actions

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


# Cómo empezar con IBM {{site.data.keyword.openwhisk_short}}
{: #getting-started}

Con {{site.data.keyword.openwhisk}}, puede utilizar su lenguaje de programación favorito para escribir código reducido que ejecute la lógica de la app de manera escalable. Puede ejecutar código a demanda con solicitudes de API basadas en HTTP o ejecutar código en respuesta a servicios
{{site.data.keyword.cloud_notm}} y sucesos de terceros. La plataforma de programación de función como servicio (Faas) se basa en el proyecto de código abierto Apache OpenWhisk.
{: shortdesc}

## Cómo trabajar con acciones
{: #gs_actions}

Con {{site.data.keyword.openwhisk}}, puede crear fragmentos de código sin estado definidos para realizar una tarea específica conocida como acción. Para obtener más información sobre las acciones y otros términos relativos a funciones, consulte la [terminología](/docs/openwhisk?topic=cloud-functions-about).
{:shortdesc}

Pulse en una opción para empezar.

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Pulse un icono para empezar rápidamente con {{site.data.keyword.openwhisk_short}}." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="Crear una acción" title="Crear una acción" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="Configuración del plug-in de la CLI de {{site.data.keyword.openwhisk_short}}" title="Configuración del plug-in de la CLI de {{site.data.keyword.openwhisk_short}}" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="Ver la arquitectura de la plataforma" title="Ver la arquitectura de la plataforma" shape="rect" coords="326, -10, 448, 218" />
</map>

## Creación de una acción en la GUI
{: #gs_hello_world}

Para empezar a utilizar {{site.data.keyword.openwhisk_short}}, pruebe a crear la plantilla de inicio rápido Hello World.

1. Cree una cuenta de [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/registration){: external} o inicie sesión en una cuenta existente.

2. Vaya al [panel de control de {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}.

2. Pulse **Empezar a crear** > **Plantillas de inicio rápido** y seleccione la plantilla **Hello World**.

3. Cree un paquete para las acciones especificando un nombre exclusivo en el campo **Nombre de paquete**.

4. Seleccione un entorno de ejecución en el menú desplegable de la sección **Acción helloworld**. Puede obtener una vista previa del código de la acción de ejemplo en cada entorno de ejecución disponible antes de desplegar la plantilla.

5. Pulse **Desplegar**. Ha creado una acción. ¡Buen trabajo!

6. Para ejecutar la acción, pulse **Invocar**. Al invocar una acción se ejecuta manualmente la lógica de la app que define la acción. En el panel **Activaciones**, puede ver el saludo "Hello stranger!" que genera la acción.

7. Opcional: pulse **Cambiar entrada** para cambiar la acción o probar una propia.

  a. Pegue el código siguiente en el recuadro **Cambiar entrada** y cambie el valor del nombre.
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. A continuación, pulse
**Invocar** para ejecutar la acción con sus actualizaciones. El resultado aparece en el panel
**Activaciones**. Puede repetir este proceso tantas veces como quiera.

Enhorabuena. Ha creado su primera acción. Para eliminar esta acción, pulse el menú de desbordamiento y seleccione **Suprimir acción**.

## Creación de una acción en la CLI
{: #gs_hello_world_cli}

Comience a trabajar rápidamente con el código de ejemplo [Hello World JavaScript](/docs/openwhisk?topic=cloud-functions-prep#prep-js). En este ejemplo se crea una acción `hello` básica, que se puede invocar manualmente para ejecutar la lógica de la app.

## ¿Qué hacer a continuación?
{: #gs_next_steps}

Así pues, ya ha completado su primer despliegue de plantilla, ¿qué puede hacer a partir de aquí?

* Familiarizarse con la [terminología](/docs/openwhisk?topic=cloud-functions-about#about_technology).
* Empezar con [sus propias acciones](/docs/openwhisk?topic=cloud-functions-actions).
* Aprender a organizar acciones en [paquetes](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Opción avanzada - Crear una [API REST sin servidor](/docs/openwhisk?topic=cloud-functions-apigateway).



