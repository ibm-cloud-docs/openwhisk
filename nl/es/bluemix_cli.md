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
ibmcloud plugin install cloud-functions
```
{: pre}


Para validar si la instalación del plugin de {{site.data.keyword.openwhisk_short}} es correcta, ejecute el mandato siguiente:
```
ibmcloud plugin list cloud-functions
```
{: pre}


La salida muestra la información de la versión instalada de {{site.data.keyword.openwhisk_short}}:
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

Puede actualizar el plugin de {{site.data.keyword.openwhisk_short}} ejecutando el mandato siguiente:
```
ibmcloud plugin update Cloud-Functions
```
{: pre}


Para obtener más información sobre el mandato del plugin, utilice `ibmcloud plugin --help` o consulte la documentación siguiente:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## Autenticación de CLI de {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_auth}

{{site.data.keyword.openwhisk_short}} está disponible en dos regiones de {{site.data.keyword.Bluemix_notm}}.

Al iniciar sesión en la CLI de {{site.data.keyword.Bluemix_notm}}, puede especificar el punto final de la API de {{site.data.keyword.Bluemix_notm}} para la región de destino, además de la organización y el espacio.

Para iniciar sesión en la región EE.UU. sur:
```
ibmcloud login -a api.ng.bluemix.net
```
{: pre}


Para iniciar sesión en la región de Reino Unido:
```
ibmcloud login -a api.eu-gb.bluemix.net
```
{: pre}


El distintivo `-a` especifica el punto final de la API de {{site.data.keyword.Bluemix_notm}} que se debe utilizar. Si se especifica el punto final de la API, la opción `-a` no es necesaria. Puede utilizar el mandato `ibmcloud api` para definir explícitamente el punto final de la API de {{site.data.keyword.Bluemix_notm}}. Para visualizar el valor actual del punto final de la API, utilice el mandato `ibmcloud target`."

El mandato `login` le solicita información, como la organización, el espacio y la contraseña, si no se especifica. Puede especificar la organización y el espacio en la línea de mandatos para omitir sus solicitudes.
```
ibmcloud login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


También puede utilizar una clave de API de {{site.data.keyword.Bluemix_notm}} para iniciar una sesión. Este método es útil cuando la cuenta está configurada con un inicio de sesión federado, que requiere que inicie sesión con el distintivo `--sso`. El uso de una clave API también le beneficia si desea definir una integración continua (CI) y desea configurar un conducto desatendido.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

Para crear una nueva clave de API mediante la CLI de {{site.data.keyword.Bluemix_notm}}, ejecute el mandato siguiente:
```
ibmcloud iam api-key-create MyKey
```
{: pre}


A continuación, utilice el valor generado de clave de API para iniciar sesión, como en el ejemplo siguiente:
```
ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


Para obtener más información sobre el mandato del plugin, utilice `ibmcloud login --help` o revise la documentación siguiente:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Utilización del plug-in CLI de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_usage}

Compruebe su configuración. Realice una invocación de bloqueo (síncrona) de echo, pasándole `hello` como un argumento, como en el siguiente ejemplo:
```
ibmcloud wsk action invoke whisk.system/utils/echo -p message hello --result
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

Para obtener una lista de mandatos para el plugin de {{site.data.keyword.openwhisk_short}}, ejecute `ibmcloud wsk` sin argumentos.

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

Puede utilizar `ibmcloud iam space-create` para crear más espacios bajo su organización, como "staging" y "production". Consulte el ejemplo siguiente:
```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```

Para obtener más información sobre el mandato `ibmcloud iam`, consulte la documentación siguiente: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

{{site.data.keyword.openwhisk_short}} tiene restricciones sobre los nombres de espacios de nombres. Para obtener más información sobre estas restricciones, consulte la documentación siguiente: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

Si ya ha iniciado la sesión, puede ejecutar el mandato `ibmcloud target` en la CLI de {{site.data.keyword.Bluemix_notm}} para cambiar regiones, organizaciones y espacios.

Utilice el siguiente mandato para mostrar todas las regiones de {{site.data.keyword.Bluemix_notm}}.

{{site.data.keyword.openwhisk_short}} sólo está soportado en las regiones `us-south` y `eu-gb`.
{: tip}

