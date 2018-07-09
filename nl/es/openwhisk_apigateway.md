---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creación de API REST sin servidor
{: #openwhisk_apigateway}

Las acciones de {{site.data.keyword.openwhisk}} pueden gestionarse mediante API directamente con la pasarela de API, que actúa como un proxy para las [acciones web](./openwhisk_webactions.html) y les proporciona características adicionales. Las características adicionales incluyen: direccionamiento de métodos HTTP, id/secretos de cliente, límites de tasas, CORS, ver el uso de API, ver los registros de respuestas y políticas de compartición de API. Para obtener más información sobre la gestión de API consulte la [documentación de gestión de API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Creación de API desde acciones web de OpenWhisk utilizando su navegador
{: #create_api_browser}

Puede utilizar el [**separador API**](https://console.bluemix.net/openwhisk/apimanagement) del [Panel de control de {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/) para realizar las tareas siguientes:

* [Crear una API de Cloud Functions](https://console.bluemix.net/openwhisk/apimanagement): Cree una API que englobe un conjunto de acciones de OpenWhisk.
* [Proteger la API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api): Aplique políticas de seguridad de API y de limitación de velocidad para proteger su API.
* [Gestionar tráfico](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api): Vea estadísticas de uso de la API y consulte los registros de respuestas.
* [Socializar y compartir](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api): Comparta la API con desarrolladores tanto dentro como fuera de {{site.data.keyword.Bluemix_notm}}.

## Creación de API desde acciones web de OpenWhisk utilizando el plug-in de CLI
{: #create_api_cli}

La sección siguiente le guía por las tareas de gestión de API utilizando el plugin de CLI de {{site.data.keyword.openwhisk_short}}. Para crear y gestionar API mediante la CLI, en primer lugar debe instalar el [plugin de CLI de {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/docs/openwhisk/bluemix_cli.html) para {{site.data.keyword.Bluemix_notm}}.

Para su comodidad, los pasos se han dividido en subtemas más reducidos a los que puede ir rápidamente utilizando la siguiente lista de tareas de API:

* [Creación de la primera API](openwhisk_apigateway.html#create_cli_api)
* [Control completo sobre la respuesta HTTP](openwhisk_apigateway.html#full_control)
* [Exposición de varias acciones web](openwhisk_apigateway.html#multiple_web_actions)
* [Exportación de la configuración](openwhisk_apigateway.html#export_config)
* [Importación de la configuración](openwhisk_apigateway.html#import_config)
* [Modificación de la configuración](openwhisk_apigateway.html#modify_config)

### Creación de su primera API utilizando la CLI
{: #create_cli_api}

1. Cree un archivo JavaScript denominado **hello.js** con el contenido siguiente:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Cree una acción web denominada **hello** mediante el archivo `hello.js` creado en el paso uno. **Nota:** Asegúrese de añadir el distintivo `--web true`.
  ```
  ibmcloud wsk action create hello hello.js --web true
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created action hello
  ```
  {: screen}

3. Cree una API con la vía de acceso `/hello`, vía `/world` y método `get` con el tipo de respuesta `json`:
  ```
  ibmcloud wsk api create /hello /world get hello --response-type json
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Se genera un nuevo URL que expone la acción `hello` mediante un método __GET__ HTTP.

4. Finalmente, envíe una solicitud HTTP al URL utilizando el mandato **curl**:
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Se invoca la acción web **hello**, que devuelve un objeto JSON que incluye el parámetro **name** enviado mediante el parámetro de consulta. Puede pasar parámetros a la acción con parámetros de consulta sencillos o utilizando el cuerpo de la solicitud. Las acciones web pueden invocar una acción de modo público sin utilizar la clave de API de autorización OpenWhisk.

### Control completo sobre la respuesta HTTP
{: #full_control}

El distintivo `--response-type` controla el URL de destino de la acción web que la pasarela API debe intermediar. Utilizando `--response-type json` devuelve el resultado completo de la acción en formato JSON y establece automáticamente la cabecera Content-Type en `application/json`.

Deseará tener el control completo sobre las propiedades de respuesta HTTP como, por ejemplo `statusCode` y `headers`, de forma que pueda devolver distintos tipos de contenido en `body`. Esto es posible mediante el distintivo `--response-type http`, que configura el URL de destino de la acción web con la extensión `http`.

Puede elegir cambiar el código de la acción para satisfacer con la devolución de las acciones web con la extensión `http`, o incluir la acción en una secuencia pasando su resultado a una nueva acción. Entonces la nueva acción puede transformar el resultado para que corresponda al formato adecuado para una respuesta HTTP. Puede obtener más información sobre los tipos de respuesta y las extensiones de acciones web en la documentación de [acciones web](./openwhisk_webactions.html).

1. Cambie el código de `hello.js` que devuelve las propiedades JSON `body`, `statusCode` y `headers`:
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: {payload:`Hello world ${name}`},
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}

2. Actualice la acción con el resultado modificado:
  ```
  ibmcloud wsk action update hello hello.js --web true
  ```
  {: pre}

3. Actualice el tipo de respuesta de la API mediante el distintivo `-- response-type http`:
  ```
  ibmcloud wsk api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Llame a la API actualizada con el siguiente mandato **curl**:
  ```bash
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  {: screen}

Ahora tiene un control completo de sus API y puede controlar el contenido. Por ejemplo, devolviendo HTML o establecer el código de estado para estados como No encontrado (404), No autorizado (401) o incluso Error interno de servidor (500).

### Exposición de varias acciones web
{: #multiple_web_actions}

Por ejemplo, si desea exponer un conjunto de acciones correspondientes a un club de lectura, puede utilizar una serie de acciones para implementar el programa de fondo para el club de lectura:

| Acción | Método HTTP | Descripción |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Obtener detalles de libros  |
| postBooks   | POST | Añadir un libro |
| putBooks    | PUT | Actualizar detalles de libros |
| deleteBooks | DELETE | Suprimir un libro |

En este ejemplo, la API se ha definido con un **parámetro path**. Cuando se utilizan parámetros path, la API se debe definir con el tipo de respuesta `http`. El valor de path, empezando por la vía de acceso básica e incluidos los valores actuales del parámetro path, está disponible en el campo `__ow_path` del parámetro JSON de la acción. Consulte la documentación del [contexto HTTP de las acciones web](./openwhisk_webactions.html#http-context) para ver más detalles, incluida información sobre más campos de contexto HTTP que están disponibles para las acciones web invocadas con el tipo de respuesta `http`.

1. Cree una API para el club de libros, denominada **Book Club**, con `/club` como vía de acceso base de URL HTTP, `books` como recurso e `{isbn}` como parámetro path que se utiliza para identificar un libro específico utilizando su número de libro estándar internacional (ISBN).
  ```bash
  ibmcloud wsk api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud wsk api create /club /books get getBooks                       --response-type http
  ibmcloud wsk api create /club /books post postBooks                     --response-type http
  ibmcloud wsk api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud wsk api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: codeblock}

  Observe que la primera acción expuesta con la vía de acceso base `/club` obtiene la etiqueta de API llamada **Book Club**. Cualquier otra acción expuesta bajo `/club` ahora estará asociada con **Book Club**.

2. Genere una lista de todas las acciones de **Book Club** que están expuestas utilizando el siguiente mandato:
  ```
  ibmcloud wsk api list /club -f
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: APIs
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: getBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: get
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: postBooks
    API Name: Book Club
    Base path: /club
    Path: /books
    Verb: post
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  Action: putBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: put
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  Action: deleteBooks
    API Name: Book Club
    Base path: /club
    Path: /books/{isbn}
    Verb: delete
    URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

3. Como divertimento, puede añadir un libro nuevo con el título **JavaScript: The Good Parts**, con un HTTP __POST__:
  ```
  curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' -H "Content-Type: application/json" https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
    "result": "success"
  }
  ```
  {: screen}

4. Obtenga una lista de libros utilizando la acción **getBooks** con HTTP __GET__:
  ```bash
  curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
    "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
  }
  ```
  {: screen}

5. Puede suprimir un libro específico mediante la acción **deleteBooks** con HTTP __DELETE__. En este ejemplo, el valor del campo `__ow_path` de la acción **deleteBooks** es `/club/books/978-0596517748`, donde `978-0596517748` es el valor real de `{isbn}` de la variable path.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

### Exportación de la configuración
{: #export_config}

1. Exporte la API denominada **Book Club** a un archivo que se pueda utilizar como base para volver a crear las API utilizando un archivo como entrada.
  ```
  ibmcloud wsk api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Pruebe el archivo swagger suprimiendo primero todos los URL expuestos bajo una vía de acceso base común utilizando el siguiente mandato:
  ```
  ibmcloud wsk api delete /club
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: deleted API /club
  ```
  {: screen}

  Puede suprimir todos los URL expuestos que utilizan la vía de acceso base `/club` o la etiqueta de nombre de API **"Book Club"**:
  {: tip}

### Importación de la configuración
{: #import_config}

1. Ahora restaure la API denominada **Book Club** utilizando el nombre de archivo `club-swagger.json`:
  ```
  ibmcloud wsk api create --config-file club-swagger.json
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created api /club/books/{isbn} get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} put for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books/{isbn} delete for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books get for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ok: created api /club/books post for action deleteBooks
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  ```
  {: screen}

2. Verifique que se ha recreado la API **Book Club**:
  ```
  ibmcloud wsk api list /club
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: apis
  Action                    Verb         API Name        URL
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books
  getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/{isbn}
  ```
  {: screen}

### Modificación de la configuración utilizando la CLI
{: #modify_config}

Para editar la configuración en el panel de control de {{site.data.keyword.openwhisk_short}}, pulse el [separador API](https://console.ng.bluemix.net/openwhisk/apimanagement) para configurar, por ejemplo, la seguridad, límites de tasas de llamadas y otras características. Una vez que haya terminado de actualizar la configuración, puede descargar el archivo de definiciones en formato JSON y volver a importarlo mediante la CLI. Esto puede resultar útil por ejemplo para un despliegue desatendido en una integración continua y un conducto de despliegue (CICD). También tiene la opción de cargar y volver a importar el archivo de definiciones de la API mediante la interfaz de usuario.
