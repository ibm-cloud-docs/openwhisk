---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

Con el paquete preinstalado `/whisk.system/cloudant` puede trabajar con una base de datos [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). No es necesario ningún enlace de servicio para utilizar este paquete.
{: shortdesc}


## Entidades disponibles
{: #cloudant_available}
En la tabla siguiente se muestra una selección de las entidades disponibles en el paquete `whisk.system/cloudant`. Puede utilizar el paquete `whisk.system/cloudant` para leer, escribir, actualizar o suprimir documentos. También puede utilizar el canal de información `changes` para estar a la escucha de cambios en una base de datos de {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

Para obtener una lista completa de las entidades que están disponibles en el paquete `/whisk.system/cloudant`, ejecute `ibmcloud fn package get /whisk.system/cloudant`.
{: note}

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Paquete | `dbname`, `host`, `username`, `password` | Trabajar con una base de datos Cloudant. |
| `/whisk.system/cloudant/read` | Acción | `dbname`, `id` | Leer un documento de la base de datos. |
| `/whisk.system/cloudant/write` | Acción | `dbname`, `overwrite`, `doc` | Escribir un documento en la base de datos. |
| `/whisk.system/cloudant/update-document` | Acción | `dbname`, `doc` | Actualizar un documento de la base de datos. |
| `/whisk.system/cloudant/changes` | Canal de información | `dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | Activar sucesos desencadenantes para cambios en una base de datos. |

El parámetro `includeDoc` ya no está soportado para su uso con el canal de información `/whisk.system/cloudant/changes`. Si ha creado desencadenantes que utilizan este parámetro, debe volver a crearlos sin el parámetro `includeDoc`.
{: deprecated}

## Enlazar el paquete `/whisk.system/cloudant` a la base de datos de {{site.data.keyword.cloudant_short_notm}}.
Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.cloud_notm}} puede utilizar el plugin de CLI de {{site.data.keyword.openwhisk}} para enlazar un servicio a una acción o paquete.
{: #cloudant_db}

**Antes de empezar**
Debe tener una instancia de {{site.data.keyword.cloudant_short_notm}}. Para crear una instancia, consulte [Iniciación a {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started).

1. Cree un enlace de paquete de `/whisk.system/cloudant` que esté configurado para su cuenta de {{site.data.keyword.cloudant_short_notm}}. En este ejemplo, el nombre de paquete es `myCloudant`.

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Comprobar que el enlace de paquete existe.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. Obtenga el nombre de la instancia de servicio que desea enlazar a una acción o paquete.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. Obtenga el nombre de las credenciales definidas para la instancia de servicio que ha obtenido en el paso anterior.

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Enlace el servicio al paquete que ha creado en el paso uno.

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verifique que las credenciales se han enlazado correctamente.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "serviceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

En este ejemplo, las credenciales del servicio {{site.data.keyword.cloudant_short_notm}} pertenecen a un parámetro denominado `__bx_creds`.

## Cómo trabajar con documentos en una base de datos de {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Puede utilizar una acción para leer, escribir, actualizar, suprimir un documento de una base de datos de {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

### Leer un documento
Puede utilizar la acción `/whisk.system/cloudant/read` para leer un documento de la base de datos de {{site.data.keyword.cloudant_short_notm}}.

**Antes de empezar**
Si no tiene un documento en la base de datos de {{site.data.keyword.cloudant_short_notm}}, puede crearlo utilizando el panel de control [{{site.data.keyword.cloudant_short_notm}}. El URL del panel de control es `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`.

Captar un documento utilizando la acción `read`. Sustituya `/_/myCloudant` por el nombre del paquete, `<database_name>` por el nombre de la base de datos, y `<document_id>` por el ID de archivo. Invoque la acción para probar la captación de un documento.

**Sintaxis del mandato**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**Ejemplo de acción read desde una base de datos de `test`**
Invoque la acción para hacer una prueba de lectura de un archivo. En este ejemplo se lee un archivo con el `id` `9f86f4955e7a38ab0169462e6ac0f476` que es un archivo vacío.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**Resultado de ejemplo**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### Escribir un documento en una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

Puede utilizar una acción para crear o actualizar documentos en una base de datos de {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

**Antes de empezar**
  Cree un  [enlace de paquete](#cloudant_db) `/whisk.system/cloudant` que esté configurado para la cuenta {{site.data.keyword.cloudant_short_notm}}.

1. Almacenar un documento usando la acción `write` en el enlace de paquete que ha creado. Sustituya `/_/myCloudant` por el nombre del paquete, sustituya `<database_name>` por el nombre de la base de datos, `<document_id>` por el ID de documento y `<test_name>` por un nombre.

  **Sintaxis del mandato**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **Ejemplo de acción write en una base de datos de `test`**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verifique que el documento existe en el panel de control de {{site.data.keyword.cloudant_short_notm}}. El URL de panel de control para la base de datos `test` tiene el formato siguiente: `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`.

  **Documento de ejemplo en el panel de control de {{site.data.keyword.cloudant_short_notm}}**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### Actualizar un documento
Puede utilizar la acción `/update-document` para actualizar un documento en la base de datos de {{site.data.keyword.cloudant_short_notm}}.
{: short desc}

**Antes de empezar**
Cree un [enlace de paquete](#cloudant_db) `/whisk.system/cloudant` que esté configurado para la cuenta {{site.data.keyword.cloudant_short_notm}}.

En el ejemplo siguiente se actualiza el documento que se ha creado en la sección [Escribir un documento en una base de datos de {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

Puede actualizar un documento de la base de datos sustituyendo `<test>` por el nombre de la base de datos y sustituyendo el distintivo `--param doc` por el `id` y el contenido del documento de la base de datos que desea actualizar.


1. Puede actualizar un documento en la base de datos `test` ejecutando el mandato siguiente. En este ejemplo se añade el valor `shade` al documento `color`. 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **Resultado**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. Para ver la actualización, vuelva a buscar el documento.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **Documento de ejemplo**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## Creación de un desencadenante utilizando la función de filtro
{: #cloudant_trigger}

Puede utilizar el canal de información `changes` para configurar un servicio para que active un desencadenante para cada cambio de su base de datos {{site.data.keyword.cloudant_short_notm}}.

**Antes de empezar**
  Cree un  [enlace de paquete](#cloudant_db) `/whisk.system/cloudant` que esté configurado para la cuenta {{site.data.keyword.cloudant_short_notm}}.

Parámetros utilizados en este ejemplo.

| Parámetro | Descripción |
| --- | --- |
| `dbname` | (Obligatorio) El nombre de la base de datos de {{site.data.keyword.cloudant_short_notm}}. |
| `iamApiKey` | (Opcional) La clave de API de IAM para la base de datos de Cloudant.  Si se especifica, este valor se utiliza como credenciales en lugar del nombre de usuario y la contraseña. |
| `iamUrl` | (Opcional) El URL de servicio de señales IAM que se utiliza cuando se especifica `iamApiKey`.  El valor predeterminado es `https://iam.cloud.ibm.com/identity/token`. | 
| `maxTriggers` | (Opcional) Dejar de activar desencadenantes cuando se alcance este límite. El valor predeterminado es infinite. |
| `filter` | (Opcional) La función de filtro que está definida en un documento de diseño. |
| `query_params` | (Opcional) Cualquier parámetro de consulta adicional que pueda ser necesario para la función de filtro. |
| `includeDoc` | (En desuso) El parámetro `includeDoc` ya no está soportado para su uso con el canal de información `/whisk.system/cloudant/changes`. |

</br>

1. Cree un desencadenante denominado `cloudantTrigger` con el canal de información `changes` en el enlace de paquete que ha creado previamente. Incluyendo `filter` y `query_params` para activar el desencadenante cuando un documento se añada (o se modifique) cuando el estado sea `new`.

  Sustituya `/_/myCloudant` por el nombre del paquete. En este ejemplo se utiliza una base de datos denominada `test`.
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. Guarde el siguiente código JavaScript como `cloudantChange.js`.

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. Cree una acción denominada `cloudantChange` que puede utilizar para observar el canal de información de cambios. Sustituya `<file_path>` por la vía de acceso del archivo `cloudantChange.js` en su sistema.

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. Cree una regla denominada `cloudantRule` para conectar la acción `cloudantChange` con el `cloudantTrigger` que ha creado anteriormente.

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. En otra ventana del terminal, empiece a sondear para poder ver cuándo se producen las activaciones.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. En su panel de control de {{site.data.keyword.cloudant_short_notm}}, modifique un documento existente o cree uno.

7. Observe las activaciones para el desencadenante `cloudantTrigger` para cada cambio de documento.

**Ejemplo de activación de `cloudantTrigger`**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### Estructura de datos de una activación de desencadenante
{: #cloudant_struct}

El contenido del suceso generado tiene los parámetros siguientes.

| Parámetro | Descripción |
| --- | --- |
| `id` | El ID del documento. |
| `seq` | El identificador de secuencia generado por {{site.data.keyword.cloudant_short_notm}}. |
| `changes` | Una matriz de objetos, cada uno de los cuales tiene un campo `rev` que contiene
el ID de revisión del documento. |

**Representación JSON de la activación de desencadenante**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

## Filtrado de sucesos de cambio de base de datos
{: #cloudant_filter}

Puede definir una función de filtro para evitar que sucesos de cambio innecesarios activen su desencadenante.

**Antes de empezar**
Cree un [enlace de paquete](#cloudant_db) `/whisk.system/cloudant` que esté configurado para la cuenta {{site.data.keyword.cloudant_short_notm}}.

Para crear una función de filtro, puede utilizar una acción.

1. Guarde el siguiente filtro JSON en un archivo denominado `design_doc.json`.

  ```json
  {
    "doc": {
      "_id": "_design/mailbox",
    "filters": {
        "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
    }
  }
  ```
  {: codeblock}

2. Cree un documento de diseño en la base de datos con la siguiente función de filtro. Sustituya `<database_name>` por el nombre de la base de datos y `<file_path>` por la vía de acceso del archivo `design_doc.json`. Invoque la acción `write` para probar de crear un documento de diseño.

**Sintaxis del mandato**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**Mandato de ejemplo para escribir un archivo `design_doc.json` en una base de datos `test`**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**Resultado de ejemplo**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


Para obtener más información sobre los documentos de diseño de {{site.data.keyword.cloudant_short_notm}}, consulte [Documentos de diseño](/docs/services/Cloudant?topic=cloudant-design-documents)

## Procesar un documento individual utilizando una secuencia de acción
{: #cloudant_seq}

Puede utilizar una secuencia de acciones en una regla para captar y procesar el documento que está asociado a un suceso de cambio {{site.data.keyword.cloudant_short_notm}}.

**Antes de empezar**
Cree un [enlace de paquete](#cloudant_db) `/whisk.system/cloudant` que esté configurado para la cuenta {{site.data.keyword.cloudant_short_notm}}. 

En este ejemplo se actualiza el documento que se ha creado en la sección [Escribir un documento en una base de datos de {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

### Cree una acción para procesar un documento individual

Para crear una acción que maneje los cambios en un documento individual, ejecute los mandatos siguientes.
{: shortdesc}

1. Guarde el código siguiente como `docChange.js`

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. Cree una acción denominada `docChange` para procesar el documento con el nombre `blue` que ha creado anteriormente. Sustituya `<file_path>` por la vía de acceso de archivo de `docChange.js`

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **Resultado**
  ```
  ok: created action docChange
  ```
  {: screen}

### Cree una secuencia con la acción `read` 

La acción `read` se puede combinar con la acción `docChange` para crear una secuencia de acciones.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **Resultado**
  ```
  ok: created action docSequence
  ```
  {: screen}

### Cree un desencadenante con el canal de información `changes`

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### Cree una regla para asociar el desencadenante a la secuencia

La acción `docSequence` se puede utilizar en una regla que active la acción sobre nuevos sucesos desencadenantes de {{site.data.keyword.cloudant_short_notm}}.

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **Resultado**
  ```
  ok: created rule docRule
  ```

  **Activación de ejemplo**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### Pruebe la secuencia

1. Pruebe `docSequence` haciendo un cambio en el archivo `blue` que ha creado anteriormente. En este ejemplo, el valor de `shade` se cambia a `indigo`.

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **Activación de ejemplo**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. Compruebe que el archivo se ha actualizado para incluir el valor de `shade` invocando la acción `read`. Sustituya `<database>` por el nombre de la base de datos.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **Resultado**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### Pasos siguientes
Ahora que está a la escucha de cambios en un documento de la base de datos {{site.data.keyword.cloudant_short_notm}}, puede activar las notificaciones de Slack para los cambios utilizando el [paquete `/whisk.system/slack`](/docs/openwhisk?topic=cloud-functions-pkg_slack).


