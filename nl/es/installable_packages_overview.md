---

copyright:
  years: 2018
lastupdated: "2018-07-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}

# Utilización de paquetes instalables
{: #installable-packages-overview}

Los paquetes instalables representan una nueva forma de gestionar e interactuar con los paquetes dentro de {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Visión general
{: #overview}

**Qué son los paquetes instalables**

Los paquetes son conjuntos empaquetados de canales de información y acciones relacionados. Cada paquete se ha diseñado para una interacción específica con servicios y proveedores de sucesos. Los paquetes instalables están disponibles para que los elija, instale y edite en función de sus necesidades.

**En qué se diferencian los paquetes instalables de los paquetes preinstalados**

[Los paquetes preinstalados](openwhisk_packages.html#browse-packages) se registran de forma automática en {{site.data.keyword.openwhisk_short}} en el espacio de nombres `/whisk.system`. Para almacenar credenciales u otros parámetros en un paquete preinstalado, debe crear [enlaces de paquete enlaces de paquete](openwhisk_packages.html#openwhisk_package_bind).

Los paquetes instalables no residen en el sistema {{site.data.keyword.openwhisk_short}}. En su lugar, los paquetes instalables se alojan externamente en repositorios Github individuales. Estos paquetes se pueden instalar directamente en su propio espacio de nombres utilizando la herramienta [wskDeploy](https://github.com/apache/incubator-openwhisk-wskdeploy#whisk-deploy-wskdeploy) y darles un nombre personalizado. Puesto que el paquete se instala en su propio espacio de nombres, puede modificar las acciones y los canales de información en el paquete según sea necesario.

## Instalación de paquetes utilizando wskDeploy
{: #installing}

Antes de empezar:
  1. [Instale el plugin {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Instale el [mandato `wskdeploy` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) y añada el binario descargado a su PATH.

Para instalar un paquete:

1. Clone el repositorio del paquete. Los repositorios de los paquetes se encuentran en páginas individuales para cada paquete en este conjunto de documentación.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. Vaya al directorio del paquete que contiene un archivo `manifest.yaml` o `manifest.yml`. El archivo `manifest.yaml` especifica la estructura general del paquete, incluido el paquete y las acciones a instalar en su espacio de nombres y todos lo metadatos que se deben incluir con el mandato `wskdeploy`. Para obtener más información sobre los archivos `manifest.yaml`, consulte la [documentación de wskdeploy ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Despliegue el paquete. Algunos paquetes precisan de determinadas variables de entorno para funcionar correctamente.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

### Ejemplo de utilización del paquete {{site.data.keyword.cos_full_notm}}
{: #example}

Para ver un ejemplo de cómo instalar un paquete, consulte el [paquete {{site.data.keyword.cos_short}}](cloud_object_storage_actions.html). {{site.data.keyword.cos_full}} es un servicio que permite a los usuarios almacenar todo tipo de archivos como, por ejemplo, imágenes, vídeos, música y texto. Para interactuar con los archivos, se almacena un almacén de datos basado en la nube de pares clave/valor en un depósito. Por lo tanto, para utilizar el [paquete de {{site.data.keyword.cos_short}}](cloud_object_storage_actions.html), primero debe crear una instancia de servicio de {{site.data.keyword.cos_short}} y, a continuación, crear un depósito. El depósito se utiliza como una variable de entorno que es necesaria para instalar este paquete.

Después de crear la instancia de servicio y el depósito, se necesitan ejecutar los siguientes mandatos para instalar el paquete:

1. Clone el repositorio del paquete.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Vaya al directorio de paquetes que contiene un archivo `manifest.yaml`. En este ejemplo, se utiliza la versión de tiempo de ejecución de Node.js del paquete {{site.data.keyword.cos_short}}.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Despliegue el paquete, utilizando su depósito como una variable de entorno. La dependencia de la variable de entorno `PACKAGE_NAME` permite dar a este paquete un nombre personalizado.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> wskdeploy
    ```
    {: pre}
