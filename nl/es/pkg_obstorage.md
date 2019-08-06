---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: object storage, bucket, package, functions

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
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Almacenamiento de objetos
{: #pkg_obstorage}

Puede ampliar la funcionalidad de su app de {{site.data.keyword.openwhisk}} integrándola con una instancia de {{site.data.keyword.cos_full}}.

**Antes de empezar** para obtener información acerca de {{site.data.keyword.cos_full_notm}}, consulte
[Acerca de Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Para obtener más información sobre cómo configurar la instancia de
{{site.data.keyword.cos_full_notm}}, consulte
[Suministro de una instancia de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Paquetes
{: #obstorage_packages}
| Paquete | Disponibilidad | Descripción |
| --- | --- | --- |
| Paquete [{{site.data.keyword.cos_full_notm}}](#pkg_obstorage_install)| Instalable | Leer, escribir y suprimir de una instancia de {{site.data.keyword.cos_full_notm}}. |
| [(Experimental) Origen de sucesos de {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_ev) | Preinstalado (sólo US-South) | Estar a la escucha de cambios en una instancia de {{site.data.keyword.cos_full_notm}}. |

## Creación de una instancia de servicio de IBM Cloud Object Storage
{: #pkg_obstorage_service}

Antes de utilizar cualquiera de los paquetes, debe solicitar una instancia de {{site.data.keyword.cos_full_notm}} y crear al menos un grupo ("bucket").

1. [Cree una instancia de servicio de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Creación de un conjunto de credenciales de servicio HMAC ](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) para la instancia de servicio de {{site.data.keyword.cos_full_notm}}. En el campo **Añadir parámetros de configuración en línea (opcional)**, añada `{"HMAC":true}`.

3. [Cree al menos un grupo](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Instalación del paquete de {{site.data.keyword.cos_full_notm}} desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_install}

Una vez tenga una instancia de servicio de {{site.data.keyword.cos_full_notm}}, puede utilizar la CLI o la interfaz de usuario de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.cos_full_notm}} en su espacio de nombres.
{: shortdesc}
{: #pkg_obstorage_cli}

El paquete instalable de {{site.data.keyword.cos_full_notm}} despliega un conjunto de acciones que puede utilizar para trabajar con la instancia de {{site.data.keyword.cos_full_notm}}. Se pueden ejecutar en Node.js o en Python. Después de instalar el paquete, puede seleccionar un entorno de ejecución. Para obtener una lista de acciones del paquete `cloud-object-storage`, consulte [Entidades disponibles](#pkg_obstorage_actions).
{: note}

**Antes de empezar**

[Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar el paquete {{site.data.keyword.cos_full_notm}}:

1. Clone el repositorio del paquete {{site.data.keyword.cos_full_notm}}.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue hasta el directorio `runtimes/nodejs` o `runtimes/python` para seleccionar un entorno de ejecución para las acciones del paquete.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. Despliegue el paquete `cloud-object-storage`.
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **Respuesta**
    ```
    Success: Deployment completed successfully.
    ```
    {: scree}

5. Verifique que el paquete `cloud-object-storage` se ha añadido a la lista de paquetes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Resultado**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. Enlace a las credenciales de la instancia de {{site.data.keyword.cos_full_notm}} que creó para el paquete. Puede incluir el distintivo `--keyname` para enlazar credenciales de servicio específicas. Para obtener más información sobre cómo enlazar servicios, consulte [Mandatos de servicio](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service).

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.cos_full_notm}}.
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## Parámetros de enlace

Puede utilizar el mandato [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind)para enlazar el punto final de un grupo con una acción específica o con el paquete `cloud-object-storage`. Sustituya `<bucket_endpoint>` por el punto final del grupo.

Cuando actualiza parámetros para un paquete, acción o desencadenante, debe especificar todos los parámetros creados previamente. De lo contrario, los parámetros creados anteriormente se eliminan. Los servicios enlazados al paquete también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios](/docs/openwhisk?topic=cloud-functions-services) al paquete.
{: important}

**Enlazar parámetros a todas las acciones de un paquete**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**Enlazar parámetros a una acción específica**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

También puede enlazar parámetros a acciones utilizando el separador **Parámetros** de la interfaz de usuario. Para añadir parámetros en la interfaz de usuario, vaya a la [página **Acciones**](https://cloud.ibm.com/openwhisk/actions){: external} y pulse en una de las acciones. A continuación, pulse **Parámetros** > **Añadir parámetro**. Debe añadir parámetros en pares de `<key>` y `<value>`.
{: tip}


## Instalación del paquete de {{site.data.keyword.cos_full_notm}} desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a la [página Crear](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleccione el espacio de nombres donde desea instalar el paquete de {{site.data.keyword.cos_full_notm}} utilizando el menú desplegable de espacios de nombres

3. Pulse **Instalar paquetes**.

4. Pulse sobre el grupo de paquetes **IBM Cloud Object Storage** y, a continuación, pulse sobre el paquete **IBM Cloud Object Storage**.

5. En la sección **Entornos de ejecución disponibles**, seleccione `Node.JS` o `Python` en la lista desplegable. A continuación, pulse **Instalar**.

6. Una vez que se ha instalado el paquete, se le redirige a la página **Acciones** donde puede buscar su nuevo paquete, que se denomina `cloud-object-storage`.

7. Para utilizar las acciones del paquete `cloud-object-storage`, debe enlazar las credenciales de servicio a la acción.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga el paso 5 de las [instrucciones de la CLI](#pkg_obstorage_cli).
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. 
  
Si enlaza las credenciales de servicio a acciones individuales, debe completar los pasos siguientes con cada acción que desee utilizar.
{: note}

1. Pulse sobre una acción del paquete `cloud-object-storage` que desee utilizar. Se abrirá la página de detalles de dicha acción.
2. En la navegación de la izquierda, pulse en **Parámetros**.
3. Especifique un nuevo parámetro. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.


Para obtener más información sobre cómo enlazar parámetros, consulte [Enlace de parámetros a paquetes](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params).


## Entidades disponibles
{: #pkg_obstorage_actions}

El paquete {{site.data.keyword.cos_full_notm}} incluye las acciones siguientes:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Paquete | `apikey`, `resource_instance_id`, `cos_hmac_keys.access_key_id`, `cos_hmac_keys.secret_access_key` | Trabajar con una instancia de {{site.data.keyword.cos_full_notm}}. |
| `/cloud-object-storage/object-write` | Acción | `bucket`, `key`, `body`, `endpoint`, `ibmAuthEndpoint` | Escribir un objeto en un grupo. |
| `/cloud-object-storage/object-read` | Acción | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Leer un objeto de un grupo. |
| `/cloud-object-storage/object-delete` | Acción | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Suprimir un objeto de un grupo. |
| `/cloud-object-storage/bucket-cors-put` | Acción | `bucket`, `corsConfig`, `endpoint`, `ibmAuthEndpoint` | Asignar una configuración de CORS a un grupo. |
| `/cloud-object-storage/bucket-cors-get` | Acción | `bucket`, `endpoint`, `ibmAuthEndpoint` | Leer la configuración de CORS de un grupo. |
| `/cloud-object-storage/bucket-cors-delete` | Acción | `bucket`, `endpoint`, `ibmAuthEndpoint` | Suprimir la configuración de CORS de un grupo. |
| `/cloud-object-storage/client-get-signed-url` | Acción | `bucket`, `key`, `operation`, `expires`, `endpoint`, `ibmAuthEndpoint` | Obtener un URL firmado para restringir la lectura, escritura y supresión de un objeto en un grupo. |

Para obtener una lista completa de las entidades disponibles, ejecute `ibmcloud fn package get cloud-object-storage`.
{: note}

### Parámetros de paquete
{: #pkg_obstorage_pkgparams}

Los siguientes parámetros de paquete, se espera que estén enlazados al paquete, y están disponibles automáticamente para todas las acciones. También es posible especificar estos parámetros al invocar una de las acciones.

| Parámetro de paquete | Descripción |
| --- | --- |
| `apikey` | El parámetro `apikey` es la clave de API de IAM para la instancia de {{site.data.keyword.cos_full_notm}}. |
| `resource_instance_id` | El parámetro `resource_instance_id` es el identificador de la instancia de {{site.data.keyword.cos_full_notm}}. |
| `cos_hmac_keys` | El parámetro `cos_hmac_keys` es las credenciales HMAC de la instancia de {{site.data.keyword.cos_full_notm}}, que incluye los valores de `access_key_id` y `secret_access_key`.  Estas credenciales las utiliza exclusivamente la acción `client-get-signed-url`.  Consulte [Utilización de credenciales HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) para obtener instrucciones sobre cómo generar credenciales HMAC para la instancia de {{site.data.keyword.cos_full_notm}}. |
 
### Parámetros de acción
{: #pkg_obstorage_actparams}

Los parámetros de acción siguientes se especifican al invocar las acciones individuales.  No todos estos parámetros se admiten en cada acción. Consulte la tabla [Entidades disponibles](#pkg_obstorage_actions) para ver qué parámetros admite cada acción.

| Parámetro de acción | Descripción |
| --- | --- |
| `bucket` | El parámetro `bucket` es el nombre del grupo de
{{site.data.keyword.cos_full_notm}}. |
| `endpoint` | El parámetro `endpoint` es el punto final de {{site.data.keyword.cos_full_notm}} que se utiliza para conectarse a su instancia de {{site.data.keyword.cos_full_notm}}. Puede localizar el punto final en la [documentación de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `expires` | El parámetro `expires` es el número de segundos para que caduque la operación de URL prefirmado.  El valor predeterminado de `expires` es de 15 minutos. |
| `ibmAuthEndpoint` | El parámetro `ibmAuthEndpoint ` es el punto final de autorización de IBM Cloud que utiliza {site.data.keyword.cos_short}} para generar una señal a partir de `apikey`. El punto final de autorización predeterminado funciona para todas las regiones de IBM Cloud. |
| `key` | El parámetro `key` es la clave de objeto de grupo. |
| `operation` | El parámetro `operation` es la operación que se va a llamar del URL prefirmado. |
| `corsConfig` | El parámetro `corsConfig` es la configuración CORS de un grupo. |


## Escritura de objetos en un grupo
{: #pkg_obstorage_write}

Utilice la acción `object-write` para escribir un objeto en un grupo de {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

En los pasos siguientes, se utiliza el nombre `test-bucket` como ejemplo. Los grupos ("buckets") en {{site.data.keyword.cos_full_notm}} deben ser únicos globalmente, de forma que deberá sustituir `test-bucket` con un nombre de grupo exclusivo.
{: note}

### Escritura de un objeto en un grupo utilizando la CLI
{: #pkg_obstorage_write_cli}
Escriba un objeto en el grupo utilizando la acción `object-write`.
{: shortdesc}


Utilice la acción `object-write` para escribir un objeto en el grupo. Si ha enlazado el punto final de grupo al paquete o a la acción `object-write`, no es necesario que incluya el punto final como un parámetro. Sustituya `<org_space>` por el nombre de Cloud Foundry Org and Space, `<test-bucket>` con el nombre del grupo y `<test.txt>` con el nombre del objeto que desea escribir.

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**Resultado de ejemplo**

```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### Escritura de un objeto en un grupo utilizando la interfaz de usuario
{: #pkg_obstorage_write_ui}


1. Vaya a la [página Acciones](https://cloud.ibm.com/openwhisk/actions){: external} en la consola de {{site.data.keyword.openwhisk_short}}.

2. Bajo el paquete `cloud-object-storage`, pulse la acción **object-write**.

3. En el panel de Código, pulse **Cambiar entrada**.

4. Especifique un objeto JSON que contenga el grupo, la clave y el cuerpo como claves de objeto.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
    }
    ```
    {: pre}

5. Pulse **Guardar**.

6. Pulse **Invocar**.

7. Verifique que la salida es parecida a la siguiente:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## Lectura de objetos de un grupo
{: #pkg_obstorage_read}

Utilice la acción `object-read` para leer un objeto de un grupo de {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

En los pasos siguientes, se utiliza el nombre `test-bucket` como ejemplo. Los grupos ("buckets") en {{site.data.keyword.cos_full_notm}} deben ser únicos globalmente, de forma que deberá sustituir `test-bucket` con un nombre de grupo exclusivo.
{: note}

### Lectura de un objeto de un grupo utilizando la CLI
{: #pkg_obstorage_read_cli}

Lea un objeto en el grupo utilizando la acción `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**Resultado de ejemplo**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### Lectura de un objeto de un grupo utilizando la interfaz de usuario
{: #pkg_obstorage_read_ui}

1. Vaya a la [página Acciones](https://cloud.ibm.com/openwhisk/actions){: external}.

2. Bajo el paquete `cloud-object-storage`, pulse la acción `object-read`.

3. En el panel de Código, pulse **Cambiar entrada**.

4. Especifique un objeto JSON que contenga el grupo y la clave como claves de objeto.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
    }
    ```
    {: pre}

5. Pulse **Guardar**.

6. Pulse **Invocar**.

7. Verifique que la salida es parecida a la siguiente:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}


## Estar a la escucha de cambios en un grupo utilizando el origen de sucesos de Object Storage (Experimental)
{: #pkg_obstorage_ev}

El paquete `/whisk.system/cos-experimental` puede ser inestable, cambiar con frecuencia de manera que no sea compatible con versiones anteriores y podría dejar de mantenerse previo aviso con poca antelación. No se recomienda el uso de este paquete en entornos de producción. Este paquete experimental solo está disponible en la región EE.UU. sur.
{: important}

Puede utilizar {{site.data.keyword.openwhisk}} para escuchar los cambios en un grupo de
[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) y utilizar una acción para procesar uno o más objetos del grupo.

<br>

**Caso de uso de ejemplo:** con el paquete `/whisk.system/cos-experimental`, puede escuchar los cambios en los datos a nivel de calle de GPS almacenados en un grupo de {{site.data.keyword.cos_full_notm}}. A continuación, cuando se produzcan los cambios, puede desencadenar la regeneración automática de un mapa de GPS de forma que los usuarios puedan tener acceso a los datos más recientes a nivel de calle en su aplicación de GPS.

### Parámetros de origen de sucesos de Object Storage (Experimental)
{: #pkg_obstorage_ev_ch}

Con el paquete `/whisk.system/cos-experimental`, puede configurar los sucesos de una instancia de {{site.data.keyword.cos_full_notm}}, e incluye el siguiente canal de información:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Paquete | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Paquete que contiene la acción de canal de información `changes`. |
| `/whisk.system/cos-experimental/changes` | Canal de información | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Activar sucesos desencadenantes sobre los cambios en un grupo de {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

Puede utilizar el canal de información `changes` para configurar el servicio de origen de sucesos de
{{site.data.keyword.cos_full_notm}} para activar un desencadenante para cada cambio que se produzca en un grupo de la instancia de
{{site.data.keyword.cos_full_notm}}.

Parámetros utilizados en este ejemplo:

| Parámetro | Descripción |
| --- | --- |
| `apikey` | (Obligatorio, a menos que esté vinculado al paquete). El parámetro `apikey` es la clave de API de IAM para la instancia de {{site.data.keyword.cos_full_notm}}.  Normalmente, este valor está vinculado al paquete. No obstante, si se especifica el valor de `apikey` al utilizar la acción de canal de información
`changes`, se utiliza el valor especificado para las credenciales en lugar del valor de apikey de las credenciales vinculadas. |
| ` auth_endpoint` | (Opcional). El parámetro `auth_endpoint` es el punto final de autorización que utiliza {{site.data.keyword.cos_full_notm}} para generar una señal de `apikey`.  El punto final predeterminado es el punto final de {{site.data.keyword.cloud}}. |
| `bucket` | (Obligatorio). El parámetro `bucket` es el nombre del grupo de
{{site.data.keyword.cos_full_notm}}. |
| `endpoint` | (Obligatorio). El parámetro `endpoint` es el punto final de
{{site.data.keyword.cos_full_notm}} utilizado para conectarse a su instancia de {{site.data.keyword.cos_full_notm}}. Puede localizar el punto final en la [documentación de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `interval` | (Opcional). El parámetro `interval` es el intervalo de sondeo de grupo, en minutos completos. El valor de `interval` debe ser de al menos 1 minuto y está establecido en 1 minuto de forma predeterminada. |

## Creación de un desencadenante para responder al canal de información de cambios (changes)
{: #pkg_obstorage_ev_trig}

Al crear el desencadenante, puede evitar el paso de sus credenciales de {{site.data.keyword.cos_full_notm}} a la acción del canal de información `changes` enlazando sus credenciales directamente al paquete `cos-experimental`.
{: shortdesc}

 1. En primer lugar, cree un enlace de paquete que se pueda modificar para incluir sus credenciales. A continuación, se crea un enlace de paquete, `myCosPkg`, en el espacio de nombres.

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. Enlace las credenciales de {{site.data.keyword.cos_full_notm}} al paquete. Al enlazar las credenciales de {{site.data.keyword.cos_full_notm}} al paquete, se enlaza el valor de `apikey` al paquete, por lo que no es necesario especificar el valor de `apikey` al invocar la acción del canal de información `changes`.

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. Cree un desencadenante denominado `cosTrigger` con el canal de información `changes` en el enlace de paquete que ha creado. Utilice su nombre de grupo y los valores de parámetros de punto final de {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **Resultado de ejemplo**

    ```
    ok: created trigger feed cosTrigger
    ```
  {: pre}
 

Cree una acción para verificar que el desencadenante, el canal de información de cambios y la regla están todos configurados y funcionando correctamente.
 
  1. Guarde el siguiente código JavaScript como `cosChange.js`. 

  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}

  2. Cree una acción denominada `cosChange` utilizando el código `cosChange.js`.

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

  3. Cree una regla para conectar la acción `cosChange` con el desencadenante `cosTrigger`.

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

  4. En una ventana aparte, empiece a sondear para ver si hay activaciones a fin de comprender con claridad lo que está sucediendo. Cuando se activa el desencadenante y se ejecuta la acción, este mandato lista los registros de activación para cada una de estas operaciones a medida que se producen.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
  5. En el panel de control de {{site.data.keyword.cos_full_notm}}, modifique un objeto de grupo existente o cree uno. Para aprender a añadir un objeto al grupo, consulte [Añadir algunos objetos al grupo](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).
  
  6. Para cada cambio en los objetos del grupo, observe las nuevas activaciones del desencadenante `cosTrigger` y la acción `cosChange`. Puede ver estas activaciones ejecutando el mandato `ibmcloud fn activation poll` en el intervalo de sondeo de grupo configurado.
  
  7. Si no puede detectar las nuevas activaciones, verifique que los valores de los parámetros
`apikey`, `endpoint` y `bucket` sean correctos.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}


### Estructura de datos de una activación de desencadenante de Object Storage
{: #pkg_obstorage_ev_data}

El contenido de los sucesos generados tiene los siguientes parámetros:

| Parámetro | Descripción |
| --- | --- |
| `file` | El archivo o los metadatos del objeto. Esta estructura se describe en [Listar objetos de un grupo específico](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets). |
| `status` | El cambio detectado.  Este valor es `added`, `modified` o
`deleted`. |
| `bucket`| El nombre del grupo de {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | El punto final de {{site.data.keyword.cos_full_notm}} utilizado para conectarse a la instancia de {{site.data.keyword.cos_full_notm}}. |
| `key` | El identificador del objeto de grupo que ha cambiado. Este valor es el mismo que
`file.Key`, pero está disponible en la parte superior del JSON del suceso desencadenante. |

**Representación JSON de ejemplo de la activación de desencadenante de cambio de grupo**

```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Creación de una acción para procesar un objeto modificado
{: #pkg_obstorage_ev_act}

Puede crear una acción individual que recupere y procese el objeto. O bien, puede crear una secuencia que utilice una acción para recuperar el objeto y otra acción para procesar el objeto.

### Creación de una acción para recuperar y procesar el objeto
{: #pkg_obstorage_ev_act_ret}

Este código de acción de ejemplo recupera y procesa el documento de notificación de cambios del grupo. Puede pasar los parámetros
`apikey` y `serviceInstanceId` directamente a la acción durante la invocación manual de la acción, pero cuando esta acción la invoca un desencadenante estos valores se deben obtener de
{{site.data.keyword.cos_full_notm}}, que debe enlazarse a la acción con el mandato `ibmcloud fn service bind`.

1. Guarde el código siguiente en un archivo .zip denominado `myCosAction.zip`.

  ```javascript
  const COS = require('ibm-cos-sdk')

  function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

    const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

    return new Promise(function(resolve, reject) {
      client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
        if (err != null) {
          console.log(err)
        reject({ err: err })
        } else {
          console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
      })
    });
}
exports.main = main;
  ```
  {: codeblock}

  Debido a que esta acción utiliza el paquete NPM `ibm-cos-sdk`, la acción se debe empaquetar como un
[módulo Node.js](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) o un
[paquete individual](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

2. Cree la acción para recuperar y procesar el objeto de {{site.data.keyword.cos_full_notm}}:

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### Creación de una secuencia de acciones para recuperar y procesar el objeto
{: #pkg_obstorage_ev_act_seq}

En lugar de incluir el código de recuperación del objeto en la acción, puede utilizar la acción `object-read` del paquete `cloud-object-storage`, que se debe [instalar manualmente](#pkg_obstorage_install).  El código de la acción solo tiene que procesar los resultados devueltos de `object-read`.
{: shortdesc}

Para crear una acción que solo procese el objeto del grupo:

1. Guarde el siguiente código como `myCosAction.js`.

  ```javascript
  function main(data) {
    if (data) {
      // Process the object
  }
  }
  ```
  {: codeblock}

2. Cree la acción para procesar únicamente el objeto de {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. Enlace las credenciales de {{site.data.keyword.cos_full_notm}} al paquete de `cloud-object-storage` que ha instalado manualmente.

  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}

4. La acción `object-read` puede estar compuesta por `myCosProcessObjectAction` para crear una secuencia de acciones.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

Además de la acción `object-read`, puede utilizar otras acciones que se incluyen en el paquete instalable de `cloud-object-storage`.
{: tip}

Para obtener una lista de las entidades disponibles, ejecute el mandato siguiente.
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### Enlace de credenciales a la acción
{: #pkg_obstorage_ev_bind}

Puede evitar el paso de credenciales confidenciales durante la invocación enlazando las credenciales de
{{site.data.keyword.cos_full_notm}} a la acción utilizando el mandato siguiente:
  ```
  ibmcloud fn service bind cloud-object-storage myCosAction
  ```
  {: pre}

### Creación de una regla para asociar la acción con el desencadenante de cambio
{: #pkg_obstorage_ev_rule}

Puede utilizar una acción o secuencia de acciones en una
[regla](/docs/openwhisk?topic=cloud-functions-rules) para captar y procesar el objeto asociado a un suceso de cambio de {{site.data.keyword.cos_full_notm}}.

Cree una regla que active la acción `MyCosAction` en nuevos sucesos desencadenantes de {{site.data.keyword.cos_full_notm}}.
  ```
  ibmcloud fn rule create myRule myCosTrigger myCosAction
  ```
  {: pre}



