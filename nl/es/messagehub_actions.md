---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: message hub, package, messages, events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Paquete {{site.data.keyword.messagehub}}
{: #catalog_message_hub}

Un paquete que permite la comunicación con las instancias de [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) para publicar y consumir mensajes utilizando la API de Kafka nativa y de alto rendimiento.
{: shortdesc}

## Configuración de un paquete de {{site.data.keyword.messagehub}} utilizando {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Cree una instancia del servicio {{site.data.keyword.messagehub}} bajo su organización actual y el espacio que utiliza para {{site.data.keyword.openwhisk}}.

2. Compruebe que el tema que desea escuchar está disponible en {{site.data.keyword.messagehub}} o cree un tema nuevo, como por ejemplo **mytopic**.

3. Actualice los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para la instancia del servicio {{site.data.keyword.messagehub}} que ha creado.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Salida de ejemplo:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. Obtenga una lista de los paquetes de su espacio de nombres para mostrar que el enlace de paquetes ya está disponible.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Ahora su enlace de paquete contiene credenciales que están asociadas a la instancia de {{site.data.keyword.messagehub}}.

## Configuración de un paquete de {{site.data.keyword.messagehub}} fuera de {{site.data.keyword.Bluemix_notm}}

Si quiere configurar {{site.data.keyword.messagehub}} fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para el servicio {{site.data.keyword.messagehub}}. Necesita la información sobre conexión y credenciales del servicio {{site.data.keyword.messagehub}}.

Cree un enlace de paquete configurado para el servicio {{site.data.keyword.messagehub}}.
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Escucha de mensajes utilizando sucesos

Para ver información detallada sobre cómo utilizar desencadenantes en {{site.data.keyword.messagehub}} para escuchar mensajes, consulte el siguiente tema sobre el [origen de los sucesos de {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub), en el que se tratan las siguientes tareas:
* [Creación de un desencadenante que realice la escucha de una instancia de {{site.data.keyword.messagehub}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger)
* [Creación de un desencadenante para un paquete de {{site.data.keyword.messagehub}} fuera de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#create_message_hub_trigger_outside)
* [Escucha de mensajes](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#message_hub_listen)
* [Ejemplos](/docs/openwhisk?topic=cloud-functions-openwhisk_catalog_message_hub#examples)

## Generación de mensajes en {{site.data.keyword.messagehub}}
{: #producing_messages}

La acción `/messaging/messageHubProduce` ha quedado en desuso y se eliminará en una fecha futura. Ya se ha eliminado en la región de Tokio. Para mantener un rendimiento óptimo, migre el uso de la acción `/messaging/messageHubProduce` para utilizar una conexión permanente cuando los datos se generen en {{site.data.keyword.messagehub}}/Kafka.
{: tip}

Para obtener más información sobre cómo producir mensajes, consulte la
[Documentación de Event Streams](/docs/services/EventStreams?topic=eventstreams-producing_messages#producing_messages).

## Referencias
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub)
- [Apache Kafka](https://kafka.apache.org)
