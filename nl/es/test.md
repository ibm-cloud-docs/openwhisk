---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Pruebas de apps sin servidor
{: #test}

Pruebe cada entidad que cree desde la CLI para verificar que la app sin servidor está funcionando o para identificar dónde se puede producir un problema.
{: shortdesc}


## Pruebas de acciones
{: #test-js}

Puede probar las acciones ejecutando el mandato `invoke`. Puede probar la acción con o sin parámetros.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

**Ejemplo Hello world**
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

**Resultado**
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### Prueba de parámetros almacenados en archivos JSON
{: #test_json_file}

Puede pasar un archivo de parámetros con formato JSON.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

**Resultado de ejemplo**
```
{
    "payload": "Hello, Dorothy from Kansas"
}
```
{: screen}


### Prueba de parámetros especificados en formato JSON
{: #test_json}

Puede pasar parámetros en formato JSON en la invocación.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

**Resultado de ejemplo**
```
{
    "payload": "Hello, Dorothy from Kansas"
}
```
{: screen}


### Prueba de las acciones de bloqueo
{: #test-block}

La invocación de la acción puede ser de bloqueo o no bloqueo. De forma predeterminada, las invocaciones no son de bloqueo. Si no necesita el resultado de la acción inmediatamente, utilice una invocación de no bloqueo.
{: shortdesc}

Las invocaciones de bloqueo utilizan un estilo de solicitud/respuesta y esperan a que el resultado de la activación esté disponible. El período de espera es el que sea menor entre 60 segundos o el [valor de límite de tiempo](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) de la acción.

Ejecute la acción en la nube ejecutando una invocación de bloqueo.

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


**Resultado de ejemplo**
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result":{
        "payload": "Hello world"
    },
      "status": "success",
      "success": true
  }
```
{: screen}

El mandato genera la siguiente información.
* El resultado de la invocación, si está disponible en el período de espera previsto
* Sin la opción `--result`, se muestra el ID de activación en el resultado. El ID de activación (`44794bd6aab74415b4e42a308d880e5b`) que se puede utilizar para recuperar los registros o el resultado de la invocación.


## Prueba de desencadenantes
{: #test_triggers}

Los desencadenantes se pueden activar (o desencadenar), utilizando un diccionario de pares de clave/valor. A veces se hace referencia a este diccionario como el suceso. Los desencadenantes se pueden activar de forma explícita mediante un usuario o de parte de un usuario por parte de un origen de sucesos externo. Como con las acciones, cada activación de un desencadenante asociado a una regla da lugar a un ID de activación.
{: shortdesc}

1. Active el desencadenante.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    Un desencadenante que no está asociado a una regla no tiene efectos visibles cuando de activa. Debido a que no hay ninguna regla asociada con este desencadenante, los parámetros que se pasan no los utilizará ninguna acción.

    **Resultado de ejemplo**

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. Compruebe que la acción se ha invocado, revisando el registro de activación más reciente.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. Obtenga más información sobre el ID de activación de la salida del mandato anterior.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    **Resultado de ejemplo**
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## Prueba de la duración de las activaciones
{: #test_time}

Compruebe cuánto tiempo ha tardado una activación en completarse obteniendo el registro de activación. Si la duración es demasiado larga o si necesita ajustar el tiempo de espera predeterminado para permitir que la función se ejecute más tiempo, puede actualizar la acción con un tiempo de espera.
{: shortdesc}

1. Obtenga el ID de activación.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Salida de ejemplo:
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. Obtenga el registro para el ID de activación.

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    El campo `duration` muestra el tiempo en milisegundos. La activación ha tardado algo más de 2 segundos en completarse.

    ```
    ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
        ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
        ...
    }
    ```
    {: screen}

3. Actualice la acción con un tiempo de espera en milisegundos.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    Ejemplo:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## Probar el uso de memoria
{: #test_memory}

Si la app está empaquetada en una imagen de Docker, puede utilizar los mandatos de Docker para comprobar el uso de memoria de la app.
{: shortdesc}

1. Cree un contenedor localmente que ejecute la imagen de Docker.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. Obtenga una lista de los contenedores para obtener un ID de contenedor.

    ```
    docker ps
    ```
    {: pre}

3. Compruebe las estadísticas del contenedor en ejecución.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. Revise el valor de uso de memoria del contenedor. Si el valor no encaja dentro de los límites del sistema, ajuste el script.

5. Una vez que haya terminado de revisar la información, puede detener el contenedor en ejecución.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. Elimine el contenedor.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}








