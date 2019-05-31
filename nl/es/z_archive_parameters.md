---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cómo trabajar con parámetros

En acciones sin servidor, los datos se proporcionan añadiendo parámetros a las acciones. Los parámetros se declaran como argumentos en la función principal (main) sin servidor.
{: shortdesc}

Puede proporcionar valores para los parámetros de dos maneras:
* **Pasar parámetros a una acción durante la invocación**: proporcione parámetros cuando se invoque la acción a través de distintivos de la CLI o de un archivo. Los parámetros proporcionados en la invocación sustituyen a cualquier parámetro predeterminado que se hubiera establecido con anterioridad.
* **Enlazar parámetros a una acción o paquete**: establezca parámetros predeterminados cuando se cree o actualice una acción o paquete. Esta opción resulta útil para los datos que se mantienen igual en cada ejecución, equivalentes a las variables de entorno en otras plataformas, o para los valores predeterminados que se pueden modificar en el momento de la invocación.

## Paso de parámetros a una acción durante la invocación
{: #pass-params-action}

Los parámetros se pueden pasar a una acción cuando se invoca.

1. Guarde el código siguiente en un archivo denominado `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Cree la acción `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Si ha utilizado anteriormente esta acción, actualícela para asegurarse de que se borren de la acción los parámetros establecidos anteriormente.
    ```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. Invoque la acción pasando los parámetros `name` y `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **Nota**: en su lugar, puede pasar un archivo de parámetros con formato JSON:
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

5. También puede pasar parámetros en un objeto estructurado a la acción. Por ejemplo, actualice la acción `hello` con lo siguiente:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    Ahora, la acción espera un único parámetro `person` con los campos `name` y `place`.

6. Invoque la acción con un único parámetro `person` que es un objeto JSON válido.
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

## Enlace de parámetros a una acción
{: #default-params-action}

Las acciones se pueden invocar con varios parámetros con nombre. Por ejemplo, la acción `hello` básica espera dos parámetros: el nombre (`name`) de una persona y el lugar (`place`) de origen.

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

En lugar de pasar todos los parámetros a una acción cada vez, puede enlazar parámetros predeterminados a la acción. Los pasos siguientes muestran cómo enlazar el parámetro `place` a la acción `hello` básica para que la acción tenga como valor predeterminado el lugar "Kansas".

1. Guarde el código siguiente en un archivo denominado `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Cree la acción `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Actualice la acción para enlazar valores de parámetro utilizando el distintivo `--param` y un par de clave/valor.

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **Nota**: en su lugar, puede pasar un archivo de parámetros con formato JSON:
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    Si modifica los parámetros de credenciales que no son de servicio, al ejecutar el mandato `action update` con nuevos parámetros, se eliminarán los parámetros actuales que no se especifiquen en el mandato `action update`. Por ejemplo, si ejecuta `action update -p key1 new-value -p key2 new-value` pero omite otros parámetros definidos, dichos parámetros dejarán de existir después de que se actualice la acción. Los servicios enlazados a la acción también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios a la acción](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

4. Invocar la acción pasando solo el parámetro `name`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

    Debido a que no ha especificado el parámetro `place` al invocar la acción, se utilizará el valor del parámetro predeterminado enlazado, `Kansas`.

5. Los parámetros enlazados se pueden sobrescribir especificando el valor de parámetro en el momento de la invocación. Invoque la acción pasando tanto `name` como `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "payload": "Hello, Dorothy from Washington, DC"
    }
    ```
    {: screen}

## Enlace de parámetros a un paquete
{: #default-params-package}

También se pueden establecer parámetros predeterminados a nivel de paquete. Los parámetros de enlace sirven como parámetros predeterminados para las acciones del paquete a menos que:

- La propia acción tenga un parámetro predeterminado
- La acción tenga un parámetro proporcionado en tiempo de invocación

En el ejemplo siguiente se define un parámetro predeterminado de `name` en el paquete `MyApp` y se muestra una acción que lo utiliza.

1. Cree un paquete, enlazando el parámetro predeterminado `name` al mismo.
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. Guarde el código siguiente en un archivo denominado `helloworld.js`.

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. Cree la acción en el paquete `MyApp`.
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    Si modifica los parámetros de credenciales que no son de servicio, al ejecutar el mandato `action update` con nuevos parámetros, se eliminarán los parámetros actuales que no se especifiquen en el mandato `action update`. Por ejemplo, si ejecuta `action update -p key1 new-value -p key2 new-value` pero omite otros parámetros definidos, dichos parámetros dejarán de existir después de que se actualice la acción. Los servicios enlazados a la acción también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios a la acción](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

3. Invoque la acción sin pasar ningún parámetro.
    ```
    ibmcloud fn action invoke --result MyApp/hello
    ```
    {: pre}

    Salida de ejemplo:
    ```
       {
           "payload": "Hello, World"
     }
    ```
    {: screen}

    Se utilizará el parámetro enlazado al paquete.

