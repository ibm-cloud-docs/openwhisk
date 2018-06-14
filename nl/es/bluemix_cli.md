---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk_short}} ofrece un potente plugin para la CLI de {{site.data.keyword.Bluemix_notm}} que permite la gestión completa del sistema {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configuración de la CLI de {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Descargue e instale la [CLI de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

## Configuración del plug-in de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Para instalar el plugin de {{site.data.keyword.openwhisk_short}}, ejecute el mandato siguiente:
```
ic plugin install cloud-functions
```
{: pre}


Para validar si la instalación del plugin de {{site.data.keyword.openwhisk_short}} es correcta, ejecute el mandato siguiente:
```
ic plugin list cloud-functions
```
{: pre}


La salida muestra la información de la versión instalada de {{site.data.keyword.openwhisk_short}}:
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

Puede actualizar el plugin de {{site.data.keyword.openwhisk_short}} ejecutando el mandato siguiente:
```
ic plugin update Cloud-Functions
```
{: pre}


Para obtener más información sobre el mandato del plugin, utilice `ic plugin --help` o consulte la documentación siguiente:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## Autenticación de CLI de {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} está disponible en dos regiones de {{site.data.keyword.Bluemix_notm}}.

Al iniciar sesión en la CLI de {{site.data.keyword.Bluemix_notm}}, puede especificar el punto final de la API de {{site.data.keyword.Bluemix_notm}} para la región de destino, además de la organización y el espacio.

Para iniciar sesión en la región EE.UU. sur:
```
ic login -a api.ng.bluemix.net
```
{: pre}


Para iniciar sesión en la región de Reino Unido:
```
ic login -a api.eu-gb.bluemix.net
```
{: pre}


El distintivo `-a` especifica el punto final de la API de {{site.data.keyword.Bluemix_notm}} que se debe utilizar. Si se especifica el punto final de la API, la opción `-a` no es necesaria. Puede utilizar el mandato `ic api` para definir explícitamente el punto final de la API de {{site.data.keyword.Bluemix_notm}}. Para visualizar el valor actual del punto final de la API, utilice el mandato `ic target`."

El mandato `login` le solicita información, como la organización, el espacio y la contraseña, si no se especifica. Puede especificar la organización y el espacio en la línea de mandatos para omitir sus solicitudes.
```
ic login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


También puede utilizar una clave de API de {{site.data.keyword.Bluemix_notm}} para iniciar una sesión. Este método es útil cuando la cuenta está configurada con un inicio de sesión federado, que requiere que inicie sesión con el distintivo `--sso`. El uso de una clave API también le beneficia si desea definir una integración continua (CI) y desea configurar un conducto desatendido.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

Para crear una nueva clave de API mediante la CLI de {{site.data.keyword.Bluemix_notm}}, ejecute el mandato siguiente:
```
ic iam api-key-create MyKey
```
{: pre}


A continuación, utilice el valor generado de clave de API para iniciar sesión, como en el ejemplo siguiente:
```
ic login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


Para obtener más información sobre el mandato del plugin, utilice `ic login --help` o revise la documentación siguiente:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Utilización del plug-in CLI de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_usage}

Compruebe su configuración. Realice una invocación de bloqueo (síncrona) de echo, pasándole `hello` como un argumento, como en el siguiente ejemplo:
```
ic wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


Consulte la salida de ejemplo siguiente:
```
{
    "message":"hello"
}
```

Una vez que configure el entorno, puede utilizar la CLI de {{site.data.keyword.openwhisk_short}} para realizar las tareas siguientes:

* Ejecutar sus fragmentos de código, o acciones, en {{site.data.keyword.openwhisk_short}}. Consulte [Creación e invocación de acciones](./openwhisk_actions.html).
* Usar desencadenantes y reglas para permitir a sus acciones responder ante sucesos. Consulte
[Creación de desencadenantes y reglas](./openwhisk_triggers_rules.html).
* Aprenda a los paquetes agrupan acciones y configure orígenes de sucesos externos. Consulte [Creación y uso de paquetes](./openwhisk_packages.html).
* Explore el catálogo de paquetes y mejore sus aplicaciones con servicios externos como un
[Origen de sucesos de Cloudant](./openwhisk_cloudant.html). Consulte [Uso de servicios habilitados para {{site.data.keyword.openwhisk_short}}](./openwhisk_catalog.html).

Para obtener una lista de mandatos para el plugin de {{site.data.keyword.openwhisk_short}}, ejecute `ic wsk` sin argumentos.

## Uso de servicios de acciones
{: #binding_services}

{{site.data.keyword.openwhisk_short}} proporciona el mandato `service bind` para poner las credenciales de servicio de {{site.data.keyword.Bluemix_notm}} a disposición del código en el tiempo de ejecución. El mandato `service bind` se puede entonces utilizar para enlazar cualquier servicio de {{site.data.keyword.Bluemix_notm}} con cualquier acción definida en {{site.data.keyword.openwhisk_short}}.

Para conocer los pasos detallados sobre cómo utilizar los servicios de acciones, consulte el tema [Uso de servicios de acciones](./binding_services.html).

## Configure la CLI de {{site.data.keyword.openwhisk_short}} para utilizar un proxy HTTPS
{: #cli_https_proxy}

La CLI de {{site.data.keyword.openwhisk_short}} se puede configurar para utilizar un proxy HTTPS. Para configurar un proxy HTTPS, se debe crear una variable de entorno llamada `HTTPS_PROXY`. La variable se debe establecer en la dirección del proxy HTTPS y el puerto correspondiente utilizando el siguiente formato:
`{PROXY IP}:{PROXY PORT}`.

## Trabajo con regiones, organizaciones y espacios
{: #region_info}

Puede crear espacios para manejar los despliegues de preproducción (transferencia) y producción creando espacios para cada uno. La creación de espacios permite a {{site.data.keyword.openwhisk_short}} tener dos espacios de nombres distintos definidos para el usuario.

Puede utilizar `ic iam space-create` para crear más espacios bajo su organización, como "staging" y "production". Consulte el ejemplo siguiente:
```
ic iam space-create "staging"
ic iam space-create "production"
```

Para obtener más información sobre el mandato `ic iam`, consulte la documentación siguiente: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} tiene restricciones sobre los nombres de espacios de nombres. Para obtener más información sobre estas restricciones, consulte la documentación siguiente: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

Si ya ha iniciado la sesión, puede ejecutar el mandato `ic target` en la CLI de {{site.data.keyword.Bluemix_notm}} para cambiar regiones, organizaciones y espacios.

Utilice el siguiente mandato para mostrar todas las regiones de {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.openwhisk_short}} sólo está soportado en las regiones `us-south` y `eu-gb`.
{: tip}

```
ic regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

Utilice el mandato `ic target` para cambiar regiones.  Por ejemplo, si desea cambiar a la región de Reino Unido y al espacio `staging`:
```
ic target -r eu-gb -s staging
```
{: pre}


Si necesita cambiar espacios dentro de la misma región, como por ejemplo de transferencia a producción, ejecute el mandato siguiente:
```
ic target -s production
```
{: pre}


Para obtener más información sobre el mandato `target`, utilice `ic target --help` o revise la documentación siguiente:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migración de la CLI de OpenWhisk al plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Con la introducción del plugin de CLI de {{site.data.keyword.openwhisk_short}}, la CLI de OpenWhisk ya no es necesaria.

### Sintaxis de mandatos
{: #command_syntax}

Todos los mandatos `wsk`, excepto el mandato `wsk bluemix login`, que ya no es necesario, funcionan del mismo modo mediante el mandato `ic wsk`.  Todos los argumentos y las opciones de mandato son los mismos.

### Host y autenticación de API
{: #api_authentication}

En la CLI de OpenWhisk, debe configurar la clave de API de autenticación y el host de API. 
Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario que configure explícitamente la clave de API y el host de API. Debe iniciar la sesión con `ic login` y seleccionar la región y el espacio de nombres como destino mediante el mandato `ic target`. Después, todos los mandatos posteriores empiezan por `ic wsk`. La autenticación de esta manera también evita acceder a la página web de {{site.data.keyword.Bluemix_notm}} para obtener la información específica para configurar el entorno de CLI.

Si necesita obtener la clave de API de autenticación de {{site.data.keyword.openwhisk_short}} que se va a utilizar desde un cliente http externo, como cURL o Postman, puede recuperarla con los mandatos siguientes:

Para obtener la clave de API actual, ejecute el mandato siguiente:
```
ic wsk property get --auth
```
{: pre}


Para obtener el host de API actual, ejecute el mandato siguiente:
```
ic wsk property get --apihost
```
{: pre}


La clave de API es específica por región, organización y espacio seleccionados como destino en el plugin de CLI de {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticación de pasarela de API
{: #apigw_authentication}

La CLI de OpenWhisk actualmente requiere que ejecute `wsk bluemix login` para poder configurar la autorización de pasarela de API para la gestión de las API utilizando el mandato `wsk api`.

Con el plugin de CLI de {{site.data.keyword.openwhisk}}, ya no es necesario ejecutar `wsk bluemix login`. En su lugar, utilice el mandato `ic login` para iniciar sesión en {{site.data.keyword.Bluemix_notm}}, y el plugin de {{site.data.keyword.openwhisk}} utilizará automáticamente su información de destino e inicio de sesión actual. Ahora puede gestionar las API utilizando el mandato `ic wsk api`.

### Migración de scripts de despliegue
{: #migrating_deploy_scripts}

Si tiene scripts que utilizan la CLI de OpenWhisk con el binario de `wsk`, todos los mandatos funcionan de la misma manera mediante el mandato `ic wsk`. Puede modificar los scripts para utilizar el plugin de CLI de {{site.data.keyword.Bluemix_notm}} o crear un alias o un derivador para que las ejecuciones actuales de `wsk` se conviertan en `ic wsk`. Los mandatos `ic login` y `ic target` de la CLI de {{site.data.keyword.Bluemix_notm}} funcionan de modo desatendido. Con la modalidad desatendida, puede configurar el entorno antes de ejecutar mandatos `ic wsk` para desplegar y gestionar las entidades de {{site.data.keyword.openwhisk_short}}.


## Historial de versiones
{: #version_history}

Un registro histórico de versiones que muestra los puntos destacados y las correcciones de errores.

1.0.7 (02-02-2018)
* `ic wsk api` ahora acepta parámetros de vía de acceso, como `/api/{id}`. Para obtener información, consulte [Pasarela de API](./openwhisk_apigateway.html)
* Se ha restaurado el soporte de proxy.
* Se ha eliminado `swift:3`.

1.0.6 (30-01-2018)
* Arreglo de error para el mandato `ic wsk service bind` para las acciones de dentro de un paquete.
