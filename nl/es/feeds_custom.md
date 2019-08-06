---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: feeds, serverless, functions

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



# Creación de canales de información de proveedores de sucesos personalizados
{: #feeds_custom}

{{site.data.keyword.openwhisk_short}} da soporte a una API abierta, donde cualquier usuario puede exponer un servicio productor de sucesos como un canal de información en un paquete.
{: shortdesc}


## Arquitectura de canal de información
{: #feeds_arch}

Puede crear un canal de información utilizando uno de los tres patrones arquitectónicos: **Ganchos**, **Sondeo**, y **Conexiones**.

### Ganchos

Con el patrón Ganchos, se configura un canal de información utilizando un [webhook](https://en.wikipedia.org/wiki/Webhook){: external} expuesto por otro servicio. En esta estrategia, se configura un webhook en un servicio externo para realizar una solicitud POST directamente en un URL y activar un desencadenante. Este método es, sin duda, la opción más fácil y atractiva para implementar canales de información de baja frecuencia.

Por ejemplo, el [paquete de GitHub](/docs/openwhisk?topic=cloud-functions-pkg_github) y el  [paquete de Notificación push](/docs/openwhisk?topic=cloud-functions-pkg_push_notifications) utilizan un webhook.


### Sondeo

Con el patrón Sondeo, se organiza una acción de {{site.data.keyword.openwhisk_short}} para sondear un punto final periódicamente y obtener datos nuevos. La creación de este patrón es relativamente fácil, pero la frecuencia de los sucesos está limitada, como es lógico, por el intervalo de sondeo.

### Conexiones

Con el patrón Conexiones, un servicio independiente mantiene una conexión persistente a una fuente de canal de información. La implementación basada en la conexión puede interactuar con un punto final de servicio utilizando intervalos de sondeo largos, o para configurar una notificación push.

Por ejemplo, el [paquete de {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-pkg_cloudant) utiliza el patrón de conexiones.



##  Implementación de acciones de canal de información
{: #feeds_actions}

La acción de canal de información es una acción y acepta los parámetros siguientes.

| Parámetro | Descripción |
| --- | --- |
| `lifecycleEvent` | `CREATE`, `READ`, `UPDATE`, `DELETE`, `PAUSE`, or `UNPAUSE`. |
| `triggerName` | El nombre completo del desencadenante que contiene los sucesos producidos desde este canal de información. |
| `authKey` | Las credenciales de autenticación básicas del usuario de {{site.data.keyword.openwhisk_short}} propietario del desencadenante. |

La acción de canal de información también puede aceptar otros parámetros necesarios para gestionar el canal de información. Por ejemplo, la acción de canal de información de los cambios de {{site.data.keyword.cloudant}} espera recibir parámetros que incluyan `dbname` y `username`.

Cuando el usuario crea un desencadenante desde la CLI con el parámetro `--feed`, automáticamente se invoca a la acción de canal de información con los parámetros adecuados.

Por ejemplo, un usuario crea un enlace de **mycloudant** para el paquete `cloudant` con un nombre de usuario y contraseña como parámetros de enlace. Cuando el usuario emite el siguiente mandato desde la CLI:
```
ibmcloud fn trigger create my_cloudant_trigger --feed mycloudant/changes -p dbName myTable
```
{: pre}

Se ejecuta algo equivalente al siguiente mandato:
```
ibmcloud fn action invoke mycloudant/changes -p lifecycleEvent CREATE -p triggerName T -p authKey <userAuthKey> -p password <password value from mycloudant binding> -p username <username value from mycloudant binding> -p dbName mytype
```
{: pre}

La acción de canal de información llamada *changes* obtiene estos parámetros y se espera que realice las acciones necesarias para configurar una secuencia de sucesos desde {{site.data.keyword.cloudant_short_notm}}. La acción de canal de información se produce utilizando la configuración adecuada, que está dirigida al desencadenante.

Para el canal de información *changes* de {{site.data.keyword.cloudant_short_notm}}, la acción habla directamente con un servicio de *desencadenante de {{site.data.keyword.cloudant_short_notm}}* que está implementado con una arquitectura basada en conexiones.

Se produce un protocolo de acción de canal de información similar para `ibmcloud fn trigger delete`, `ibmcloud fn trigger update` e `ibmcloud fn trigger get`.

## Implementación de canales de información con ganchos
{: #feeds_hooks}

Configure un canal de información utilizando un gancho cuando un productor de sucesos admita un recurso webhook/callback.

Con este método no es necesario configurar ningún servicio persistente fuera de {{site.data.keyword.openwhisk_short}}. Toda la gestión de canales de información se genera de forma natural a través de las **acciones de canal de información** de {{site.data.keyword.openwhisk_short}}, que negocian directamente con una API webhook de terceros.

Al invocarse con el mandato `CREATE`, la acción del canal de información simplemente instala un webhook para otro servicio, solicitando al servicio remoto que PUBLIQUE notificaciones en el URL de `fireTrigger` pertinente en {{site.data.keyword.openwhisk_short}}.

El webhook recibe instrucciones de enviar notificaciones a un URL como:

`POST /namespaces/{namespace}/triggers/{triggerName}`

El formulario con la solicitud POST se interpreta como un documento JSON que define los parámetros en el suceso desencadenante. Las reglas de {{site.data.keyword.openwhisk_short}} pasan los parámetros del desencadenante a las acciones para activarlas como resultado del suceso.

## Implementación de canales de información con sondeo
{: #feeds_polling}

Puede configurar una acción para sondear un origen de canal de información totalmente dentro de {{site.data.keyword.openwhisk_short}}, sin necesidad de configurar conexiones persistentes ni ningún servicio externo.

Para los canales de información que no disponen de webhook, pero no necesitan un volumen elevado ni tiempos de respuesta de latencia bajos, puede utilizar el sondeo.

Para configurar un canal de información basado en sondeos, la acción de canal de información sigue los siguientes pasos cuando se llama al mandato `CREATE`:

1. La acción de canal de información configura un desencadenante periódico con una frecuencia específica, utilizando el canal de información `whisk.system/alarms`.
2. El desarrollador del canal de información crea una acción `pollMyService` que sondea el servicio remoto y devuelve los sucesos nuevos.
3. La acción de canal de información configura una *regla* *T -> pollMyService*.

Este procedimiento implementa un desencadenante basado en sondeo utilizando únicamente acciones de {{site.data.keyword.openwhisk_short}}, sin necesidad de un servicio independiente.

## Implementación de canales de información utilizando conexiones
{: #feeds_connections}

Las dos opciones de arquitectura anteriores son fáciles y rápidas de implementar. Sin embargo, si desea un canal de información de alto rendimiento, puede utilizar conexiones persistentes, sondeo largo o técnicas similares.

Puesto que las acciones de {{site.data.keyword.openwhisk_short}} deben ser de ejecución corta, una acción no puede mantener una conexión persistente con un tercero. En lugar de eso, puede configurar un servicio independiente, denominado **servicios de proveedor**, fuera de {{site.data.keyword.openwhisk_short}} que se ejecute continuamente. Un servicio de proveedor puede mantener conexiones con orígenes de sucesos de terceros que soporten el sondeo largo u otras notificaciones basadas en conexiones.

El servicio del proveedor tiene una API REST que permite a la **acción de canal de información** de {{site.data.keyword.openwhisk_short}} controlar el canal de información. El servicio de proveedor actúa como un proxy entre el proveedor de suceso y {{site.data.keyword.openwhisk_short}}. Cuando recibe sucesos de un tercero, los envía a {{site.data.keyword.openwhisk_short}} activando un desencadenante.

El canal **cambios** de {{site.data.keyword.cloudant_short_notm}} es el ejemplo canónico, ya que configura un servicio `cloudanttrigger` que media entre las notificaciones de {{site.data.keyword.cloudant_short_notm}} a través de una conexión persistente y desencadenantes de {{site.data.keyword.openwhisk_short}}.


El canal de información **alarm** se implementa con un patrón parecido.

La arquitectura basada en conexiones es la opción de rendimiento más alto, pero las operaciones son más laboriosas que las arquitecturas de sondeo y ganchos.






