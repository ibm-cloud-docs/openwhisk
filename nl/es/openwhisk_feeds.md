---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Proveedores de sucesos personalizados
{: #openwhisk_feeds}

{{site.data.keyword.openwhisk_short}} da soporte a una API abierta, donde cualquier usuario puede exponer un servicio productor de sucesos como un canal de información en un paquete. La siguiente sección describe las opciones de arquitectura e implementación para proporcionar canales de información personalizados propios.
{: shortdesc}

Este material está pensado para usuarios avanzados de {{site.data.keyword.openwhisk_short}} que deseen publicar sus propios canales de información. La mayoría de los usuarios de {{site.data.keyword.openwhisk_short}} pueden omitir la siguiente sección sobre arquitectura.

## Arquitectura de canal de información

Existen al menos tres patrones de arquitectura para crear un canal de información: **Ganchos**, **Sondeo** y **Conexiones**.

### Ganchos
En el patrón *Ganchos*, se configura un canal de información utilizando un recurso [webhook](https://en.wikipedia.org/wiki/Webhook) expuesto por otro servicio.   En esta estrategia, se configura un webhook en un servicio externo para PUBLICAR directamente en un URL y activar un desencadenante. Este método es, sin duda, la opción más fácil y atractiva para implementar canales de información de baja frecuencia.

<!-- The github feed is implemented using webhooks.  Put a link here when we have the open repo ready -->

### Sondeo
En el patrón "Sondeo", se organiza una acción de {{site.data.keyword.openwhisk_short}} para sondear un punto final periódicamente y obtener datos nuevos. La creación de este patrón es relativamente fácil, pero la frecuencia de los sucesos está limitada, como es lógico, por el intervalo de sondeo.

### Conexiones
En el patrón "Conexiones", un servicio independiente mantiene una conexión persistente con una fuente de canal de información. La implementación basada en conexión puede interactuar con un punto final de servicio mediante un sondeo largo o configurar una notificación push.

<!-- Our cloudant changes feed is connection based.  Put a link here to
an open repo -->

<!-- What is the foundation for the Message Hub feed? If it is "connections" then lets put a link here as well -->

## Diferencia entre el canal de información y el desencadenante

Los canales de información y los desencadenantes están muy relacionados, pero técnicamente son conceptos distintos.   

- {{site.data.keyword.openwhisk_short}} procesa **sucesos** que fluyen en el sistema.

- Un **desencadenante** es el nombre técnico de una clase de suceso. Cada suceso pertenece exactamente a un desencadenante; por analogía, un desencadenante parece un *tema* de un sistema pub/sub basado en temas. Una **regla** *T -> A* significa que "cuando llega un suceso desde el desencadenante *T*, se invoca la acción *A* con la carga útil del desencadenante.

- Un **canal de información** es una corriente de sucesos que pertenecen a un desencadenante *T*. Un canal de información se controla mediante una **acción de canal de información** que gestiona la creación, supresión, pausa y reanudación de la corriente de sucesos que forman el canal de información. La acción de canal de información suele interactuar con los servicios externos que producen los sucesos, utilizando la API REST que gestiona las notificaciones.

##  Implementación de acciones de canal de información

La *acción de canal de información* es una *acción* de {{site.data.keyword.openwhisk_short}} normal, y acepta los siguientes parámetros:
* **lifecycleEvent**: Uno de los siguientes: 'CREATE', 'READ', 'UPDATE', 'DELETE', 'PAUSE' o 'UNPAUSE'.
* **triggerName**: El nombre completo del desencadenante que contiene los sucesos producidos desde este canal de información.
* **authKey**: Las credenciales de autenticación básicas del usuario de {{site.data.keyword.openwhisk_short}} propietario del desencadenante.

La acción de canal de información también puede aceptar otros parámetros necesarios para gestionar el canal de información. Por ejemplo, la acción de canal de información de los cambios de {{site.data.keyword.cloudant}} espera recibir parámetros que incluyan *'dbname'*, *'username'*, etc.

Cuando el usuario crea un desencadenante desde la CLI con el parámetro **--feed**, el sistema invoca automáticamente la acción de canal de información con los parámetros apropiados.

Por ejemplo, suponga que el usuario crea un enlace de **mycloudant** para el paquete `cloudant` con un nombre de usuario y contraseña como parámetros de enlace. Cuando el usuario emita el siguiente mandato desde la CLI:
```
ibmcloud fn trigger create T --feed mycloudant/changes -p dbName myTable
```
{: pre}

Entonces, el sistema realiza una acción parecida al siguiente mandato:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

La acción de canal de información llamada *changes* obtiene estos parámetros y se espera que realice las acciones necesarias para configurar una secuencia de sucesos desde {{site.data.keyword.cloudant_short_notm}}. La acción de canal de información se produce utilizando la configuración adecuada, que está dirigida al desencadenante *T*.

Para el canal de información *changes* de {{site.data.keyword.cloudant_short_notm}}, la acción se pone en contacto directamente con un servicio *desencadenante de {{site.data.keyword.cloudant_short_notm}}* que está implementado con una arquitectura basada en conexión.

Se produce un protocolo de acción de canal de información similar para `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` e `ibmcloud fn trigger get`.

## Implementación de canales de información con ganchos

Es fácil configurar un canal de información utilizando un gancho si el productor del suceso admite un recurso webhook/callback.

Con este método _no es necesario_ configurar ningún servicio persistente fuera de {{site.data.keyword.openwhisk_short}}. Toda la gestión de canales de información se genera de forma natural a través de las *acciones de canal de información* de {{site.data.keyword.openwhisk_short}}, que negocian directamente con una API webhook de terceros.

Al invocarse con el mandato `CREATE`, la acción del canal de información simplemente instala un webhook para otro servicio, solicitando al servicio remoto que PUBLIQUE notificaciones en el URL de `fireTrigger` pertinente en {{site.data.keyword.openwhisk_short}}.

El webhook recibe instrucciones de enviar notificaciones a un URL como:

`POST /namespaces/{namespace}/triggers/{triggerName}`

El formulario con la solicitud POST se interpreta como un documento JSON que define los parámetros en el suceso desencadenante. Las reglas de {{site.data.keyword.openwhisk_short}} pasan los parámetros del desencadenante a las acciones para activarlas como resultado del suceso.

## Implementación de canales de información con sondeo

Se puede configurar una *acción* de {{site.data.keyword.openwhisk_short}} para sondear un origen de canal de información completamente dentro de {{site.data.keyword.openwhisk_short}}, sin necesidad de configurar conexiones persistentes o servicios externos.

Para los canales de información que no disponen de webhook, pero no necesitan un volumen elevado ni tiempos de respuesta de latencia bajos, el sondeo es una opción atractiva.

Para configurar un canal de información basado en sondeos, la acción de canal de información sigue los siguientes pasos cuando se llama al mandato `CREATE`:

1. La acción de canal de información configura un desencadenante periódico (*T*) con la frecuencia deseada, utilizando el canal de información `whisk.system/alarms`.
2. El desarrollador del canal de información crea una acción `pollMyService` que sondea el servicio remoto y devuelve los sucesos nuevos.
3. La acción de canal de información configura una *regla* *T -> pollMyService*.

Este procedimiento implementa un desencadenante basado en sondeo utilizando únicamente acciones de {{site.data.keyword.openwhisk_short}}, sin necesidad de un servicio independiente.

## Implementación de canales de información utilizando Conexiones

Las dos opciones de arquitectura anteriores son fáciles y rápidas de implementar. Sin embargo, si desea un canal de información de alto rendimiento, no existe ningún sustituto para conexiones persistentes, sondeo largo o técnicas similares.

Puesto que las acciones de {{site.data.keyword.openwhisk_short}} deben ser de ejecución corta, una acción no puede mantener una conexión persistente con un tercero. En lugar de eso, puede configurar un servicio independiente, denominado *servicios de proveedor*, fuera de {{site.data.keyword.openwhisk_short}} que se ejecute continuamente. Un servicio de proveedor puede mantener conexiones con orígenes de sucesos de terceros que soporten el sondeo largo u otras notificaciones basadas en conexiones.

El servicio del proveedor tiene una API REST que permite a la *acción de canal de información* de {{site.data.keyword.openwhisk_short}} controlar el canal de información. El servicio de proveedor actúa como un proxy entre el proveedor de suceso y {{site.data.keyword.openwhisk_short}}. Cuando recibe sucesos de un tercero, los envía a {{site.data.keyword.openwhisk_short}} activando un desencadenante.

El canal *changes* de {{site.data.keyword.cloudant_short_notm}} es el ejemplo canónico, ya que configura un servicio `cloudanttrigger` que media entre las notificaciones de {{site.data.keyword.cloudant_short_notm}} a través de una conexión persistente y desencadenantes de {{site.data.keyword.openwhisk_short}}.
<!-- TODO: add a reference to the open source implementation -->

El canal de información *alarm* se implementa con un patrón parecido.

La arquitectura basada en conexión es la opción de rendimiento más alto, pero impone más sobrecarga en las operaciones en comparación con las arquitecturas de sondeo y gancho.
