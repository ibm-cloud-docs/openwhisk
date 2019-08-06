---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, functions, serverless, javascript, node, node.js

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



# Creación de acciones
{: #actions}

Cree una acción, que es una función de nivel superior que devuelve un objeto JSON. Puede combinar acciones en un paquete para simplificar la gestión de las acciones.
{: shortdesc}

Antes de empezar:
Para crear una acción, el código fuente debe cumplir determinados requisitos. Por ejemplo, si desea crear una acción a partir de un código que esté contenido en varios archivos, empaquete el código como un solo archivo antes de crear la acción. Consulte [Preparación del código de la app para funcionar sin servidor](/docs/openwhisk?topic=cloud-functions-prep) para obtener detalles sobre los requisitos de cada tiempo de ejecución.


## Creación de acciones desde la CLI
{: #actions_cli}

1. Cree una acción.
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  **Ejemplo**
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  ok: created action hello
  ```
  {: screen}

  Sugerencias:
  - Para ahorrar en costes, puede establecer límites.
      - Para establecer un límite para el uso de memoria, incluya `--memory VALUE` en el mandato create, con el valor en megabytes.
      - Para establecer un tiempo de espera, incluya `--timeout VALUE` en el mandato create, con el valor en milisegundos.
  - Si ha empaquetado el código como una imagen de Docker, incluya `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` en el mandato create en lugar de la vía de acceso local a la app y el distintivo --kind. Para una mejor gestión de las imágenes, no utilice la etiqueta `latest` siempre que sea posible. Cuando se utiliza la etiqueta `latest`, se utiliza la imagen que tiene esa etiqueta, que quizás no será siempre la imagen creada más recientemente.

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  
2. Verifique que la acción está en la lista de acciones.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  actions
  hello       private
  ```
  {: screen}


## Actualización de apps o tiempos de ejecución en acciones
{: #actions_update}

Puede ejecutar el mandato de actualización siempre que tenga que actualizar el código en la app o para migrar a una versión más reciente de un tiempo de ejecución. Por ejemplo, debido a que Node.js versión 8 está en la modalidad de mantenimiento, es posible que desee cambiar el entorno de ejecución a Node.js 10.

Al migrar a una nueva versión de tiempo de ejecución, es posible que tenga que cambiar el código de la app para que se ajuste a la nueva versión de tiempo de ejecución. En la mayoría de los casos, las versiones de entorno de ejecución son compatibles.
{: tip}

1. Actualice la app localmente.

2. Si ha empaquetado la app como imagen de Docker, cargue la imagen más reciente en Docker Hub. Esto permite al sistema extraer la nueva imagen de Docker la próxima vez que ejecute el código para la acción. Si hay un contenedor en ejecución que utiliza una versión anterior de la imagen de Docker, las nuevas invocaciones siguen utilizando esta imagen. Debe ejecutar el mandato de forma que las nuevas invocaciones empiecen a ejecutarse en la nueva imagen.

3. Actualice una acción e incluya la vía de acceso local a la app o a la imagen de Docker.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    **Ejemplo**

    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: updated action hello
    ```
    {: screen}

    Si ha empaquetado el código como una imagen de Docker, incluya `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` en el mandato create en lugar de la vía de acceso a la app local y el distintivo `--kind`. Para una mejor gestión de las imágenes, no utilice la etiqueta `latest` siempre que sea posible. Cuando se utiliza la etiqueta `latest`, se utiliza la imagen que tiene esa etiqueta, que quizás no será siempre la imagen creada más recientemente. 

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## Enlace de parámetros a acciones
{: #actions_params}

Puede enlazar parámetros a acciones para establecer parámetros predeterminados. Los parámetros enlazados sirven como parámetros predeterminados para las acciones a menos que se suministren parámetros en el momento de la invocación.
{: shortdesc}

Antes de empezar, [cree la acción](#actions_cli).

Para enlazar los parámetros:

1. Actualice una acción y enlace los parámetros predeterminados a la acción.

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **Ejemplo**

    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: updated action MyApp
    ```
    {: screen}

    Si modifica los parámetros de credenciales que no son de servicio, al ejecutar el mandato `action update` con nuevos parámetros, se eliminarán los parámetros actuales que no se especifiquen en el mandato `action update`. Por ejemplo, si ejecuta `action update -p key1 new-value -p key2 new-value` pero omite otros parámetros definidos, dichos parámetros dejarán de existir después de que se actualice la acción. Los servicios que hubiera enlazados a la acción también se eliminan. Si ha enlazado un servicio, debe volver a [enlazar los servicios a la acción](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Verifique que los parámetros están enlazados a la acción.

    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

Opcional: para borrar los parámetros que se habían enlazado previamente, actualice la acción sin incluir ningún parámetro.

```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## Encadenar acciones como secuencias de acciones
{: #actions_seq}

Puede crear una acción que encadene una secuencia de acciones. El resultado de una acción se pase como argumento a la acción siguiente.
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

Los parámetros que se pasan entre acciones de la secuencia son explícitos, salvo los predeterminados. Por tanto, los parámetros que se pasan a la secuencia de acción solo están disponibles para la primera acción de la secuencia. El resultado de la primera acción de la secuencia se convierte en el objeto JSON de entrada de la segunda acción de la secuencia, y así sucesivamente. Este objeto no incluye ninguno de los parámetros que originalmente se han pasado a la secuencia, a menos que la primera acción los incluya en el resultado. Los parámetros de entrada a una acción se fusionan con los parámetros predeterminados de la acción, dando prioridad a los primeros y sustituyendo los parámetros predeterminados coincidentes.

Una secuencia no tiene un tiempo de espera excedido global distinto de los tiempos de espera de cada acción dentro de la secuencia. Debido a que una secuencia se trata como un conducto de operaciones, un error en una acción interrumpe el conducto. Si una acción excede el tiempo de espera, toda la secuencia finaliza con ese error.

A continuación, al crear una regla o invocar a las acciones, utilice el nombre de la secuencia.


## Empaquetado de acciones
{: #actions_pkgs}

En {{site.data.keyword.openwhisk}}, puede utilizar paquetes para empaquetar un conjunto de acciones y canales de información relacionados y compartirlos con otros. Los paquetes también permiten compartir los parámetros entre todas las entidades del paquete.
{: shortdesc}

Un paquete puede incluir *actions* y *feeds* (acciones y canales de información).
- Una acción es un segmento de código que se ejecuta en {{site.data.keyword.openwhisk_short}}. Por ejemplo, el paquete {{site.data.keyword.cloudant}} incluye acciones para leer y escribir registros en una base de datos {{site.data.keyword.cloudant_short_notm}}.
- Un canal de información sirve para configurar un origen de suceso externo para activar sucesos desencadenantes. Por ejemplo, el paquete Alarma incluye un canal de información que puede activar un desencadenante en una frecuencia especificada.


1. Cree un paquete.

  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. Obtenga un resumen del paquete. Fíjese que el paquete está vacío.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  package /myNamespace/custom
  ```
  {: screen}

4. Cree una acción e inclúyala en el paquete. La creación de una acción en un paquete precisa que se añada un nombre de paquete como prefijo del nombre de la acción. No se permite
anidamiento de paquetes. Un paquete solo puede contener acciones y no puede contener otros paquetes.

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. Obtenga un resumen del paquete.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## Enlace de parámetros a paquetes
{: #actions_pkgs_params}

Puede establecer parámetros predeterminados para todas las entidades de un paquete estableciendo los parámetros a nivel de paquete, que todas las acciones del paquete heredan.

Los parámetros de enlace sirven como parámetros predeterminados para las acciones del paquete a menos que:

- La propia acción tenga un parámetro predeterminado
- La acción tenga un parámetro proporcionado en tiempo de invocación

Antes de empezar, cree un paquete que incluya por lo menos una acción.

1. Actualice un paquete y enlace el parámetro predeterminado al mismo.

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **Ejemplo**

    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: updated package MyApp
    ```
    {: screen}

    Si modifica los parámetros de credenciales que no son de servicio, al ejecutar el mandato `package update` con nuevos parámetros, se eliminarán los parámetros actuales que no se especifiquen en el mandato `package update`. Por ejemplo, si ejecuta `package update -p key1 new-value -p key2 new-value` pero omite otros parámetros definidos, dichos parámetros dejarán de existir después de que se actualice el paquete. Los servicios enlazados al paquete también se eliminan, por lo que después de actualizar otros parámetros deberá volver a [enlazar servicios al paquete](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Verifique que los parámetros están enlazados al paquete.

    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. Verifique que los parámetros han sido heredados por el paquete.

    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    **Resultado de ejemplo**

    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## Compartir paquetes de acciones
{: #actions_pkgs_share}

Tras depurar y probar las acciones y los canales de información que comprende un paquete, éste se puede compartir con todos los usuarios de {{site.data.keyword.openwhisk_short}}. Compartir el paquete posibilita que los usuarios enlacen al paquete, invoquen acciones
en el mismo y creen reglas de {{site.data.keyword.openwhisk_short}} y acciones de secuencia. Las acciones
e información de entrada dentro de un paquete compartido son _públicas_. Si el paquete es privado, todo su contenido es también privado.
{: shortdesc}

1. Compartir el paquete con todos los usuarios.

  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. Mostrar la propiedad `publish` del paquete para verificar que ahora es true.

  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. Obtenga una descripción del paquete para proporcionar a otras personas el nombre completo del paquete de forma que puedan enlazarlo o invocar acciones en el mismo. El nombre completo incluye el espacio de nombres, que es este ejemplo es el espacio de nombres `myNamespace`.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Resultado de ejemplo**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## Variables de entorno para acciones
{: #actions_envvars}

El entorno de acción contiene varias variables de entorno que son específicas de la acción que se está ejecutando. Las propiedades están accesibles en el entorno del sistema para todos los tiempos de ejecución admitidos. Estas propiedades permiten que las acciones funcionen mediante programación con activos a través de la API REST o permiten establecer una alarma interna cuando la acción está a punto de alcanzar su presupuesto de tiempo permitido.
{: shortdesc}

| Propiedad | Descripción |
| -------- | ----------- |
| `__OW_API_HOST` | El host de API para el despliegue que ejecuta esta acción. |
| `__OW_API_KEY` | La clave de API del sujeto que está invocando la acción. Esta variable sólo se proporciona para espacios de nombres clásicos basados en CF. |
| `__OW_NAMESPACE` | El ID del espacio de nombres (GUID). Para los espacios de nombres clásicos basados en CF, este ID se construye a partir de los nombres de org y de espacio. |
| `__OW_NAMESPACE_CRN` | El nombre de recurso de nube de espacio de nombres [CRN](/docs/overview?topic=overview-crn). El CRN solo está disponible para espacios de nombres habilitados para IAM
| `__OW_ACTION_NAME` | El nombre completo calificado de la acción en ejecución. |
| `__OW_IAM_NAMESPACE_API_KEY` | La clave de API para espacios de nombres habilitados para IAM. Consulte [Establecimiento de políticas de acceso](/docs/openwhisk?topic=cloud-functions-namespaces#namespace-access) para su uso. |
| `__OW_IAM_API_URL` | El punto final de servicio utilizado para operaciones IAM, como por ejemplo obtener una señal de la clave de API. Esta variable solo está disponible para espacios de nombres habilitados para IAM |
| `__OW_ACTIVATION_ID` | El ID de activación para esta instancia de acción en ejecución. |
| `__OW_DEADLINE` | El tiempo aproximado, en milisegundos de epoch, en el que esta acción consume toda su cuota de duración. |

### Incorporación de variables de entorno de acción en la app
{: #actions_envvars_app}

Para ver los valores de una acción, incluya la visualización de los mismos en el código de la app y póngalos como salida en los resultados.

**Ejemplo para Python**
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

Después de actualizar y activar el código en una acción, el resultado incluye el nombre completo de la acción.
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
{: screen}



