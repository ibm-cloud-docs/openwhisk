---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: triggers, serverless

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


# Creación de desencadenantes para sucesos
{: #triggers}

Un desencadenante es una declaración a la que desea reaccionar en un determinado tipo de suceso, ya sea un usuario o mediante un origen de sucesos.
{: shortdesc}

A continuación se muestran ejemplos de desencadenantes.
- Sucesos de actualización de ubicación
- Documentar las cargas en un sitio web
- Correos electrónicos entrantes



## Creación de desencadenantes desde la CLI
{: #triggers_create}


1. Cree el desencadenante. Los desencadenantes se deben crear directamente dentro de un espacio de nombres y no se pueden crear dentro de paquetes.
    ```
    ibmcloud fn trigger create TRIGGER_NAME
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: created trigger TRIGGER_NAME
    ```
    {: screen}

2. Verifique que se haya creado el desencadenante.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    triggers
    /NAMESPACE/TRIGGER_NAME                            private
    ```
    {: screen}



A continuación, puede [probar el desencadenante](/docs/openwhisk?topic=cloud-functions-test#test_triggers) o [crear una regla](/docs/openwhisk?topic=cloud-functions-rules) para asociar el desencadenante a una acción.



## Diferencia entre el canal de información y el desencadenante
{: #triggers_difference}

Los canales de información y los desencadenantes están muy relacionados, pero técnicamente son conceptos distintos.

- {{site.data.keyword.openwhisk_short}} procesa **sucesos** que fluyen en el sistema.

- Un **desencadenante** es el nombre de una clase de suceso. Cada suceso pertenece exactamente a un desencadenante; por analogía, un desencadenante parece un tema de un sistema pub/sub basado en temas. Una **regla** significa que cuando llega un suceso desde el desencadenante, se invoca la acción con la carga útil del desencadenante.

- Un **canal de información** es una forma cómoda de configurar un origen de sucesos externo para activar sucesos desencadenantes que {{site.data.keyword.openwhisk_short}} pueda consumir. Un canal de información es una secuencia de sucesos que pertenecen todos a algún desencadenante. Los paquetes preinstalados, los paquetes instalables y los propios paquetes personalizados pueden contener canales de información.  Un canal de información lo controla una **acción de canal de información**, que maneja la creación, supresión, pausa y reanudación de la corriente de sucesos que comprenden un canal de información. La acción de canal de información suele interactuar con los servicios externos que producen los sucesos, utilizando la API REST que gestiona las notificaciones.

Ejemplos de canales de información:
- Un canal de información de cambio de datos de {{site.data.keyword.cloudant}} que activa un suceso desencadenante cada vez que se añade o modifica un documento en una base de datos
- Un canal de información Git que desencadena un suceso por cada confirmación para un repositorio Git



## Creación de un desencadenante para un canal de información
{: #triggers_feeds}

En este ejemplo se muestra cómo utilizar un canal de información del paquete Alarms para activar un desencadenante una vez por minuto y cómo usar una regla para invocar una acción una vez por minuto.

1. Obtenga una descripción de la lista de entidades del paquete `/whisk.system/alarms`.

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
2. Obtenga una descripción del canal de información en el paquete `/whisk.system/alarms` para ver los parámetros que puede utilizar.

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

3. Cree una app. Ejemplo `hello.js`:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Crear una acción.
  ```
  ibmcloud fn action create hello hello.js
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

  Puede ver que se producen activaciones cada minuto para el desencadenante, la regla y la acción. La acción recibe los parámetros `{"name":"Mork", "place":"Ork"}` en cada invocación.


