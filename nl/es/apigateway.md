---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: serverless, rest api, gateway, web actions, functions

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


# Creación de API REST sin servidor
{: #apigateway}

Puede utilizar API para gestionar directamente las [acciones web](/docs/openwhisk?topic=cloud-functions-actions_web) de {{site.data.keyword.openwhisk}}. 
{: shortdesc}

No se da soporte a la creación de API con la Pasarela de API para los espacios de nombres basados en IAM. En su lugar, utilice un espacio de nombres basado en Cloud Foundry.
{: important}

## ¿Por qué utilizar las API REST con {{site.data.keyword.openwhisk_short}}?

Puede utilizar la pasarela de API como un proxy para las acciones web. La pasarela de API proporciona direccionamiento de métodos HTTP, secretos e ID de cliente, límites de tasas, CORS, visualización del uso de las API, visualización de registros de respuesta y políticas de compartición.

Para obtener más información sobre la gestión de API consulte la [documentación de gestión de API](/docs/api-management?topic=api-management-manage_openwhisk_apis).

## Creación de su primera API
{: #api_create}

Debe tener permisos de `SpaceDeveloper` en el espacio de Cloud Foundry para crear API REST. Los permisos de espacio se pueden ver ejecutando `ibmcloud account space-roles <org>`.
{: note}

Antes de empezar, instale el [plugin de CLI de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli_install).

1. Guarde el código siguiente en un archivo de JavaScript denominado `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello, ${name}!`};
  }
  ```
  {: codeblock}

2. Cree una acción web con el nombre `hello` utilizando el archivo que ha creado. Asegúrese de añadir el distintivo `--web true`. Sustituya `<filepath>` por la vía de acceso del archivo `hello.js`.

  ```
  ibmcloud fn action create hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  ok: created action hello
  ```
  {: screen}

3. Cree una API con la vía de acceso `/hello`, vía `/world`, método `get` y el tipo de respuesta `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Resultado de ejemplo**
  Se genera un nuevo URL que expone la acción `hello` utilizando un método HTTP `GET`.

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  
4. Envíe una solicitud HTTP de prueba al URL utilizando el siguiente mandato cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=Jane
  ```
  {: pre}

  **Salida de ejemplo**
  Se invoca la acción web `hello` que devuelve un objeto JSON que incluye el parámetro `name` en el parámetro de consulta. Puede pasar parámetros a la acción con parámetros de consulta sencillos o utilizando el cuerpo de la solicitud. Las acciones web pueden invocar públicamente una acción sin utilizar autenticación.

  ```
  {
  "payload": "Hello, Jane!"
  }
  ```
  {: screen}



## Utilización del control completo sobre respuestas HTTP
{: #api_control}

El distintivo `--response-type` controla el URL de destino de la acción web que la pasarela API debe intermediar. Por ejemplo, cuando se utiliza el distintivo `--response-type json`, el resultado completo de la acción se devuelve en formato JSON y la cabecera `Content-Type` se establece automáticamente en `application/json`.

Para devolver tipos de contenido diferentes en el cuerpo, utilice el control completo sobre las propiedades de respuesta HTTP como, por ejemplo, `statusCode` y `headers`. Utilice el distintivo `--response-type http` para configurar el URL de destino de la acción web con la extensión `http`. Puede cambiar el código de la acción para satisfacer con la devolución de las acciones web con la extensión `http`, o incluir la acción en una secuencia pasando su resultado a una nueva acción. Entonces la nueva acción puede transformar el resultado para que corresponda al formato adecuado para una respuesta HTTP. Puede obtener más información sobre los tipos de respuesta y las extensiones de acciones web en la documentación de [acciones web](/docs/openwhisk?topic=cloud-functions-actions_web).

1. Guarde el siguiente código como `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello, ${name}!`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js`.
  ```
  ibmcloud fn action update hello <filepath>/hello.js --web true
  ```
  {: pre}

  **Resultado**
  ```
  ok: updated action hello
  ```
  {: screen}

3. Actualice el tipo de respuesta de la API mediante el distintivo `--response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

  **Resultado**
  ```
  ok: created API /hello/world GET for action /_/hello https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

4. Llame a la API actualizada con el siguiente mandato cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  {
  "payload": "Hello, Serverless API!"
  }
  ```
  {: screen}

## Modificación de la configuración
{: #api_modify_config}

Después de crear la configuración, puede utilizar el [separador de API](https://cloud.ibm.com/openwhisk/apimanagement){: external} en el panel de control de {{site.data.keyword.openwhisk_short}} para modificar la configuración de las siguientes maneras.

* [Crear una API de {{site.data.keyword.openwhisk_short}}](/docs/services/api-management?topic=api-management-manage_openwhisk_apis#manage_openwhisk_apis) que abarque un conjunto de acciones de {{site.data.keyword.openwhisk_short}}.
* [Proteger su API](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) aplicando seguridad de API y políticas de limitación de tasas de uso.
* [Gestionar el tráfico](/docs/services/api-management?topic=api-management-manage_apis#settings_api_manage_apis) visualizando las estadísticas de uso de la API y verificando los registros de respuesta.
* [Socializar y compartir](/docs/services/api-management?topic=api-management-manage_apis#share_api_manage_apis) sus API con desarrolladores dentro y fuera de {{site.data.keyword.cloud_notm}}.

</br>
Una vez que haya terminado de actualizar la configuración, puede descargar el archivo de definiciones en formato JSON y volver a importarlo mediante la CLI. La descarga y la importación de la configuración puede resultar útil, por ejemplo, para un despliegue desatendido en una integración continua y un conducto de despliegue (CICD). También puede cargar y volver a importar el archivo de definición de API utilizando la interfaz de usuario.



