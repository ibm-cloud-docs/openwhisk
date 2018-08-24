---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Creación de API REST sin servidor
{: #openwhisk_apigateway}

Las API permiten gestionar directamente las acciones de {{site.data.keyword.openwhisk}}. API Gateway actúa como un proxy para las [acciones web](./openwhisk_webactions.html) y proporciona direccionamiento de métodos HTTP, secretos e ID de cliente, límites de tasas, CORS, visualización del uso de las API, visualización de registros de respuesta y políticas de compartición.
{: shortdesc}

Para obtener más información sobre la gestión de API consulte la [documentación de gestión de API](/docs/api-management/manage_openwhisk_apis.html#manage_openwhisk_apis).

## Creación de su primera API
{: #create_cli_api}

Antes de empezar, instale el [plugin de CLI de {{site.data.keyword.openwhisk_short}}](bluemix_cli.html).

1. Guarde el código siguiente en un archivo de JavaScript denominado `hello.js`.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}

2. Cree una acción web denominada `hello` utilizando el archivo que ha creado. **Nota:** Asegúrese de añadir el distintivo `--web true`.
  ```
  ibmcloud fn action create hello hello.js --web true
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created action hello
  ```
  {: screen}

3. Cree una API con la vía de acceso `/hello`, vía `/world`, método `get` y el tipo de respuesta `json`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world
  ```
  {: screen}

  Se genera un nuevo URL que expone la acción `hello` mediante un método GET HTTP.

4. Envíe una solicitud HTTP de prueba al URL utilizando el mandato cURL.
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world?name=OpenWhisk
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
  "payload": "Hello world OpenWhisk"
  }
  ```
  {: screen}

Se invoca la acción web `hello`, que devuelve un objeto JSON que incluye el parámetro **name** en el parámetro de consulta. Puede pasar parámetros a la acción con parámetros de consulta sencillos o utilizando el cuerpo de la solicitud. Las acciones web pueden invocar públicamente una acción sin utilizar la clave de API de autorización de {{site.data.keyword.openwhisk_short}}.

## Utilización del control completo sobre respuestas HTTP
{: #full_control}

El distintivo `--response-type` controla el URL de destino de la acción web que la pasarela API debe intermediar. Por ejemplo, cuando se utiliza el distintivo `--response-type json`, el resultado completo de la acción se devuelve en formato JSON y la cabecera **Content-Type** se establece automáticamente en `application/json `.

Para devolver tipos de contenido diferentes en el cuerpo, utilice el control completo sobre las propiedades de respuesta HTTP como, por ejemplo, **statusCode** y **headers**. Utilice el distintivo `--response-type http` para configurar el URL de destino de la acción web con la extensión `http`. Puede cambiar el código de la acción para satisfacer con la devolución de las acciones web con la extensión `http`, o incluir la acción en una secuencia pasando su resultado a una nueva acción. Entonces la nueva acción puede transformar el resultado para que corresponda al formato adecuado para una respuesta HTTP. Puede obtener más información sobre los tipos de respuesta y las extensiones de acciones web en la documentación de [acciones web](./openwhisk_webactions.html).

1. Cambie el código de la acción `hello.js` que devuelve las propiedades JSON `body`, `statusCode` y `headers`.
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

2. Actualice la acción con el resultado modificado.
  ```
  ibmcloud fn action update hello hello.js --web true
  ```
  {: pre}

3. Actualice el tipo de respuesta de la API mediante el distintivo `-- response-type http`.
  ```
  ibmcloud fn api create /hello /world get hello --response-type http
  ```
  {: pre}

4. Llame a la API actualizada con el siguiente mandato cURL.
  ```
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

## Exposición de varias acciones web
{: #multiple_web_actions}

Puede exponer varias acciones web para implementar el sistema de fondo de su app. Por ejemplo, para exponer un conjunto de acciones correspondientes a un club de lectura, puede utilizar una serie de acciones para implementar el programa de fondo para el club de lectura:

| Acción | Método HTTP | Descripción |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Obtener detalles de libros  |
| postBooks   | POST | Añadir un libro |
| putBooks    | PUT | Actualizar detalles de libros |
| deleteBooks | DELETE | Suprimir un libro |

En este ejemplo, la API se ha definido con un parámetro path. Cuando se utilizan parámetros path, la API se debe definir con el tipo de respuesta `http`. El valor de path, empezando por la vía de acceso básica e incluidos los valores actuales del parámetro path, está disponible en el campo `__ow_path` del parámetro JSON de la acción. Para obtener más detalles sobre los campos de contexto HTTP, consulte la documentación de [web actions Contexto HTTP de acciones web](./openwhisk_webactions.html#http-context).

Para intentar este ejemplo de acciones web de Book Club:

1. Cree una API para el club de libros, denominada `Book Club`, con `/club` como vía de acceso base del URL HTTP, `books` como recurso e `{isbn}` como parámetro path que se utiliza para identificar un libro específico utilizando su número de libro estándar internacional (ISBN).
  ```
  ibmcloud fn api create -n "Book Club" /club /books/{isbn} get getBooks --response-type http
  ibmcloud fn api create /club /books get getBooks                       --response-type http
  ibmcloud fn api create /club /books post postBooks                     --response-type http
  ibmcloud fn api create /club /books/{isbn} put putBooks                --response-type http
  ibmcloud fn api create /club /books/{isbn} delete deleteBooks          --response-type http
  ```
  {: pre}

  La primera acción que se expone con la vía de acceso base `/club` está etiquetada con el nombre `Book Club`. Cualquier otra acción expuesta bajo `/club` ahora estará asociada con `Book Club`.

2. Genere una lista de todas las acciones de `Book Club` que están expuestas.
  ```
  ibmcloud fn api list /club -f
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

3. Añada un libro titulado `JavaScript: The Good Parts` utilizando una llamada POST HTTP.
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

4. Obtenga una lista de los libros mediante una llamada GET HTTP para la acción `getBooks`.
  ```
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

5. Suprima un libro específico con una llamada DELETE HTTP para la acción `deleteBooks`. En este ejemplo, el valor del campo `__ow_path` de la acción `deleteBooks` es `/club/books/978-0596517748`, donde `978-0596517748` es el valor real de `{isbn}` de la variable path.
  ```bash
  curl -X DELETE https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/club/books/978-0596517748
  ```
  {: pre}

## Exportación e importación de la configuración
{: #export_import_config}

Para exportar o importar una configuración, puede continuar utilizando el ejemplo de Book Club.

1. Exporte la API de `Book Club` en un archivo denominado `club-swagger.json `. Este archivo se puede utilizar como una base para volver a crear las API utilizando un archivo como la entrada.
  ```
  ibmcloud fn api get "Book Club" > club-swagger.json
  ```
  {: pre}

2. Pruebe el archivo swagger suprimiendo primero todos los URL expuestos bajo una vía de acceso base común.
  ```
  ibmcloud fn api delete /club
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: deleted API /club
  ```
  {: screen}

  Puede suprimir todos los URL expuestos que utilizan la vía de acceso base `/club` o la etiqueta de nombre de API `"Book Club"`.
  {: tip}

3. Restaura la API de `Book Club` utilizando el archivo `club-swagger.json`.
  ```
  ibmcloud fn api create --config-file club-swagger.json
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

4. Verifique que se ha recreado la API `Book Club`.
  ```
  ibmcloud fn api list /club
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

## Modificación de la configuración
{: #modify_config}

Una vez haya creado la configuración, puede utilizar el [**separador de API**](https://console.bluemix.net/openwhisk/apimanagement) en el panel de control de {{site.data.keyword.openwhisk_short}} para modificar la configuración de las siguientes maneras.

* [Crear una API de {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/apimanagement) que abarque un conjunto de acciones de {{site.data.keyword.openwhisk_short}}.
* [Proteger su API](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) aplicando seguridad de API y políticas de limitación de tasas de uso.
* [Gestionar el tráfico](https://console.bluemix.net/docs/apis/management/manage_apis.html#settings_api) visualizando las estadísticas de uso de la API y verificando los registros de respuesta.
* [Socializar y compartir](https://console.bluemix.net/docs/apis/management/manage_apis.html#share_api) sus API con desarrolladores dentro y fuera de {{site.data.keyword.Bluemix_notm}}.

Una vez que haya terminado de actualizar la configuración, puede descargar el archivo de definiciones en formato JSON y volver a importarlo mediante la CLI. La descarga y la importación de la configuración puede resultar útil, por ejemplo, para un despliegue desatendido en una integración continua y un conducto de despliegue (CICD). También tiene la opción de cargar y volver a importar el archivo de definiciones de la API mediante la interfaz de usuario.
