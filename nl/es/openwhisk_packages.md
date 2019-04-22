---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: packages, browse, binding, trigger, feeds, share

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Organización de acciones en paquetes
{: #openwhisk_packages}

En {{site.data.keyword.openwhisk}}, puede utilizar paquetes para agrupar un conjunto de acciones relacionadas y compartirlas con otros.
{: shortdesc}

Un paquete puede incluir *actions* y *feeds* (acciones y canales de información).
- Una acción es un segmento de código que se ejecuta en {{site.data.keyword.openwhisk_short}}. Por ejemplo, el paquete {{site.data.keyword.cloudant}} incluye acciones para leer y escribir registros en una base de datos {{site.data.keyword.cloudant_short_notm}}.
- Un canal de información sirve para configurar un origen de suceso externo para activar sucesos desencadenantes. Por ejemplo, el paquete Alarma incluye un canal de información que puede activar un desencadenante en una frecuencia especificada.

Toda entidad de {{site.data.keyword.openwhisk_short}}, incluyendo los paquetes, pertenece a un
*espacio de nombres*, y el nombre completo de una entidad es `/namespaceName/[packageName]/entityName`. Para obtener más información, consulte las [directrices de denominación](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities).

En las secciones siguientes se describe cómo examinar paquetes y usar para ellos los desencadenantes y canales de información. Además, si está interesado en contribuir con sus propios paquetes al catálogo, lea las secciones sobre la creación y compartición de paquetes.

## Examinar paquetes
{: #browse-packages}

Hay varios paquetes registrados con {{site.data.keyword.openwhisk_short}}. Puede obtener una lista de paquetes de un espacio de nombres, lista de las entidades de un paquete y obtener una descripción de las entidades individuales en un paquete.

1. Obtener una lista de paquetes en el espacio de nombres `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Salida de lista de paquetes:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Obtener una lista de entidades en el paquete `/whisk.system/cloudant`.
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  Salida de ejemplo:
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Esta salida muestra que el paquete {{site.data.keyword.cloudant_short_notm}} proporciona dos acciones, `read` y `write`, y un canal de información de desencadenante llamado `changes`. El canal de información `changes` provoca que los desencadenantes se activen cuando se añaden documentos a la base de datos {{site.data.keyword.cloudant_short_notm}} especificada.

  El paquete {{site.data.keyword.cloudant_short_notm}} también define los parámetros `username`, `password`, `host` y `port`. Estos parámetros se deben especificar para que las acciones y los canales de información tengan sentido. Los parámetros permiten que las acciones funcionen en una cuenta {{site.data.keyword.cloudant_short_notm}} específica, por ejemplo.

3. Obtener una descripción de la acción `/whisk.system/cloudant/read`.
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  Salida de ejemplo:
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Esta salida muestra que la acción `read` de {{site.data.keyword.cloudant_short_notm}} precisa de tres parámetros, incluyendo la base de datos y
el ID de documento a recuperar.

## Invocación de acciones de un paquete
{: #openwhisk_package_invoke}

Puede invocar acciones de un paquete, como con el resto de acciones. Los próximos pasos muestran cómo invocar
la acción `greeting` en el paquete `/whisk.system/samples` con distintos parámetros.

1. Obtener una descripción de la acción `/whisk.system/samples/greeting`.
  ```
  ibmcloud fn action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  Salida de ejemplo:
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  Observe que la acción `greeting` acepta dos parámetros: `name` y `place`.

2. Invoque la acción sin ningún parámetro.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  La salida es un mensaje genérico porque no se han especificado parámetros.

3. Invoque la acción con parámetros.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Observe que la salida utiliza los parámetros `name` y `place` que se pasaron a la acción.

## Creación y utilización de enlaces de paquete
{: #openwhisk_package_bind}

Aunque puede utilizar las entidades en un paquete directamente, es posible que acabe pasando siempre los mismos parámetros a la acción. Puede simplificar el proceso enlazando a un paquete y especificando parámetros predeterminados, que las acciones del paquete heredan.

Por ejemplo, en el paquete `/whisk.system/cloudant`, puede establecer los valores
`username`, `password` y `dbname` predeterminados en un enlace de paquete, y dichos valores
se pasan automáticamente a cualquier acción del paquete.

En el ejemplo sencillo siguiente, enlaza al paquete `/whisk.system/samples`.

1. Enlazar al paquete `/whisk.system/samples` y establecer un valor de parámetro `place` predeterminado.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Obtener una descripción del enlace de paquete.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  Salida de ejemplo:
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Tenga en cuenta que todas las acciones del paquete `/whisk.system/samples` están disponibles en el enlace del
paquete `valhallaSamples`.

3. Invocar una acción en el enlace de paquete.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Observe en el resultado que la acción hereda el parámetro `place` que ha establecido cuando ha creado el enlace del paquete `valhallaSamples`.

4. Invoque una acción y sobrescriba el valor de parámetro predeterminado.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Fíjese que el valor del parámetro `place` especificado con la invocación de la acción sobrescribe
el valor predeterminado establecido en el enlace del paquete `valhallaSamples`.

## Creación y uso de canales de información de desencadenante
{: #openwhisk_package_trigger}

Los canales de información ofrecen una forma cómoda de configurar un origen de suceso externo para activar dichos sucesos para un desencadenante de {{site.data.keyword.openwhisk_short}}. En este ejemplo se muestra cómo utilizar un canal de información del paquete Alarms para activar un desencadenante una vez por minuto y cómo usar una regla para invocar una acción una vez por minuto.

1. Obtener una descripción del canal de información en el paquete `/whisk.system/alarms`.
  ```
  ibmcloud fn package get --summary /whisk.system/alarms
  ```
  {: pre}

  Salida de ejemplo:
  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Salida de ejemplo:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  El canal de información `/whisk.system/alarms/alarm` acepta dos parámetros:
  - `cron`: una especificación crontab de cuándo activar el desencadenante.
  - `trigger_payload`: el valor de parámetro payload a establecer en cada suceso desencadenante.

2. Crear un desencadenante que se active cada minuto.
  ```
  ibmcloud fn trigger create everyOneMinute --feed /whisk.system/alarms/alarm -p cron "* * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created trigger feed everyOneMinute
  ```
  {: screen}

3. Cree un archivo denominado `hello.js` con el código de acción siguiente:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Asegúrese de que la acción exista.
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

5. Crear una regla que invoque la acción `hello` cada vez que se active el desencadenante
`everyOneMinute`.
  ```
  ibmcloud fn rule create myRule everyOneMinute hello
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. Comprobar que la acción se está invocando, sondeando los registros de activación.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Puede ver que se observan activaciones cada minuto para el desencadenante, la regla y la acción. La acción recibe los parámetros `{"name":"Mork", "place":"Ork"}` en cada invocación.

## Creación de un paquete
{: #openwhisk_packages_create}

Un paquete se utiliza para organizar un conjunto de acciones y canales de información relacionados.
También permite la compartición de los parámetros entre todas las entidades del paquete.

Para crear un paquete personalizado con una acción sencilla en él, pruebe el ejemplo siguiente:

1. Cree un paquete llamado `custom`.
  ```
  ibmcloud fn package create custom
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created package custom
  ```
  {: screen}

2. Obtenga un resumen del paquete.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Salida de ejemplo:
  ```
  package /myNamespace/custom
  ```
  {: screen}

  Fíjese que el paquete está vacío.

3. Cree un archivo llamado `identity.js` que contenga el código de acción siguiente. Esta acción
devuelve todos los parámetros de entrada.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Crear una acción denominada `identity` en el paquete `custom`.
  ```
  ibmcloud fn action create custom/identity identity.js
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: created action custom/identity
  ```
  {: screen}

  La creación de una acción en un paquete precisa que se añada un nombre de paquete como prefijo del nombre de la acción. No se permite
anidamiento de paquetes. Un paquete solo puede contener acciones y no puede contener otros paquetes.

5. Obtenga otra vez un resumen del paquete.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Salida de ejemplo:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  Ahora podrá ver la acción `custom/identity` en su espacio de nombres.

6. Invocar la acción en el paquete.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {}
  ```
  {: screen}

Puede establecer parámetros predeterminados para todas las entidades de un paquete estableciendo los parámetros a nivel de paquete, que todas las acciones del paquete heredan. Para ver cómo funciona esta herencia, pruebe el ejemplo siguiente:

1. Actualizar el paquete `custom` con dos parámetros: `city` y `country`.
  ```
  ibmcloud fn package update custom --param city Austin --param country USA
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: updated package custom
  ```
  {: screen}

2. Mostrar los parámetros en el paquete `custom` y acción `identity`, y ver cómo la acción `identity` del paquete hereda los parámetros del paquete.
  ```
  ibmcloud fn package get custom parameters
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: got package custom, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

  ```
  ibmcloud fn action get custom/identity parameters
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: got action custom/identity, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

3. Invocar la acción `identity` sin parámetros para comprobar que la acción realmente hereda los parámetros.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. Invocar la acción `identity` con algunos parámetros. Los parámetros de invocación se fusionan con los parámetros del paquete; los parámetros de invocación prevalecen sobre los parámetros del paquete.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  Salida de ejemplo:
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## Compartición de un paquete
{: #openwhisk_packages_share}

Tras depurar y probar las acciones y los canales de información que comprende un paquete, éste se puede compartir con todos los usuarios de {{site.data.keyword.openwhisk_short}}. La compartición del paquete posibilita que los usuarios enlacen al paquete, invoquen acciones en el mismo y creen reglas de {{site.data.keyword.openwhisk_short}} y acciones de secuencia.

1. Compartir el paquete con todos los usuarios:
  ```
  ibmcloud fn package update custom --shared yes
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: updated package custom
  ```
  {: screen}

2. Mostrar la propiedad `publish` del paquete para verificar que ahora es true.
  ```
  ibmcloud fn package get custom publish
  ```
  {: pre}

  Salida de ejemplo:
  ```
  ok: got package custom, displaying field publish

  true
  ```
  {: screen}

Ahora otros pueden utilizar su paquete `custom`, incluyendo el enlace al paquete o directamente invocando una
acción sobre él. Otros usuarios deben conocer los nombres completos del paquete para enlazar o invocar acciones sobre él. Las acciones
e información de entrada dentro de un paquete compartido son _públicas_. Si el paquete es privado, todo su contenido es también privado.

1. Obtener una descripción del paquete para mostrar los nombres completos del paquete y la acción.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Salida de ejemplo:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  En el ejemplo anterior, se trabaja con el espacio de nombres `myNamespace` y este espacio de nombres aparece en el nombre completo.
