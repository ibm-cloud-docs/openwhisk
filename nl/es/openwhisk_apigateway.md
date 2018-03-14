---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Pasarela de API
{: #openwhisk_apigateway}

La gestión de API permite gestionar acciones de OpenWhisk.

La pasarela de API actúa como un proxy para las [acciones web](./openwhisk_webactions.html) y les proporciona características adicionales. Las características adicionales incluyen: direccionamiento de métodos HTTP, id/secretos de cliente, límites de tasas, CORS, ver el uso de API, ver los registros de respuestas y políticas de compartición de API.
Para obtener más información sobre la gestión de API consulte la [documentación de gestión de API](/docs/apis/management/manage_openwhisk_apis.html#manage_openwhisk_apis).
{: shortdesc}

## Creación de API desde acciones web de OpenWhisk utilizando su navegador.

Con la pasarela de API puede exponer una acción de OpenWhisk como una API. Después de definir la API, puede aplicar políticas de seguridad y de limitación de tasas, ver el uso de la API y los registros de respuestas y definir políticas de compartición de API.
En el panel de control de OpenWhisk, pulse el [separador API](https://console.ng.bluemix.net/openwhisk/apimanagement).


## Creación de API desde acciones web de OpenWhisk utilizando la interfaz de línea de mandatos

### Configuración de la interfaz de línea de mandatos de OpenWhisk

Configure la interfaz de línea de mandatos de OpenWhisk con el host de API.

Hay dos regiones de {{site.data.keyword.Bluemix_notm}} disponibles que requieren su propio host de API exclusivo y una clave de autorización.

* EE.UU. Sur
  * Host de API: `openwhisk.ng.bluemix.net`

* Reino Unido
  * Host de API: `openwhisk.eu-gb.bluemix.net`

Ejecute el siguiente mandato para definir el host de API para la región de Bluemix deseada:

EE. UU. Sur:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Reino Unido:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Si necesita cambiar de región, debe volver a configurar la CLI con el host de API y la clave de autorización, ya que la clave de autorización es específica por región.
{: tip}

Los artefactos (por ejemplo, acciones, reglas, paquetes) son también específicos de la región. Por lo tanto, si utiliza el mismo artefacto en varias regiones, debe desplegarlo en cada región deseada.

Para poder utilizar el mandato `wsk api`, el archivo de configuración de interfaz de línea de mandatos `~/.wskprops` debe contener la señal de acceso de Bluemix.

Para obtener la señal de acceso, utilice el siguiente mandato de interfaz de línea de mandatos:
```
wsk bluemix login
```
{: pre}

Para obtener más información sobre este mandato, ejecute:
```
wsk bluemix login -h
```
{: pre}

Si el mandato `wsk bluemix login` falla con el error `BMXLS0202E: Está utilizando un ID de usuario federado, utilice un código de acceso de una vez para iniciar una sesión con la opción --sso`, inicie una sesión con la CLI de {{site.data.keyword.Bluemix_notm}} con `bluemix login` y, a continuación, especifique `wsk bluemix login --sso`.
{: tip}

### Creación de su primera API utilizando la CLI

1. Crear un archivo JavaScript con el contenido siguiente. Para este ejemplo, el nombre de archivo es 'hello.js'.
  ```javascript
  function main({name:name='Serverless API'}) {
      return {payload: `Hello world ${name}`};
  }
  ```
  {: codeblock}
  
2. Cree una acción web desde la siguiente función JavaScript. En este ejemplo, la acción se llama 'hello'. Asegúrese de añadir el distintivo `--web true`.
  ```
  wsk action create hello hello.js --web true
  ```
  {: pre}

  ```
  ok: created action hello
  ```
  
3. Cree una API con la vía de acceso `/hello`, vía `/world` y método `get` con el tipo de respuesta `json`:
  ```
  wsk api create /hello /world get hello --response-type json
  ```

  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  Se genera un nuevo URL que expone la acción `hello` mediante un método __GET__ HTTP.
  
4. Finalmente, envíe una solicitud HTTP al URL.
  ```
  $ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world?name=OpenWhisk
  ```

  ```json
  {
  "payload": "Hello world OpenWhisk"
  }
  ```

  Se invoca la acción web `hello`, que devuelve un objeto JSON que incluye el parámetro `name` enviado mediante el parámetro de consulta. Puede pasar parámetros a la acción mediante parámetros de consulta sencillos o mediante el cuerpo de la solicitud. Las acciones web pueden invocar una acción de modo público sin la clave de API de autorización OpenWhisk.
  
### Control completo sobre la respuesta HTTP
  
  El distintivo `--response-type` controla el URL de destino de la acción web que la pasarela API debe intermediar. Utilizando `--response-type json` devuelve el resultado completo de la acción en formato JSON y establece automáticamente la cabecera Content-Type en `application/json`. 
  
  Una vez haya empezado, deseará tener un control completo sobre las propiedades de respuesta HTTP como, por ejemplo `statusCode` o `headers`, y devolver distintos tipos de contenido en `body`. Esto es posible mediante el distintivo `--response-type http`, que configura el URL de destino de la acción web con la extensión `http`.

  Puede elegir cambiar el código de la acción para satisfacer con la devolución de las acciones web con la extensión `http`, o incluir la acción en una secuencia pasando su resultado a una nueva acción. Entonces la nueva acción puede transformar el resultado para que corresponda al formato adecuado para una respuesta HTTP. Puede obtener más información sobre los tipos de respuesta y las extensiones de acciones web en la documentación de [acciones web](./openwhisk_webactions.html).

  Cambie el código de `hello.js` que devuelve las propiedades JSON `body`, `statusCode` y `headers`
  ```javascript
  function main({name:name='Serverless API'}) {
      return {
        body: new Buffer(JSON.stringify({payload:`Hello world ${name}`})).toString('base64'),
        statusCode:200,
        headers:{ 'Content-Type': 'application/json'}
      };
  }
  ```
  {: codeblock}
  Tenga en cuenta que body se debe devolver codificado en `base64` y no como una serie.
  
  Actualice la acción con el resultado modificado: 
  ```
  wsk action update hello hello.js --web true
  ```
  {: pre}

  Actualice la API con `--response-type http`: 
  ```
  wsk api create /hello /world get hello --response-type http
  ```
  {: pre}
  
  Llame a la clave de API actualizada: 
  ```
  curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/hello/world
  ```
  {: pre}

  ```
  {
  "payload": "Hello world Serverless API"
  }
  ```
  Ahora tiene un control completo de sus API y puede controlar el contenido. Por ejemplo, devolviendo HTML o establecer el código de estado para estados como No encontrado (404), No autorizado (401) o incluso Error interno de servidor (500).

### Exposición de varias acciones web

Por ejemplo, si desea exponer un conjunto de acciones correspondientes a un club de lectura, puede utilizar una serie de acciones para implementar el programa de fondo para el club de lectura:

| Acción | Método HTTP | Descripción |
| ----------- | ----------- | ------------ |
| getBooks    | GET | Obtener detalles de libros  |
| postBooks   | POST | Añadir un libro |
| putBooks    | PUT | Actualizar detalles de libros |
| deleteBooks | DELETE | Suprimir un libro |

Cree una API para el club de lectura, llamada `Book Club`, con `/club` como vía de acceso base de URL HTTP y `books` como su recurso.
```
wsk api create -n "Book Club" /club /books get getBooks --response-type http
wsk api create /club /books post postBooks              --response-type http
wsk api create /club /books put putBooks                --response-type http
wsk api create /club /books delete deleteBooks          --response-type http
```

Observe que la primera acción expuesta con la vía de acceso base `/club` obtiene la etiqueta de API llamada `Book Club`. Cualquier otra acción expuesta bajo `/club` estará asociada con `Book Club`.

Genere una lista de todas las acciones que están expuestas utilizando el siguiente mandato:
```
wsk api list -f
```
{: pre}

```
ok: APIs
Action: getBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: get
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: postBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: post
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: putBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: put
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
Action: deleteBooks
  API Name: Book Club
  Base path: /club
  Path: /books
  Verb: delete
  URL: https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Como divertimento, puede añadir un libro nuevo, `JavaScript: The Good Parts`, con un HTTP __POST__:
```
curl -X POST -d '{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}' https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": "success"
}
```

Obtenga una lista mediante la acción `getBooks` a través de HTTP __GET__
```
curl -X GET https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
```
{
  "result": [{"name":"JavaScript: The Good Parts", "isbn":"978-0596517748"}]
}
```

### Exportación de la configuración
Exporte la API denominada `Book Club` a un archivo que se pueda utilizar como base para volver a crear las API utilizando un archivo como entrada. 
```
wsk api get "Book Club" > club-swagger.json
```
{: pre}

Pruebe el archivo swagger suprimiendo primero todos los URL expuestos bajo una vía de acceso base común.
Puede suprimir todos los URL expuestos que utilizan la vía de acceso base `/club` o la etiqueta de nombre de API `"Book Club"`:
```
wsk api delete /club
```
```
ok: deleted API /club
```
### Modificación de la configuración

Para editar la configuración en el panel de control OpenWhisk, pulse el [separador API](https://console.ng.bluemix.net/openwhisk/apimanagement) para configurar, por ejemplo, la seguridad, límites de tasas de llamadas y otras características.

### Importación de la configuración

Ahora restaure la API denominada `Book Club` utilizando el archivo `club-swagger.json`
```
wsk api create --config-file club-swagger.json
```
{: pre}

```
ok: created api /books delete for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books get for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books post for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
ok: created api /books put for action deleteBook
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```

Verifique que se ha recreado la API:
```
wsk api list /club
```
{: pre}

```
ok: apis
Action                    Verb         API Name        URL
getBooks                   get         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
postBooks                 post         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
putBooks                   put         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
deleteBooks             delete         Book Club       https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/21ef035/club/books
```
