---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Anotaciones
{: #openwhisk_annotations}

Las acciones, desencadenantes, reglas y paquetes de {{site.data.keyword.openwhisk}} (conocidos de forma colectiva como activos) pueden contener `anotaciones`. Las anotaciones se adjuntan a los activos del mismo modo que se adjuntan parámetros con una `clave` que define un nombre y un `valor` que define el valor. Se recomienda definirlos desde la interfaz de línea de mandatos (CLI) utilizando el distintivo `--annotation` o `-a` para abreviar.
{: shortdesc}

Razón fundamental: se han añadido anotaciones a {{site.data.keyword.openWhisk_short}} para permitir que se experimente sin cambiar el esquema del activo subyacente. En el momento de redactar este documento, de forma deliberada no se han definido las `anotaciones` que se permiten. Sin embargo, como las anotaciones se utilizan mucho para difundir los cambios semánticos, es importante empezar a documentarlas.

El uso principal de las anotaciones actualmente consiste en documentar acciones y paquetes. Muchos de los paquetes del catálogo de {{site.data.keyword.openwhisk_short}} contienen anotaciones, como: una descripción de la funcionalidad que ofrecen sus acciones, parámetros para utilizar al enlazar el paquete, parámetros de invocación o si un parámetro es "secreto" (por ejemplo, una contraseña) o no. Las anotaciones se inventan según sea necesario, por ejemplo, para permitir la integración de IU.

A continuación se muestra un ejemplo de conjunto de anotaciones para una acción `echo` que devuelve sus argumentos de entrada sin modificar (es decir, `function main(args) { return args }`). Esta acción es útil para registrar parámetros de entrada, por ejemplo, como parte de una secuencia o de una regla.
```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

Entre las anotaciones que describen **paquetes**, se incluyen:

- `description`: Una sucinta descripción del paquete.
- `parameters`: Una matriz que describe los parámetros que abarca el paquete.

Entre las anotaciones que describen **acciones**, se incluyen:

- `description`: Una sucinta descripción de la acción.
- `parameters`: Una matriz que describe las acciones necesarias para ejecutar la acción.
- `sampleInput`: Un ejemplo que muestra el esquema de entrada con los valores típicos.
- `sampleOutput`: Un ejemplo que muestra el esquema de salida, generalmente para `sampleInput`.

Entre las anotaciones que describen **parámetros**, se incluyen:

- `name`: El nombre del parámetro.
- `description`: Una sucinta descripción del parámetro.
- `doclink`: Un enlace con más documentación sobre el parámetro (útil para señales OAuth).
- `required`: True para los parámetros obligatorios y false para los opcionales.
- `bindTime`: True si se especifica el parámetro cuando se enlaza un paquete.
- `type`: El tipo de parámetro, que puede ser `password`, `array` (aunque se puede utilizar en más situaciones).

Las anotaciones _no_ se comprueban. Aunque puede suceder que se utilicen anotaciones para inferir si una composición de dos acciones en una secuencia es válida, por ejemplo, el sistema aún no lo hace.

## Anotaciones específicas de acciones web
{: #annotations-specific-to-web-actions}

Recientemente, se ha ampliado la API principal con nuevas características. Para permitir que paquetes y acciones participen en estas características, se han incorporado las siguientes nuevas anotaciones con significado semántico. Estas anotaciones se deben establecer explícitamente en `true` para que tengan efecto. Si se cambia el valor `true` por `false`, el activo adjunto se excluye de la nueva API. Las anotaciones no tienen ningún otro significado en el sistema. Consulte las anotaciones siguientes:

- `web-export`: solo se aplica a una acción. Si está definida, permite que las llamadas REST puedan acceder a su acción correspondiente _sin_ autenticación. Se llaman [_acciones web_](openwhisk_webactions.html) porque permiten utilizar acciones de OpenWhisk desde un navegador, por ejemplo. Es importante tener en cuenta que el _propietario_ de la acción web es el responsable del coste de ejecutarlas en el sistema. Es decir, el _propietario_ de la acción también es el propietario del registro de activaciones.
- `final`: solo se aplica a una acción. Convierte en inalterables todos los parámetros de acción ya definidos. Un parámetro de una acción que lleva la anotación no se puede modificar mediante parámetros durante la invocación una vez que el valor del parámetro está definido mediante el paquete que lo contiene o la definición de la acción.
- `raw-http`: solo se aplica a una acción en presencia de una anotación `web-export`. Si aparece, los parámetros de cuerpo y consulta de la solicitud HTTP se pasan parámetros como propiedades reservadas.
- `web-custom-options`: Cuando se establece, esta anotación permite una acción web para responder a las solicitudes OPTIONS con cabeceras personalizadas, de lo contrario, se aplica una [respuesta CORS predeterminada](openwhisk_webactions.html#options-requests).
- `require-whisk-auth`: esta anotación protege la acción web de modo que solo se invoque mediante solicitudes que especifiquen las credenciales de autenticación adecuadas. Cuando se establece en un valor booleano, controla si el valor de autenticación básica de la solicitud (es decir, la clave de autenticación de Whisk) se autenticará o no. El valor `true` autentica las credenciales y el valor `false` invoca la acción sin ninguna autenticación. Cuando se establece en un número o una serie, este valor debe coincidir con el valor de la cabecera `X-Require-Whisk-Auth` de la solicitud. En ambos casos, es importante tener en cuenta que el _propietario_ de la acción de la web es el responsable del coste de ejecutarlas en el sistema (es decir, el _propietario_ de la acción también es el propietario del registro de activaciones).

## Anotaciones específicas de las activaciones

El sistema puede decorar los registros de activación con las anotaciones siguientes:

- `path`: el nombre de la vía de acceso completa de la acción que ha generado la activación. Tenga en cuenta que, si esta activación ha sido el resultado de una acción en un enlace de paquete, la vía de acceso hace referencia al paquete padre.
- `kind`: el tipo de acción ejecutada, y uno de los tipos de tiempo de ejecución de OpenWhisk admitidos.
- `limits`: el tiempo, la memoria y los límites de registro a los que está sujeta esta activación.

Para activaciones relacionadas con la secuencia, el sistema genera las anotaciones siguientes:

- `topmost`: solo está presente para una acción de secuencia externa.
- `causedBy`: solo está presente para acciones incluidas en una secuencia.

Por último, con el objeto de proporcionar transparencia de rendimiento, las activaciones también registran:

- `waitTime`: el tiempo de espera en el sistema interno de OpenWhisk. Se trata de, aproximadamente, el tiempo transcurrido desde que el controlador recibe la solicitud de activación hasta que el invocador suministra un contenedor para la acción. Actualmente, este valor sólo está presente para activaciones que no sean de secuencia. Para las secuencias, esta información se puede obtener del registro de activación de secuencia de `topmost`.
- `initTime`: el tiempo dedicado a inicializar la función. Si este valor está presente, la acción ha necesitado inicialización y representa un inicio en frío. En las activaciones en caliente, se omite la inicialización y, en este caso, la anotación no se genera.

A continuación, se muestra un ejemplo de cómo aparecerían estas anotaciones en un registro de activación.

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
