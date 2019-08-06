---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: annotations, annotate, package, parameters, actions, functions

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


# Anotaciones
{: #annotations}

Los paquetes, las reglas, los desencadenantes y las acciones de {{site.data.keyword.openwhisk}} (a las que colectivamente se les hace referencia como entidades) pueden incluir anotaciones.
{: shortdesc}

Las anotaciones se adjuntan las entidades en forma de parámetros. Las anotaciones constan de una `clave` que define un nombre y un `valor` que define el valor de la clave. Las anotaciones se utilizan con más frecuencia para documentar acciones y paquetes. Los paquetes del catálogo de {{site.data.keyword.openwhisk_short}} llevan anotaciones. Estas anotaciones incluyen descripciones de la funcionalidad que ofrecen sus acciones, los parámetros que se pueden utilizar en el momento de enlazar el paquete, los parámetros en el momento de la invocación o si dichos parámetros son secreto. Las anotaciones se añaden según sea necesario, por ejemplo, para permitir la integración con la interfaz de usuario.

Las entidades se documentan desde la CLI utilizando el distintivo `--annotation` o `-a`.

## Anotaciones de acción
{: #annotations_action}

| Anotación | Descripción |
| --- | --- |
| `description` | Una descripción de la acción. |
| `parameters` | Una matriz que describe las acciones necesarias para ejecutar la acción. |
| `sampleInput` | Un ejemplo que muestra el esquema de entrada con los valores típicos. |
| `sampleOutput` | Un ejemplo que muestra el esquema de salida, generalmente para `sampleInput`. |



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

Las siguientes anotaciones de acción web se deben establecer explícitamente en `true` para habilitar la interactividad con las API.

| Anotación | Descripción |
| --- | --- | 
| `web-export` | Cuando se aplica a una acción, la acción pasa a ser una [acción web](/docs/openwhisk?topic=cloud-functions-actions_web). La acción pasa a estar accesible desde llamadas REST sin autenticación de forma que los usuarios pueden acceder a dichas acciones desde un navegador. El propietario de la acción web incurre en un coste al ejecutarlas. Es decir, el propietario de la acción también es el propietario del registro de activaciones. |
| `final` | Cuando se aplica a una acción, los parámetros de acción definidos con anterioridad no se pueden sobrescribir por parámetros proporcionados durante la invocación. |
| `raw-http` | Cuando se aplica a una acción que tiene la anotación `web-export`, los parámetros de cuerpo y consulta de la solicitud HTTP se pasan a la acción como propiedades reservadas. |
| `web-custom-options` | Habilita que la acción web responda a las solicitudes OPTIONS con cabeceras personalizadas. De lo contrario, se aplica una [respuesta CORS predeterminada](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options). |
| `require-whisk-auth` | La acción web sólo puede ser invocada por las solicitudes que proporcionen las credenciales de autenticación adecuadas. Cuando se establece en un valor booleano, controla si el valor de autenticación básica de la solicitud está autenticada. El valor `true` autentica las credenciales y el valor `false` invoca la acción sin ninguna autenticación. Cuando se establece en un entero o en una serie, este valor debe coincidir con el valor de la cabecera `X-Require-Whisk-Auth` de la solicitud. |

## Anotaciones de paquete
{: #annotations_package}

| Anotación | Descripción |
| --- | --- |
| `description` | Una descripción del paquete. |
| `parameters` | Una matriz que describe los parámetros que abarca el paquete. |

## Anotaciones de parámetros
{: #annotations_parameter}

| Anotación | Descripción |
| --- | --- |
| `name` | El nombre del parámetro. |
| `description` | Una sucinta descripción del parámetro. |
| `doclink` | Un enlace con más documentación sobre el parámetro (útil para señales OAuth). |
| `required` | True para los parámetros obligatorios y false para los opcionales. |
| `bindTime` | True si se especifica el parámetro cuando se enlaza un paquete. |
| `type` | El tipo de parámetro, que puede ser `password`, `array` (aunque se puede utilizar en más situaciones). |

## Anotaciones de activación
{: #annotations_activation}

Puede documentar los registros de activación con las siguientes anotaciones:

| Anotación | Descripción |
| --- | --- |
| `path` | El nombre de la vía de acceso completa de la acción que ha generado la activación. Si esta activación ha sido el resultado de una acción en un enlace de paquete, la vía de acceso hace referencia al paquete padre. |
| `kind` | El tipo de acción ejecutada, y uno de los tipos de entorno de ejecución de {{site.data.keyword.openwhisk_short}} soportados. |
| `limits` | El tiempo, la memoria y los límites de registro a los que está sujeta esta activación. |

Para activaciones relacionadas con secuencias, el sistema genera las anotaciones siguientes:

| Anotación | Descripción |
| --- | --- |
| `topmost` | Esta anotación solo está presente para una acción de secuencia externa. |
| `causedBy` | Esta anotación solo está presente para acciones incluidas en una secuencia. |
| `waitTime` | El tiempo invertido en esperar en el sistema {{site.data.keyword.openwhisk_short}} interno. Este tiempo es aproximadamente el tiempo transcurrido desde la recepción de la solicitud de activación hasta el momento en que el invocador ha suministrado un contenedor para la acción. Este valor sólo está presente para activaciones que no sean de secuencia. Para las secuencias, esta información se puede obtener del registro de activación de secuencia de `topmost`. |
| `initTime` | El tiempo invertido en inicializar la función. Si este valor está presente, la acción ha necesitado inicialización y representa un inicio en frío. En las activaciones en caliente, se omite la inicialización y, en este caso, la anotación no se genera. |

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




