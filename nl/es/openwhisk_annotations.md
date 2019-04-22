---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: annotations, annotate, package, parameters, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Anotaciones
{: #openwhisk_annotations}

Los paquetes, las reglas, los desencadenantes y las acciones de {{site.data.keyword.openwhisk}} (a las que colectivamente se les hace referencia como entidades) pueden incluir anotaciones.
{: shortdesc}

Las anotaciones se adjuntan las entidades en forma de parámetros. Las anotaciones constan de una `clave` que define un nombre y un `valor` que define el valor de la clave. Las anotaciones se utilizan con más frecuencia para documentar acciones y paquetes. Muchos de los paquetes del catálogo de {{site.data.keyword.openwhisk_short}} contienen anotaciones como, por ejemplo, la descripción de la funcionalidad que ofrecen sus acciones, los parámetros que se pueden utilizar en el momento de enlazar el paquete, los parámetros en el momento de la invocación o si dichos parámetros son secretos. Las anotaciones se añaden según sea necesario, por ejemplo, para permitir la integración con la interfaz de usuario.

Las entidades se documentan desde la CLI utilizando el distintivo `--annotation` o `-a`.

## Anotaciones de acción
{: #action}

Entre las anotaciones que describen las acciones, se incluyen:

- `description`: Una sucinta descripción de la acción.
- `parameters`: Una matriz que describe las acciones necesarias para ejecutar la acción.
- `sampleInput`: Un ejemplo que muestra el esquema de entrada con los valores típicos.
- `sampleOutput`: Un ejemplo que muestra el esquema de salida, generalmente para `sampleInput`.



El código siguiente es un ejemplo de un conjunto de anotaciones para una acción `echo` que devuelve sus argumentos de entrada sin modificar. Esta acción es útil para registrar parámetros de entrada, por ejemplo, como parte de una secuencia o de una regla.

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Anotaciones de acción web
{: #annotations-specific-to-web-actions}

Las siguientes anotaciones de acción web se deben establecer explícitamente en `true` para habilitar la interactividad con las API:

- `web-export`: Cuando se aplica a una acción, la acción pasa a ser una [acción web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions). La acción pasa a estar accesible desde llamadas REST sin autenticación de forma que los usuarios pueden acceder a dichas acciones desde un navegador.
    * **Nota**: El propietario de la acción web incurre en un coste al ejecutarla en el sistema. Es decir, el propietario de la acción también es el propietario del registro de activaciones.
- `final`: Cuando se aplica a una acción, todos los parámetros de acción definidos con anterioridad pasan a ser inmutables. Los parámetros no se pueden modificar con otros parámetros proporcionados durante la invocación.
- `raw-http`: Cuando se aplica a una acción que tiene la anotación `web-export`, los parámetros de cuerpo y consulta de la solicitud HTTP se pasan a la acción como propiedades reservadas.
- `web-custom-options`: Habilita que la acción web responda a las solicitudes OPTION con cabeceras personalizadas. De lo contrario, se aplica una [respuesta CORS predeterminada](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#options-requests).
- `require-whisk-auth`: La acción web sólo puede ser invocada por las solicitudes que proporcionen las credenciales de autenticación adecuadas.
    * Cuando se establece en un valor booleano, controla si el valor de autenticación básica de la solicitud está autenticada. El valor `true` autentica las credenciales y el valor `false` invoca la acción sin ninguna autenticación.
    * Cuando se establece en un entero o en una serie, este valor debe coincidir con el valor de la cabecera `X-Require-Whisk-Auth` de la solicitud.
    * **Nota**: El propietario de la acción web incurre en un coste al ejecutarla en el sistema. Es decir, el propietario de la acción también es el propietario del registro de activaciones.

## Anotaciones de paquete
{: #package}

Entre las anotaciones que describen los paquetes, se incluyen:

- `description`: Una sucinta descripción del paquete.
- `parameters`: Una matriz que describe los parámetros que abarca el paquete.

## Anotaciones de parámetros
{: #parameter}

Entre las anotaciones que describen los parámetros, se incluyen:

- `name`: El nombre del parámetro.
- `description`: Una sucinta descripción del parámetro.
- `doclink`: Un enlace con más documentación sobre el parámetro (útil para señales OAuth).
- `required`: True para los parámetros obligatorios y false para los opcionales.
- `bindTime`: True si se especifica el parámetro cuando se enlaza un paquete.
- `type`: El tipo de parámetro, que puede ser `password`, `array` (aunque se puede utilizar en más situaciones).

## Anotaciones de activación
{: #activation}

Puede documentar los registros de activación con las siguientes anotaciones:

- `path`: el nombre de la vía de acceso completa de la acción que ha generado la activación. Tenga en cuenta que, si esta activación ha sido el resultado de una acción en un enlace de paquete, la vía de acceso hace referencia al paquete padre.
- `kind`: el tipo de acción ejecutada, y uno de los tipos de tiempo de ejecución de OpenWhisk admitidos.
- `limits`: el tiempo, la memoria y los límites de registro a los que está sujeta esta activación.

Para activaciones relacionadas con secuencias, el sistema genera las anotaciones siguientes:

- `topmost`: solo está presente para una acción de secuencia externa.
- `causedBy`: solo está presente para acciones incluidas en una secuencia.

Con el objeto de proporcionar transparencia de rendimiento, las activaciones también registran:

- `waitTime`: el tiempo de espera en el sistema interno de OpenWhisk. Se trata de, aproximadamente, el tiempo transcurrido desde que el controlador recibe la solicitud de activación hasta que el invocador suministra un contenedor para la acción. Actualmente, este valor sólo está presente para activaciones que no sean de secuencia. Para las secuencias, esta información se puede obtener del registro de activación de secuencia de `topmost`.
- `initTime`: el tiempo dedicado a inicializar la función. Si este valor está presente, la acción ha necesitado inicialización y representa un inicio en frío. En las activaciones en caliente, se omite la inicialización y, en este caso, la anotación no se genera.

En el ejemplo siguiente se muestran estas anotaciones tal como podrían aparecer en un registro de activación:

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}
