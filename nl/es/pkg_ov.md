---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Incorporación de paquetes
{: #pkg_ov}

Los paquetes son conjuntos empaquetados de canales de información y acciones relacionados. Cada paquete se ha diseñado para una interacción específica con servicios y proveedores de sucesos. Algunos paquetes ya vienen instalados con {{site.data.keyword.openwhisk}} y puede utilizarlos, pero también puede instalar otros.
{: shortdesc}

## Visión general
{: #pkg_overview}

[Los paquetes preinstalados](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse) se registran de forma automática en {{site.data.keyword.openwhisk_short}} en el espacio de nombres `/whisk.system`. Puede utilizarlos sin necesidad de realizar ninguna instalación.

Los paquetes instalables están a su disposición para instalarlos, editarlos y utilizarlos según sus necesidades. Los paquetes instalables no residen en el sistema {{site.data.keyword.openwhisk_short}}. En su lugar, los paquetes instalables se alojan externamente en repositorios GitHub individuales.

Puede instalar estos paquetes usted mismo directamente en su espacio de nombres y poner a cada paquete un nombre personalizado. Puesto que el paquete se instala en su propio espacio de nombres, puede modificar las acciones y los canales de información en el paquete según sea necesario.



## Examinar los paquetes preinstalados
{: #pkg_browse}

Hay varios paquetes que ya vienen registrados en {{site.data.keyword.openwhisk_short}}. Puede obtener una lista de paquetes de un espacio de nombres, listar las entidades de un paquete y obtener una descripción de las entidades individuales que hay en un paquete.
{: shortdesc}

1. Obtener una lista de paquetes en el espacio de nombres `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Salida de lista de paquetes:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Obtener una lista de las entidades de un paquete.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Ejemplo**
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Esta salida muestra que el paquete {{site.data.keyword.cloudant_short_notm}} incluye acciones y un canal de información. Por ejemplo, dos acciones, `read` y `write`, y un canal de información de desencadenantes denominado `changes`. El canal de información `changes` provoca que los desencadenantes se activen cuando se añaden documentos a la base de datos {{site.data.keyword.cloudant_short_notm}} especificada.

  El paquete {{site.data.keyword.cloudant_short_notm}} también define los parámetros `username`, `password`, `host` y `port`. Estos parámetros se deben especificar para que las acciones y los canales de información tengan sentido. Los parámetros permiten que las acciones funcionen en una cuenta {{site.data.keyword.cloudant_short_notm}} específica, por ejemplo.

3. Obtener una descripción de una acción o un canal de información para ver los parámetros que necesita.

  **Ejemplo**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Esta salida muestra que la acción `read` de {{site.data.keyword.cloudant_short_notm}} precisa de tres parámetros, incluyendo la base de datos y
el ID de documento a recuperar.



## Enlazar parámetros a paquetes preinstalados
{: #pkg_bind}

Aunque puede utilizar las entidades en un paquete directamente, es posible que acabe pasando siempre los mismos parámetros a la acción. Puede simplificar el proceso enlazando a un paquete y especificando parámetros predeterminados, que las acciones del paquete heredan.
{: shortdesc}

Por ejemplo, en el paquete `/whisk.system/cloudant`, puede establecer los valores
`username`, `password` y `dbname` predeterminados en un enlace de paquete, y dichos valores
se pasan automáticamente a cualquier acción del paquete.

En el ejemplo siguiente, se enlaza al paquete `/whisk.system/samples`.

1. Enlazar al paquete `/whisk.system/samples` y establecer un valor de parámetro `place` predeterminado.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Obtener una descripción del enlace de paquete.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Tenga en cuenta que todas las acciones del paquete `/whisk.system/samples` están disponibles en el enlace del
paquete `valhallaSamples`.

3. Invocar una acción en el enlace de paquete.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Observe en el resultado que la acción hereda el parámetro `place` que ha establecido cuando ha creado el enlace del paquete `valhallaSamples`.

4. Invoque una acción y sobrescriba el valor de parámetro predeterminado.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Fíjese que el valor del parámetro `place` especificado con la invocación de la acción sobrescribe
el valor predeterminado establecido en el enlace del paquete `valhallaSamples`.



## Añadir sus propios paquetes
{: #pkg_add}

Puede crear un paquete de código local o un clon de cualquier repositorio de GitHub.
{: shortdesc}

**Antes de empezar**
- [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).
- Cree un archivo `manifest.yaml` o `manifest.yml` para su app y guárdelo en el directorio raíz. El archivo `manifest.yaml` especifica la estructura general del paquete, incluyendo todos lo metadatos que se deben incluir con el mandato `ibmcloud fn deploy`. Para obtener más información sobre los archivos `manifest.yaml`, consulte la [documentación de <ph class="ignoreSpelling">wskdeploy</ph>](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}.

Para añadir un paquete:

1. Clone el repositorio del paquete.
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Vaya al directorio que contiene el archivo `manifest.yaml`.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Algunos paquetes precisan de determinadas variables de entorno para funcionar correctamente. Si es el caso, incluya las variables de entorno con el mandato `deploy`. Por ejemplo, puede elegir un nombre para el paquete y especificarlo con la variable PACKAGE_NAME.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Ejemplo de paquete de {{site.data.keyword.cos_full_notm}}
{: #pkg_ex}

Para ver un ejemplo de cómo instalar un paquete, consulte el [paquete {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage). {{site.data.keyword.cos_full}} es un servicio que permite a los usuarios almacenar todo tipo de archivos como, por ejemplo, imágenes, vídeos, música y texto. Para interactuar con los archivos, se almacena un almacén de datos basado en la nube de pares clave/valor en un grupo. Por lo tanto, para utilizar el [paquete {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage), primero debe crear una instancia de servicio de {{site.data.keyword.cos_full_notm}} y, a continuación, crear un grupo. El grupo se utiliza como una variable de entorno que es necesaria para instalar este paquete.

Después de crear la instancia de servicio y el grupo, puede instalar el paquete utilizando los mandatos siguientes:

1. Clone el repositorio del paquete.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Vaya al directorio de paquetes que contiene un archivo `manifest.yaml`. En este ejemplo, se utiliza la versión de tiempo de ejecución de Node.js del paquete {{site.data.keyword.cos_full_notm}}.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Despliegue el paquete, utilizando su grupo como una variable de entorno.  Puede dar al paquete un nombre personalizado utilizando la variable de entorno `PACKAGE_NAME`.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