```
ibmcloud regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

Utilice el mandato `ibmcloud target` para cambiar regiones.  Por ejemplo, si desea cambiar a la región de Reino Unido y al espacio `staging`:
```
ibmcloud target -r eu-gb -s staging
```
{: pre}


Si necesita cambiar espacios dentro de la misma región, como por ejemplo de transferencia a producción, ejecute el mandato siguiente:
```
ibmcloud target -s production
```
{: pre}


Para obtener más información sobre el mandato `target`, utilice `ibmcloud target --help` o revise la documentación siguiente:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migración de la CLI de OpenWhisk al plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Con la introducción del plugin de CLI de {{site.data.keyword.openwhisk_short}}, la CLI de OpenWhisk ya no es necesaria.

### Sintaxis de mandatos
{: #command_syntax}

Todos los mandatos `wsk`, excepto el mandato `wsk bluemix login`, que ya no es necesario, funcionan del mismo modo mediante el mandato `ibmcloud wsk`.  Todos los argumentos y las opciones de mandato son los mismos.

### Host y autenticación de API
{: #api_authentication}

En la CLI de OpenWhisk, debe configurar la clave de API de autenticación y el host de API. 
Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario que configure explícitamente la clave de API y el host de API. Debe iniciar la sesión con `ibmcloud login` y seleccionar la región y el espacio de nombres como destino mediante el mandato `ibmcloud target`. Después, todos los mandatos posteriores empiezan por `ibmcloud wsk`. La autenticación de esta manera también evita acceder a la página web de {{site.data.keyword.Bluemix_notm}} para obtener la información específica para configurar el entorno de CLI.

Si necesita obtener la clave de API de autenticación de {{site.data.keyword.openwhisk_short}} que se va a utilizar desde un cliente http externo, como cURL o Postman, puede recuperarla con los mandatos siguientes:

Para obtener la clave de API actual, ejecute el mandato siguiente:
```
ibmcloud wsk property get --auth
```
{: pre}


Para obtener el host de API actual, ejecute el mandato siguiente:
```
ibmcloud wsk property get --apihost
```
{: pre}


La clave de API es específica por región, organización y espacio seleccionados como destino en el plugin de CLI de {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticación de pasarela de API
{: #apigw_authentication}

La CLI de OpenWhisk actualmente requiere que ejecute `wsk bluemix login` para poder configurar la autorización de pasarela de API para la gestión de las API utilizando el mandato `wsk api`.

Con el plugin de CLI de {{site.data.keyword.openwhisk}}, ya no es necesario ejecutar `wsk bluemix login`. En su lugar, utilice el mandato `ibmcloud login` para iniciar sesión en {{site.data.keyword.Bluemix_notm}}, y el plugin de {{site.data.keyword.openwhisk}} utilizará automáticamente su información de destino e inicio de sesión actual. Ahora puede gestionar las API utilizando el mandato `ibmcloud wsk api`.

### Migración de scripts de despliegue
{: #migrating_deploy_scripts}

Si tiene scripts que utilizan la CLI de OpenWhisk con el binario de `wsk`, todos los mandatos funcionan de la misma manera mediante el mandato `ibmcloud wsk`. Puede modificar los scripts para utilizar el plugin de CLI de {{site.data.keyword.Bluemix_notm}} o crear un alias o un derivador para que las ejecuciones actuales de `wsk` se conviertan en `ibmcloud wsk`. Los mandatos `ibmcloud login` y `ibmcloud target` de la CLI de {{site.data.keyword.Bluemix_notm}} funcionan de modo desatendido. Con la modalidad desatendida, puede configurar el entorno antes de ejecutar mandatos `ibmcloud wsk` para desplegar y gestionar las entidades de {{site.data.keyword.openwhisk_short}}.


## Historial de versiones
{: #version_history}

Un registro histórico de versiones que muestra los puntos destacados y las correcciones de errores.

1.0.7 (02-02-2018)
* `ibmcloud wsk api` ahora acepta parámetros de vía de acceso, como `/api/{id}`. Para obtener información, consulte [Pasarela de API](./openwhisk_apigateway.html)
* Se ha restaurado el soporte de proxy.
* Se ha eliminado `swift:3`.

1.0.6 (30-01-2018)
* Arreglo de error para el mandato `ibmcloud wsk service bind` para las acciones de dentro de un paquete.
