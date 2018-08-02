---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Origen de sucesos de Message Hub
{: #openwhisk_catalog_message_hub}

Puede crear un desencadenante que reacciona cuando se publiquen mensajes en una instancia de {{site.data.keyword.messagehub_full}} mediante canales de información. Aprenda a crear desencadenantes de {{site.data.keyword.messagehub}} con o sin {{site.data.keyword.Bluemix}}, a escuchar mensajes y a manejar mensajes por lotes.
{: shortdesc}

## Paquete de {{site.data.keyword.messagehub}}

La acción `/messaging/messageHubProduce` ha quedado en desuso y se eliminará en una fecha futura. Para mantener un rendimiento óptimo, migre el uso de la acción `/messaging/messageHubProduce` para utilizar una conexión permanente cuando los datos se generen en Message Hub o Kafka.
{: tip}

Este paquete permite la comunicación con las instancias de [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub) para publicar y consumir mensajes utilizando la API de Kafka nativa y de alto rendimiento. Para obtener más información sobre el paquete de {{site.data.keyword.messagehub}}, cómo configurarlo y cómo generar mensajes, consulte el tema sobre el [paquete de {{site.data.keyword.messagehub}}](./messagehub_actions.html).

## Creación de un desencadenante que realice la escucha de una instancia de {{site.data.keyword.messagehub}}
{: #create_message_hub_trigger}

Para crear un desencadenante que reaccione cuando se publican mensajes en una instancia de {{site.data.keyword.messagehub}}, debe utilizar el canal de información denominado `/messaging/messageHubFeed`. La acción de canal de información admite los siguientes parámetros:

|Nombre|Tipo|Descripción|
|---|---|---|
|kafka_brokers_sasl|Matriz JSON de series|Este parámetro es una matriz de series de caracteres `<host>:<port>` que comprenden los intermediarios de la instancia de {{site.data.keyword.messagehub}}|
|user|Serie|Su nombre de usuario de {{site.data.keyword.messagehub}}.|
|password|Serie|Su contraseña de {{site.data.keyword.messagehub}}.|
|topic|Serie|El tema que desea que escuche el desencadenante.|
|kafka_admin_url|Serie de URL|El URL de la interfaz REST de administración de {{site.data.keyword.messagehub}}.|
|isJSONData|Booleano (Opcional - default=false)|Si tiene el valor `true`, el proveedor intenta analizar el valor del mensaje como JSON antes de pasarlo como carga útil del desencadenante.|
|isBinaryKey|Booleano (Opcional - default=false)|Si tiene el valor `true`, el proveedor codifica el valor de la clave como Base64 antes de pasarlo como carga útil del desencadenante.|
|isBinaryValue|Booleano (Opcional - default=false)|Si tiene el valor `true`, el proveedor codifica el valor del mensaje como Base64 antes de pasarlo como carga útil del desencadenante.|

Aunque esta lista de parámetros puede parecer larga, se pueden establecer automáticamente mediante el mandato de plug-in de CLI `ibmcloud fn package refresh`.

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

5. Ahora todo lo que tiene que hacer es crear un desencadenante que se active cuando se publiquen mensajes nuevos en el tema {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1/messageHubFeed -p topic mytopic
  ```
  {: pre}

## Creación de un desencadenante para un paquete de {{site.data.keyword.messagehub}} fuera de {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_trigger_outside}

Si quiere configurar {{site.data.keyword.messagehub}} fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para el servicio {{site.data.keyword.messagehub}}. Necesita la información sobre conexión y credenciales del servicio {{site.data.keyword.messagehub}}.

1. Cree un enlace de paquete configurado para el servicio de {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
  ```
  {: pre}

2. Ahora puede crear un desencadenante utilizando el nuevo paquete que se activará cuando se publiquen mensajes nuevos en el tema {{site.data.keyword.messagehub}}.
  ```
  ibmcloud fn trigger create MyMessageHubTrigger -f myMessageHub/messageHubFeed -p topic mytopic -p isJSONData true
  ```
  {: pre}

## Escucha de mensajes
{: #message_hub_listen}

Después de crear un desencadenante, el sistema supervisa el tema específico en el servicio de mensajería. Cuando se publiquen nuevos mensajes, se activa el desencadenante.

La carga útil del desencadenante contiene un campo `messages`, que es una matriz de los mensajes que se han publicado desde la última vez que se activó el desencadenante. Cada objeto de mensaje de la matriz contiene los siguientes campos:
- topic
- partition
- offset
- key
- value

En términos de Kafka, los campos deberían resultar evidentes. Sin embargo, `key` tiene una función denominada `isBinaryKey` que permite que `key` transmita datos binarios. Además, el campo `value` requiere una especial consideración. Dispone de los campos `isJSONData` e `isBinaryValue` para gestionar los mensajes binarios y JSON. Estos campos, `isJSONData` e `isBinaryValue`, no se pueden utilizar juntos.

Por ejemplo, si `isBinaryKey` se ha establecido en `true` al crear el desencadenante, `key` se codifica como una serie Base64 cuando se devuelva de su carga útil de un desencadenante activado.

Si se publica el valor de `key` `Some key` con `isBinaryKey` establecido en `true`, la carga útil del desencadenante es similar a la siguiente:
```json
{
    "messages": [
       {
            "partition": 0,
            "key": "U29tZSBrZXk=",
            "offset": 421760,
            "topic": "mytopic",
            "value": "Some value"
        }
    ]
}
```
{: codeblock}

Si el parámetro `isJSONData` se ha establecido `false` (o no se ha establecido) al crear el desencadenante, el campo `value` es el valor sin formato del mensaje publicado. Sin embargo, si `isJSONData` se ha establecido en `true` al crear el desencadenante, el sistema intenta analizar este valor como objeto JSON en la medida de lo posible. Si el análisis se realiza correctamente, `value` en la carga útil del desencadenante es el objeto JSON resultante.

Si se publica el mensaje `{"title": "Some string", "amount": 5, "isAwesome": true}` con `isJSONData` establecido en `true`, la carga útil del desencadenante puede ser similar al ejemplo siguiente:
```json
{
  "messages": [
       {
      "partition": 0,
        "key": null,
        "offset": 421760,
        "topic": "mytopic",
        "value": {
          "amount": 5,
            "isAwesome": true,
            "title": "Some string"
        }
    }
  ]
}
```
{: codeblock}

Sin embargo, si se publica el mismo contenido de mensaje con `isJSONData` establecido en `false`, la carga útil del desencadenante sería similar al ejemplo siguiente:
```json
{
  "messages": [
       {
      "partition": 0,
      "key": null,
      "offset": 421761,
      "topic": "mytopic",
      "value": "{\"title\": \"Some string\", \"amount\": 5, \"isAwesome\": true}"
    }
  ]
}
```
{: codeblock}

Al igual que sucede con `isJSONData`, si `isBinaryValue` se ha establecido en `true` durante la creación del desencadenante, el `value` resultante en la carga útil del desencadenante se codifica como una serie Base64.

Si se publica el valor de `value` `Some data` con `isBinaryValue` establecido en `true`, la carga útil del desencadenante puede ser similar al ejemplo siguiente:
```json
{
  "messages": [
       {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "U29tZSBkYXRh"
    }
  ]
}
```
{: codeblock}

Si se publica el mismo mensaje sin `isBinaryData` establecido en `true`, la carga útil del desencadenante se parecerá a la del siguiente ejemplo:
```json
{
  "messages": [
       {
      "partition": 0,
      "key": null,
      "offset": 421760,
      "topic": "mytopic",
      "value": "Some data"
    }
  ]
}
```
{: codeblock}

### Los mensajes se colocan por lotes
Observe que la carga útil del desencadenante contiene una matriz de mensajes. Si estos mensajes se producen rápidamente en su sistema de mensajería, el canal de información intenta colocar por lotes los mensajes publicados en una sola activación del desencadenante. El proceso por lotes permite publicar los mensajes en el desencadenante de forma más rápida y eficiente.

Tenga en cuenta que, si el desencadenante activa acciones de codificación, el número de mensajes de la carga útil no está técnicamente enlazado, aunque siempre es mayor que 0. Consulte el siguiente ejemplo de un mensaje por lotes (observe el campo en el valor *offset*):
```json
{
  "messages": [
       {
        "partition": 0,
         "key": null,
         "offset": 100,
         "topic": "mytopic",
         "value": {
            "amount": 5
         }
      },
      {
        "partition": 0,
         "key": null,
         "offset": 101,
         "topic": "mytopic",
         "value": {
            "amount": 1
         }
      },
      {
        "partition": 0,
         "key": null,
         "offset": 102,
         "topic": "mytopic",
         "value": {
            "amount": 999
         }
      }
  ]
}
```

## Ejemplos
{: #examples}

### Integración de OpenWhisk con {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full_notm}} e IBM Data Science Experience
[Aquí puede encontrar](https://medium.com/openwhisk/transit-flexible-pipeline-for-iot-data-with-bluemix-and-openwhisk-4824cf20f1e0) un ejemplo que integra OpenWhisk con {{site.data.keyword.messagehub}}, Node Red, IBM Watson IoT, {{site.data.keyword.cos_full}} y el servicio de IBM Data Science Experience (Spark).

## Referencias
- [{{site.data.keyword.messagehub}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
