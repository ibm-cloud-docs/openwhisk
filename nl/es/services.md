---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: services, serverless

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


# Adición de servicios de IBM Cloud
{: #services}

Puede utilizar la funcionalidad de incorporación de servicios de IBM Cloud en su app.
{: shortdesc}

**¿Cómo puedo añadir servicios de IBM Cloud a mi app?**

1. Puede codificar llamadas a la API REST en su app. Esta opción puede ser la forma más rápida de comunicarse con un servicio de IBM Cloud.
2. Puede utilizar un paquete preinstalado o instalable para incorporar funcionalidad. Puede utilizar las acciones y los canales de información que están almacenados en los paquetes dentro del código de la app. Esta opción podría reducir algo el código, lo que podría ser útil si su app ya roza los límites del sistema.


**¿Cómo se configuran los parámetros a los que debe acceder mi app?**

Estos parámetros pueden incluir valores que permiten reutilizar la app con datos diferentes o pueden incluir valores que necesita el servicio, como por ejemplo credenciales. 
1. Puede codificar parámetros en su app. Esta opción quizás no sea la forma más segura de almacenar información confidencial, como por ejemplo las credenciales.
2. Puede enlazar los parámetros a la app enlazándolos a una acción o a un paquete.


## Enlace de un servicio a una acción o paquete
{: #services_bind}

Enlace cualquier servicio de {{site.data.keyword.Bluemix_notm}} a cualquier acción. Cuando se enlaza un servicio, se crea un nuevo parámetro en la acción existente que contiene las credenciales de la instancia de servicio.

**Nota**: no se pueden enlazar varias instancias del mismo servicio a una acción o paquete. Sólo se puede enlazar una instancia de un servicio. 

Antes de empezar, [cree una acción](/docs/openwhisk?topic=cloud-functions-actions) y [defina las credenciales](/docs/resources?topic=resources-externalapp#externalapp) del servicio que desea enlazar con la acción.

1. Obtenga el nombre del servicio y de la instancia de servicio que desea enlazar a una acción o paquete. En la salida del ejemplo, `composer` es el servicio y `Composer-qp` es el nombre de la instancia de servicio.
    ```
    ibmcloud service list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Obtenga el nombre de las credenciales definidas para una instancia de servicio.

    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    Ejemplo:
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Invoking 'cf service-keys Composer-qp'...

    Obteniendo claves para la instancia de servicio Composer-qp como <su ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Enlace el servicio a una acción. El mandato `ibmcloud fn service bind` de {{site.data.keyword.openwhisk_short}} pone sus credenciales de servicio de {{site.data.keyword.Bluemix_notm}} a disposición del código de {{site.data.keyword.openwhisk_short}} en tiempo de ejecución.
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME] [--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    <table>
    <caption>Visión general de los componentes del mandato <code>ibmcloud fn service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de los componentes del mandato <code>ibmcloud fn service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE</code></td>
    <td>El nombre del servicio que está enlazando.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>El nombre de la acción o del paquete al que desea enlazar el servicio.</td>
    </tr>
    <tr>
    <td>--instance <code>INSTANCE_NAME</code></td>
    <td>Opcional: especifique un nombre de instancia de servicio. Si no especifica un nombre de instancia de servicio, se selecciona la primera instancia correspondiente al servicio.</td>
    </tr>
    <tr>
    <td>--keyname <code>CREDENTIALS_NAME</code></td>
    <td>Opcional: especifique un nombre de las credenciales. Si no especifica el nombre de las credenciales, se seleccionan las primeras credenciales de la instancia de servicio.</td>
    </tr>
    </tbody></table>

    Por ejemplo, para enlazar un servicio de Composer de {{site.data.keyword.ibmwatson}} a una acción denominada `hello`:
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    Salida:
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verifique que las credenciales se han enlazado correctamente. La acción a la que el servicio está enlazada no admite distintivos personalizados, pero sí admite los distintivos de depuración y de salida detallada.
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got action Hello World
{
        "parameters": [
        {
                "key": "var1",
            "value": "val1"
            },
            {
                "key": "dog",
            "value": "cat"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    En este ejemplo, las credenciales correspondiente al servicio Composer, junto con cualquier otra credencial correspondiente a otros tipos de servicio, pertenecen a un parámetro denominado `__bx_creds`. La acción busca el parámetro de enlace `__bx_creds` y elimina la referencia al tipo de servicio listado. Si ese tipo de servicio es el único que se lista, la acción presenta un valor nulo en el valor de parámetro de `__bx_creds`. Si hay más de un servicio enlazado a la acción, el parámetro `__bx_creds` se mantiene con los servicios que todavía están enlazados.

Para obtener más información sobre cómo pasar parámetros a una acción o a un paquete, consulte [Enlace de parámetros a acciones](/docs/openwhisk?topic=cloud-functions-actions#actions_params).




## Desenlazar servicios de acciones
{: #services_unbind}

Al desenlazar un servicio de una acción o paquete, se eliminan los enlaces de servicio existentes.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
