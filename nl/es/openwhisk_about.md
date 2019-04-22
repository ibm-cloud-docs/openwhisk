---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: platform architecture, openwhisk, couchdb, kafka

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Arquitectura de la plataforma
{: #openwhisk_about}

{{site.data.keyword.openwhisk}} es una plataforma de cálculo dirigida por sucesos, también conocida como Computación sin servidor o Function as a Service (FaaS), que ejecuta código en respuesta a sucesos o invocaciones directas.
{: shortdesc}

## Tecnología de {{site.data.keyword.openwhisk_short}}
{: #technology}

Obtenga más información acerca de algunos conceptos básicos de la tecnología subyacente a {{site.data.keyword.openwhisk_short}}:

<dl>
  <dt>Acción</dt>
    <dd>Una [acción](/docs/openwhisk?topic=cloud-functions-openwhisk_actions) es una parte de código que realiza una tarea específica. Una acción se puede escribir en el lenguaje de su elección, como pequeños fragmentos de código JavaScript o código Swift, o bien código binario personalizado. Especifique la acción en Cloud Functions como código fuente o como una imagen de Docker.
    <br><br>Una acción realiza un trabajo cuando se invoca directamente utilizando la API de {{site.data.keyword.openwhisk_short}}, CLI o el SDK de iOS. Una acción también puede responder automáticamente a los sucesos desde servicios de {{site.data.keyword.Bluemix_notm}} y servicios de terceros utilizando un desencadenante.</dd>
  <dt>Secuencia</dt>
    <dd>Se puede encadenar un conjunto de acciones en una [secuencia](/docs/openwhisk?topic=cloud-functions-openwhisk_create_action_sequence) sin tener que escribir código. Una secuencia es una cadena de acciones, invocadas en orden, donde el resultado de una se pasa como entrada a la siguiente acción. Esto le permite combinar acciones existentes juntas para una reutilización rápida y sencilla. A continuación, se puede invocar una secuencia como si fuera una acción, mediante una API REST o automáticamente en respuesta a sucesos.
  </dd>
  <dt>Suceso</dt>
    <dd>Como ejemplos de sucesos se pueden citar cambios en los registros de base de datos, lecturas de sensor IoT que sobrepasen una
temperatura determinada, nuevas confirmaciones de código en un repositorio GitHub o solicitudes HTTP sencillas desde apps web o de móvil. Los sucesos de orígenes de sucesos externos e internos se ponen en el canal por medio de un desencadenante, y las reglas permiten acciones de respuesta para dichos sucesos.</dd>
  <dt>Desencadenante</dt>
    <dd>Los [desencadenantes](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_triggers_create) son un canal con nombre para una clase de sucesos. Un desencadenante es una declaración a la que desea reaccionar en un determinado tipo de suceso, ya sea un usuario o mediante un origen de sucesos.</dd>
  <dt>Regla</dt>
    <dd>Una [regla](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use) asocia un desencadenante con una acción. Cada vez que se activa el desencadenante, la regla utiliza el suceso desencadenante como entrada e invoca a la acción asociada. Con el conjunto adecuado de reglas, es posible que un único suceso desencadenante invoque varias acciones, o que
una acción se invoque como respuesta a sucesos de distintos desencadenantes.</dd>
  <dt>Canal de información</dt>
    <dd>Un [canal de información](/docs/openwhisk?topic=cloud-functions-openwhisk_feeds#openwhisk_feeds) es una forma cómoda de configurar un origen de sucesos externo para activar sucesos desencadenantes que {{site.data.keyword.openwhisk_short}} pueda consumir. Por ejemplo, un canal de información Git podría activar un suceso desencadenante para cada confirmación en un repositorio Git.</dd>
  <dt>Paquete</dt>
    <dd>Las integraciones con servicios y proveedores de sucesos se pueden añadir con paquetes. Un [paquete](/docs/openwhisk?topic=cloud-functions-openwhisk_packages) es una agrupación de canales de información y acciones. Un canal de información es un fragmento de código que configura un origen de suceso externo para activar sucesos desencadenantes. Por ejemplo, un desencadenante creado con un canal de información de cambios de {{site.data.keyword.cloudant}} configura un servicio para que active el desencadenante siempre que se modifique un documento o se añada a una base de datos {{site.data.keyword.cloudant_short_notm}}. Las acciones en paquetes representan lógica reutilizable que un proveedor de servicios puede poner a disponibilidad de los desarrolladores para que puedan utilizar el servicio como un origen de sucesos e invocar las API de dicho servicio.
    <br><br>Un catálogo de paquetes existente ofrece una forma rápida de mejorar las aplicaciones con prestaciones útiles, y para acceder a servicios externos en el ecosistema. Algunos de los servicios externos que tienen paquetes de {{site.data.keyword.openwhisk_short}} son {{site.data.keyword.cloudant_short_notm}}, The Weather Company, Slack y GitHub.</dd>
</dl>

## Funcionamiento de {{site.data.keyword.openwhisk_short}}
{: #openwhisk_how}

Para explicar detalladamente todos los componentes, hagamos un seguimiento de una invocación de una acción a través del sistema de {{site.data.keyword.openwhisk_short}}. Una invocación ejecuta el código que el usuario ha incorporado en el sistema y devuelve los resultados de la ejecución. La figura siguiente muestra la arquitectura de {{site.data.keyword.openwhisk_short}} de alto nivel.

![Arquitectura de {{site.data.keyword.openwhisk_short}}](./images/OpenWhisk.png)


## Cómo funciona el proceso interno de OpenWhisk
{: #openwhisk_internal}

¿Qué pasa entre bastidores en OpenWhisk?

OpenWhisk es un proyecto de código abierto que combina componentes que incluyen Nginx, Kafka, Docker y CouchDB para formar un servicio de programación basado en sucesos sin servidor.

<img src="images/OpenWhisk_flow_of_processing.png" width="550" alt="El flujo interno del proceso subyacente a OpenWhisk" style="width:550px; border-style: none"/>

### Entrar en el sistema: nginx

En primer lugar, la API de usuario de OpenWhisk se basa en HTTP y sigue un diseño RESTful. Por lo tanto, el mandato que se envía mediante la CLI es una solicitud HTTP realizada al sistema OpenWhisk. El mandato específico se convierte a groso modo en:
```
POST /api/v1/namespaces/$userNamespace/actions/myAction
Host: $openwhiskEndpoint
```
{: screen}

Observe la variable *$userNamespace*. Un usuario tiene acceso al menos a un espacio de nombres. Para simplificar, suponga que el usuario es el propietario del espacio de nombres en el que se coloca *myAction*.

El primer punto de entrada en el sistema se realiza a través de **nginx**, “un servidor proxy invertido y HTTP”. Se utiliza para la terminación SSL y para reenviar las llamadas HTTP adecuadas al siguiente componente.

### Entrar en el sistema: Controlador

Nginx reenvía la solicitud HTTP al **controlador**, el siguiente componente en la vía de acceso mediante OpenWhisk. Es una implementación basada en Scala de la API REST real (basada en **Akka** y **Spray**) y por lo tanto sirve como interfaz para todo lo que el usuario puede hacer. Incluidas las solicitudes [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) destinadas a las entidades de OpenWhisk y la invocación de acciones.

En primer lugar, el Controlador determina lo que el usuario intenta hacer. Lo hace basándose en el método HTTP utilizado en la solicitud HTTP. En el caso anterior, el usuario emite una solicitud POST a una acción existente, que el Controlador convierte en una **invocación de una acción**.

Dado el papel central que juega el Controlador (de ahí su nombre), dicho componente participará de alguna manera en todos los pasos siguientes.

### Autenticación y autorización: CouchDB

Ahora el Controlador verifica quién es el usuario (*Autenticación*) y si tiene privilegio para hacer lo que quiere hacer con esta entidad (*Autorización*). Las credenciales incluidas en la solicitud se comparan con la base de datos llamada **subjects** de una instancia **CouchDB**.

En este caso, se comprueba que el usuario existe en la base de datos de OpenWhisk y que tiene privilegio para invocar la acción *myAction*, que se supone que es una acción en un espacio de nombres propiedad del usuario. Esto último otorga al usuario el privilegio de invocar la acción.

Se abre la puerta al siguiente paso del proceso.

### Obtención de la acción: CouchDB… de nuevo

Como ahora el Controlador está seguro de que el usuario tiene permiso y privilegios para invocar la acción, carga esta acción (en este caso *myAction*) desde la base de datos **whisks** en CouchDB.

El registro de la acción contiene principalmente el código que se va a ejecutar y los parámetros predeterminados que el usuario desea pasar a la acción, junto con los parámetros que ha incluido en la solicitud de invocación real. También contiene las restricciones de recursos que se imponen sobre su ejecución, como por ejemplo la memoria que tiene permitido consumir.

En este caso en concreto, la acción no toma ningún parámetro (la definición de parámetros de la función es una lista vacía). Por lo tanto, se presupone que no se ha definido ningún parámetro predeterminado, ni ningún parámetro específico para la acción, lo que constituye el caso más simple desde este punto de vista.


### Quién hay para invocar la acción: Equilibrador de carga

El Equilibrador de carga, que forma parte del Controlador, tiene una visión global de los ejecutores disponibles en el sistema mediante la comprobación continua del estado del sistema. Estos ejecutores también se denominan **Invocadores**. Puesto que sabe qué Invocadores están disponibles, el Equilibrador de carga elige uno de ellos para invocar la acción solicitada.

### Hagan cola: Kafka

A partir de ahora, la solicitud de invocación que se ha enviado está sujeta principalmente a dos problemas potenciales:

1. El sistema puede colgarse, con lo que se perdería la invocación.
2. El sistema puede estar sometido a una carga tan pesada que la invocación tenga que esperar a que terminen otras invocaciones.

La solución en ambos casos es **Kafka**, “un sistema de mensajería de tipo publicación-suscripción distribuido de alto rendimiento”. El Controlador y el Invocador solo se comunican a través de mensajes que Kafka guarda en almacenamiento intermedio de forma permanente. Kafka incrementa la carga de almacenamiento intermedio en memoria, aumentando el riesgo de que se produzca una *OutOfMemoryException* del Controlador y del Invocador, así que se asegura que no se pierdan los mensajes en el caso de que se cuelgue el sistema.

Para que se invoque la acción, el Controlador publica un mensaje en Kafka que contiene la acción que se debe invocar y los parámetros que hay que pasar a dicha acción (en este caso, ninguno). Este mensaje se dirige al Invocador que ha elegido el Controlador de la lista que ha obtenido de Consul.

Cuando Kafka confirma que ha recibido el mensaje, se responde a la solicitud HTTP al usuario con **ActivationId**. El usuario puede utilizarlo más adelante para acceder a los resultados de la invocación específica. Se trata de un modelo de invocación asíncrona, donde la solicitud HTTP termina en cuanto el sistema acepta la solicitud de invocar la acción. Hay disponible un modelo síncrono (que recibe el nombre de invocación de bloqueo), pero no se describe aquí.

### Invocación del código: Invocador

El **Invocador** es el centro de OpenWhisk. La función del Invocador es invocar una acción. También se implementa en Scala. Pero va mucho más allá. Para ejecutar acciones de forma aislada y segura utiliza **Docker**.

Docker sirve para configurar un nuevo entorno de encapsulación automática (denominado *contenedor*) para cada acción que se invoca de forma rápida, aislada y controlada. Para cada invocación de una acción se genera un contenedor Docker y se inserta el código de la acción. A continuación, el código se ejecuta utilizando los parámetros que se le han pasado, se obtiene el resultado y el contenedor se destruye. Las optimizaciones de rendimiento pueden realizarse en esta etapa para reducir la sobrecarga y agilizar los tiempos de respuesta.

En este caso, teniendo una acción basada en *Node.js*, el Invocador inicia un contenedor Node.js. A continuación, inserta el código procedente de *myAction*, lo ejecuta sin parámetros, extrae el resultado, guarda los registros y destruye el contenedor Node.js de nuevo.

### Almacenamiento de los resultados: CouchDB de nuevo

Cuando el Invocador obtiene el resultado, se almacena en la base de datos **whisks** como una activación bajo el ActivationId. La base de datos **whisks** se encuentra en **CouchDB**.

En este caso específico, el Invocador obtiene el objeto JSON resultante de la acción, recupera el registro que ha escrito Docker, los coloca en el registro de activación y lo almacena en la base de datos. Consulte el ejemplo siguiente:
```json
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
{: codeblock}

Observe cómo el registro contiene el resultado devuelto y los registros escritos. También contiene la hora de inicio y de finalización de la invocación de la acción. Los registros de activación contienen más campos, pero en este ejemplo se han reducido para simplificar.

Ahora puede volver a utilizar la API REST (volver al paso 1) para obtener su activación y, por lo tanto, el resultado de la acción. Para ello, ejecute este mandato:

```bash
ibmcloud fn activation get 31809ddca6f64cfc9de2937ebd44fbb9
```
{: pre}

### Resumen

Puede ver cómo una sencilla acción **ibmcloud fn action invoked myAction** pasa por las distintas fases del sistema {{site.data.keyword.openwhisk_short}}. El propio sistema consta principalmente de sólo dos componentes personalizados, el **Controlador** y el **Invocador**. Todo lo demás ya se encuentra allí, desarrollado por muchos miembros de la comunidad de código abierto.

Puede encontrar información adicional sobre {{site.data.keyword.openwhisk_short}} en los temas siguientes:

* [Nombres de entidad](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities)
* [Semánticas de acción](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_semantics)
* [Límites](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits)
* [Referencia de API REST](/apidocs/functions)
