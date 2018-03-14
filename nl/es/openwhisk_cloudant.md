---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Uso del paquete Cloudant
{: #openwhisk_catalog_cloudant}
El paquete `/whisk.system/cloudant` le permite trabajar con una base de datos Cloudant. Incluye las acciones e información de entrada siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Paquete | dbname, host, username, password | Trabajar con una base de datos Cloudant |
| `/whisk.system/cloudant/read` | Acción | dbname, id | Leer un documento de la base de datos |
| `/whisk.system/cloudant/write` | Acción | dbname, overwrite, doc | Escribir un documento en la base de datos |
| `/whisk.system/cloudant/changes` | Canal de información | dbname, filter, query_params, maxTriggers | Activar sucesos desencadenantes para cambios en la BD |
{: shortdesc}

En los temas siguientes se muestra la configuración de una base de datos Cloudant, la configuración de un paquete asociado y cómo utilizar acciones e información de entrada (Feeds) en el paquete `/whisk.system/cloudant`.

## Configuración de una base de datos Cloudant en {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_in}

Si utiliza OpenWhisk desde {{site.data.keyword.Bluemix_notm}}, OpenWhisk crea automáticamente enlaces de paquete para sus instancias de servicio de Cloudant. Si no utiliza OpenWhisk y Cloudant desde {{site.data.keyword.Bluemix_notm}}, continúe en el paso siguiente.

1. Crear una instancia de servicio de Cloudant en su [panel de control](http://console.ng.Bluemix.net) de {{site.data.keyword.Bluemix_notm}}.

  Asegúrese de crear una clave de credenciales para cada nueva instancia de servicio.

2. Actualizar los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para cada instancia de servicio Cloudant con una clave de credencial definida.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```

  Ahora su enlace de paquete contiene credenciales que están asociadas a la instancia de servicio de Cloudant.

3. Compruebe si el enlace de paquete creado anteriormente está configurado con su host de instancia de servicio de {{site.data.keyword.Bluemix_notm}} de Cloudant y las credenciales.

  ```
  wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters
  ```
  ```json
  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```

## Configuración de una base de datos Cloudant fuera de {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_outside}

Si no utiliza OpenWhisk en {{site.data.keyword.Bluemix_notm}} o si quiere configurar su base de datos Cloudant fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para su cuenta Cloudant. Necesita el nombre de host, nombre de usuario y contraseña de la cuenta Cloudant.

1. Cree un enlace de paquete configurado para su cuenta Cloudant.

  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}
  

2. Comprobar que el enlace de paquete existe.

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myCloudant private binding
  ```


## Atender a cambios en una base de datos Cloudant
{: #openwhisk_catalog_cloudant_listen}

### Filtrado de sucesos de cambio de base de datos

Es posible definir una función de filtro para evitar que sucesos de cambio innecesarios activen su desencadenante.

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

Cree un documento de diseño en la base de datos con la función de filtro
```
wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
La información para el nuevo documento de diseño se imprime en la pantalla.
```json
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```

### Creación de un desencadenante utilizando la función de filtro

Puede utilizar la información de entrada `changes` para configurar un servicio para que active un desencadenante para cada cambio de su base de datos Cloudant. Los parámetros son según se indica a continuación:

- `dbname`: nombre de la base de datos Cloudant.
- `maxTriggers`: dejar de activar desencadenantes cuando se alcanza este límite. El valor predeterminado es infinite.
- `filter`: Función de filtro que está definida en un documento de diseño.
- `query_params`: Parámetros de consulta adicionales para la función de filtro.


1. Cree un desencadenante con la información de entrada `changes` en el enlace de paquete que ha creado anteriormente. Incluyendo `filter` y `query_params` para activar el desencadenante cuando un documento se añada o modifique cuando el estado sea `new`.
Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.

  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}
  ```
  ok: created trigger feed myCloudantTrigger
  ```

2. Sondeo de activaciones.

  ```
  wsk activation poll
  ```
  {: pre}

3. En su panel de control de Cloudant, modifique un documento existente o cree uno nuevo.

4. Observe las nuevas activaciones para el desencadenante `myCloudantTrigger` para cada cambio de documento únicamente si el estado del documento es `new` en base a la función de filtro y los parámetros de consulta.
  
  **Nota**: si no detecta nuevas activaciones, consulte las secciones siguientes sobre la lectura y escritura
en una base de datos Cloudant. Probar los siguientes pasos de lectura y escritura le ayuda a comprobar si sus credenciales de Cloudant son correctas.
  
  Ahora puede crear reglas y asociarlas a acciones para reaccionar a actualizaciones de documento.
  
  El contenido de los sucesos generados tiene los siguientes parámetros:
  
  - `id`: el ID del documento
  - `seq`: el identificador de secuencia generado por Cloudant.
  - `changes`: una matriz de objetos, cada uno de los cuales tiene un campo `rev` que contiene
el ID de revisión del documento.
  
  La representación JSON del suceso desencadenante es según se indica a continuación:
  
  ```json
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```
  
## Escritura en una base de datos Cloudant
{: #openwhisk_catalog_cloudant_write}

Puede utilizar una acción para almacenar un documento en una base de datos Cloudant llamada `testdb`. Asegúrese de que
esta base de datos exista en su cuenta Cloudant.

1. Almacenar un documento usando la acción `write` en el enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.

  ```
  wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287
  ```
  ```json
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```

2. Compruebe que el documento exista, buscándolo en su panel de control Cloudant.

  El URL de panel de control para la base de datos `testdb` es parecido a: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


## Leer de una base de datos Cloudant
{: #openwhisk_catalog_cloudant_read}

Puede utilizar una acción para obtener un documento de una base de datos Cloudant llamada `testdb`. Asegúrese de que
esta base de datos exista en su cuenta Cloudant.

- Obtener un documento usando la acción `read` en el enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.

  ```
  wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```json
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```

## Utilización de una secuencia de acciones y de un desencadenante de cambios para procesar un documento desde una base de datos Cloudant
{: #openwhisk_catalog_cloudant_read_change notoc}

Puede utilizar una secuencia de acciones en una regla para captar y procesar el documento que está asociado a un suceso de cambio Cloudant.

Código de ejemplo de una acción que maneja un documento:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```

Cree la acción para procesar el documento desde Cloudant:
```
wsk action create myAction myAction.js
```
{: pre}

Para leer un documento desde la base de datos, puede utilizar la acción `read` del paquete de Cloudant.
La acción `read` puede estar compuesta de `myAction` para crear una secuencia de acciones.
```
wsk action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

Se puede utilizar la acción `sequenceAction` en una regla que active la acción sobre nuevos sucesos de desencadenante de Cloudant.
```
wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota**: el desencadenante de `cambios` de Cloudant utilizado para dar soporte al parámetro `includeDoc` ya no recibe soporte.
  Puede volver a crear desencadenantes creados anteriormente con `includeDoc`. Siga estos pasos para volver a crear el desencadenante: 
  ```
  wsk trigger delete myCloudantTrigger
  ```
  {: pre}
  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
  ```
  {: pre}

  Puede utilizar el ejemplo para crear una secuencia de acciones para leer el documento modificado e invocar las acciones existentes.
  No olvide inhabilitar las reglas que ya no sean válidas y crear nuevas utilizando el patrón de la secuencia de acciones.

