---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: web actions, serverless, functions

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


# Creación de acciones web
{: #actions_web}

Cuando se crea una acción web, el resultado es un URL que se puede utilizar para desencadenar la acción desde cualquier app web. 
{: shortdesc}

## ¿Por qué utilizar acciones web en lugar de acciones estándar?

### 1. Ejecutar acciones web de forma anónima

Las activaciones de acciones web están asociadas al usuario que ha creado la acción, más que al llamante de la acción. Por lo general, para las llamadas de API a aplicaciones como Github, incluiría un nombre de usuario y una señal con la llamada de API para un usuario específico o para un ID funcional. Cuando se utiliza una acción web, no se requieren estos tipos de credenciales. Una acción web es accesible a través de una interfaz REST sin necesidad de credenciales.

Aunque no es obligatorio utilizar credenciales con acciones web, puede implementar su propio flujo de autenticación y autorización, u OAuth. Para configurar una acción web con credenciales, consulte [Protección de acciones web](#actions_web_secure).

### 2. Utilizar cualquier tipo de solicitud HTTP

By default, actions only De forma predeterminada, las acciones sólo aceptan solicitudes `POST`, pero las acciones web se pueden invocar a través de cualquiera de estos métodos HTTP: `GET`, `POST`, `PUT`, `PATCH` y `DELETE`, así como `HEAD` y `OPTIONS`.

### 3. Desencadenar una acción web desde cualquier lugar

Cuando se crea una acción web de {{site.data.keyword.openwhisk}}, se genera un URL para invocar esa acción desde cualquier aplicación basada en web. Las acciones que no son acciones web requieren autenticación y deben responder con un objeto JSON. 

Se puede utilizar una vía de acceso de API de acción web con cURL, `wget`, o incluso se puede especificar directamente en el navegador. Se puede invocar una acción web utilizando un URL estructurado como se indica a continuación: `https://<apihost>/api/v1/web/<namespace>/<packageName>/<actionName>.<ext>`.

### 4. Crear menos entidades de {{site.data.keyword.openwhisk_short}}

Debido a que puede invocar una acción web desde cualquier lugar, no es necesario que cree otras entidades {{site.data.keyword.openwhisk_short}} como desencadenantes o reglas.

## ¿Cómo funcionan las acciones web?

Las acciones web se pueden invocar sin autenticación y se pueden utilizar para implementar manejadores HTTP que respondan con contenido de `headers`, `statusCode` y `body` de distintos tipos.

Las acciones web deben devolver un objeto JSON. Sin embargo, el controlador trata una acción web de forma distinta si su resultado incluye una o más de las siguientes opciones como [propiedades JSON](#web_action_properties) de nivel superior.
{: shortdesc}

## Características disponibles de las acciones web
{: #actions_web_extra}

Las acciones web dan soporte a las características siguientes:

| Característica | Descripción |
| --- | --- |
| [Extensiones de contenido](#extra_features) | Puede especificar un tipo de contenido para las solicitudes HTTP como, por ejemplo, `.json`, `.html`, `.http`, `.svg` o `.text`. Si no se especifica ningún tipo de contenido, se presupone la extensión `.http`. Puede especificar un tipo de contenido añadiendo una extensión al nombre de acción en el URI de modo que para hacer referencia a una acción `demo/hello` se utilizaría `/demo/hello.svg`. Las extensiones `.json` y `.http` no necesitan una vía de acceso de proyección; en cambio, las extensiones `.html`, `.svg` y `.text`, sí. Se presupone que la vía de acceso predeterminada coincide con el nombre de la extensión. Para invocar una acción web y recibir una respuesta `.html`, la acción debe responder con un objeto JSON que conste de una propiedad de nivel superior denominada `html` (o la respuesta debe estar en la vía de acceso explícita). En otras palabras, `/<namespace>/demo/hello.html`  es equivalente a proyectar explícitamente la propiedad `html`, como en  `/<namespace>/demo/hello.html/html`. El nombre completo de la acción debe incluir el nombre de su paquete, que es `default` si la acción no está en un paquete con nombre. |
| [Proyección de campos desde el resultado](#projecting_fields) | La vía de acceso que sigue al nombre de la acción se utiliza para proyectar uno o varios niveles de respuesta. Por ejemplo, `/demo/hello.html/body`. Esta característica admite cualquier acción web que devuelve un diccionario, como por ejemplo: `{body: "..." }`, para proyectar la propiedad `body` y devolver directamente su valor de serie en lugar de su valor de diccionario. La vía de acceso proyectada sigue un modelo de vía de acceso absoluta (como en XPath). |
| [Parámetros de consulta y de cuerpo como entrada](#query_test) | La acción recibe parámetros de consulta y parámetros en el cuerpo de la solicitud. El orden de prioridad para fusionar parámetros es el siguiente: parámetros de paquete, parámetros de acción, parámetros de consulta y parámetros de cuerpo. Cada uno de estos parámetros puede sustituir cualquier valor anterior en caso de solapamiento. Por ejemplo, `/demo/hello.http?name=Jane` puede pasar el argumento `{name: "Jane"}` a la acción. |
| [Datos de formulario](#form_data) | Además de `application/json` estándar, las acciones web pueden recibir como entrada datos de formulario codificados como URL `application/x-www-form-urlencoded data`.
| [Activaciones utilizando varios verbos HTTP](#actions_web_options) | Se puede invocar una acción web mediante uno de estos métodos HTTP: `GET`, `POST`, `PUT`, `PATCH` y `DELETE`, así como `HEAD` y `OPTIONS`. |
| [Gestión de entidades con cuerpo que no es JSON y HTTP sin formato](#actions_web_raw_enable) | Una acción web puede aceptar un cuerpo de solicitud HTTP que
no sea un objeto JSON y puede optar por recibir siempre estos valores como valores opacos (texto sin formato cuando no están en un archivo binario, o serie codificada en base64 en caso contrario). |

## Creación de una acción web
{: #actions_web_example}

Para crear una acción web: 

1. Guarde el siguiente código JavaScript como `hello.js`.

  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Cree el paquete `demo`. El nombre de paquete es `default` a menos que se especifique explícitamente otro nombre.
  ```
  ibmcloud fn package create demo
  ```
  {: pre}

3. Cree la acción `hello`. En este ejemplo, los valores de `packageName/actionName` son `demo/hello`. Sustituya la variable `<filepath>` con la vía de acceso del archivo `hello.js` y establezca el distintivo `--web` en `true`. 

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

4. Invoque o pruebe la acción `hello` sin ningún parámetro. Sustituya las variables `<apihost>` y `<namespace>`. Para obtener el valor de `<apihost>`, ejecute `ibmcloud fn property get --apihost`. Ejemplo de `<apihost>`: `us-south.functions.cloud.ibm.com`.

  Para los espacios de nombres habilitados para IAM, sustituya la variable `<namespace>` por el ID de espacio de nombres. Para obtener el ID, ejecute `ibmcloud fn namespace get <namespace_name>`. 
  {: note}

  a. Puede probar la acción web de una de estas dos formas: 
    * Abriendo un URL utilizando la estructura siguiente `https://<apihost>/api/v1/web/<namespace>/demo/hello` en el navegador.
    * Probando la acción utilizando un mandato cURL.
      ```
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

    * Probando la acción utilizando un mandato `wget`.  
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello
      ```
      {: pre}

  b. El código de acción devuelve el siguiente diccionario.
    ```
    {body: `<html><body><h3>${msg}</h3></body></html>`}
    ``` 
    {: screen}
    
  También puede probar la acción devolviendo sólo la propiedad `body` utilizando el mandato siguiente:
  {: #projecting_fields}

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello.html/body
    ```
    {: pre}

    **Resultado de ejemplo**

    Dado que no se ha especificado el parámetro `<name>`, se devuelve el mensaje siguiente.
    ```
    <html><body><h3>You did not tell me who you are.</h3></body></html>
    ```
    {: screen}

5. Ahora intente definir el parámetro `<name>`. Pruebe la acción con un parámetro `<name>` de una de estas dos formas:
  * Abriendo `https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane` en el navegador. 
  * Probando la acción utilizando un mandato cURL.

    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}
  * Probando la acción utilizando un mandato `wget`.  
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello?name=Jane
    ```
    {: pre}

  **Resultado de ejemplo**
  ```
  <html><body><h3>Hello, Jane!</h3></body></html>
  ```
  {: screen}


**Pasos siguientes**

Añada el URL de la acción web `hello` a su app web y pruébela allí.

### Propiedades JSON de acción web
{: #web_action_properties}

El valor predeterminado `Content-Type` para una respuesta HTTP es `application/json`, y el cuerpo puede ser cualquier valor JSON permitido. Si su `Content-Type` no es `application/json`, debe especificar un `Content-Type` en las cabeceras (`headers`) del código de acción.

Si se alcanza el [límite de tamaño de resultados](/docs/openwhisk?topic=cloud-functions-limits) para las acciones, la respuesta falla. Si sabe que el resultado de la acción es superior a 5 MB, configure un [almacén de objetos](/docs/openwhisk?topic=cloud-functions-pkg_obstorage).

| Propiedad JSON | Descripción |
| --- | --- |
| `headers`| Un objeto JSON en el que las claves son nombres de cabecera y los valores son valores de serie, numéricos o booleanos. Para enviar varios valores para una sola cabecera, el valor de la cabecera es una matriz JSON de varios valores. No hay ninguna cabecera establecida de forma predeterminada. |
| `statusCode` | Un código de estado HTTP válido. Si el contenido del cuerpo está presente, el valor predeterminado es
`200 OK`. Si no hay contenido de cuerpo presente, el valor predeterminado es `204 No Content`. |
| `body` | Una serie que puede ser texto sin formato, una matriz u objeto JSON, o una serie codificada en base64 para datos binarios. El cuerpo se considera vacío si es `null`, la serie vacía `""` o es indefinido. El valor predeterminado es un cuerpo vacío. |

El [controlador](/docs/openwhisk?topic=cloud-functions-about#about_controller) pasa las cabeceras especificadas por acciones, el código de estado o el cuerpo al cliente HTTP que finaliza la solicitud o respuesta. Si no se declara la cabecera `Content-Type` en la sección `headers` del resultado de la acción, el cuerpo se interpreta como `application/json` para valores que no sean de tipo serie y `text/html` en cualquier otro caso. Si se define la cabecera `Content-Type`, el controlador determina si la respuesta son datos binarios o texto sin formato y decodifica la serie utilizando el decodificador base64 según sea necesario. Si el cuerpo no se decodifica correctamente, se devuelve un error al cliente.

El propietario de la acción web es el propietario de todos los registros de activación y es el responsable del coste de ejecutarlas en el sistema, independientemente de cómo se haya invocado la acción.
{: note}

#### Parámetros protegidos
Los parámetros de acción están protegidos y solo se pueden cambiar actualizando la acción. Los parámetros finalizan automáticamente para habilitar las acciones web.

```
ibmcloud fn action create /<namespace>/demo/hello hello.js --parameter name Jane --web true
```
{: pre}


El resultado de estos cambios es que `name` se vincula a `Jane` y no lo pueden modificar ni los parámetros de la consulta ni los del cuerpo debido a la anotación final. Este diseño protege la acción frente a parámetros de consulta o de cuerpo que intenten modificar este valor, ya sea intencionadamente o por accidente.

### Realización de una redirección HTTP utilizando una acción web
{: #http_redirect}
Puede utilizar esta característica en una aplicación web para redirigir a un usuario a la nueva versión de su sitio.

**Antes de empezar**
Cree el paquete `demo` y la acción web `hello` completando los pasos del apartado [Creación de una acción web](#actions_web_example).

Para crear una acción web que realice una redirección HTTP:

1. Guarde el código como `hello.js`.

  ```javascript
  function main() {
    return {
      headers: { location: 'https://cloud.ibm.com/openwhisk/' },
      statusCode: 302
    }
  }
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js`. Sustituya `<filepath>` por la vía de acceso del archivo `hello.js`.

  ```
  ibmcloud fn action create demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Pruebe la acción web `hello`. Sustituya las variables `<apihost>` y `<namespace>`. Puede probar la acción web de una de estas dos formas:

  * Abriendo el URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` en el navegador. 
  * Ejecutando el siguiente mandato cURL:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Ejecutando el siguiente mandato `wget`:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

  **Resultado de ejemplo** 
  
  Esta acción web de ejemplo redirige el navegador al [panel de control de {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk/){: external}.

### Configurar las cookies utilizando una acción web
{: #multiple_cookie}
Puede utilizar esta característica en una aplicación web para almacenar una señal web JSON como una cookie de sesión después de un inicio de sesión satisfactorio.

Para crear una acción web que establezca varias cookies:

**Antes de empezar**
Cree el paquete `demo` y la acción web `hello` completando los pasos del apartado [Creación de una acción web](#actions_web_example).

1. Guarde el código como `hello.js`.
  ```javascript
  function main() {
    return {
      headers: {
        'Set-Cookie': [
          'UserID=Jane; Max-Age=3600; Version=',
        'SessionID=asdfgh123456; Path = /'
        ],
      'Content-Type': 'text/html'
      },
    statusCode: 200,
    body: '<html><body><h3>hello</h3></body></html>' }
  }
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js`. Sustituya `<filepath>` por la vía de acceso del archivo `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}
    
3. Borre las cookies de su navegador antes de probar la acción.

4. Pruebe la acción web `hello` abriendo el URL en el navegador. Sustituya las variables `<apihost>` y `<namespace>` y abra `https://<apihost>/api/v1/web/<namespace>/demo/hello`. Ejemplo de `<apihost>`: `us-south.functions.cloud.ibm.com`.

**Resultado**

Se establecen las cookies `UserID=Jane` y `SessionID=asdfgh123456` en las herramientas de desarrollador del navegador.


### Devolver una imagen utilizando una acción web
{: #return_image}
Puede utilizar esta característica en una aplicación web para devolver la imagen de la bandera de un país según la configuración regional del usuario.

**Antes de empezar** 

Cree el paquete `demo` y la acción web `hello` completando los pasos del apartado [Creación de una acción web](#actions_web_example).

Para crear una acción web que devuelva `image/png`: 

1. Guarde el código como `hello.js`.

  ```javascript
  function main() {
      let png = '<base 64 encoded string';
      return { headers: { 'Content-Type': 'image/png' },
              statusCode: 200,
              body: png };
  }
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js`. Sustituya `<filepath>` por la vía de acceso del archivo `hello.js`.

  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Pruebe la acción en el navegador o utilizando un mandato cURL. Sustituya las variables `<apihost>` y `<namespace>`. Puede probar la acción web de una de estas dos formas:

  * Abriendo el URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` en el navegador. 
  * Ejecutando el siguiente mandato cURL.
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Ejecutando el siguiente mandato `wget`.
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}



### Devolver JSON utilizando una acción web
{: #return_json}
Puede utilizar esta característica en una aplicación web para devolver un objeto JSON de información de IP de usuario.

**Antes de empezar** 

Cree el paquete `demo` y la acción web `hello` completando los pasos del apartado [Creación de una acción web](#actions_web_example).

Para crear una acción web que devuelva `application/json`:

1. Guarde el código como `hello.js`.
  ```javascript
  function main(params) {
      return {
          statusCode: 200,
          headers: { 'Content-Type': 'application/json' },
          body: params
      };
  }
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js`. Sustituya `<filepath>` por la vía de acceso del archivo `hello.js`.
  ```
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Pruebe la acción en el navegador o utilizando un mandato cURL. Sustituya las variables `<apihost>` y `<namespace>`. Puede probar la acción web de una de estas dos formas:
  * Abriendo el URL `https://<apihost>/api/v1/web/<namespace>/demo/hello` en el navegador. 
  * Ejecutando el siguiente mandato cURL:
    ```
    curl https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}
  * Ejecutando el siguiente mandato `wget`:
    ```
    wget https://<apihost>/api/v1/web/<namespace>/demo/hello
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    {
      "__ow_headers": {
        "accept": "*/*",
        "accept-encoding": "gzip",
        "cdn-loop": "cloudflare",
        "cf-connecting-ip": "XX.XXX.XXX.XXX",
        "cf-ipcountry": "US",
        "cf-ray": "4d9f3e442a86cf28-IAD",
        "cf-visitor": "{\"scheme\":\"https\"}",
        "host": "<apihost>",
        "user-agent": "curl/7.54.0",
        "x-forwarded-for": "XX.XXX.XX.XXX, XX.XXX.XX.XXX",
        "x-forwarded-host": "<apihost>",
        "x-forwarded-port": "443",
        "x-forwarded-proto": "https",
        "x-global-k8fdic-transaction-id": "11fd03071bd0841d3a00f52354ab880f",
        "x-real-ip": "XXX.XX.XX.XX",
        "x-request-id": "11fd03071bd0841d3a00f52354ab880f"
      },
      "__ow_method": "get",
      "__ow_path": ""
    }
    ```
    {: screen}


### Contexto HTTP
{: #actions_web_context}

Todas las acciones web, cuando se invocan, reciben detalles de solicitud HTTP como parámetros de entrada al argumento de la acción.

| Parámetro HTTP | Tipo | Descripción |
| --- | --- | --- |
| `__ow_method` | Serie | El método HTTP de la solicitud. |
| `__ow_headers` | Correlacionar serie con serie | Las cabeceras de solicitud. |
| `__ow_path` | Serie | La vía de acceso de la solicitud que no coincide (la comparación finaliza al consumirse la extensión de la acción). |
| `__ow_user` | Serie | Espacio de nombres que identifica el asunto autenticado de {{site.data.keyword.openwhisk_short}}. |
| `__ow_body` | Serie | Entidad del cuerpo de la solicitud, como serie codificada en base64 cuando el contenido es un archivo binario o una serie de texto sin formato en caso contrario. |
| `__ow_query` | Serie | Parámetros de la consulta procedentes de la solicitud como serie sin analizar. |

Una solicitud no puede sustituir ninguno de los parámetros `__ow_` mencionados. Si lo hiciera, la solicitud fallaría con el estado 400 Solicitud errónea.

El valor `__ow_user` solo aparece cuando la acción web tiene una [anotación que indica que requiere autenticación](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) y permite que una acción web implemente su propia política de autorización. `__ow_query` solo está disponible cuando una acción web elige manejar la [solicitud HTTP "sin procesar"](#actions_web_raw_enable). `__ow_query` es una serie que contiene los parámetros de la consulta analizados del URI (separados por `&`). La propiedad `__ow_body` aparece en solicitudes HTTP sin procesar o cuando la entidad de la solicitud HTTP no es un objeto JSON ni datos de formulario. De lo contrario, las acciones web reciben parámetros de consulta y de cuerpo como propiedades de primera clase en los argumentos de la acción. Los parámetros de cuerpo prevalecen sobre los parámetros de consulta, que a su vez prevalecen sobre los parámetros de acción y de paquete.

### Soporte para puntos finales HTTPS
{: #actions_web_endpoint}

Protocolos SSL admitidos: TLS 1.2, TLS 1.3 ([versión de borrador 18](https://tools.ietf.org/html/draft-ietf-tls-tls13-18){: external})

### Alteración del contenido de respuesta de la acción web
{: #extra_features}
Puede modificar el contenido de la respuesta de una acción web para que devuelva distintos tipos de contenido utilizando [Extensiones de contenido](#actions_web_extra). 
{: shortdesc}

**Antes de empezar**

Cree el paquete `demo` y la acción web `hello` completando los pasos del apartado [Creación de una acción web](#actions_web_example).

Para alterar la respuesta de una acción web:

1. Guarde el siguiente código como `hello.js`.

  ```javascript
  function main(params) {
      return { response: params };
}
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js`. Sustituya `<filepath>` por la vía de acceso del archivo `hello.js`.

  ```bash
  ibmcloud fn action update demo/hello <filepath>/hello.js --web true
  ```
  {: pre}

3. Pruebe la acción en el navegador o utilizando un mandato cURL. Sustituya las variables `<apihost>` y `<namespace>`.

  a. Devuelva JSON de una de estas dos formas:
    * Abriendo `https://<apihost>/api/v1/web/<namespace>/demo/hello.json` en el navegador web. 
    * Ejecutando el siguiente mandato cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}
    * Ejecutando el siguiente mandato `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json
      ```
      {: pre}

      **Resultado de ejemplo**

      ```
      {
        "response": {
          "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  b. Pruebe la acción utilizando un parámetro de consulta. Puede probar la acción de una de estas
dos formas:
  {: #query_test}

    * Ejecutando el siguiente mandato cURL.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

    * Ejecutando el siguiente mandato `wget`.

        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
        ```
        {: pre}

      **Resultado de ejemplo**
      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "get",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  c. También puede probar la acción web utilizando datos de formulario. Puede probar la acción web de una de estas dos formas:
  {: #form_data}
  
    * Ejecutando el siguiente mandato cURL.

        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}
      
    * Ejecutando el siguiente mandato `wget`.
        ```
        wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -d "name":"Jane"
        ```
        {: pre}

      **Resultado de ejemplo**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "10",
            "content-type": "application/x-www-form-urlencoded",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  d. Puede especificar un objeto JSON ejecutando el siguiente mandato. Puede probar la acción web de una de estas dos formas:
    * Ejecutando el siguiente mandato cURL.
        ```bash
        curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}
      
    * Ejecutando el siguiente mandato `wget`.
        ```
        wget https://<apihost>/api/v1/web/{namespace/demo/hello.json -H 'Content-Type: application/json' -d '{"name":"Jane"}'
        ```
        {: pre}

      **Resultado de ejemplo**

      ```
      {
        "response": {
          "name": "Jane",
    "__ow_method": "post",
    "__ow_headers": {
            "accept": "*/*",
            "connection": "close",
            "content-length": "15",
            "content-type": "application/json",
            "host": "172.17.0.1",
            "user-agent": "curl/7.43.0"
          },
          "__ow_path": ""
        }
      }
      ```
      {: screen}

  e. También puede devolver el valor de `name` como texto de una de estas dos formas:
  * Ejecutando el siguiente mandato cURL.

      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}
  * Ejecutando el siguiente mandato `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.text/response/name?name=Jane
      ```
      {: pre}

    **Resultado de ejemplo**

    ```
    Jane
    ```
    {: screen}

    En las acciones estándar, los parámetros de consulta, datos de formulario y entidades de cuerpo de objeto JSON se tratan como diccionarios ya que se puede acceder directamente a sus valores como propiedades de entrada de la acción. Este comportamiento no se aplica a las acciones web, que manejan las entidades de solicitud HTTP, ni cuando la acción web recibe una entidad que no es un objeto JSON.
    {: note}

  f. Puede establecer el `Content-Type` de una de estas dos formas.
  * Ejecutando el siguiente mandato cURL.  
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}
    
  * Ejecutando el siguiente mandato `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json -H 'Content-Type: text/plain' -d "Jane"
      ```
      {: pre}

    **Resultado de ejemplo**

    ```
    {
      "response": {
        "__ow_method": "post",
    "__ow_headers": {
          "accept": "*/*",
          "connection": "close",
          "content-length": "4",
          "content-type": "text/plain",
          "host": "172.17.0.1",
          "user-agent": "curl/7.43.0"
        },
        "__ow_path": "",
        "__ow_body": "Jane"
      }
    }
    ```
    {: screen}

## Protección de acciones web
{: #actions_web_secure}

**Antes de empezar**
Cree el paquete `demo` y la acción web `hello` completando los pasos del apartado [Creación de una acción web](#actions_web_example).

De forma predeterminada, cualquier persona puede invocar una acción web utilizando el URL de invocación. Puede utilizar la [anotación de acción web](/docs/openwhisk?topic=cloud-functions-annotations#annotations-specific-to-web-actions) `require-whisk-auth` para proteger la acción web de una de estas dos formas:
  1. Estableciendo la anotación `require-whisk-auth` a `true`. Cuando la anotación `require-whisk-auth` está establecida en `true`, la acción web autenticará las credenciales de autorización básicas de la solicitud de invocación comparándola con la clave de autenticación whisk del propietario de la acción web. Cuando se establece en un número o una serie de caracteres sensible a mayúsculas y minúsculas, la solicitud de invocación de la acción web debe incluir la cabecera `X-Require-Whisk-Auth` establecida en este mismo número o serie sensible a mayúsculas y minúsculas. Las acciones web seguras devuelven el mensaje `No autorizado` cuando la validación de credenciales falla.

  2. Permitiendo que la anotación `require-whisk-auth` se establezca automáticamente utilizando el distintivo
`--web-secure`. Cuando el distintivo `--web-secure` tiene el valor `true`, se genera un número aleatorio como valor de la anotación `require-whisk-auth`. Si tiene el valor `false`, la anotación `require-whisk-auth` se elimina.  Si tiene cualquier otro valor, se utiliza dicho valor como el valor de la anotación `require-whisk-auth`.

Para probar una acción web segura:

1. Guarde el siguiente código JavaScript como `hello.js`.
  ```javascript
  function main({name}) {
    var msg = 'You did not tell me who you are.';
  if (name) {
      msg = `Hello, ${name}!`
    }
    return {body: `<html><body><h3>${msg}</h3></body></html>`}
  }
  ```
  {: codeblock}

2. Actualice la acción web `hello` con la nueva versión del código `hello.js` y establezca el distintivo `--web secure` en `true`.
  ```bash
  ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true
  ```
  {: pre}

3. Obtenga la acción web `hello` para ver el valor de `require-whisk-auth` generado aleatoriamente.

  ```bash
  ibmcloud fn action get demo/hello
  ```
  {: pre}

    **Resultado de ejemplo**

    El valor `require-whisk-auth` se ha establecido en `7819991076995522`.
    ```
    {
      "namespace": "<namespace>/demo",
      "name": "hello",
      "version": "0.0.34",
      "exec": {
          "kind": "nodejs:10",
          "binary": false
      },
      "annotations": [
          {
              "key": "web-export",
              "value": true
          },
          {
              "key": "raw-http",
              "value": false
          },
          {
              "key": "final",
              "value": true
          },
          {
              "key": "require-whisk-auth",
              "value": 7819991076995522
          },
          {
              "key": "exec",
              "value": "nodejs:10"
          }
      ],
    "limits": {
          "timeout": 60000,
          "memory": 256,
          "logs": 10,
          "concurrency": 1
      },
    "publish": false
}
    ```
    {: screen}

Para probar que la autenticación está funcionando:

1. Pruebe la acción web `hello` sin establecer el parámetro `X-Require-Whisk-Auth` para verificar que requiere autenticación. Esta prueba dará como resultado un error. Puede probar la acción web de una de estas dos formas:

  * Probando la acción web utilizando un mandato cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}
    
  * Probando la acción web utilizando un mandato `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane
      ```
      {: pre}

   **Resultado de ejemplo**

    ```
      {
      "code": "4c4423556547f6ac764ee192d4ed27a6",
      "error": "Authentication is possible but has failed or not yet been provided."
    }
    ```
    {: screen}

    La invocación falla porque no se ha proporcionado el valor `X-Require-Whisk-Auth`.
    {: note}

2. Ahora, pruebe la acción web `hello` y proporcione el valor `X-Require-Whisk-Auth` generado aleatoriamente. Sustituya los valores de `<apihost>` y `<namespace>`. Sustituya el valor de `<my-secret>` por el número generado aleatoriamente que ha creado en el paso 3. Puede probar la acción web de una de estas dos formas:
  * Probando la acción web utilizando un mandato cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  * Probando la acción web utilizando un mandato `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}

  **Resultado de ejemplo**
    
    ```
    {
    "body": "<html><body><h3>Hello, Jane!</h3></body></html>"
    }
    ```
    {: screen}

Para probar una acción web utilizando un valor de `require-whisk-auth` personalizado:

1. Actualice la acción web `hello` con su propio valor de `require-whisk-auth`. A continuación, intente probar la acción web especificando el valor de `X-Require-Whisk-Auth` durante la invocación.

  a. Establezca un valor de `require-whisk-auth` donde `<my-secret>` es la señal de autenticación sensible a las mayúsculas y minúsculas.
    ```bash
    ibmcloud fn action update demo/hello /<filepath>/hello.js --web true --web-secure true --require-whisk-auth <mysecret>
    ```
    {: pre}
  
  b. Pruebe la acción web e incluya el valor de `<my-secret>`. Puede probar la acción web de una de estas dos formas:
  * Probando la acción web utilizando un mandato cURL.
      ```bash
      curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}
  * Probando la acción utilizando un mandato `wget`.
      ```
      wget https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X GET -H "X-Require-Whisk-Auth: <my-secret>"
      ```
      {: pre}


## Manejo de HTTP sin procesar
{: #actions_web_raw}

Una acción web puede optar por interpretar y procesar un cuerpo HTTP directamente, sin la promoción de un objeto JSON a las propiedades de primera clase disponibles para la entrada de la acción web (es decir, `args.name` frente a analizar `args.__ow_query`). Este proceso se hace mediante `raw-http` [annotation](/docs/openwhisk?topic=cloud-functions-annotations). Utilizando el ejemplo anterior, pero ahora como una acción web HTTP "sin procesar" que recibe `name` como parámetro de consulta y como valor JSON en el cuerpo de la solicitud HTTP:
```bash
curl https://<apihost>/api/v1/web/<namespace>/demo/hello.json?name=Jane -X POST -H "Content-Type: application/json" -d '{"name":"Jane"}'
```
{: pre}


**Resultado de ejemplo**
```
{
  "response": {
    "__ow_method": "post",
    "__ow_query": "name=Jane",
    "__ow_body": "eyJuYW1lIjoiSmFuZSJ9",
    "__ow_headers": {
      "accept": "*/*",
      "connection": "close",
      "content-length": "15",
      "content-type": "application/json",
      "host": "172.17.0.1",
      "user-agent": "curl/7.43.0"
    },
    "__ow_path": ""
  }
}
```
{: screen}

{{site.data.keyword.openwhisk_short}} utiliza la infraestructura [Akka HTTP](https://doc.akka.io/docs/akka-http/current/?language=scala){: external} para [determinar qué tipos de contenido son archivos binarios y cuáles son texto sin formato](https://doc.akka.io/api/akka-http/10.0.4/akka/http/scaladsl/model/MediaTypes$.html){: external}.

### Habilitación del manejo de HTTP sin procesar
{: #actions_web_raw_enable}

Puede crear una serie de acciones web de HTTP sin procesar estableciendo `--web` en `raw`.
```bash
ibmcloud fn action create demo/hello /<filepath>/hello.js --web raw
```
{: pre}

### Decodificación de contenido de cuerpo binario de Base64
{: #actions_web_decode}

Al procesar contenido HTTP sin procesar, el contenido `__ow_body` se codifica en Base64 si la solicitud `Content-Type` es de tipo binario. Las siguientes funciones muestran cómo decodificar el contenido del cuerpo en Node, Python y Swift.

1. Guarde el código de ejemplo en el lenguaje preferido en un archivo llamado `decode.<ext>`. Sustituya `<ext>` por la extensión de archivo del código de ejemplo de su lenguaje preferido.

  **Nodo**
  {: #actions_web_decode_js}

  ```javascript
  function main(args) {
      decoded = new Buffer(args.__ow_body, 'base64').toString('utf-8')
      return {body: decoded}
  }
  ```
  {: codeblock}

  **Python**
  {: #actions_web_decode_python}

  ```python
  def main(args):
    try:
        decoded = args['__ow_body'].decode('base64').strip()
        return {"body": decoded}
    except:
        return {"body": "Could not decode body from Base64."}
  ```
  {: codeblock}

  **Swift**
  {: #actions_web_decode_swift}

  ```swift
  extension String {
      func base64Decode() -> String? {
          guard let data = Data(base64Encoded: self) else {
              return nil
          }

          return String(data: data, encoding: .utf8)
      }
  }

func main(args: [String:Any]) -> [String:Any] {
      if let body = args["__ow_body"] as? String {
          if let decoded = body.base64Decode() {
              return [ "body" : decoded ]
        }
      }

      return ["body": "Could not decode body from Base64."]
  }
  ```
  {: codeblock}

2. Cree una acción web HTTP sin procesar con el código de ejemplo ejecutando el mandato siguiente. En este ejemplo, la función Node se guarda como `decode.js`. Sustituya la vía de acceso de archivo por la vía de acceso del archivo `decode` y actualice la extensión de archivo para que coincida con la extensión del código de ejemplo que ha utilizado.

  ```bash
  ibmcloud fn action create decode <filepath>/decode.js --web raw
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  ok: created action decode
  ```
  {: screen}

3. Pruebe la acción `decode` ejecutando el siguiente mandato cURL. 
    ```bash
    curl -k -H "content-type: application" -X POST -d "Decoded body" https://<apihost>/api/v1/web/<namespace>/default/decode.json
    ```
    {: pre}

  **Resultado de ejemplo**
    ```
    {
      "body": "Decoded body"
    }
    ```
    {: screen}

## Solicitudes de opciones
{: #actions_web_options}

De forma predeterminada, una solicitud `OPTIONS` realizada a una acción web da como resultado que las cabeceras de CORS se añadan automáticamente a las cabeceras de respuesta. Estas cabeceras permiten todos los orígenes y los verbos HTTP `OPTIONS`, `GET`, `DELETE`, `POST`, `PUT`, `HEAD` y `PATCH`.
{: shortdesc}

Consulte las siguientes cabeceras:
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: OPTIONS, GET, DELETE, POST, PUT, HEAD, PATCH
Access-Control-Allow-Headers: Authorization, Content-Type
```

Como alternativa, se pueden manejar solicitudes `OPTIONS` manualmente mediante una acción web. Para habilitar esta opción, añada una anotación de `web-custom-options` con un valor de `true` a una acción web. Cuando esta característica está habilitada, las cabeceras CORS no se añaden automáticamente a la respuesta de la solicitud. En su lugar, debe añadir las cabeceras de forma programática.

Para crear respuestas personalizadas a las solicitudes `OPTIONS`:

1. Guarde el código siguiente en un archivo denominado `custom-options.js`.

  ```js
  function main(params) {
    if (params.__ow_method == "options") {
      return {
        headers: {
          'Access-Control-Allow-Methods': 'OPTIONS, GET',
        'Access-Control-Allow-Origin': 'example.com'
        },
      statusCode: 200
    }
    }
  }
  ```
  {: codeblock}

2. Cree la acción web. Establezca `--web-custom-options` a `true`.

  ```bash
  ibmcloud fn action create custom-option <filepath>/custom-options.js --web true -a web-custom-options true
  ```
  {: pre}

3. Pruebe la acción utilizando el siguiente mandato cURL.

  ```bash
  $ curl https://<apihost>/api/v1/web/<namespace>/default/custom-option.http -kvX OPTIONS
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  < HTTP/1.1 200 OK
< Server: nginx/1.11.13
< Content-Length: 0
< Connection: keep-alive
< Access-Control-Allow-Methods: OPTIONS, GET
< Access-Control-Allow-Origin: example.com
  ```
  {: screen}

## Manejo de errores
{: #actions_web_errors}

Cuando una acción de {{site.data.keyword.openwhisk_short}} falla, hay dos modalidades de anomalía posibles. La primera se conoce como _error de aplicación_ y es similar a una excepción interceptada: la acción devuelve un objeto JSON que contiene una propiedad `error` de nivel superior. La segunda es un _error de desarrollador_, que se produce cuando la acción falla y no genera ninguna respuesta (similar a una excepción no capturada). En el caso de acciones web, el controlador trata los errores de aplicación del siguiente modo:

- Cualquier proyección de vía de acceso especificada se pasa por alto y el controlador proyecta en su lugar la propiedad `error`.
- El controlador aplica el manejo de contenido implícito según la extensión de la acción al valor de la propiedad `error`.

Los desarrolladores deben saber cómo se pueden utilizar las acciones web y deben generar respuestas adecuadas a los errores. Por ejemplo, una acción web que se utilice con la extensión `.http` devuelve una respuesta HTTP, como por ejemplo `{error: { statusCode: 400 }`. Si no es así, se produce una discrepancia entre el `Content-Type` implícito a partir de la extensión y el `Content-Type` de la acción en la respuesta al error. Hay que tener especial cuidado con las acciones web que son secuencias, de forma que los componentes que forman una secuencia puedan generar errores adecuados cuando es necesario.



## Inhabilitación de acciones web
{: #actions_web_disable}

Puede inhabilitar una acción web estableciendo el distintivo `--web` en `false` o `no` en la CLI. Sustituya `<packageName>/<actionName>` y `<filepath>/<filename>` por el nombre de paquete, el nombre de acción web, la vía de acceso de archivo y el nombre de archivo del archivo de código.

```bash
ibmcloud fn action update <packageName>/<actionName> <filepath>/<filename> --web false
```
{: pre}



