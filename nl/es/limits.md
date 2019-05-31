---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions

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

# Detalles y límites del sistema
{: #limits}

En las secciones siguientes se proporcionan detalles sobre el sistema {{site.data.keyword.openwhisk}} y los valores límite.
{: shortdesc}

## Límites del sistema
{: #limits_syslimits}

### Acciones
{: #limits_actions}

{{site.data.keyword.openwhisk_short}} tiene algunos límites del sistema, incluyendo la cantidad de memoria que puede utilizar una acción y
cuántas invocaciones de acción se permiten por minuto.

En la tabla siguiente se proporciona una lista con los límites predeterminados de las acciones.

| Límite | Descripción | Valor predeterminado | Mínimo | Máximo |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](#limits_codesize) | El tamaño máximo del código de acción en MB. | 48 | 1 | 48 |
| [concurrent](#limits_concurrent) | No se pueden enviar más de N activaciones por espacio de nombres ya sea ejecutándose o en cola para la ejecución. | 1000 | 1 | 1000* |
| [logs](#limits_logs) | Un contenedor no tiene permiso para escribir más de N MB en stdout. | 10 | 0 | 10 |
| [memory](#limits_memory) | Un contenedor no tiene permiso para asignar más de n MB de memoria. | 256 | 128 | 2048 |
| [minuteRate](#limits_minuterate) | No se pueden enviar más de N activaciones por espacio de nombres por minuto. | 5000 | 1 | 5000* |
| [openulimit](#limits_openulimit) | El número máximo de archivos abiertos para una acción. | 1024 | 0 | 1024 |
| [parameters](#limits_parameters) | El tamaño máximo de los parámetros que se pueden adjuntar en MB. | 5 | 0 | 5 |
| [proculimit](#limits_proculimit) | El número máximo de procesos disponibles para una acción. | 1024 | 0 | 1024 |
| [result](#limits_result) | El tamaño máximo de los resultados de la invocación de acción en MB. | 5 | 0 | 5 |
| [sequenceMaxActions](#limits_sequencemax) | El número máximo de acciones que puede contener una secuencia determinada. | 50 | 0 | 50* |
| [timeout](#limits_timeout) | Un contenedor no tiene permiso para ejecutarse más de N milisegundos. | 60000 | 100 | 600000 |

### Aumento de los límites fijos
{: #limits_increase}

Los valores límite seguidos de (*) son fijos, pero se pueden aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Si desea aumentar el valor límite, póngase en contacto con el soporte de IBM para abrir una incidencia directa desde la [consola web de IBM {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk).
  1. Seleccione **Support**.
  2. Seleccione **Add Ticket** en el menú desplegable.
  3. Seleccione **Technical** para el tipo de incidencia.
  4. Seleccione **Functions** para el área técnica de soporte.

#### codeSize (MB) (fijo: 48 MB)
{: #limits_codesize}
* El tamaño máximo de código de la acción es de 48 MB.
* Para las acciones JavaScript, utilice una herramienta para concatenar todo el código fuente, incluidas las dependencias, en un único archivo empaquetado.
* Este límite es fijo y no se puede cambiar.

#### concurrent (fijo: 1000*)
{: #limits_concurrent}
* El número de activaciones que se están ejecutando o en cola para su ejecución para un espacio de nombres no puede superar 1000.
* Este valor límite es fijo, pero se puede aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Consulte la sección [Aumento de los límites fijos](/docs/openwhisk?topic=cloud-functions-limits#limits_increase) para obtener instrucciones detalladas sobre cómo aumentar este límite.

#### logs (MB) (predeterminado: 10 MB)
{: #limits_logs}
* El límite de registro N está en el intervalo [0 MB..10 MB] y se establece por acción.
* Un usuario puede cambiar el límite de registro de acción cuando se crea o se actualiza una acción.
* Los registros que superen el límite establecido se truncan, de forma que se ignora cualquier entrada de registro nueva, y se añade un aviso como última salida de la activación para indicar que la activación ha sobrepasado el límite de registro establecido.

#### memory (MB) (predeterminado: 256 MB)
{: #limits_memory}
* El límite de memoria M está en el intervalo [128 MB..2048 MB] y se establece por acción en MB.
* Un usuario puede cambiar el límite de memoria cuando se crea una acción.
* Un contenedor no puede utilizar más memoria que la que asigna el límite.

#### minuteRate (fijo: 5000*)
{: #limits_minuterate}
* El límite de tasa N se establece en 5000 y limita el número de invocaciones de acción en espacios de 1 minuto.
* Una llamada de la CLI o API que sobrepase este límite recibe un código de error correspondiente al código de estado de HTTP `429: DEMASIADAS SOLICITUDES`.
* Este valor límite es fijo, pero se puede aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Consulte la sección [Aumento de los límites fijos](/docs/openwhisk?topic=cloud-functions-limits#limits_increase) para obtener instrucciones detalladas sobre cómo aumentar este límite.

#### openulimit (fijo: 1024:1024)
{: #limits_openulimit}
* El número máximo de archivos abiertos para una acción es de 1024 (tanto para el límite absoluto como el flexible).
* Este límite es fijo y no se puede cambiar.
* Cuando se invoca una acción, el mandato de ejecución de docker utiliza el argumento `--ulimit nofile=1024:1024` para establecer el valor de `openulimit`.
* Para obtener más información, consulte la documentación de referencia de la línea de mandatos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parameters (fijo: 5 MB)
{: #limits_parameters}
* El límite de tamaño para los parámetros totales al crear o actualizar una acción/paquete/desencadenante es 5 MB.
* La creación o actualización de una entidad con parámetros demasiado grandes se rechaza.
* Este límite es fijo y no se puede cambiar.

#### proculimit (fijo: 1024:1024)
{: #limits_proculimit}
* El número máximo de procesos disponibles para el contenedor de acciones es de 1024.
* Este límite es fijo y no se puede cambiar.
* Cuando se invoca una acción, el mandato de ejecución de docker utiliza el argumento `--pids-limit 1024` para establecer el valor de `proculimit`.
* Para obtener más información, consulte la documentación de referencia de la línea de mandatos [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### result (fijo: 5 MB)
{: #limits_result}
* El tamaño máximo de salida del resultado de invocación de una acción en MB.
* Este límite es fijo y no se puede cambiar.

#### sequenceMaxActions (fijo: 50*)
{: #limits_sequencemax}
* El número máximo de acciones que puede contener una secuencia determinada.
* Este límite es fijo y no se puede cambiar.

#### timeout (ms) (predeterminado: 60s)
{: #limits_timeout}
* El límite de tiempo de espera N está en el intervalo [100 ms..600000 ms] y se establece por acción, en milisegundos.
* Un usuario puede cambiar el límite de tiempo de espera cuando se crea una acción.
* Un contenedor que se ejecuta más de N milisegundos, se finaliza.

### Desencadenantes
{: #limits_triggers}

Los desencadenantes están sujetos a una tasa de activación por minuto tal como se indica en la tabla siguiente.

| Límite | Descripción | Valor predeterminado | Mínimo | Máximo |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](#limits_triggersminuterate) | No se pueden activar más de N desencadenantes por espacio de nombres por minuto. | 5000* | 5000* | 5000* |

### Aumento de los límites fijos
{: #limits_triggersfixed}

Los valores límite seguidos de (*) son fijos, pero se pueden aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Si desea aumentar el valor límite, póngase en contacto con el soporte de IBM para abrir una incidencia directa desde la [consola web de IBM {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk).
  1. Seleccione **Support**.
  2. Seleccione **Add Ticket** en el menú desplegable.
  3. Seleccione **Technical** para el tipo de incidencia.
  4. Seleccione **Functions** para el área técnica de soporte.

#### minuteRate (fijo: 5000*)
{: #limits_triggersminuterate}

* El límite de tasa N se establece en 5000 y limita el número de desencadenantes que un usuario puede activar en intervalos de 1 minuto.
* Un usuario no puede cambiar el límite del desencadenante cuando se crea un desencadenante.
* Una llamada de la CLI o API que sobrepase este límite recibe un código de error correspondiente al código de estado de HTTP `429: DEMASIADAS SOLICITUDES`.
* Este valor límite es fijo, pero se puede aumentar si un caso de negocio puede justificar valores límite superiores de seguridad. Consulte la sección [Aumento de los límites fijos](#limits_triggersfixed) para obtener instrucciones detalladas sobre cómo aumentar este límite.


## Entidades de {{site.data.keyword.openwhisk_short}}
{: #limits_entities_ov}

### Espacios de nombres y paquetes
{: #limits_namespaces}

Las acciones, desencadenantes y reglas de {{site.data.keyword.openwhisk_short}} pertenecen a un espacio de nombres y, a veces, a un paquete.

Los paquetes pueden contener acciones y canales de información. Un paquete no puede contener otro paquete, por lo que no se permite anidamiento de paquetes. Además, las entidades no tienen que estar obligatoriamente contenidas en un paquete.

Puede crear nuevos espacios de nombres basados en IAM ejecutando `ibmcloud fn namespace create`.  Los espacios de nombres basados en Cloud Foundry se componen de una combinación de un nombre de organización y de espacio. Por ejemplo, si tiene como destino la organización `user@email.com` y el espacio `dev`, su destino será el espacio de nombres basado en Cloud Foundry de {{site.data.keyword.openwhisk_short}} denominado `user@email.com_dev`. 

El espacio de nombres `/whisk.system` se reserva para entidades distribuidas con el sistema {{site.data.keyword.openwhisk_short}}.


### Nombres completos
{: #limits_fullnames}

El nombre completo de una entidad es `/namespaceName/[packageName]/entityName`. Observe que
se utiliza `/` para delimitar espacios de nombres, paquetes y entidades. Además, los espacios de nombres deben
tener una `/` como prefijo.

Para mayor comodidad, el espacio de nombres se puede obviar si es el espacio de nombres predeterminado del usuario. Por ejemplo, supongamos un usuario cuyo espacio de nombres predeterminado es `/myOrg`. A continuación se muestran ejemplos de los nombres completos de una serie de entidades y sus alias.

| Nombre completo | Alias | Espacio de nombres | Paquete | Nombre |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Puede utilizar este esquema de denominación cuando usa la CLI de {{site.data.keyword.openwhisk_short}}, entre otros lugares.

### Nombres de entidad
{: #limits_entities}

Los nombres de todas las entidades incluidas las acciones, desencadenantes, reglas, paquetes y los espacios de nombres están en una secuencia de caracteres que cumplen el formato siguiente:

* El primer carácter debe ser un carácter alfanumérico o un signo de subrayado.
* Los caracteres posteriores pueden ser alfanuméricos, espacios o cualquiera de los siguientes valores: `_`, `@`, `.`, `-`.
* El último carácter no puede ser un espacio.

Concretamente, un nombre debe coincidir con la siguiente expresión regular (expresada con la sintaxis de metacaracteres de Java): `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Semánticas de acción
{: #limits_semantics}

En las secciones siguientes se describen los detalles sobre las acciones de
{{site.data.keyword.openwhisk_short}}.

### Falta de estado
{: #limits_stateless}

Las implementaciones de acciones son sin estado, o *idempotent*. Aunque el sistema no impone esta propiedad, no hay garantía de que cualquier estado mantenido por una acción esté disponible entre invocaciones.

Además, se puede dar la creación de varias instancias de una acción, teniendo cada creación de instancias su propio estado. Una invocación de acción se podría asignar a cualquiera de estas creaciones de instancias.

### Entrada y salida de invocación
{: #limits_invocationio}

La entrada y salida de una acción es un diccionario de pares de clave/valor. La clave es una serie, y el valor un valor JSON válido.

### Ordenación de invocaciones de acciones
{: #limits_ordering}

Las invocaciones de una acción no están ordenadas. Si el usuario invoca una acción dos veces desde la línea de mandatos o la API REST, la segunda invocación podría ejecutarse antes que la primera. Si las acciones tienen efectos secundarios, se podrían observar en cualquier orden.

Además, no se garantiza que las acciones se ejecuten automáticamente. Dos acciones se pueden ejecutar de forma simultánea y tener efectos secundarios que se entrelacen. OpenWhisk no asegura ningún modelo de coherencia simultáneo concreto en cuanto a efectos secundarios. Los efectos secundarios de simultaneidad dependen de la implementación.

### Ejecuciones de acciones
{: #limits_exec}

Cuando se recibe una solicitud de invocación, el sistema registra la solicitud y asigna una activación.

El sistema devuelve un ID de activación (con una invocación sin bloqueo) que confirma que se ha recibido.
Si se produce un error de red o de otro tipo antes de recibir una respuesta HTTP, es posible que {{site.data.keyword.openwhisk_short}} haya recibido y procesado la solicitud.

El sistema intenta invocar la acción una vez, lo que tiene uno de los cuatro resultados siguientes:
- *success*: La invocación de la acción se ha completado correctamente.
- *application error*: La invocación de la acción ha sido correcta, pero la acción ha devuelto un valor de error a propósito, por ejemplo debido a una condición previa de los argumentos que no se cumpla.
- *action developer error*: La acción se ha invocado, pero se ha completado de forma anómala, por ejemplo, la acción no ha detectado una excepción o se ha producido un error de sintaxis.
- *whisk internal error*: El sistema no ha podido invocar la acción.
El resultado se registra en el campo `status` del registro de activación, como documento en la sección siguiente.

Cada invocación que se recibe correctamente, y que se podría facturar al usuario, tiene un registro de activación.

Si el resultado es *error de desarrollador de acción*, es posible que la acción ejecute de forma parcial y genere efectos colaterales visibles externamente. El usuario es responsable de comprobar si dichos efectos colaterales se han producido y de emitir lógica de reintento si se desea. Determinados *errores internos de whisk* indican que la acción empieza a ejecutarse, pero falla antes de que la acción registre su finalización.

## Registro de activación
{: #limits_activation}

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
