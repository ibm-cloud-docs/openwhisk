---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# Entornos de ejecución
{: #runtimes}
Las apps se pueden codificar y ejecutar en lenguajes de programación, como Javascript o Python. Hay muchos entornos de ejecución disponibles de forma predeterminada con {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

Consulte los tiempos de ejecución disponibles para IBM Cloud Functions en cada región. Los siguientes enlaces devuelven una respuesta JSON.

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

La sección `runtimes` de la respuesta contiene el conjunto de entornos de ejecución disponibles.

La sección `image` contiene el nombre de la imagen del entorno de ejecución en [Docker Hub](https://hub.docker.com/) y el código que se utiliza.


Los ejemplos siguientes apuntan a las imágenes `ibmfunctions/action-nodejs-v10` y `openwhisk/nodejs8action`.
Las etiquetas pueden ser números de versión como `1.9.0` o la forma abreviada de un código hash de confirmación de git, como
`b99d71e`.

Campos de imagen de ejemplo.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs8action:b99d71e"
  ```

Los entornos de ejecución se actualizan de forma periódica. Estas actualizaciones incluyen arreglos de seguridad y actualizaciones de versión menores a los paquetes dentro de los entornos de ejecución. Es posible que las actualizaciones de versión menores hagan que se pierda la compatibilidad con versiones anteriores. Las actualizaciones del entorno de ejecución pueden afectar a las acciones. Debe migrar las acciones que utilizan entorno de ejecución a una versión más reciente actualizándolo.

Las apps que se ejecutan en entornos de ejecución en desuso no se pueden completar satisfactoriamente hasta que se actualice el entorno de ejecución a una versión soportada. Si está intentando identificar un problema en una acción que da un error, para identificar si un entorno de ejecución está en desuso, compruebe si aparece `deprecated=true` en la respuesta de consulta. Para actualizar el entorno de ejecución,
consulte [Cambio del entorno de ejecución de acciones](/docs/openwhisk?topic=cloud-functions-actions#actions_update)

Estos tiempos de ejecución están en desuso:
<ul>
  <li><code>nodejs:6</code> (en desuso)</li>
  <li><code>php:7.1</code> (en desuso)</li>
  <li><code>php:7.2</code> (en desuso)</li>
  <li><code>swift:3</code> (en desuso)</li>
  <li><code>swift:3.1.1</code> (en desuso)</li>
  <li><code>swift:4.1</code> (en desuso)</li>
  <li><code>ballerina:0.990</code> (en desuso)</li>
</ul>






## Entornos de ejecución de JavaScript
{: #openwhisk_ref_javascript_environments}

Las acciones JavaScript se pueden ejecutar en Node.js versión 8 o 10. De forma predeterminada, todas las acciones de Node.js se ejecutan en un entorno de la versión 10.



### Entorno Node.js versión 10 con SDK de IBM
{: #openwhisk_ref_javascript_environments_10}
Se utiliza el entorno Node.js versión 10 si se especifica de forma explícita el distintivo `--kind` con un valor de `nodejs:10` al crear o actualizar una acción.

#### Migración de `nodejs:8` a `nodejs:10`
- El paquete npm `ibm_db` no está disponible en `nodejs:10`. El paquete `ibm_db` no admite Node.js 10. Puede realizar el seguimiento del progreso de este problema consultando
[issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541).
- El paquete npm `cloudant` no está disponible en `nodejs:10`; el paquete ha quedado en desuso, por lo que necesita utilizar el paquete npm oficial [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0 al importar el módulo nodejs (por ejemplo, `require('@cloudant/cloudant')`); además,
[v3.x solo devuelve Promises](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x).
- El paquete npm `cradle` no está disponible en `nodejs:10`.
- El paquete npm `log4js` no está disponible en `nodejs:10`. Puede realizar el seguimiento del problema en
[log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)
- El paquete npm `watson-developer-cloud` no está disponible en `nodejs:10`. Puede realizar el seguimiento del progreso de la nueva versión en este problema: [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)

Encontrará información detallada sobre el entorno de ejecución nodejs versión 10 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md).

#### Paquetes de Node.js 10.15

   - [10.15.0](https://nodejs.org/en/blog/release/v10.15.0/)

 Paquetes de NPM:
   - [amqplib](https://www.npmjs.com/package/amqplib): una biblioteca para crear clientes AMQP 0-9-1 para Node.JS.
   - [apn](https://www.npmjs.com/package/apn): un módulo Node.js para interactuar con el servicio de notificaciones push de Apple.
   - [async](https://www.npmjs.com/package/async): proporciona funciones para trabajar con funciones asíncronas.
   - [bent](https://www.npmjs.com/package/bent): cliente HTTP funcional para Node.js con asincronía y espera.
   - [bodyparser](https://www.npmjs.com/package/body-parser): analizar los cuerpos de las solicitudes de entrada en un middleware antes que los manejadores, disponibles en la propiedad req.body.
   - [btoa](https://www.npmjs.com/package/btoa): un puerto de la función btoa del navegador.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver): controlador de DataStax Node.js para Apache Cassandra.
   - [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant): esta es la biblioteca oficial de Cloudant para Node.js.
   - [commander](https://www.npmjs.com/package/commander): la solución completa para las interfaces de línea de mandatos de Node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator): conversor de direcciones de IU o API de Compose para bases de datos Scylla.
   - [consul:](https://www.npmjs.com/package/consul): un cliente para Consul, que incluye configuración y descubrimiento de servicio.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser): analizar cabeceras de cookies y rellenar req.cookies con un objeto codificado por los nombres de cookie.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch): cliente oficial de Elasticsearch de bajo nivel para Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler): middleware manejador de errores solo de desarrollo.
   - [etcd3](https://www.npmjs.com/package/etcd3): cliente listo para producción de alta calidad para la API etcdv3 basada en almacenamiento intermedio de protocolo.
   - [formidable](https://www.npmjs.com/package/formidable): módulo Node.js para analizar datos de formulario, especialmente cargas de archivos.
   - [glob](https://www.npmjs.com/package/glob): combinar archivos mediante patrones que utiliza el shell, como estrellas y elementos por el estilo.
   - [gm](https://www.npmjs.com/package/gm): GraphicsMagick e ImageMagick para Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - SDK de {{site.data.keyword.cos_full}} para Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db): una interfaz asíncrona/síncrona para node.js, para IBM DB2 e IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf): el cliente node.js se utiliza para simplificar la interacción con la plataforma de Internet de las cosas de IBM Watson.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite): conversión de codificación de caracteres de JS puro.
   - [jsdom](https://www.npmjs.com/package/jsdom): jsdom es una implementación JavaScript pura de muchos estándares de la web, especialmente los estándares WHATWG DOM y HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce): biblioteca de API de Salesforce para aplicaciones JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken): una implementación de señales web de JSON.
   - [lodash](https://www.npmjs.com/package/lodash): la biblioteca de Lodash exportada como módulos Node.js.
   - [marked](https://www.npmjs.com/package/marked): un completo analizador y compilador de markdown, escrito en JavaScript. Diseñado para ofrecer rapidez.
   - [merge](https://www.npmjs.com/package/merge): fusionar varios objetos en uno y, opcionalmente, crear un nuevo objeto clonado.
   - [moment](https://www.npmjs.com/package/moment): una biblioteca de fechas JavaScript ligera para analizar, validar, manipular y formatear fechas.
   - [mongodb](https://www.npmjs.com/package/mongodb): el controlador oficial de MongoDB para Node.js.
   - [mysql](https://www.npmjs.com/package/mysql): controlador de node.js para mysql.
   - [mustache](https://www.npmjs.com/package/mustache): mustache.js es una implementación del sistema de plantillas mustache en JavaScript.
   - [nano](https://www.npmjs.com/package/nano): controlador minimalista de couchdb para Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer): enviar correos electrónicos desde Node.js, ¡es pan comido!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server): módulo completo, compatible y bien probado para implementar un proveedor/servidor de OAuth2 con express en Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk): biblioteca de cliente JavaScript para la plataforma OpenWhisk. Proporciona un derivador alrededor de las API de OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp): convierte una serie de vía de acceso, como /user/:name, en una expresión regular que luego se puede comparar con vías de acceso URL.
   - [pg](https://www.npmjs.com/package/pg): cliente PostgreSQL de no bloqueo para node.js. JavaScript puro y enlaces a libpq nativos opcionales.
   - [process](https://www.npmjs.com/package/process): require('process'); como cualquier otro módulo.
   - [pug](https://www.npmjs.com/package/pug): implementa el lenguaje de creación de plantillas Pug.
   - [redis](https://www.npmjs.com/package/redis): se trata de un completo cliente Redis para Node.js con muchas funciones.
   - [request](https://www.npmjs.com/package/request): Request está diseñado para ser la forma más sencilla posible de realizar llamadas HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise): la versión simplificada del cliente de solicitudes HTTP 'request' con soporte de Promise. Basado en Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf): el mandato de UNIX rm -rf para Node.
   - [semver](https://www.npmjs.com/package/semver): mantenimiento de versiones semánticas para Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail): proporciona soporte de correo electrónico mediante la API de SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error): serializar un error a un objeto sin formato.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon): middleware de Node.js para el servicio de favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io): Socket.IO permite la comunicación bidireccional basada en sucesos en tiempo real.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client): infraestructura de aplicaciones en tiempo real para socket.io.
   - [superagent](https://www.npmjs.com/package/superagent): SuperAgent es una biblioteca progresiva de solicitudes HTTP del lado del cliente, y un módulo Node.js con la misma API, que tiene muchas funciones de cliente HTTP de alto nivel.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools): un paquete que proporciona varias herramientas para la integración y la interacción con Swagger.
   - [twilio](https://www.npmjs.com/package/twilio): un derivador para la API de Twilio, relacionado con voz, vídeo y mensajería.
   - [underscore](https://www.npmjs.com/package/underscore): Underscore.js es una biblioteca de programas de utilidad para JavaScript que proporciona soporte para los aspectos funcionales habituales (each, map, reduce, filter...) sin ampliar ningún objeto JavaScript principal.
   - [url-pattern](https://www.npmjs.com/package/url-pattern): analiza los URL correspondientes a parámetros de vía de acceso más fácilmente que un comparador de series regex.
   - [uuid](https://www.npmjs.com/package/uuid): generación simple y rápida de UUID RFC4122.
   - [validator](https://www.npmjs.com/package/validator): una biblioteca de sanitizantes y validadores de series.
   - [vcap_services](https://www.npmjs.com/package/vcap_services): analizar y devolver las credenciales de servicio de la variable de entorno VCAP_SERVICES que proporciona IBM Cloud.
   - [when](https://www.npmjs.com/package/when): When.js es una sólida implementación de Promises/A+ y when() "a prueba de bombas", que incluye un completo corrector de compatibilidad ES6 Promise.
   - [winston](https://www.npmjs.com/package/winston): una biblioteca de registro asíncrono multitransporte para node.js. "CHILL WINSTON! ... I put it in the logs."
   - [ws](https://www.npmjs.com/package/ws): ws es una implementación de cliente y servidor WebSocket fácil de utilizar, ultrarrápida y probada de forma exhaustiva.
   - [xlsx](https://www.npmjs.com/package/xlsx): analizador y escritor para varios formatos de hojas de cálculo.
   - [xml2js](https://www.npmjs.com/package/xml2js): sencillo conversor de objetos de XML a JavaScript. Admite conversión bidireccional.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest): node-XMLHttpRequest es un derivador para el cliente http incorporado para emular el objeto XMLHttpRequest del navegador.
   - [yauzl](https://www.npmjs.com/package/yauzl): otra biblioteca de extracción para Node.

### Entorno Node.js versión 8 con SDK de IBM
{: #openwhisk_ref_javascript_environments_8}
Se utiliza el entorno Node.js versión 8 si se especifica de forma explícita el distintivo `--kind` con un valor de `nodejs:8` al crear o actualizar una acción.

Node.js versión 8 está en modalidad de mantenimiento y estará disponible hasta diciembre de 2019. Consulte la
[Planificación de releases de Node.js](https://github.com/nodejs/Release).
{: deprecated}

#### Paquetes de Node.js 8.15

 - [8.15.0](https://nodejs.org/en/blog/release/v8.15.0)

  - [amqplib](https://www.npmjs.com/package/amqplib): una biblioteca para crear clientes AMQP 0-9-1 para Node.JS.
   - [apn](https://www.npmjs.com/package/apn): un módulo Node.js para interactuar con el servicio de notificaciones push de Apple.
   - [async](https://www.npmjs.com/package/async): proporciona funciones para trabajar con funciones asíncronas.
   - [bent](https://www.npmjs.com/package/bent): cliente HTTP funcional para Node.js con asincronía y espera.
   - [bodyparser](https://www.npmjs.com/package/body-parser): analizar los cuerpos de las solicitudes de entrada en un middleware antes que los manejadores, disponibles en la propiedad req.body.
   - [btoa](https://www.npmjs.com/package/btoa): un puerto de la función btoa del navegador.
   - [cassandra-driver](https://www.npmjs.com/package/cassandra-driver): controlador de DataStax Node.js para Apache Cassandra.
   - [cloudant](https://www.npmjs.com/package/cloudant): esta es la biblioteca oficial de Cloudant para Node.js.
   - [@cloudant/cloudant](https://www.npmjs.com/package/cloudant): esta es la biblioteca oficial de Cloudant para Node.js.
   - [commander](https://www.npmjs.com/package/commander): la solución completa para las interfaces de línea de mandatos de Node.js.
   - [composeaddresstranslator](https://www.npmjs.com/package/composeaddresstranslator): conversor de direcciones de IU o API de Compose para bases de datos Scylla.
   - [consul:](https://www.npmjs.com/package/consul): un cliente para Consul, que incluye configuración y descubrimiento de servicio.
   - [cookie-parser](https://www.npmjs.com/package/cookie-parser): analizar cabeceras de cookies y rellenar req.cookies con un objeto codificado por los nombres de cookie.
   - [elasticsearch](https://www.npmjs.com/package/elasticsearch): cliente oficial de Elasticsearch de bajo nivel para Node.js.
   - [errorhandler](https://www.npmjs.com/package/errorhandler): middleware manejador de errores solo de desarrollo.
   - [etcd3](https://www.npmjs.com/package/etcd3): cliente listo para producción de alta calidad para la API etcdv3 basada en almacenamiento intermedio de protocolo.
   - [formidable](https://www.npmjs.com/package/formidable): módulo Node.js para analizar datos de formulario, especialmente cargas de archivos.
   - [glob](https://www.npmjs.com/package/glob): combinar archivos mediante patrones que utiliza el shell, como estrellas y elementos por el estilo.
   - [gm](https://www.npmjs.com/package/gm): GraphicsMagick e ImageMagick para Node.
   - [ibm-cos-sdk](https://www.npmjs.com/package/ibm-cos-sdk) - SDK de {{site.data.keyword.cos_full}} para Node.js
   - [ibm_db](https://www.npmjs.com/package/ibm_db): una interfaz asíncrona/síncrona para node.js, para IBM DB2 e IBM Informix.
   - [ibmiotf](https://www.npmjs.com/package/ibmiotf): el cliente node.js se utiliza para simplificar la interacción con la plataforma de Internet de las cosas de IBM Watson.
   - [iconv-lite](https://www.npmjs.com/package/iconv-lite): conversión de codificación de caracteres de JS puro.
   - [jsdom](https://www.npmjs.com/package/jsdom): jsdom es una implementación JavaScript pura de muchos estándares de la web, especialmente los estándares WHATWG DOM y HTML.
   - [jsforce](https://www.npmjs.com/package/jsforce): biblioteca de API de Salesforce para aplicaciones JavaScript.
   - [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken): una implementación de señales web de JSON.
   - [lodash](https://www.npmjs.com/package/lodash): la biblioteca de Lodash exportada como módulos Node.js.
   - [log4js](https://www.npmjs.com/package/log4js): es una conversión de la infraestructura log4js diseñada para trabajar con Node.
   - [marked](https://www.npmjs.com/package/marked): un completo analizador y compilador de markdown, escrito en JavaScript. Diseñado para ofrecer rapidez.
   - [merge](https://www.npmjs.com/package/merge): fusionar varios objetos en uno y, opcionalmente, crear un nuevo objeto clonado.
   - [moment](https://www.npmjs.com/package/moment): una biblioteca de fechas JavaScript ligera para analizar, validar, manipular y formatear fechas.
   - [mongodb](https://www.npmjs.com/package/mongodb): el controlador oficial de MongoDB para Node.js.
   - [mysql](https://www.npmjs.com/package/mysql): controlador de node.js para mysql.
   - [mustache](https://www.npmjs.com/package/mustache): mustache.js es una implementación del sistema de plantillas mustache en JavaScript.
   - [nano](https://www.npmjs.com/package/nano): controlador minimalista de couchdb para Node.js.
   - [nodemailer](https://www.npmjs.com/package/nodemailer): enviar correos electrónicos desde Node.js, ¡es pan comido!
   - [oauth2-server](https://www.npmjs.com/package/oauth2-server): módulo completo, compatible y bien probado para implementar un proveedor/servidor de OAuth2 con express en Node.js.
   - [openwhisk](https://www.npmjs.com/package/openwhisk): biblioteca de cliente JavaScript para la plataforma OpenWhisk. Proporciona un derivador alrededor de las API de OpenWhisk.
   - [path-to-regex](https://www.npmjs.com/package/path-to-regexp): convierte una serie de vía de acceso, como /user/:name, en una expresión regular que luego se puede comparar con vías de acceso URL.
   - [pg](https://www.npmjs.com/package/pg): cliente PostgreSQL de no bloqueo para node.js. JavaScript puro y enlaces a libpq nativos opcionales.
   - [process](https://www.npmjs.com/package/process): require('process'); como cualquier otro módulo.
   - [pug](https://www.npmjs.com/package/pug): implementa el lenguaje de creación de plantillas Pug.
   - [redis](https://www.npmjs.com/package/redis): se trata de un completo cliente Redis para Node.js con muchas funciones.
   - [request](https://www.npmjs.com/package/request): Request está diseñado para ser la forma más sencilla posible de realizar llamadas HTTP.
   - [request-promise](https://www.npmjs.com/package/request-promise): la versión simplificada del cliente de solicitudes HTTP 'request' con soporte de Promise. Basado en Bluebird.
   - [rimraf](https://www.npmjs.com/package/rimraf): el mandato de UNIX rm -rf para Node.
   - [semver](https://www.npmjs.com/package/semver): mantenimiento de versiones semánticas para Nodejs
   - [@sendgrid/mail](https://www.npmjs.com/package/@sendgrid/mail): proporciona soporte de correo electrónico mediante la API de SendGrid.
   - [serialize-error](https://www.npmjs.com/package/serialize-error): serializar un error a un objeto sin formato.
   - [serve-favicon](https://www.npmjs.com/package/serve-favicon): middleware de Node.js para el servicio de favicon.
   - [socket.io](https://www.npmjs.com/package/socket.io): Socket.IO permite la comunicación bidireccional basada en sucesos en tiempo real.
   - [socket.io-client](https://www.npmjs.com/package/socket.io-client): infraestructura de aplicaciones en tiempo real para socket.io.
   - [superagent](https://www.npmjs.com/package/superagent): SuperAgent es una biblioteca progresiva de solicitudes HTTP del lado del cliente, y un módulo Node.js con la misma API, que tiene muchas funciones de cliente HTTP de alto nivel.
   - [swagger-tools](https://www.npmjs.com/package/swagger-tools): un paquete que proporciona varias herramientas para la integración y la interacción con Swagger.
   - [twilio](https://www.npmjs.com/package/twilio): un derivador para la API de Twilio, relacionado con voz, vídeo y mensajería.
   - [underscore](https://www.npmjs.com/package/underscore): Underscore.js es una biblioteca de programas de utilidad para JavaScript que proporciona soporte para los aspectos funcionales habituales (each, map, reduce, filter...) sin ampliar ningún objeto JavaScript principal.
   - [url-pattern](https://www.npmjs.com/package/url-pattern): analiza los URL correspondientes a parámetros de vía de acceso más fácilmente que un comparador de series regex.
   - [uuid](https://www.npmjs.com/package/uuid): generación simple y rápida de UUID RFC4122.
   - [validator](https://www.npmjs.com/package/validator): una biblioteca de sanitizantes y validadores de series.
   - [vcap_services](https://www.npmjs.com/package/vcap_services): analizar y devolver las credenciales de servicio de la variable de entorno VCAP_SERVICES que proporciona IBM Cloud.
   - [watson-developer-cloud](https://www.npmjs.com/package/watson-developer-cloud): biblioteca de cliente de Node.js para utilizar los servicios de Watson Developer Cloud, un conjunto de API que utilizan la computación cognitiva para resolver problemas complejos.
   - [when](https://www.npmjs.com/package/when): When.js es una sólida implementación de Promises/A+ y when() "a prueba de bombas", que incluye un completo corrector de compatibilidad ES6 Promise.
   - [winston](https://www.npmjs.com/package/winston): una biblioteca de registro asíncrono multitransporte para node.js. "CHILL WINSTON! ... I put it in the logs."
   - [ws](https://www.npmjs.com/package/ws): ws es una implementación de cliente y servidor WebSocket fácil de utilizar, ultrarrápida y probada de forma exhaustiva.
   - [xml2js](https://www.npmjs.com/package/xml2js): sencillo conversor de objetos de XML a JavaScript. Admite conversión bidireccional.
   - [xmlhttprequest](https://www.npmjs.com/package/xmlhttprequest): node-XMLHttpRequest es un derivador para el cliente http incorporado para emular el objeto XMLHttpRequest del navegador.
   - [yauzl](https://www.npmjs.com/package/yauzl): otra biblioteca de extracción para Node.

Encontrará información detallada sobre el entorno de ejecución Node.js versión 8 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md).


## Entornos de ejecución Python
{: #openwhisk_ref_python_environments}

Puede elegir entre dos versiones de entorno de ejecución diferentes para las acciones de Python. De forma predeterminada, todas las acciones de Python se ejecutan en un entorno de la versión 2.


### Acciones Python 3.7 (basadas en Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

Las acciones Python 3.7 se ejecutan con Python 3.7.x. Para utilizar este entorno de ejecución, especifique el parámetro de CLI `--kind python:3.7` al crear o actualizar una acción.

El entorno de ejecución contiene paquetes SDK para los servicios de IBM Cloud disponibles que las acciones de Python pueden utilizar, además de las bibliotecas estándares de Python 3.7.

#### Paquetes de Python 3.7.2

 - [3.7.2](https://github.com/docker-library/python/blob/ab8b829cfefdb460ebc17e570332f0479039e918/3.7/stretch/Dockerfile)

 Paquetes de Python:
 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - etcd3
 - Flask
 - gevent
 - greenlet
 - grpcio
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - protobuf
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six
 - soupsieve
 - tenacity
 - tornado
 - Twisted
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - websocket-client
 - Werkzeug
 - zope.interface

Encontrará información detallada sobre el entorno de ejecución Python 3.7 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md).

### Acciones Python 3.6.8 (basadas en Debian Jessie)
{: #openwhisk_ref_python_environments_3.6}

Las acciones Python 3 se ejecutan con Python 3.6.x. Para utilizar este entorno de ejecución, especifique el parámetro de CLI `--kind python:3.6` al crear o actualizar una acción.

El tiempo de ejecución contiene paquetes SDK para los servicios de IBM Cloud disponibles que las acciones de Python pueden utilizar, además de las bibliotecas estándares de Python 3.6.

#### Paquetes de Python 3.6.8

Versión de Python:
 - [3.6.8](https://github.com/docker-library/python/blob/721671c28aad96ad2c1970e83c2af71ceff15f1b/3.6/jessie/slim/Dockerfile)

 - Paquetes de Python:
 - asn1crypto
 - attrs
 - autobahn
 - Automat
 - beautifulsoup4
 - botocore
 - cassandra-driver
 - certifi
 - cffi
 - chardet
 - Click
 - cloudant
 - constantly
 - cryptography
 - cssselect
 - docutils
 - elasticsearch
 - Flask
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - ibm-cos-sdk
 - ibm-cos-sdk-core
 - ibm-cos-sdk-s3transfer
 - ibm-db
 - ibmcloudsql
 - idna
 - incremental
 - itsdangerous
 - Jinja2
 - jmespath
 - kafka-python
 - lxml
 - MarkupSafe
 - numpy
 - pandas
 - parsel
 - pika
 - Pillow
 - psycopg2
 - pyarrow
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pymongo
 - pyOpenSSL
 - python-dateutil
 - pytz
 - queuelib
 - redis
 - requests
 - scikit-learn
 - scipy
 - Scrapy
 - service-identity
 - simplejson
 - six=
 - soupsieve
 - tornado
 - Twisted
 - txaio
 - urllib3
 - virtualenv
 - w3lib
 - watson-developer-cloud
 - Werkzeug
 - zope.interface

Encontrará información detallada sobre el entorno de ejecución Python 3.6 en [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md).

### Acciones de Python 2

A menos que se especifique el distintivo `--kind` al crear o actualizar una acción, las acciones Python 2 se ejecutarán con Python 2.7.15.

Al crear acciones de python mediante virtualenv, utilice la imagen de docker `openwhisk/python2action`.
Las acciones de Python 2 pueden utilizar los paquetes siguientes, además de las bibliotecas estándares de Python 2.7.

#### Paquetes de Python 2

 - asn1crypto
 - attrs
 - Automat
 - beautifulsoup4
 - certifi
 - cffi
 - chardet
 - Click
 - constantly
 - cryptography
 - cssselect
 - enum34
 - Flask
 - functools32
 - gevent
 - greenlet
 - httplib2
 - hyperlink
 - idna
 - incremental
 - ipaddress
 - itsdangerous
 - Jinja2
 - kafka-python
 - lxml
 - MarkupSafe
 - parsel
 - pyasn1
 - pyasn1-modules
 - pycparser
 - PyDispatcher
 - PyHamcrest
 - pyOpenSSL
 - python-dateutil
 - queuelib
 - requests
 - Scrapy
 - service-identity
 - simplejson
 - six
 - Twisted
 - urllib3
 - virtualenv=
 - w3lib
 - Werkzeug
 - zope.interface

Encontrará información detallada sobre el entorno de ejecución Python 2 en [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md).


## Entorno de ejecución Swift
{: #swift-actions}

De forma predeterminada, todas las acciones de Swift se ejecutan en un entorno de la versión 4.2.

Los entornos de ejecución de acciones Swift 4.x no incluyen ningún paquete; siga las instrucciones correspondientes a [acciones de swift empaquetadas](/docs/openwhisk?topic=cloud-functions-prep#prep_swift42_single) para incluir dependencias mediante Package.swift.

Las acciones Swift 4.2 pueden utilizar los paquetes siguientes cuando se utiliza un solo archivo de origen Swift:
- Watson Developer Cloud SDK versión 1.2.0, https://github.com/watson-developer-cloud/swift-sdk


### SwiftyJSON con un solo archivo de acción de origen
Si tiene una acción que no está compilada, y utiliza el paquete **SwiftyJSON**, debe precompilar la acción y especificar la versión de SwiftyJSON que desea utilizar para la acción de tipo `swift:4.2`.


## Entorno de ejecución PHP
{: #openwhisk_ref_php}

De forma predeterminada, todas las acciones de PHP se ejecutan en un entorno de la versión 7.3.

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

## Entorno de ejecución Docker
{: #openwhisk_ref_docker}

Las acciones Docker ejecutan un binario proporcionado por el usuario en un contenedor Docker. El binario se ejecuta en una imagen Docker basada en [python:3.6-alpine](https://hub.docker.com/r/library/python), por lo que el binario debe ser compatible con dicha distribución.

El esqueleto de Docker es una forma cómoda de crear imágenes Docker compatibles con OpenWhisk. Puede instalar el esqueleto con el mandato de plugin de CLI `ibmcloud fn sdk install docker`.

El programa binario principal debe estar en `/action/exec` dentro del contenedor. El ejecutable recibe los argumentos de entrada desde una serie de argumento de línea de mandatos, que se puede deserializar como un objeto `JSON`. Debe devolver un resultado utilizando `stdout` como una serie de una línea de `JSON` serializado.

Puede incluir los pasos de compilación o dependencias modificando el `Dockerfile` incluido en `dockerSkeleton`.



## Entorno de ejecución Go
{: #runtimes_go}

De forma predeterminada, todas las acciones de Go se ejecutan en un entorno de la versión 1.11.



## Entorno de ejecución Java
{: #runtimes_java}

De forma predeterminada, todas las acciones de Java se ejecutan en un entorno de la versión 8.



## Entorno de ejecución Ruby
{: #runtimes_ruby}

De forma predeterminada, todas las acciones de Ruby se ejecutan en un entorno de la versión 2.5.



## Entorno de ejecución .NET Core
{: #runtimes_dotnet}

De forma predeterminada, todas las acciones de .NET Core se ejecutan en un entorno de la versión 2.2.
