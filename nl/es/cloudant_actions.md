---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Paquete Cloudant
{: #cloudant_actions}

El paquete `/whisk.system/cloudant` le permite trabajar con una base de datos [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) e incluye las siguientes acciones y canales de información:

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | paquete | dbname, host, username, password | Trabajar con una base de datos Cloudant. |
| `/whisk.system/cloudant/read` | acción | dbname, id | Leer un documento de la base de datos. |
| `/whisk.system/cloudant/write` | acción | dbname, overwrite, doc | Escribir un documento en la base de datos. |
| `/whisk.system/cloudant/changes` | canal de información | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Activar sucesos desencadenantes para cambios en una base de datos. |
{: shortdesc}

Las siguientes secciones le indican cómo configurar una base de datos de {{site.data.keyword.cloudant_short_notm}} y cómo leerla y escribir en la misma.
Para obtener más información sobre cómo utilizar canales de información con el paquete `/whisk.system/cloudant`, consulte [Origen de sucesos de {{site.data.keyword.cloudant_short_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

## Configuración de una base de datos {{site.data.keyword.cloudant_short_notm}} en {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, puede utilizar el
[plugin de CLI de {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) para enlazar un servicio a una acción o paquete.

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
  

## Lectura desde una base de datos {{site.data.keyword.cloudant_short_notm}}
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

## Escritura en una base de datos {{site.data.keyword.cloudant_short_notm}}
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
