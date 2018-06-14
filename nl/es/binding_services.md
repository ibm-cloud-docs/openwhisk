---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Uso de servicios de acciones
{: #binding_services}

Puede utilizar el [plugin de CLI de {{site.data.keyword.openwhisk}}](./bluemix_cli.html) para enlazar un servicio a una acción. {{site.data.keyword.openwhisk_short}} proporciona el mandato `service bind` para poner las credenciales de servicio de {{site.data.keyword.Bluemix}} a disposición del código de Cloud Functions en el tiempo de ejecución. El mandato `service bind` no se debe confundir con el mandato `cf bind-service`, que está disponible en Cloud Foundry. Se trata simplemente de una forma automatizada para crear un nuevo parámetro en la acción existente que contiene las credenciales de servicio. El mandato {{site.data.keyword.openwhisk_short}} `service bind` es más flexible y permite enlazar cualquier servicio de {{site.data.keyword.Bluemix_notm}} con cualquier acción definida en {{site.data.keyword.openwhisk_short}}. La única salvedad es que debe tener credenciales definidas para el servicio que desea enlazar.
{: shortdesc}

## Cómo enlazar un servicio a una acción
{: #cli_bind}

Para enlazar un servicio a una acción, utilice el mandato `ic wsk service bind` que proporciona el [plugin de CLI de {{site.data.keyword.openwhisk_short}}](./bluemix_cli.html). Encontrará información adicional en la sección [Limitaciones](./binding_services.html#limitations).

Sintaxis de uso con `bind`:
```
ic wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

El mandato `service bind` requiere un tipo de servicio y un nombre de acción con el que enlazar. Por ejemplo, si desea enlazar un servicio de conversación de Watson con una acción denominada `hello`, entonces la invocación será similar al siguiente mandato:
```
ic wsk service bind conversation hello
```
{: pre}

Que genera la salida siguiente:
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

Este mandato busca servicios existentes de conversación de Watson en el espacio actual, utiliza el primer servicio de conversación que encuentra y luego recupera todas las credenciales que pertenecen a este servicio. Utilizando el primer grupo de credenciales que pertenecen a este servicio, se enlazan dichas credenciales como parámetro a la acción `hello` especificada. La salida muestra exactamente a qué servicio está enlazada la acción, y qué conjunto de credenciales de dicho servicio se ha utilizado para el enlace.

Para verificar que las credenciales se hayan enlazado correctamente, emita el mandato siguiente:
```
ic wsk action get hello parameters
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

Desde aquí, puede ver que las credenciales para este servicio de conversación (además de cualquier otras credenciales para otros tipos de servicio) pertenecen a un parámetro denominado `__bx_creds`, que ahora puede utilizarse desde el código de la acción como cualquier otro parámetro enlazado. La acción escoge el primer servicio de conversación disponible que incluye el primer conjunto de credenciales definidas en ese servicio. 

Para obtener más información sobre pasar parámetros a una acción, y cómo se ven afectadas las credenciales cuando se realiza una operación `action update`, consulte el siguiente documento [Creación e invocación de acciones](openwhisk_actions.html#openwhisk_pass_params).

El mandato `wsk service` admite los dos distintivos siguientes:

<dl>
    <dt>--instance</dt>
    <dd>El nombre del servicio específico del tipo que desea utilizar.</dd>
    <dt>--keyname</dt>
    <dd>El nombre de las credenciales específicas dentro del servicio que desea utilizar.</dd>
</dl>

Para saber cómo utilizar estos distintivos, consulte el ejemplo siguiente. Mediante el mandato anterior `ic wsk service bind`, supongamos que en realidad eran dos servicios de conversación y que el valor predeterminado de la acción acabó enlazando las credenciales/el servicio incorrectos. Podría volver a ejecutar el mandato con los distintivos `--instance` y `--keyname` para asegurarse de enlazar el servicio correcto con la acción correcta. En primer lugar, compruebe qué servicios están disponibles y qué credenciales están enlazadas con ellos. Si quisiéramos listas nuestros servicios, deberíamos ver una salida como la siguiente:

```
ic service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

En esta salida vemos que **Conversation-qp** es el primero de dos servicios listados, y es al que acabó enlazando el mandato `ic wsk service bind conversation hello` inicial. Quizá desee enlazar al servicio **Conversation-cu** en su lugar. Para estar absolutamente seguro, puede comprobar qué credenciales contiene **Conversation-uc**, para asegurarse de que realiza el enlace utilizando el conjunto adecuado de credenciales.

```
ic service keys Conversation-uc
Invocando 'cf service-keys Conversation-uc'...

Obteniendo claves para la instancia de servicio Conversation-uc como [su ID]...

name
Credentials-1
Credentials-2
```

Desea enlazar a "Credentials-2" desde este servicio. Para asegurarse de que la acción aplica el comportamiento deseado, ejecute el mandato siguiente:
```
ic wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

Que genera la salida siguiente:
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

En la salida, verá que el conjunto correcto de credenciales está enlazado a la acción. De nuevo, para verificarlo, puede observar el siguiente mandato `ic wsk action get`.
```
ic wsk action get hello parameters
```
{: pre}

Que produce los siguientes resultados:
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

Se admiten los indicadores de depuración normales están soportados, y se imprimen cabeceras de respuesta de llamadas.

## Cómo desenlazar un servicio de una acción
{: #cli_unbind}

Para desenlazar un servicio de una acción, utilice el mandato `ic wsk service unbind`. El mandato `service unbind` elimina los enlaces existentes creados por el mandato `service bind`.

Sintaxis de uso con `unbind`:
```
ic wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## Limitaciones
{: #limitations}

La acción `service` no admite distintivos personalizados, pero sí admite la depuración habitual y los distintivos detallados. La acción busca el parámetro de enlace `__bx_creds` y elimina la referencia al tipo de servicio listado. Si ese tipo de servicio es el único que se lista, la acción presenta un valor nulo en el valor de parámetro de `__bx_creds`. Si hay más de un servicio enlazado a la acción, el parámetro `__bx_creds` se mantiene con los servicios que todavía están enlazados.

Sólo puede enlazar un servicio de cada tipo a una acción. Enlazar varios servicios del mismo tipo dentro de una sola acción no está admitido.
{: tip}

