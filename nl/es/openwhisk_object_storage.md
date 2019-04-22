---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (Experimental) Origen de sucesos de Object Storage
{: #cloud_object_storage}

El paquete `/whisk.system/cos-experimental` puede ser inestable, cambiar con frecuencia de manera que no sea compatible con versiones anteriores y podría dejar de mantenerse previo aviso con poca antelación. No se recomienda el uso de este paquete en entornos de producción. Este paquete experimental solo está disponible actualmente en la región EE.UU. sur.
{: important}

En este ejemplo, aprenderá a: 
* [Escuchar cambios](#listening_to_cos_bucket_changes) en una instancia de {{site.data.keyword.cos_full}}.
* [Crear un desencadenante](#creating_a_trigger_cos) para responder a dichos cambios.
* [Crear acciones](#creating_action_to_process_object) para recuperar y procesar los cambios.
* [Crear una regla](#associating_action_with_change_trigger) para asociar la acción al desencadenante de cambios.
<br>

**Caso de uso de ejemplo:** con el paquete `/whisk.system/cos-experimental`, puede escuchar los cambios en los datos a nivel de calle de GPS almacenados en un grupo de {{site.data.keyword.cos_full_notm}}. A continuación, cuando se produzcan los cambios, puede desencadenar la regeneración automática de un mapa de GPS, de manera que los usuarios puedan tener acceso a los datos más recientes a nivel de calle en su aplicación de GPS.

## Acerca de IBM Cloud Object Storage
{: #cloud_object_storage_info}

**Antes de empezar:** para obtener información acerca de {{site.data.keyword.cos_full_notm}}, consulte
[Acerca de Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage). Para obtener más información sobre cómo configurar la instancia de
{{site.data.keyword.cos_full_notm}}, consulte
[Suministro de una instancia de {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage).

## Escucha de cambios en un grupo de IBM Cloud Object Storage
{: #listening_to_cos_bucket_changes}

Puede utilizar {{site.data.keyword.openwhisk}} para escuchar los cambios en un grupo de
[{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) y utilizar una acción para procesar uno o más objetos del grupo. 

El paquete `/whisk.system/cos-experimental` le permite configurar sucesos de una instancia de
{{site.data.keyword.cos_full_notm}} e incluye el canal de información siguiente:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | paquete | apikey, auth_endpoint, bucket, endpoint, interval | Paquete que contiene la acción de canal de información `changes`. |
| `/whisk.system/cos-experimental/changes` | canal de información | apikey, auth_endpoint, bucket, endpoint, interval | Activar sucesos desencadenantes sobre los cambios en un grupo de {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

Puede utilizar el canal de información `changes` para configurar el servicio de origen de sucesos de
{{site.data.keyword.cos_full_notm}} para activar un desencadenante para cada cambio que se produzca en un grupo de la instancia de
{{site.data.keyword.cos_full_notm}}.

Parámetros utilizados en este ejemplo:

**apikey**: _(Necesario, a menos que esté vinculado al paquete)_. El parámetro `apikey` es la clave de API de IAM para la instancia de {{site.data.keyword.cos_full_notm}}.  Normalmente, este valor está vinculado al paquete. No obstante, si se especifica el valor de `apikey` al utilizar la acción de canal de información
`changes`, se utiliza el valor especificado para las credenciales en lugar del valor de apikey de las credenciales vinculadas.

**auth_endpoint**: _(Opcional)_. El parámetro `auth_endpoint` es el punto final de autorización utilizado por {{site.data.keyword.cos_full_notm}} para generar una señal a partir de `apikey`.  El punto final predeterminado es el punto final de {{site.data.keyword.Bluemix}}.

**bucket**: _(Necesario)_. El parámetro `bucket` es el nombre del grupo de
{{site.data.keyword.cos_full_notm}}.

**endpoint**: _(Necesario)_. El parámetro `endpoint` es el punto final de
{{site.data.keyword.cos_full_notm}} utilizado para conectarse a su instancia de {{site.data.keyword.cos_full_notm}}. Puede localizar el punto final en la [documentación de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints).

**interval**: _(Opcional)_. El parámetro `interval` es el intervalo de sondeo de grupo, en minutos completos. El valor de `interval` debe ser de al menos 1 minuto y está establecido en 1 minuto de forma predeterminada.

## Creación de un desencadenante para responder al canal de información de cambios (changes)
{: #creating_a_trigger_cos}

Al crear el desencadenante, puede evitar el paso de sus credenciales de {{site.data.keyword.cos_full_notm}} a la acción del canal de información `changes` enlazando sus credenciales directamente al paquete `cos-experimental`.
 {: shortdesc}
 
 1. En primer lugar, cree un enlace de paquete que se pueda modificar para incluir sus credenciales. A continuación, se crea un enlace de paquete, `myCosPkg`, en el espacio de nombres.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Enlace las credenciales de {{site.data.keyword.cos_short}} al paquete. 
 Al enlazar las credenciales de {{site.data.keyword.cos_short}} al paquete, se enlazará el valor de `apikey` al paquete, por lo que no necesitará especificar el valor de `apikey` al invocar la acción del canal de información `changes`. 
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Cree un desencadenante denominado `myCosTrigger` con el canal de información `changes` en el enlace de paquete que ha creado. Utilice su nombre de grupo y los valores de parámetros de punto final de {{site.data.keyword.cos_short}}.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
    Salida de ejemplo:
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. Empiece a sondear para ver si hay activaciones a fin de comprender con claridad lo que está sucediendo.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. Cree una acción para observar el canal de información de cambios. Por ejemplo, una acción denominada `showCosChange` que contiene el código JavaScript siguiente:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
 6. Cree una regla para conectar la acción `showCosChange` con el desencadenante `myCosTrigger`:
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. En el panel de control de {{site.data.keyword.cos_short}}, modifique un objeto de grupo existente o cree uno. Para aprender a añadir un objeto al grupo, consulte [Añadir algunos objetos al grupo](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects).
 
 8. Para cada cambio en los objetos del grupo, observe las nuevas activaciones del desencadenante `myCosTrigger` y la acción `showCosChange`. Estas activaciones aparecen dentro del intervalo de sondeo de grupo configurado.

Si no puede detectar las nuevas activaciones, verifique que los valores de los parámetros
`apikey`, `endpoint` y `bucket` sean correctos.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Estructura de los datos de un suceso desencadenante
{: #data_structure_trigger_event}

El contenido de los sucesos generados tiene los siguientes parámetros:

  - `file`: el archivo o metadatos de objeto.
  - `status`: el cambio detectado.  Este valor es `added`, `modified` o
`deleted`.
  - `bucket`: el nombre del grupo de {{site.data.keyword.cos_short}}.
  - `endpoint`: el punto final de {{site.data.keyword.cos_short}} utilizado para conectarse a la instancia de
{{site.data.keyword.cos_short}}.
  - `key`: el identificador del objeto de grupo que ha cambiado. Este valor es el mismo que
`file.Key`, pero está disponible en la parte superior del JSON del suceso desencadenante.

Representación JSON de ejemplo del suceso desencadenante de cambios del grupo:
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

## Creación de una acción para procesar el objeto modificado
{: #creating_action_to_process_object}

Puede crear una acción individual que recupere y procese el objeto. O bien, puede crear una secuencia que utilice una acción para recuperar el objeto y otra acción para procesar el objeto.

### Creación de una acción para recuperar y procesar el objeto
{: #creating_action_to_retrieve_object}

Este código de acción de ejemplo recupera y procesa el documento de notificación de cambios del grupo. Puede pasar los parámetros
`apikey` y `serviceInstanceId` directamente a la acción durante la invocación manual de la acción, pero cuando esta acción la invoque un desencadenante estos valores se deben obtener de
{{site.data.keyword.cos_short}}, que debe enlazarse a la acción con el mandato `ibmcloud fn service bind`.

Código de ejemplo:

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
```
{: codeblock}

Debido a que esta acción utiliza el paquete npm `ibm-cos-sdk`, la acción se debe empaquetar como un
[módulo Node.js](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action) o un
[paquete individual](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action).

Tras empaquetar esta acción en un archivo .zip, cree la acción para recuperar y procesar el objeto desde {{site.data.keyword.cos_short}}:

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

[Enlace](#cos_binding_credentials_to_action) las credenciales de {{site.data.keyword.cos_short}} a esta acción. A continuación, [cree una regla](#associating_action_with_change_trigger) para invocar esta acción cuando se active el desencadenante.

### Creación de una secuencia de acciones para recuperar y procesar el objeto

En lugar de incluir el código de recuperación del objeto en la acción, puede utilizar la acción `object-read` del paquete
{{site.data.keyword.cos_short}}, que se debe [instalar manualmente](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation).  El código de la acción solo tendría que procesar los resultados devueltos por `object-read`.

Código de ejemplo de una acción que solo procesa el objeto del grupo:
```javascript
function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. Cree la acción para procesar únicamente el objeto de {{site.data.keyword.cos_short}}:
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. Enlace las credenciales de {{site.data.keyword.cos_short}} con el enlace de paquete `cos-experimental`.
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. La acción `object-read` puede estar compuesta por `myCosProcessObjectAction` para crear una secuencia de acciones.
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

Además de la acción `object-read`, puede utilizar otras acciones incluidas en el paquete instalable de
{{site.data.keyword.cos_short}}.

[Enlace](#cos_binding_credentials_to_action) las credenciales de {{site.data.keyword.cos_short}} a esta acción. A continuación, [cree una regla](#associating_action_with_change_trigger) para invocar esta acción cuando se active el desencadenante.

 ## Enlace de credenciales a la acción
 {: #cos_binding_credentials_to_action}
 
 Puede evitar el paso de credenciales confidenciales durante la invocación enlazando las credenciales de
{{site.data.keyword.cos_short}} a la acción con el mandato siguiente:
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## Creación de una regla para asociar la acción con el desencadenante de cambio
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

Puede utilizar una acción o secuencia de acciones en una
[regla](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use) para captar y procesar el objeto asociado a un suceso de cambio de {{site.data.keyword.cos_short}}.

Cree una regla que active la acción `MyCosAction` en nuevos sucesos desencadenantes de {{site.data.keyword.cos_short}}.
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



