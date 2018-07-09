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

# Detalles y límites del sistema
{: #openwhisk_reference}

En las secciones siguientes se proporcionan detalles sobre el sistema {{site.data.keyword.openwhisk}} y los valores límite.
{: shortdesc}

## Entidades de {{site.data.keyword.openwhisk_short}}
{: #openwhisk_entities}

### Espacios de nombres y paquetes
{: #openwhisk_entities_namespaces}

Las acciones, desencadenantes y reglas de {{site.data.keyword.openwhisk_short}} pertenecen a un espacio de nombres y, a veces, a un paquete.

Los paquetes pueden contener acciones y canales de información. Un paquete no puede contener otro paquete, por lo que no se permite anidamiento de paquetes. Además, las entidades no tienen que estar obligatoriamente contenidas en un paquete.

En {{site.data.keyword.Bluemix_notm}}, un par organización+espacio corresponde a un espacio de nombres de {{site.data.keyword.openwhisk_short}}. Por ejemplo,
la organización `BobsOrg` y el espacio `dev` corresponderían al espacio de nombres
`/BobsOrg_dev` de {{site.data.keyword.openwhisk_short}}.

Puede crear sus propios espacios de nombres si está autorizado a ello. El espacio de nombres `/whisk.system`
se reserva para entidades distribuidas con el sistema {{site.data.keyword.openwhisk_short}}.

### Nombres completos
{: #openwhisk_entities_fullyqual}

El nombre completo de una entidad es `/namespaceName[/packageName]/entityName`. Observe que
se utiliza `/` para delimitar espacios de nombres, paquetes y entidades. Además, los espacios de nombres deben
tener una `/` como prefijo.

Por comodidad, el espacio de nombres se puede dejar fuera si es el *espacio de nombre predeterminado* del usuario.

Por ejemplo, supongamos un usuario cuyo espacio de nombres predeterminado es `/myOrg`. A continuación hay ejemplos
de los nombres completos de una serie de entidades y sus alias.

| Nombre completo | Alias | Espacio de nombres | Paquete | Nombre |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Puede utilizar este esquema de denominación cuando usa la CLI de {{site.data.keyword.openwhisk_short}}, entre otros lugares.

### Nombres de entidad
{: #openwhisk_entities_names}

Los nombres de todas las entidades incluidas las acciones, desencadenantes, reglas, paquetes y los espacios de nombres están en una secuencia de caracteres que cumplen el formato siguiente:

* El primer carácter debe ser un carácter alfanumérico o un signo de subrayado.
* Los caracteres posteriores pueden ser alfanuméricos, espacios o cualquiera de los siguientes valores: `_`, `@`, `.`, `-`.
* El último carácter no puede ser un espacio.

Concretamente, un nombre debe coincidir con la siguiente expresión regular (expresada con la sintaxis de metacaracteres de Java): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semánticas de acción
{: #openwhisk_semantics}

En las secciones siguientes se describen los detalles sobre las acciones de
{{site.data.keyword.openwhisk_short}}.

### Falta de estado
{: #openwhisk_semantics_stateless}

Las implementaciones de acciones son sin estado, o *idempotent*. Aunque el sistema no impone esta propiedad, no hay garantía de que cualquier estado mantenido por una acción esté disponible entre invocaciones.

Además, se puede dar la creación de varias instancias de una acción, teniendo cada creación de instancias su propio estado. Una
invocación de acción se podría asignar a cualquiera de estas creaciones de instancias.

### Entrada y salida de invocación
{: #openwhisk_semantics_invocationio}

La entrada y salida de una acción es un diccionario de pares de clave/valor. La clave es una serie, y el valor un valor JSON válido.

### Ordenación de invocaciones de acciones
{: #openwhisk_ordering}

Las invocaciones de una acción no están ordenadas. Si el usuario invoca una acción dos veces desde la línea de mandatos
o la API REST, la segunda invocación podría ejecutarse antes que la primera. Si las acciones tienen efectos secundarios,
se podrían observar en cualquier orden.

Además, no se garantiza que las acciones se ejecuten automáticamente. Dos acciones se pueden ejecutar de forma simultánea
y tener efectos secundarios que se entrelacen. OpenWhisk no asegura ningún modelo de coherencia simultáneo concreto en cuanto a efectos secundarios. Los efectos secundarios de simultaneidad dependen de la implementación.

### Garantías de ejecución de acción
{: #openwhisk_atmostonce}

Cuando se recibe una solicitud de invocación, el sistema registra la solicitud y asigna una activación.

El sistema devuelve un ID de activación (con una invocación sin bloqueo) que confirma que se ha recibido.
Si se produce un error de red o de otro tipo antes de recibir una respuesta HTTP, es posible que {{site.data.keyword.openwhisk_short}} haya recibido y procesado la solicitud.

El sistema intenta invocar la acción una vez, lo que tiene uno de los cuatro resultados siguientes:
- *success*: La invocación de la acción se ha completado correctamente.
- *application error*: La invocación de la acción ha sido correcta, pero la acción ha devuelto un valor de error a propósito, por ejemplo debido a una condición previa de los argumentos que no se cumpla.
- *action developer error*: La acción se ha invocado, pero se ha completado de forma anómala, por ejemplo, la acción no ha detectado una excepción o se ha producido un error de sintaxis.
- *whisk internal error*: El sistema no ha podido invocar la acción.
El resultado se registra en el campo `status` del registro de activación,
como documento en la sección siguiente.

Cada invocación que se recibe correctamente, y que se podría facturar al usuario, tiene un registro de activación.

Si el resultado es *error de desarrollador de acción*, es posible que la acción ejecute de forma parcial y genere efectos colaterales visibles externamente. El usuario es responsable de comprobar si dichos efectos colaterales se han producido y de emitir lógica de reintento si se desea. Determinados *errores internos de whisk* indican que la acción empieza a ejecutarse, pero falla antes de que la acción registre su finalización.

## Registro de activación
{: #openwhisk_ref_activation}

Cada invocación de acción y activación de desencadenante tiene como resultado un registro de activación.

Un registro de activación contiene los campos siguientes:

- *activationId*: el ID de activación.
- *start* y *end*: indicaciones de fecha y hora que registran el inicio y final de la activación. Los valores
están en [formato de hora UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* y `name`: el espacio de nombres y el nombre de la entidad.
- *logs*: una matriz de series con los registros generados por la acción durante su activación. Cada elemento de matriz corresponde
con una salida de línea de `stdout` o `stderr` para la acción, e incluye la hora y secuencia de la salida de registro. La estructura es la siguiente: `TIMESTAMP STREAM: LOG_OUTPUT`.
- *response*: un diccionario que define las claves `success`, `status` y `result`:
  - *status*: el resultado de activación, que puede ser uno de los valores siguientes: "success", "application error", "action developer error", "whisk internal error".
  - *success*: es `true` solo si el estado es `"success"`
- *result*: un diccionario que contiene el resultado de activación. Si la activación ha sido correcta, el resultado contiene el valor devuelto por la acción. Si la activación no ha sido correcta, `result` contiene la clave `error`,
generalmente con una explicación del fallo.

## Acciones JavaScript
{: #openwhisk_ref_javascript}

### Prototipo de función
{: #openwhisk_ref_javascript_fnproto}

Las acciones JavaScript de {{site.data.keyword.openwhisk_short}} se ejecutan en un tiempo de ejecución Node.js.

Las acciones que están escritas en JavaScript se deben confinar a un único archivo. El archivo puede contener varias funciones pero, por convenio, debe existir una función llamada `main` y es la que se llama cuando se invoca la acción. Por ejemplo, el ejemplo siguiente muestra una acción con varias funciones.
```javascript
function main() {
    return { payload: helper() }
}

function helper() {
    return new Date();
}
```
{: codeblock}

Los parámetros de entrada de la acción se pasan como un objeto JSON como un parámetro a la función
`main`. El resultado de la activación correcta también es un objeto JSON pero se devuelve de forma distinta,
según si la acción es síncrona o asíncrona, según se describe en la sección siguiente.

### Comportamiento síncrono y asíncrono
{: #openwhisk_ref_javascript_synchasynch}

Es frecuente para funciones de JavaScript continuar la ejecución en una función de devolución de llamada incluso después de la devolución. Para ajustarse a este comportamiento, una activación de una acción de JavaScript puede ser *síncrona* o *asíncrona*.

Una activación de una acción de JavaScript es **síncrona** si la función main sale bajo una de las condiciones siguientes:

- La función main sale sin ejecutar una sentencia `return`.
- La función main sale al ejecutar una sentencia `return` que devuelve cualquier valor
*excepto* un Promise.

Consulte el siguiente ejemplo de una acción síncrona:

```javascript
// una acción en la que cada vía tiene como resultado una activación síncrona
function main(params) {
  if (params.payload == 0) {
     return;
  } else if (params.payload == 1) {
     return {payload: 'Hello, World!'};
  } else if (params.payload == 2) {
     return {error: 'payload must be 0 or 1'};
  }
}
```
{: codeblock}

La activación de una acción de JavaScript es **asíncrona** si la función main sale devolviendo un Promise. En este caso, el sistema presupone que la acción sigue en ejecución, hasta que se haya cumplimentado o rechazado el Promise.
Empiece por crear una instancia de un nuevo objeto Promise y pasarlo a una función de devolución de llamada. La devolución de llamada tiene dos argumentos, resolve y reject, ambos son funciones. Todo el código asíncrono va dentro de una devolución de llamada.

En el siguiente ejemplo se muestra cómo rellenar un Promise llamando a la función resolve.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         resolve({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Este ejemplo muestra cómo rechazar un Promise llamando a la función reject.
```javascript
function main(args) {
     return new Promise(function(resolve, reject) {
       setTimeout(function() {
         reject({ done: true });
       }, 100);
    })
}
```
{: codeblock}

Es posible que una acción sea síncrona en varias entradas y asíncrona en otras, como se muestra en el siguiente ejemplo.
```javascript
function main(params) {
     if (params.payload) {
        // activación asíncrona
         return new Promise(function(resolve, reject) {
          setTimeout(function() {
            resolve({ done: true });
       }, 100);
    })
     } else {
        // activación síncrona
         return {done: true};
      }
}
```
{: codeblock}

Independientemente de si la activación es síncrona o asíncrona, la invocación de la acción puede ser o no de bloqueo (blocking o non-blocking).

### Objeto whisk global de JavaScript eliminado

El objeto `whisk` se ha eliminado; migre sus acciones nodejs para que utilicen métodos alternativos.
Para las funciones `whisk.invoke()` y `whisk.trigger()`, utilice [openwhisk](https://www.npmjs.com/package/openwhisk) instalado de la biblioteca del cliente.
Para `whisk.getAuthKey()`, puede obtener el valor de la clave de API de la variable de entorno `__OW_API_KEY`.
Para `whisk.error()`, puede devolver un objeto Promise rechazado (es decir, Promise.reject).

### Entornos de ejecución JavaScript
{: #openwhisk_ref_javascript_environments}

Las acciones JavaScript se pueden ejecutar en Node.js versión 6 o Node.js versión 8.
Actualmente las acciones JavaScript se ejecutan de forma predeterminada en un entorno de Node.js versión 6.
### Empaquetado de paquetes npm con las acciones
Cualquier paquete `npm` que no esté preinstalado en el entorno de Node.js se puede empaquetar como dependencia al crear o actualizar la acción.

Para obtener más información, consulte [Empaquetado de una acción como un módulo Node.js](./openwhisk_actions.html#openwhisk_js_packaged_action) o [Empaquetado de una acción como un paquete individual](./openwhisk_actions.html#openwhisk_js_webpack_action).

### Entorno Node.js versión 8 con SDK de IBM
{: #openwhisk_ref_javascript_environments_8}
Se utiliza el entorno Node.js versión 8.11.1 si se especifica de forma explícita el distintivo `--kind` con un valor de `nodejs:8` al crear o actualizar una acción.

Los paquetes siguientes están preinstalados en el entorno de Node.js versión 8.11.1:
  - [amqplib v0.5.2](https://www.npmjs.com/package/amqplib): una biblioteca para crear clientes AMQP 0-9-1 para Node.JS.
  - [apn v2.2.0](https://www.npmjs.com/package/apn): un módulo Node.js para interactuar con el servicio de notificaciones push de Apple.
  - [async v2.6.1](https://www.npmjs.com/package/async): proporciona funciones para trabajar con funciones asíncronas.
  - [bent v1.1.0](https://www.npmjs.com/package/bent) - cliente HTTP funcional para Node.js w/ async/await.
  - [body-parser v1.18.3](https://www.npmjs.com/package/body-parser) - middleware de análisis del cuerpo de Node.js. Analice los cuerpos de las solicitudes de entrada en un middleware antes que los manejadores, disponibles en la propiedad req.body.
  - [btoa v1.2.1](https://www.npmjs.com/package/btoa): un puerto de la función btoa del navegador.
  - [cassandra-driver v3.5.0](https://www.npmjs.com/package/cassandra-driver) - controlador de DataStax Node.js para Apache Cassandra.
  - [cloudant v1.10.0](https://www.npmjs.com/package/cloudant): esta es la biblioteca oficial de Cloudant para Node.js.
  - [@cloudant/cloudant v2.2.0](https://www.npmjs.com/package/cloudant): esta es la biblioteca oficial de Cloudant para Node.js.
  - [commander v2.15.1](https://www.npmjs.com/package/commander): la solución completa para las interfaces de línea de mandatos de Node.js.
  - [composeaddresstranslator v1.0.4](https://www.npmjs.com/package/composeaddresstranslator) - conversor de direcciones de IU o API de Compose para bases de datos Scylla.
  - [consul v0.32.0](https://www.npmjs.com/package/consul): un cliente para Consul, que incluye configuración y descubrimiento de servicio.
  - [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser): analizar cabeceras de cookies y rellenar req.cookies con un objeto codificado por los nombres de cookie.
  - [cradle v0.7.1](https://www.npmjs.com/package/cradle): un cliente CouchDB, de almacenamiento en memoria caché y alto nivel para Node.js.
  - [elasticsearch v15.0.0](https://www.npmjs.com/package/elasticsearch): cliente oficial de Elasticsearch de bajo nivel para Node.js.
  - [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler): middleware manejador de errores solo de desarrollo.
  - [etcd3 v0.2.11](https://www.npmjs.com/package/etcd3): cliente listo para producción de alta calidad para la API etcdv3 basada en almacenamiento intermedio de protocolo.
  - [express v4.16.3](https://www.npmjs.com/package/express): infraestructura de web rápida y minimalista para el nodo.
  - [express-session v1.15.6](https://www.npmjs.com/package/express-session): permite gestionar sesiones desde dentro de una app Express.
  - [formidable v1.2.1](https://www.npmjs.com/package/formidable): módulo Node.js para analizar datos de formulario, especialmente cargas de archivos.
  - [glob v7.1.2](https://www.npmjs.com/package/glob): combinar archivos mediante patrones que utiliza el shell, como estrellas y elementos por el estilo.
  - [gm v1.23.1](https://www.npmjs.com/package/gm): GraphicsMagick e ImageMagick para Node.
  - [ibm-cos-sdk v1.2.1](https://www.npmjs.com/package/ibm-cos-sdk) - SDK de {{site.data.keyword.cos_full}} para Node.js
  - [ibm_db v2.4.0](https://www.npmjs.com/package/ibm_db): una interfaz asíncrona/síncrona para node.js, para IBM DB2 e IBM Informix.
  - [ibmiotf v0.2.41](https://www.npmjs.com/package/ibmiotf): el cliente node.js se utiliza para simplificar la interacción con la plataforma de Internet de las cosas de IBM Watson.
  - [iconv-lite v0.4.23](https://www.npmjs.com/package/iconv-lite): conversión de codificación de caracteres de JS puro.
  - [jsdom v11.10.0](https://www.npmjs.com/package/jsdom) - jsdom es una implementación JavaScript pura de muchos estándares de la web, especialmente los estándares WHATWG DOM y HTML.
  - [jsonwebtoken v8.2.2](https://www.npmjs.com/package/jsonwebtoken): una implementación de señales web de JSON.
  - [lodash v4.17.10](https://www.npmjs.com/package/lodash): la biblioteca de Lodash que se exporta como módulos Node.js.
  - [log4js v2.8.0](https://www.npmjs.com/package/log4js): es una conversión de la infraestructura log4js diseñada para trabajar con Node.
  - [marked v0.4.0](https://www.npmjs.com/package/marked): un completo analizador y compilador de markdown, escrito en JavaScript. Diseñado para ofrecer rapidez.
  - [merge v1.2.0](https://www.npmjs.com/package/merge): fusionar varios objetos en uno y, opcionalmente, crear un nuevo objeto clonado.
  - [moment v2.22.2](https://www.npmjs.com/package/moment): una biblioteca de fechas JavaScript ligera para analizar, validar, manipular y formatear fechas.
  - [mongodb v3.0.10](https://www.npmjs.com/package/mongodb): el controlador oficial de MongoDB para Node.js.
  - [mysql v2.15.0](https://www.npmjs.com/package/mysql): controlador de node.js para mysql.
  - [mustache v2.3.0](https://www.npmjs.com/package/mustache): mustache.js es una implementación del sistema de plantillas mustache en JavaScript.
  - [nano v6.4.4](https://www.npmjs.com/package/nano): controlador minimalista de couchdb para Node.js.
  - [nodemailer v4.6.5](https://www.npmjs.com/package/nodemailer): enviar correos electrónicos desde Node.js, ¡es pan comido!
  - [oauth2-server v3.0.0](https://www.npmjs.com/package/oauth2-server): módulo completo, compatible y bien probado para implementar un proveedor/servidor de OAuth2 con express en Node.js.
  - [openwhisk v3.15.0](https://www.npmjs.com/package/openwhisk): biblioteca de cliente JavaScript para la plataforma OpenWhisk. Proporciona un derivador alrededor de las API de OpenWhisk.
  - [path-to-regex v2.2.1](https://www.npmjs.com/package/path-to-regexp): convierte una serie de vía de acceso, como /user/:name, en una expresión regular que luego se puede comparar con vías de acceso URL.
  - [pg v7.4.3](https://www.npmjs.com/package/pg): cliente PostgreSQL de no bloqueo para node.js. JavaScript puro y enlaces a libpq nativos opcionales.
  - [process v0.11.10](https://www.npmjs.com/package/process): require('process'); como cualquier otro módulo.
  - [pug v2.0.3](https://www.npmjs.com/package/pug): implementa el lenguaje de creación de plantillas Pug.
  - [redis v2.8.0](https://www.npmjs.com/package/redis): se trata de un completo cliente Redis para Node.js con muchas funciones.
  - [request v2.87.0](https://www.npmjs.com/package/request): Request está diseñado para ser la forma más sencilla posible de realizar llamadas HTTP.
  - [request-promise v4.2.2](https://www.npmjs.com/package/request-promise): la versión simplificada del cliente de solicitudes HTTP 'request' con soporte de Promise. Basado en Bluebird.
  - [rimraf v2.6.2](https://www.npmjs.com/package/rimraf): el mandato de UNIX rm -rf para Node.
  - [semver v5.5.0](https://www.npmjs.com/package/semver): mantenimiento de versiones semánticas para Nodejs
  - [@sendgrid/mail@6.2.1](https://www.npmjs.com/package/@sendgrid/mail): proporciona soporte de correo electrónico mediante la API de SendGrid.
  - [serve-favicon v2.5.0](https://www.npmjs.com/package/serve-favicon): middleware de Node.js para el servicio de favicon.
  - [superagent v3.8.3](https://www.npmjs.com/package/superagent): SuperAgent es una biblioteca progresiva de solicitudes HTTP del lado del cliente, y un módulo Node.js con la misma API, que tiene muchas funciones de cliente HTTP de alto nivel.
  - [twilio v3.17.2](https://www.npmjs.com/package/twilio): un derivador para la API de Twilio, relacionado con voz, vídeo y mensajería.
  - [underscore v1.9.1](https://www.npmjs.com/package/underscore): Underscore.js es una biblioteca de programas de utilidad para JavaScript que proporciona soporte para los aspectos funcionales habituales (each, map, reduce, filter...) sin ampliar ningún objeto JavaScript principal.
  - [url-pattern v1.0.3](https://www.npmjs.com/package/url-pattern): analiza los URL correspondientes a parámetros de vía de acceso más fácilmente que un comparador de series regex.
  - [uuid v3.2.1](https://www.npmjs.com/package/uuid): generación simple y rápida de UUID RFC4122.
  - [validator v10.3.0](https://www.npmjs.com/package/validator): una biblioteca de sanitizantes y validadores de series.
  - [watson-developer-cloud v3.4.5](https://www.npmjs.com/package/watson-developer-cloud): biblioteca de cliente de Node.js para utilizar los servicios de Watson Developer Cloud, un conjunto de API que utilizan la computación cognitiva para resolver problemas complejos.
  - [when v3.7.8](https://www.npmjs.com/package/when): When.js es una sólida implementación de Promises/A+ y when() "a prueba de bombas", que incluye un completo corrector de compatibilidad ES6 Promise.
  - [winston v2.4.2](https://www.npmjs.com/package/winston): una biblioteca de registro asíncrono multitransporte para node.js. "TRANQUILO, WINSTON ... yo lo pongo en los registros".
  - [ws v5.2.0](https://www.npmjs.com/package/ws): ws es una implementación de cliente y servidor WebSocket fácil de utilizar, ultrarrápida y probada de forma exhaustiva.
  - [xml2js v0.4.19](https://www.npmjs.com/package/xml2js): sencillo conversor de objetos de XML a JavaScript. Admite conversión bidireccional.
  - [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest): node-XMLHttpRequest es un derivador para el cliente http incorporado para emular el objeto XMLHttpRequest del navegador.
  - [yauzl v2.9.1](https://www.npmjs.com/package/yauzl): otra biblioteca de descompresión de archivos .zip para Node. Para comprimir.

### Entorno de Node.js versión 6
{: #openwhisk_ref_javascript_environments_6}
Se utiliza el entorno Node.js 6.14.0 si se especifica de forma explícita el distintivo `--kind` con un valor de `nodejs:6` al crear o actualizar la acción.

Los paquetes siguientes están disponibles para su uso en el entorno de Node.js 6.14.0:

- [apn v2.1.2](https://www.npmjs.com/package/apn): un módulo Node.js para interactuar con el servicio de notificaciones push de Apple.
- [async v2.1.4](https://www.npmjs.com/package/async): proporciona funciones para trabajar con funciones asíncronas.
- [btoa v1.1.2](https://www.npmjs.com/package/btoa): un puerto de la función btoa del navegador.
- [cheerio v0.22.0](https://www.npmjs.com/package/cheerio): implementación rápida, flexible y óptima de jQuery principal, diseñada específicamente para el servidor.
- [cloudant v1.6.2](https://www.npmjs.com/package/cloudant): la biblioteca oficial de Cloudant para Node.js.
- [commander v2.9.0](https://www.npmjs.com/package/commander): la solución completa para las interfaces de línea de mandatos de Node.js.
- [consul v0.27.0](https://www.npmjs.com/package/consul): un cliente para Consul, que incluye configuración y descubrimiento de servicio.
- [cookie-parser v1.4.3](https://www.npmjs.com/package/cookie-parser): analizar cabeceras de cookies y rellenar req.cookies con un objeto codificado por los nombres de cookie.
- [cradle v0.7.1](https://www.npmjs.com/package/cradle): un cliente CouchDB, de almacenamiento en memoria caché y alto nivel para Node.js.
- [errorhandler v1.5.0](https://www.npmjs.com/package/errorhandler): middleware manejador de errores solo de desarrollo.
- [glob v7.1.1](https://www.npmjs.com/package/glob): combinar archivos mediante patrones que utiliza el shell, como estrellas y elementos por el estilo.
- [gm v1.23.0](https://www.npmjs.com/package/gm): GraphicsMagick e ImageMagick para Node.
- [lodash v4.17.2](https://www.npmjs.com/package/lodash): la biblioteca de Lodash que se exporta como módulos Node.js.
- [log4js v0.6.38](https://www.npmjs.com/package/log4js): una conversión de la infraestructura log4js diseñada para trabajar con Node.
- [iconv-lite v0.4.15](https://www.npmjs.com/package/iconv-lite): conversión de codificación de caracteres de JS puro.
- [marked v0.3.6](https://www.npmjs.com/package/marked): un completo analizador y compilador de markdown, escrito en JavaScript. Diseñado para ofrecer rapidez.
- [merge v1.2.0](https://www.npmjs.com/package/merge): fusionar varios objetos en uno para crear un nuevo objeto clonado.
- [moment v2.17.0](https://www.npmjs.com/package/moment): una biblioteca de fechas JavaScript ligera para analizar, validar, manipular y formatear fechas.
- [mongodb v2.2.11](https://www.npmjs.com/package/mongodb): el controlador oficial de MongoDB para Node.js.
- [mustache v2.3.0](https://www.npmjs.com/package/mustache): Mustache.js es una implementación del sistema de plantillas mustache en JavaScript.
- [nano v6.2.0](https://www.npmjs.com/package/nano): controlador minimalista de couchdb para Node.js.
- [node-uuid v1.4.7](https://www.npmjs.com/package/node-uuid): UUID en desuso empaquetado.
- [nodemailer v2.6.4](https://www.npmjs.com/package/nodemailer): enviar correos electrónicos desde Node.js, ¡es pan comido!
- [oauth2-server v2.4.1](https://www.npmjs.com/package/oauth2-server): módulo completo, compatible y bien probado para implementar un proveedor/servidor de OAuth2 con express en Node.js.
- [openwhisk v3.14.0](https://www.npmjs.com/package/openwhisk): biblioteca de cliente JavaScript para la plataforma OpenWhisk. Proporciona un derivador alrededor de las API de OpenWhisk.
- [pkgcloud v1.4.0](https://www.npmjs.com/package/pkgcloud): pkgcloud es una biblioteca estándar para Node.js que no tiene en cuenta las diferencias entre varios proveedores de nube.
- [process v0.11.9](https://www.npmjs.com/package/process): require('process'); como cualquier otro módulo.
- [pug v2.0.0-beta6](https://www.npmjs.com/package/pug): implementa el lenguaje de creación de plantillas Pug.
- [redis v2.6.3](https://www.npmjs.com/package/redis): un completo cliente Redis para Node.js con muchas funciones.
- [request v2.79.0](https://www.npmjs.com/package/request): Request es la forma más sencilla posible de realizar llamadas HTTP.
- [request-promise v4.1.1](https://www.npmjs.com/package/request-promise): la versión simplificada del cliente de solicitudes HTTP 'request' con soporte de Promise. Basado en Bluebird.
- [rimraf v2.5.4](https://www.npmjs.com/package/rimraf): el mandato de UNIX rm -rf para Node.
- [semver v5.3.0](https://www.npmjs.com/package/semver): admite el mantenimiento de versiones semánticas.
- [sendgrid v4.7.1](https://www.npmjs.com/package/sendgrid): proporciona soporte de correo electrónico con la API de SendGrid.
- [serve-favicon v2.3.2](https://www.npmjs.com/package/serve-favicon): middleware de Node.js para el servicio de favicon.
- [socket.io v1.6.0](https://www.npmjs.com/package/socket.io): Socket.IO permite la comunicación bidireccional basada en sucesos en tiempo real.
- [socket.io-client v1.6.0](https://www.npmjs.com/package/socket.io-client): soporte del lado del cliente para Socket.IO.
- [superagent v3.0.0](https://www.npmjs.com/package/superagent): SuperAgent es una biblioteca progresiva de solicitudes HTTP del lado del cliente, y un módulo Node.js con la misma API, que tiene muchas funciones de cliente HTTP de alto nivel.
- [swagger-tools v0.10.1](https://www.npmjs.com/package/swagger-tools): herramientas relacionadas con el uso de Swagger, una forma de documentar API.
- [tmp v0.0.31](https://www.npmjs.com/package/tmp): un creador sencillo de archivos temporales y directorios para node.js.
- [twilio v2.11.1](https://www.npmjs.com/package/twilio): un derivador para la API de Twilio, relacionado con voz, vídeo y mensajería.
- [underscore v1.8.3](https://www.npmjs.com/package/underscore): Underscore.js es una biblioteca de programas de utilidad para JavaScript que admite los requisitos funcionales habituales (each, map, reduce, filter...) sin ampliar ningún objeto JavaScript principal.
- [uuid v3.0.0](https://www.npmjs.com/package/uuid): generación simple y rápida de UUID RFC4122.
- [validator v6.1.0](https://www.npmjs.com/package/validator): una biblioteca de sanitizantes y validadores de series.
- [watson-developer-cloud v2.29.0](https://www.npmjs.com/package/watson-developer-cloud): biblioteca de cliente de Node.js para utilizar los servicios de Watson Developer Cloud, un conjunto de API que utilizan la computación cognitiva para resolver problemas complejos.
- [when v3.7.7](https://www.npmjs.com/package/when): When.js es una sólida implementación de Promises/A+ y when() a prueba de bombas, que incluye un completo corrector de compatibilidad ES6 Promise.
- [winston v2.3.0](https://www.npmjs.com/package/winston): una biblioteca de registro asíncrono multitransporte para node.js. "TRANQUILO, WINSTON ... yo lo pongo en los registros".
- [ws v1.1.1](https://www.npmjs.com/package/ws): ws es una implementación de cliente y servidor WebSocket fácil de utilizar, ultrarrápida y probada de forma exhaustiva.
- [xml2js v0.4.17](https://www.npmjs.com/package/xml2js): sencillo conversor de objetos de XML a JavaScript. Admite conversión bidireccional.
- [xmlhttprequest v1.8.0](https://www.npmjs.com/package/xmlhttprequest): node-XMLHttpRequest es un derivador para el cliente http incorporado para emular el objeto XMLHttpRequest del navegador.
- [yauzl v2.7.0](https://www.npmjs.com/package/yauzl): otra biblioteca de descompresión de archivos .zip para Node. Para comprimir.


## Entornos de tiempo de ejecución Python
{: #openwhisk_ref_python_environments}

OpenWhisk da soporte a la ejecución de acciones de Python utilizando dos versiones diferentes de tiempo de ejecución.

### Acciones de Python 3 (basadas en Jessie)
{: #openwhisk_ref_python_environments_jessie}

Las acciones de Python 3 se ejecutan con Python 3.6.5. Para utilizar este tiempo de ejecución, especifique el parámetro de la CLI de `wsk` `--kind python-jessie:3` cuando cree o actualice una acción.
Al crear acciones de python mediante virtualenv, utilice la imagen de docker `ibmfunctions/action-python-v3`.
El tiempo de ejecución contiene paquetes SDK para los servicios de IBM Cloud disponibles que las acciones de Python pueden utilizar, además de las bibliotecas estándares de Python 3.6.

Versión de Python:
- [3.6.5](https://github.com/docker-library/python/blob/a1aa406bfd8c7b129e6e0ee0ba972b863624ac0d/3.6/jessie/Dockerfile)

Paquetes de Python:
- asn1crypto==0.24.0
- attrs==17.4.0
- Automat==0.6.0
- beautifulsoup4==4.6.0
- botocore==1.9.4
- cassandra-driver==3.14.0
- certifi==2018.1.18
- cffi==1.11.5
- chardet==3.0.4
- click==6.7
- cloudant==2.8.1
- constantly==15.1.0
- cryptography==2.1.4
- cssselect==1.0.3
- docutils==0.14
- elasticsearch==6.2.0
- Flask==1.0.2
- gevent==1.2.2
- greenlet==0.4.13
- httplib2==0.11.3
- hyperlink==18.0.0
- ibm-cos-sdk==2.1.1
- ibm-cos-sdk-core==2.1.1
- ibm-cos-sdk-s3transfer==2.1.1
- ibm-db==2.0.8a0
- ibmcloudsql==0.2.13
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.10
- jmespath==0.9.3
- kafka-python==1.4.3
- lxml==4.2.1
- MarkupSafe==1.0
- numpy==1.14.4
- pandas==0.23.0
- parsel==1.4.0
- pika==0.11.2
- Pillow==5.1.0
- psycopg2==2.7.4
- pyasn1==0.4.2
- pyasn1-modules==0.2.1
- pycparser==2.18
- PyDispatcher==2.0.5
- pymongo==3.6.1
- pyOpenSSL==17.5.0
- pysolr==3.7.0
- python-dateutil==2.7.3
- pytz==2018.3
- queuelib==1.4.2
- redis==2.10.6
- requests==2.18.4
- scikit-learn==0.19.1
- scipy==1.1.0
- Scrapy==1.5.0
- service-identity==17.0.0
- simplejson==3.15.0
- six==1.11.0
- tornado==5.0.2
- Twisted==18.4.0
- urllib3==1.22
- virtualenv==16.0.0
- w3lib==1.19.0
- watson-developer-cloud==1.3.5
- Werkzeug==0.14.1
- zope.interface==4.4.3

### Acciones de Python 3 (basadas en Alpine)
{: #openwhisk_ref_python_environments_alpine}

Las acciones de Python 3 se ejecutan con Python 3.6.1. Para utilizar este tiempo de ejecución, especifique el parámetro de la CLI de `wsk` `--kind python:3` cuando cree o actualice una acción.
Al crear acciones de python mediante virtualenv, utilice la imagen de docker `openwhisk/python3action`.
Las acciones de Python pueden utilizar los paquetes siguientes, además de las bibliotecas estándares de Python 3.6.

Paquetes de Python:
- asn1crypto==0.23.0
- attrs==17.3.0
- Automat==0.6.0
- beautifulsoup4==4.5.3
- cffi==1.11.2
- click==6.7
- constantly==15.1.0
- cryptography==2.1.3
- cssselect==1.0.1
- Flask==0.12
- gevent==1.2.1
- greenlet==0.4.12
- httplib2==0.10.3
- idna==2.6
- incremental==17.5.0
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.4
- lxml==3.7.3
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.5
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.6.0
- queuelib==1.4.2
- requests==2.13.0
- Scrapy==1.3.3
- service-identity==17.0.0
- simplejson==3.10.0
- six==1.11.0
- Twisted==17.1.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

### Acciones de Python 2

Las acciones de Python 2 se ejecutan con Python 2.7.12, que es el tiempo de ejecución predeterminado para acciones de Python, a menos que se especifique el distintivo `--kind` al crear o actualizar una acción. Para seleccionar este tiempo de ejecución de forma explícita, utilice `--kind python:2`.
Al crear acciones de python mediante virtualenv, utilice la imagen de docker `openwhisk/python2action`.
Las acciones de Python 2 pueden utilizar los paquetes siguientes, además de las bibliotecas estándares de Python 2.7.

Paquetes de Python:
- asn1crypto==0.23.0
- attrs==17.2.0
- beautifulsoup4==4.5.1
- cffi==1.11.1
- click==6.7
- cryptography==2.0.3
- cssselect==1.0.1
- enum34==1.1.6
- Flask==0.11.1
- gevent==1.1.2
- greenlet==0.4.12
- httplib2==0.9.2
- idna==2.6
- ipaddress==1.0.18
- itsdangerous==0.24
- Jinja2==2.9.6
- kafka-python==1.3.1
- lxml==3.6.4
- MarkupSafe==1.0
- parsel==1.2.0
- pyasn1==0.3.7
- pyasn1-modules==0.1.4
- pycparser==2.18
- PyDispatcher==2.0.5
- pyOpenSSL==17.3.0
- python-dateutil==2.5.3
- queuelib==1.4.2
- requests==2.11.1
- Scrapy==1.1.2
- service-identity==17.0.0
- simplejson==3.8.2
- six==1.11.0
- Twisted==16.4.0
- virtualenv==15.1.0
- w3lib==1.18.0
- Werkzeug==0.12.2
- zope.interface==4.4.3

## Acciones Swift
{: #swift-actions}

### Swift 3
Las acciones Swift 3 se ejecutan con Swift 3.1.1 `--kind swift:3.1.1`. Especifique siempre el tipo `swift:3.1.1`, ya que las versiones previas de Swift no están admitidas.

Debe migrar todas las acciones de Swift para utilizar el tipo `swift:3.1.1`. Como práctica recomendada, proporcione siempre el tipo específico al crear o actualizar acciones.
{: tip}

Las acciones Swift 3.1.1 pueden utilizar los paquetes siguientes cuando se utiliza un solo archivo de origen Swift:
- KituraNet versión 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON versión 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK versión 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Las acciones Swift 4 se ejecutan utilizando Swift 4.1 `--kind swift:4.1`.

Siga las instrucciones correspondientes a [acciones de swift empaquetadas](./openwhisk_actions.html#packaging-an-action-as-a-swift-executable) para incluir dependencias mediante Package.swift.

Las acciones Swift 4.1 pueden utilizar los paquetes siguientes cuando se utiliza un solo archivo de origen Swift:
- Watson Developer Cloud SDK versión 0.27.0, https://github.com/watson-developer-cloud/swift-sdk

### Migración de Swift 3.1.1 a Swift 4.1

#### SwiftyJSON con un solo archivo de acción de origen
Si tiene una acción `swift:3.1.1` que no está compilada, solo un archivo de origen que utiliza el paquete **SwiftyJSON**,
debe precompilar la acción y especificar la versión de SwiftyJSON que desea utilizar para la acción de tipo `swift:4.1`. Tenga en cuenta que, a partir de Swift 4.1, dispone de un mejor soporte para gestionar los datos JSON de forma nativa.

## Acciones PHP
{: #openwhisk_ref_php}

Las acciones PHP se ejecutan con PHP 7.1.18. Para utilizar este tiempo de ejecución, especifique el parámetro de la CLI de `wsk` `--kind php:7.1` cuando cree o actualice una acción. Este comportamiento es el predeterminado al crear una acción con un archivo que tiene una extensión `.php`.

Las siguientes extensiones PHP están disponibles, además de las estándares:

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

### Paquetes de Composer
Los siguientes paquetes de Composer también están disponibles:

- guzzlehttp/guzzle       v6.7.3
- ramsey/uuid             v3.6.3

## Acciones de Docker
{: #openwhisk_ref_docker}

Las acciones Docker ejecutan un binario proporcionado por el usuario en un contenedor Docker. El binario se ejecuta en una imagen Docker basada en [python:2.7.12-alpine](https://hub.docker.com/r/library/python), por lo que el binario debe ser compatible con dicha distribución.

El esqueleto de Docker es una forma cómoda de crear imágenes Docker compatibles con OpenWhisk. Puede instalar el esqueleto con el mandato de plug-in de CLI `ibmcloud wsk sdk install docker`.

El programa binario principal debe estar en `/action/exec` dentro del contenedor. El ejecutable recibe los argumentos de entrada desde una serie de argumento de línea de mandatos, que se puede deserializar como un objeto `JSON`. Debe devolver un resultado utilizando `stdout` como una serie de una línea de `JSON` serializado.

Puede incluir los pasos de compilación o dependencias modificando el `archivo de Docker` incluido en `dockerSkeleton`.

## API REST
{: #openwhisk_ref_restapi}
La información sobre la API REST de {{site.data.keyword.openwhisk_short}} se encuentra en la [referencia de API REST](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

## Límites del sistema
{: #openwhisk_syslimits}

### Acciones
{{site.data.keyword.openwhisk_short}} tiene algunos límites del sistema, incluyendo la cantidad de memoria que puede utilizar una acción y
cuántas invocaciones de acción se permiten por minuto.

En la tabla siguiente se proporciona una lista con los límites predeterminados de las acciones.

| Límite | Descripción | Valor predeterminado | Mínimo | Máximo |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](openwhisk_reference.html#openwhisk_syslimits_codesize) | El tamaño máximo del código de acción en MB. | 48 | 1 | 48 |
| [concurrent](openwhisk_reference.html#openwhisk_syslimits_concurrent) | No se pueden enviar más de N activaciones por espacio de nombres ya sea ejecutándose o en cola para la ejecución. | 1000 | 1 | 1000* |
| [logs](openwhisk_reference.html#openwhisk_syslimits_logs) | Un contenedor no tiene permiso para escribir más de N MB en stdout. | 10 | 0 | 10 |
| [memory](openwhisk_reference.html#openwhisk_syslimits_memory) | Un contenedor no tiene permiso para asignar más de n MB de memoria. | 256 | 128 | 512 |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_minuterate) | No se pueden enviar más de N activaciones por espacio de nombres por minuto. | 5000 | 1 | 5000* |
| [openulimit](openwhisk_reference.html#openwhisk_syslimits_openulimit) | El número máximo de archivos abiertos para una acción. | 1024 | 0 | 1024 |
| [parameters](openwhisk_reference.html#openwhisk_syslimits_parameters) | El tamaño máximo de los parámetros que se pueden adjuntar en MB. | 1 | 0 | 1 |
| [proculimit](openwhisk_reference.html#openwhisk_syslimits_proculimit) | El número máximo de procesos disponibles para una acción. | 1024 | 0 | 1024 |
| [result](openwhisk_reference.html#openwhisk_syslimits_result) | El tamaño máximo de los resultados de la invocación de acción en MB. | 1 | 0 | 1 |
| [sequenceMaxActions](openwhisk_reference.html#openwhisk_syslimits_sequencemax) | El número máximo de acciones que puede contener una secuencia determinada. | 50 | 0 | 50* |
| [timeout](openwhisk_reference.html#openwhisk_syslimits_timeout) | Un contenedor no tiene permiso para ejecutarse más de N milisegundos. | 60000 | 100 | 600000 |

### Aumento de los límites fijos
{: #increase_fixed_limit}

Los valores límite seguidos de (*) son fijos, pero se pueden aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Si desea aumentar el valor límite, póngase en contacto con el soporte de IBM para abrir una incidencia directa desde la [consola web de IBM {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/).
  1. Seleccione **Support**.
  2. Seleccione **Add Ticket** en el menú desplegable.
  3. Seleccione **Technical** para el tipo de incidencia.
  4. Seleccione **Functions** para el área técnica de soporte.

#### codeSize (MB) (fijo: 48 MB)
{: #openwhisk_syslimits_codesize}
* El tamaño máximo de código de la acción es de 48 MB.
* Se recomienda que, para una acción de JavaScript, se utilice una herramienta para concatenar todo el código fuente, incluidas las dependencias, en un único archivo empaquetado.
* Este límite es fijo y no se puede cambiar.

#### concurrent (fijo: 1000*)
{: #openwhisk_syslimits_concurrent}
* El número de activaciones que se están ejecutando o en cola para su ejecución para un espacio de nombres no puede superar 1000.
* Este valor límite es fijo, pero se puede aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Consulte la sección [Aumento de los límites fijos](openwhisk_reference.html#increase_fixed_limit) para obtener instrucciones detalladas sobre cómo aumentar este límite.

#### logs (MB) (predeterminado: 10 MB)
{: #openwhisk_syslimits_logs}
* El límite de registro N está en el intervalo [0 MB..10 MB] y se establece por acción.
* Un usuario puede cambiar el límite de registro de acción cuando se crea o se actualiza una acción.
* Los registros que superen el límite establecido se truncan, de forma que se ignora cualquier entrada de registro nueva, y se añade un aviso como última salida de la activación para indicar que la activación ha sobrepasado el límite de registro establecido.

#### memory (MB) (predeterminado: 256 MB)
{: #openwhisk_syslimits_memory}
* El límite de memoria M está en el intervalo [128 MB..512 MB] y se establece por acción en MB.
* Un usuario puede cambiar el límite de memoria cuando se crea una acción.
* Un contenedor no puede utilizar más memoria que la que asigna el límite.

#### minuteRate (fijo: 5000*)
{: #openwhisk_syslimits_minuterate}
* El límite de tasa N se establece en 5000 y limita el número de invocaciones de acción en espacios de 1 minuto.
* Una llamada de la CLI o API que sobrepase este límite recibe un código de error correspondiente al código de estado de HTTP `429: DEMASIADAS SOLICITUDES`.
* Este valor límite es fijo, pero se puede aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Consulte la sección [Aumento de los límites fijos](openwhisk_reference.html#increase_fixed_limit) para obtener instrucciones detalladas sobre cómo aumentar este límite.

#### openulimit (fijo: 1024:1024)
{: #openwhisk_syslimits_openulimit}
* El número máximo de archivos abiertos para una acción es de 1024 (tanto para el límite absoluto como el flexible).
* Este límite es fijo y no se puede cambiar.
* Cuando se invoca una acción, el mandato de ejecución de docker utiliza el argumento `--ulimit nofile=1024:1024` para establecer el valor de `openulimit`.
* Para obtener más información, consulte la documentación de referencia de la línea de mandatos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parameters (fijo: 1 MB)
{: #openwhisk_syslimits_parameters}
* El límite de tamaño para los parámetros totales al crear o actualizar una acción/paquete/desencadenante es 1 MB.
* La creación o actualización de una entidad con parámetros demasiado grandes se rechaza.
* Este límite es fijo y no se puede cambiar.

#### proculimit (fijo: 1024:1024)
{: #openwhisk_syslimits_proculimit}
* El número máximo de procesos disponibles para el contenedor de acciones es de 1024.
* Este límite es fijo y no se puede cambiar.
* Cuando se invoca una acción, el mandato de ejecución de docker utiliza el argumento `--pids-limit 1024` para establecer el valor de `proculimit`.
* Para obtener más información, consulte la documentación de referencia de la línea de mandatos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### result (fijo: 1 MB)
{: #openwhisk_syslimits_result}
* El tamaño máximo de salida del resultado de invocación de una acción en MB.
* Este límite es fijo y no se puede cambiar.

#### sequenceMaxActions (fijo: 50*)
{: #openwhisk_syslimits_sequencemax}
* El número máximo de acciones que puede contener una secuencia determinada.
* Este límite es fijo y no se puede cambiar.

#### timeout (ms) (predeterminado: 60s)
{: #openwhisk_syslimits_timeout}
* El límite de tiempo de espera N está en el intervalo [100 ms..600000 ms] y se establece por acción, en milisegundos.
* Un usuario puede cambiar el límite de tiempo de espera cuando se crea una acción.
* Un contenedor que se ejecuta más de N milisegundos, se finaliza.

### Desencadenantes

Los desencadenantes están sujetos a una tasa de activación por minuto tal como se indica en la tabla siguiente.

| Límite | Descripción | Valor predeterminado | Mínimo | Máximo |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](openwhisk_reference.html#openwhisk_syslimits_tminuterate) | No se pueden activar más de N desencadenantes por espacio de nombres por minuto. | 5000* | 5000* | 5000* |

### Aumento de los límites fijos
{: #increase_fixed_tlimit}

Los valores límite seguidos de (*) son fijos, pero se pueden aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Si desea aumentar el valor límite, póngase en contacto con el soporte de IBM para abrir una incidencia directa desde la [consola web de IBM {{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/).
  1. Seleccione **Support**.
  2. Seleccione **Add Ticket** en el menú desplegable.
  3. Seleccione **Technical** para el tipo de incidencia.
  4. Seleccione **Functions** para el área técnica de soporte.

#### minuteRate (fijo: 5000*)
{: #openwhisk_syslimits_tminuterate}

* El límite de tasa N se establece en 5000 y limita el número de desencadenantes que un usuario puede activar en intervalos de 1 minuto.
* Un usuario no puede cambiar el límite del desencadenante cuando se crea un desencadenante.
* Una llamada de la CLI o API que sobrepase este límite recibe un código de error correspondiente al código de estado de HTTP `429: DEMASIADAS SOLICITUDES`.
* Este valor límite es fijo, pero se puede aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Consulte la sección [Aumento de los límites fijos](openwhisk_reference.html#increase_fixed_tlimit) para obtener instrucciones detalladas sobre cómo aumentar este límite.
