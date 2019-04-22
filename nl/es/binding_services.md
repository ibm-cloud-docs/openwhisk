---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: binding services, serverless, actions, unbinding

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Enlace de servicios con acciones
{: #binding_services}

Puede utilizar el [plugin {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) CLI para enlazar un servicio con una acción o paquete. El mandato `ibmcloud fn service bind` de {{site.data.keyword.openwhisk_short}} pone sus credenciales de servicio de {{site.data.keyword.Bluemix_notm}} a disposición del código de {{site.data.keyword.openwhisk_short}} en el tiempo de ejecución.
{: shortdesc}


No confunda el mandato `ibmcloud fn service bind` con el mandato `cf bind-service`, que está disponible en Cloud Foundry.
{: tip}


## Enlace de un servicio a una acción o paquete
{: #cli_bind}

Puede enlazar cualquier servicio de {{site.data.keyword.Bluemix_notm}} con cualquier acción definida en {{site.data.keyword.openwhisk_short}}. Al enlazar un servicio, se crea un nuevo parámetro en la acción existente que contiene las credenciales de la instancia de servicio.

**Nota**: Sólo puede enlazar un servicio de cada tipo a una acción o paquete. No se admite el enlace de servicios del mismo tipo.

Antes de empezar, [defina las credenciales](/docs/resources?topic=resources-externalapp#externalapp) para el servicio que desea enlazar.

1. Obtenga el nombre de la instancia de servicio que desea enlazar a una acción o paquete.
    ```
    ibmcloud service list
    ```
    {: pre}

    Salida de ejemplo:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Obtenga el nombre de las credenciales definidas para la instancia de servicio que ha obtenido en el paso anterior.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Invocando 'cf service-keys Conversation-qp'...

    Obteniendo claves para la instancia de servicio Conversation-qp como <su ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Enlace el servicio a una acción.
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Visión general de los componentes del mandato <code>ibmcloud fn service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de los componentes del mandato <code>ibmcloud fn service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>El tipo de servicio que está enlazando.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>El nombre de la acción o del paquete al que desea enlazar el servicio.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>Opcional: especifique un nombre de instancia de servicio. Si no especifica un nombre de instancia de servicio, se selecciona la primera instancia correspondiente al servicio.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>Opcional: especifique un nombre de conjunto de credenciales. Si no especifica un nombre de conjunto de credenciales, se selecciona el primer conjunto de credenciales correspondiente a la instancia de servicio.</td>
    </tr>
    </tbody></table>

    Por ejemplo, para enlazar un servicio de conversación de {{site.data.keyword.ibmwatson}} a una acción denominada `hello`:
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
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
                    "conversation": {
                        "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    En este ejemplo, las credenciales correspondiente al servicio de conversación, junto con cualquier otra credencial correspondiente a otros tipos de servicio, pertenecen a un parámetro denominado `__bx_creds`. La acción busca el parámetro de enlace `__bx_creds` y elimina la referencia al tipo de servicio listado. Si ese tipo de servicio es el único que se lista, la acción presenta un valor nulo en el valor de parámetro de `__bx_creds`. Si hay más de un servicio enlazado a la acción, el parámetro `__bx_creds` se mantiene con los servicios que todavía están enlazados.

Para obtener más información sobre cómo pasar parámetros a una acción o a un paquete y sobre cómo se ven afectadas las credenciales durante las operaciones `update`, consulte el apartado sobre [Cómo trabajar con parámetros](/docs/openwhisk?topic=cloud-functions-working-with-parameters#pass-params-action).


Si cambia el nombre de la organización o el espacio que contiene las entidades, se creará un espacio de nombres con el nuevo nombre. Las entidades que contiene el espacio de nombres antiguo no serán visibles en el nuevo espacio de nombres y se planificará su supresión. Si ha realizado el cambio de forma accidental, puede revertirlo y guardar las entidades antes de que se supriman.
{: tip}


## Desenlace de un servicio de una acción o paquete
{: #cli_unbind}

Puede desenlazar un servicio de una acción o paquete. Al desenlazar un servicio, se eliminan los enlaces existentes creados por el mandato `service bind`.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
