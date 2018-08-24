---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Respuestas ante sucesos con reglas y desencadenantes
{: #openwhisk_triggers}

Los desencadenantes y reglas de {{site.data.keyword.openwhisk}} aportan prestaciones dirigidas por sucesos a la plataforma. Los sucesos de orígenes de sucesos externos e internos se ponen en el canal por medio de un desencadenante, y las reglas permiten sus acciones de respuesta para dichos sucesos.
{: shortdesc}

## Conceptos generales
{: #definitions}

### Desencadenantes
{: #openwhisk_triggers_create}

Los desencadenantes son un canal con nombre para una clase de sucesos.
{: shortdesc}

Un desencadenante es una declaración a la que desea reaccionar en un determinado tipo de suceso, ya sea un usuario o mediante un origen de sucesos. A continuación se muestran ejemplos de desencadenantes.
- Un desencadenante de sucesos de actualización de ubicación
- Un desencadenante de subidas de documento a un sitio web
- Un desencadenante de correos electrónicos de entrada

Los desencadenantes se pueden activar (o desencadenar), utilizando un diccionario de pares de clave/valor. A veces se hace referencia a este diccionario como el suceso. Los desencadenantes se pueden activar de forma explícita mediante un usuario o de parte de un usuario por parte de un origen de sucesos externo. Como con las acciones, cada activación de un desencadenante asociado a una regla da lugar a un ID de activación. Un desencadenante que no está asociado a una regla no tiene efectos visibles cuando de activa.

Un canal de información es una forma cómoda de configurar un origen de sucesos externo para activar sucesos desencadenantes que {{site.data.keyword.openwhisk_short}} pueda consumir. A continuación se muestran ejemplos de canales de información.
- Un canal de información de cambio de datos de {{site.data.keyword.cloudant}} que activa un suceso desencadenante cada vez que se añade o modifica un documento en una base de datos
- Un canal de información Git que desencadena un suceso por cada confirmación para un repositorio Git

### Reglas
{: #openwhisk_rules_use}

Una regla asocia un desencadenante con una acción.
{: shortdesc}

Cada vez que se activa el desencadenante, la regla utiliza el suceso desencadenante como entrada e invoca a la acción asociada. Con el conjunto adecuado de reglas, es posible que un único suceso desencadenante invoque varias acciones, o que
una acción se invoque como respuesta a sucesos de distintos desencadenantes.

Por ejemplo, pensemos en un sistema con las acciones siguientes.
- `classifyImage` - Acción que detecta los objetos de una imagen y los clasifica.
- `thumbnailImage`- Acción que crea una versión de miniatura de una imagen.

Además, suponga que dos orígenes de sucesos activan los desencadenantes siguientes.
- `newTweet` - Desencadenante que se activa cuando se publica un nuevo tweet.
- `imageUpload` - Desencadenante que se activa cuando se sube una imagen a un sitio web.

Puede configurar reglas para que un único suceso desencadenante invoque varias acciones, y hacer que varios desencadenantes invoquen la misma acción.
- Regla `newTweet -> classifyImage`
- Regla `imageUpload -> classifyImage`
- Regla `imageUpload -> thumbnailImage`

Las tres reglas establecen el comportamiento siguiente.
- Se clasifican las imágenes de ambos tweets.
- Se clasifican las imágenes cargadas.
- Se genera una versión en miniatura.

## Creación de desencadenantes para sucesos de canal
{: #openwhisk_triggers_fire}

En los pasos siguientes se muestra cómo crear un desencadenante de ejemplo para enviar actualizaciones de ubicación de usuario y cómo activar manualmente el desencadenante.

1. Cree el desencadenante. Los desencadenantes se deben crear directamente dentro de un espacio de nombres y no se pueden crear dentro de paquetes.
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: created trigger locationUpdate
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
  /someNamespace/locationUpdate                            private
    ```
    {: screen}
    El desencadenante sirve como un canal con nombre en el que se pueden activar los sucesos.

3. Active un suceso desencadenante.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Debido a que no hay ninguna regla asociada con este desencadenante, los parámetros que se pasan no los utilizará ninguna acción. Salida de ejemplo:
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

En la sección siguiente, puede asociar el desencadenante con una acción mediante la creación de una regla.

## Uso de reglas para asociar desencadenantes con acciones
{: #openwhisk_rules_assoc}

Las reglas se utilizan para asociar un desencadenante con una acción. Cada vez que se activa un suceso desencadenante, se invoca a la acción con los parámetros del suceso desencadenante.

Después de crear el [desencadenante `locationUpdate`](#openwhisk_triggers_fire), los siguientes pasos muestran cómo crear una regla de ejemplo que llama a la acción `hello` siempre que se publica una actualización de ubicación.

1. Cree un archivo denominado 'hello.js' con el código de acción siguiente:
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: pre}

2. Cree la acción `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Cree la regla `myRule` para asociar el desencadenante `locationUpdate` con la acción `hello`. Las reglas se deben crear directamente dentro de un espacio de nombres y no se pueden crear dentro de paquetes.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. Active el desencadenante `locationUpdate`. Cada vez que se da un suceso desencadenante, se llama a la acción `hello` con los parámetros del suceso.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Compruebe que la acción `hello` se ha invocado, revisando el registro de activación más reciente.
    ```
    ibmcloud fn activation list --limit 1 hello
    ```
    {: pre}

    Salida de ejemplo:
    ```
    activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. Obtenga más información sobre el ID de activación de la salida del mandato anterior.
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    Verá que la acción `hello` ha recibido la carga del suceso y ha devuelto la serie prevista.

7. Para inhabilitar la regla, puede ejecutar el mandato siguiente.
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

También puede utilizar reglas para asociar desencadenantes con secuencias. Por ejemplo, puede crear una secuencia de acción denominada `recordLocationAndHello` que se active mediante la regla `anotherRule`:
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
