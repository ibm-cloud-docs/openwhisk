---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: cloudant, event, action, trigger, sequence

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Cloudant
{: #pkg_cloudant}

El paquete preinstalado `/whisk.system/cloudant` le permite trabajar con una base de datos [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). No es necesario ningún enlace de servicio para utilizar este paquete.


## Acciones y canales de información disponibles
{: #cloudant_available}

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | paquete | dbname, host, username, password | Trabajar con una base de datos Cloudant. |
| `/whisk.system/cloudant/read` | acción | dbname, id | Leer un documento de la base de datos. |
| `/whisk.system/cloudant/write` | acción | dbname, overwrite, doc | Escribir un documento en la base de datos. |
| `/whisk.system/cloudant/changes` | canal de información | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Activar sucesos desencadenantes para cambios en una base de datos. |
{: shortdesc}

### Configuración de una base de datos {{site.data.keyword.cloudant_short_notm}} en {{site.data.keyword.Bluemix_notm}}
{: #cloudant_db}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, puede utilizar el plugin de CLI de {{site.data.keyword.openwhisk}} para enlazar un servicio a una acción o paquete.

En primer lugar debe crear manualmente un enlace de paquete para la cuenta de
{{site.data.keyword.cloudant_short_notm}}.

1. Cree un enlace de paquete configurado para su cuenta {{site.data.keyword.cloudant_short_notm}}.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Comprobar que el enlace de paquete existe.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. Obtenga el nombre de la instancia de servicio que desea enlazar a una acción o paquete.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    Salida de ejemplo:
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

    Salida de ejemplo:
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Enlace el servicio al paquete que ha creado en el paso 1.
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verifique que las credenciales se han enlazado correctamente.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
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

    En este ejemplo, las credenciales del servicio Cloudant pertenecen a un parámetro denominado `__bx_creds`.


### Lectura desde una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Puede utilizar una acción para obtener un documento de una base de datos {{site.data.keyword.cloudant_short_notm}} llamada **testdb**. Asegúrese de que
esta base de datos exista en su cuenta {{site.data.keyword.cloudant_short_notm}}.

- Obtener un documento usando la acción **read** en el enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

### Escritura en una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

Puede utilizar una acción para almacenar un documento en una base de datos {{site.data.keyword.cloudant_short_notm}} llamada **testdb**. Asegúrese de que
esta base de datos exista en su cuenta {{site.data.keyword.cloudant_short_notm}}.

1. Almacenar un documento usando la acción **write** en el enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Compruebe que el documento exista, buscándolo en su panel de control {{site.data.keyword.cloudant_short_notm}}.

  El URL de panel de control para la base de datos **testdb** es parecido a: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


### Creación de un desencadenante utilizando la función de filtro
{: #cloudant_trigger}

Puede utilizar el canal de información `changes` para configurar un servicio para que active un desencadenante para cada cambio de su base de datos {{site.data.keyword.cloudant_short_notm}}.

Los parámetros que se utilizan en este ejemplo son los siguientes:

**dbname**: el nombre de la base de datos {{site.data.keyword.cloudant_short_notm}} _(obligatorio)_.

**iamApiKey**: la clave de API de IAM para la base de datos Cloudant.  Si se especifica, se utilizará como las credenciales en lugar del nombre de usuario y la contraseña _(opcional)_.

**iamUrl**: el URL de servicio de la señal de IAM que se utiliza cuando se especifica
`iamApiKey`.  Tiene como valor predeterminado `https://iam.bluemix.net/identity/token` _(opcional)_.

**maxTriggers**: dejar de activar desencadenantes cuando se alcanza este límite _(opcional)_. El valor predeterminado es infinite.

**filter**: Función de filtro que está definida en un documento de diseño _(opcional)_.

**query_params**: Parámetros de consulta adicionales para la función de filtro _(opcional)_.

1. Cree un desencadenante denominado **myCloudantTrigger** con el canal de información `changes` en el enlace de paquete que ha creado anteriormente. Incluyendo `filter` y `query_params` para activar el desencadenante cuando un documento se añada (o se modifique) cuando el estado sea `new`.

  Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. Empiece a sondear para ver si hay activaciones a fin de comprender con claridad lo que está sucediendo.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. Cree una acción que se pueda utilizar para observar el efecto del canal de información de cambios. Por ejemplo, una acción denominada **showCloudantChange** que contiene el código JavaScript siguiente:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. Cree una regla para conectar la acción **showCloudantChange** con el desencadenante creado anteriormente:
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Cree acciones y una regla para asociarlas al desencadenante **myCloudantTrigger**.

6. En su panel de control de {{site.data.keyword.cloudant_short_notm}}, modifique un documento existente o cree uno nuevo. El documento debe tener un campo _status_, con el valor **new**.

7. Observe las nuevas activaciones para el desencadenante **myCloudantTrigger** para cada cambio de documento únicamente si el estado del documento es **new** con base a la función de filtro y los parámetros de consulta.

Pruebe los siguientes pasos de lectura y escritura para ayudar a comprobar si sus credenciales de {{site.data.keyword.cloudant_short_notm}} son correctas.

### Estructura de los datos de un suceso desencadenante
{: #cloudant_struct}

El contenido de los sucesos generados tiene los siguientes parámetros:

  - `id`: el ID del documento
  - `seq`: el identificador de secuencia generado por {{site.data.keyword.cloudant_short_notm}}.
  - `changes`: una matriz de objetos, cada uno de los cuales tiene un campo `rev` que contiene
el ID de revisión del documento.

La representación JSON del suceso desencadenante es según se indica a continuación:
```json
{
    "dbname": "testdb",
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

### Filtrado de sucesos de cambio de base de datos
{: #cloudant_filter}

Opcionalmente puede definir una función de filtro para evitar que sucesos de cambio innecesarios activen su desencadenante.

Para crear una nueva función de filtro, puede utilizar una acción.

Cree un archivo de documento json `design_doc.json` con la siguiente función de filtro:
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

Cree un documento de diseño en la base de datos con la siguiente función de filtro:
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

La información para el nuevo documento de diseño se imprime en la pantalla:
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### Utilización de una secuencia de acciones y de un desencadenante de cambios para procesar un documento desde una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_seq}

Puede utilizar una secuencia de acciones en una regla para captar y procesar el documento que está asociado a un suceso de cambio {{site.data.keyword.cloudant_short_notm}}.

Código de ejemplo de una acción que maneja un documento:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

Cree la acción para procesar el documento desde {{site.data.keyword.cloudant_short_notm}}:
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

Para leer un documento desde la base de datos, puede utilizar la acción `read` del paquete de {{site.data.keyword.cloudant_short_notm}}.
La acción `read` puede estar compuesta de `myAction` para crear una secuencia de acciones.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

Se puede utilizar la acción `sequenceAction` en una regla que active la acción sobre nuevos sucesos desencadenantes de {{site.data.keyword.cloudant_short_notm}}.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota:** el desencadenante de `cambios` de {{site.data.keyword.cloudant_short_notm}} utilizado para dar soporte al parámetro `includeDoc` ya no recibe soporte.

Puede volver a crear desencadenantes creados anteriormente con `includeDoc`. Siga estos pasos para volver a crear el desencadenante:
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

Puede utilizar el ejemplo para crear una secuencia de acciones para leer el documento modificado e invocar las acciones existentes. No olvide inhabilitar las reglas que ya no sean válidas y crear nuevas utilizando el patrón de la secuencia de acciones.

