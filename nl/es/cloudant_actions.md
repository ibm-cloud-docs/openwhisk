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

# Cloudant
{: #cloudant_actions}

El paquete `/whisk.system/cloudant` le permite trabajar con una base de datos [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) e incluye las siguientes acciones y canales de información:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | paquete | dbname, host, username, password | Trabajar con una base de datos Cloudant. |
| `/whisk.system/cloudant/read` | acción | dbname, id | Leer un documento de la base de datos. |
| `/whisk.system/cloudant/write` | acción | dbname, overwrite, doc | Escribir un documento en la base de datos. |
| `/whisk.system/cloudant/changes` | Canal de información | dbname, filter, query_params, maxTriggers | Activar sucesos desencadenantes para cambios en una base de datos. |
{: shortdesc}

Las siguientes secciones le indican cómo configurar una base de datos de {{site.data.keyword.cloudant_short_notm}} y cómo leerla y escribir en la misma.
Para obtener más información sobre cómo utilizar canales de información con el paquete `/whisk.system/cloudant`, consulte [Origen de sucesos de {{site.data.keyword.cloudant_short_notm}}](./openwhisk_cloudant.html).

## Configuración de una base de datos {{site.data.keyword.cloudant_short_notm}} en {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, {{site.data.keyword.openwhisk_short}} crea automáticamente enlaces de paquete para sus instancias de servicio de {{site.data.keyword.cloudant_short_notm}}. Si no utiliza {{site.data.keyword.openwhisk_short}} y {{site.data.keyword.cloudant_short_notm}} desde
{{site.data.keyword.Bluemix_notm}}, continúe en la siguiente sección.

1. Cree una instancia de servicio de {{site.data.keyword.cloudant_short_notm}} en su [panel de control de {{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

  Asegúrese de crear una clave de credenciales para cada nueva instancia de servicio.

2. Actualice los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para cada instancia de servicio {{site.data.keyword.cloudant_short_notm}} con una clave de credencial definida.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Salida de ejemplo:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  Ahora su enlace de paquete contiene credenciales que están asociadas a la instancia de servicio de {{site.data.keyword.cloudant_short_notm}}.

3. Compruebe si el enlace de paquete creado anteriormente está configurado con su host de instancia de servicio de {{site.data.keyword.Bluemix_notm}} de {{site.data.keyword.cloudant_short_notm}} y las credenciales.

  ```
  ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

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
  {: screen}

## Configuración de una base de datos {{site.data.keyword.cloudant_short_notm}} fuera de {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_nocloud}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}} o si quiere configurar su base de datos {{site.data.keyword.cloudant_short_notm}} fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para su cuenta {{site.data.keyword.cloudant_short_notm}}. Necesita el nombre de host, nombre de usuario y contraseña de la cuenta {{site.data.keyword.cloudant_short_notm}}.

1. Cree un enlace de paquete configurado para su cuenta {{site.data.keyword.cloudant_short_notm}}.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. Comprobar que el enlace de paquete existe.
  ```
  wsk package list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## Leer de una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

Puede utilizar una acción para obtener un documento de una base de datos {{site.data.keyword.cloudant_short_notm}} llamada **testdb**. Asegúrese de que
esta base de datos exista en su cuenta {{site.data.keyword.cloudant_short_notm}}.

- Obtener un documento usando la acción **read** en el enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.
  ```
  ibmcloud wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
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

## Escritura en una base de datos {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

Puede utilizar una acción para almacenar un documento en una base de datos {{site.data.keyword.cloudant_short_notm}} llamada **testdb**. Asegúrese de que
esta base de datos exista en su cuenta {{site.data.keyword.cloudant_short_notm}}.

1. Almacenar un documento usando la acción **write** en el enlace de paquete que ha creado anteriormente. Asegúrese de sustituir `/_/myCloudant` con el nombre de su paquete.
  ```
  ibmcloud wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
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
