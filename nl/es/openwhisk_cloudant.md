---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Origen de sucesos de Cloudant
{: #openwhisk_cloudant}

Aprenda a detectar si hay cambios en una base de datos de {{site.data.keyword.cloudant}}, a filtrar los sucesos de cambio de la base de datos y a utilizar una secuencia de acciones para procesar un documento procedente de una base de datos de {{site.data.keyword.cloudant_short_notm}}. El paquete `/whisk.system/cloudant` le permite trabajar con una base de datos {{site.data.keyword.cloudant_short_notm}} e incluye las siguientes acciones y canales de información:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | paquete | dbname, host, username, password | Trabajar con una base de datos Cloudant. |
| `/whisk.system/cloudant/read` | acción | dbname, id | Leer un documento de la base de datos. |
| `/whisk.system/cloudant/write` | acción | dbname, overwrite, doc | Escribir un documento en la base de datos. |
| `/whisk.system/cloudant/changes` | Canal de información | dbname, filter, query_params, maxTriggers | Activar sucesos desencadenantes para cambios en una base de datos. |
{: shortdesc}

En las siguientes secciones se muestra la configuración de un paquete asociado y cómo utilizar acciones y canales de información en el paquete `/whisk.system/cloudant`. Para obtener más información sobre cómo configurar la base de datos de {{site.data.keyword.cloudant_short_notm}} y cómo leerla o escribir en la misma, consulte el tema sobre [Acciones de {{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html).

## Creación de un desencadenante utilizando la función de filtro

Puede utilizar el canal de información `changes` para configurar un servicio para que active un desencadenante para cada cambio de su base de datos {{site.data.keyword.cloudant_short_notm}}.

Los parámetros que se utilizan en este ejemplo son los siguientes:

**dbname**: el nombre de la base de datos {{site.data.keyword.cloudant_short_notm}} _(obligatorio)_.

**maxTriggers**: dejar de activar desencadenantes cuando se alcanza este límite _(opcional)_. El valor predeterminado es infinite.

**filter**: Función de filtro que está definida en un documento de diseño _(opcional)_.

**query_params**: Parámetros de consulta adicionales para la función de filtro _(opcional)_.

1. Cree un desencadenante denominado **myCloudantTrigger** con el canal de información `changes` en el enlace de paquete que ha creado anteriormente. Incluyendo `filter` y `query_params` para activar el desencadenante cuando un documento se añada (o se modifique) cuando el estado sea `new`.

  Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.
  ```
  ibmcloud wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
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
  ibmcloud wsk activation poll
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
  ibmcloud wsk rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Cree acciones y una regla para asociarlas al desencadenante **myCloudantTrigger**.

6. En su panel de control de {{site.data.keyword.cloudant_short_notm}}, modifique un documento existente o cree uno nuevo. El documento debe tener un campo _status_, con el valor **new**.

7. Observe las nuevas activaciones para el desencadenante **myCloudantTrigger** para cada cambio de documento únicamente si el estado del documento es **new** en base a la función de filtro y los parámetros de consulta.

Si no detecta nuevas activaciones, consulte el tema [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) que muestra cómo leer y escribir en una base de datos {{site.data.keyword.cloudant_short_notm}}. Pruebe los siguientes pasos de lectura y escritura para ayudar a comprobar si sus credenciales de {{site.data.keyword.cloudant_short_notm}} son correctas.
{: tip}

## Estructura de los datos de un suceso del desencadenante

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

## Filtrado de sucesos de cambio de base de datos

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
ibmcloud wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
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

## Utilización de una secuencia de acciones y de un desencadenante de cambios para procesar un documento desde una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #openwhisk_catalog_cloudant_read_change notoc}

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
ibmcloud wsk action create myAction myAction.js
```
{: pre}

Para leer un documento desde la base de datos, puede utilizar la acción `read` del paquete de {{site.data.keyword.cloudant_short_notm}}.
La acción `read` puede estar compuesta de `myAction` para crear una secuencia de acciones.
```
ibmcloud wsk action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

Se puede utilizar la acción `sequenceAction` en una regla que active la acción sobre nuevos sucesos de desencadenante de {{site.data.keyword.cloudant_short_notm}}.
```
ibmcloud wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota:** el desencadenante de `cambios` de {{site.data.keyword.cloudant_short_notm}} utilizado para dar soporte al parámetro `includeDoc` ya no recibe soporte.

Puede volver a crear desencadenantes creados anteriormente con `includeDoc`. Siga estos pasos para volver a crear el desencadenante:
```
ibmcloud wsk trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

Puede utilizar el ejemplo para crear una secuencia de acciones para leer el documento modificado e invocar las acciones existentes. No olvide inhabilitar las reglas que ya no sean válidas y crear nuevas utilizando el patrón de la secuencia de acciones.
