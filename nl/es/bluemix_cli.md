---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Configuración del plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

{{site.data.keyword.openwhisk}} ofrece un potente plugin para la CLI de {{site.data.keyword.Bluemix_notm}} que permite la gestión completa del sistema {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configuración de la CLI de {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Descargue e instale la CLI de {{site.data.keyword.Bluemix_notm}} e inicie la sesión.
{: shortdesc}

1. Descargue e instale la [CLI de {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. {{site.data.keyword.openwhisk_short}} está disponible en las regiones de EE.UU. sur y Reino Unido de {{site.data.keyword.Bluemix_notm}}. Si no se ha especificado el punto final de la API de {{site.data.keyword.Bluemix_notm}}, especifique uno con el distintivo `-a`.

    * Para iniciar sesión en la región EE.UU. sur:
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * Para iniciar sesión en la región de Reino Unido:
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  Puede utilizar el mandato `ibmcloud api` para definir explícitamente el punto final de la API de {{site.data.keyword.Bluemix_notm}}. Para visualizar el valor actual del punto final de la API, utilice el mandato `ibmcloud target`.
  {: tip}

3. El mandato `ibmcloud login` le solicita información, como la organización, el espacio y la contraseña, si no se especifica.

  Puede especificar la organización y el espacio en el inicio de sesión para omitir sus solicitudes. Utilice los siguientes distintivos:`ibmcloud login -o <ORG> -s <SPACE>`.
  {: tip}

También puede utilizar una clave de API de {{site.data.keyword.Bluemix_notm}} para iniciar una sesión. Este método es útil cuando la cuenta está configurada con un inicio de sesión federado, que requiere que inicie sesión con el distintivo `--sso`. El [uso de una clave API](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key) también le beneficia si desea definir una integración continua (CI) y desea configurar un conducto desatendido.

1. Cree una nueva clave de API.
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. Utilice el valor generado de la clave de API para iniciar la sesión.
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
Para obtener más información sobre el mandato `ibmcloud login`, utilice `ibmcloud login --help` o consulte el tema [Mandatos de IBM Cloud (bx)](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login).

## Configuración del plug-in de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Descargue e instale el plug-in de {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

1. Instale el plug-in de {{site.data.keyword.openwhisk_short}}.
    ```
    ibmcloud plugin install cloud-functions
    ```
    {: pre}

2. Verifique que esté instalado el plug-in de CLI.
    ```
    ibmcloud plugin list cloud-functions
    ```
    {: pre}

    Salida:
    ```
    Plugin Name          Version
    Cloud-Functions      1.0.16
    ```
    {: screen}

3. Realice una invocación de bloqueo (síncrona) de `echo`. Pase `hello` como un argumento.
    ```
    ibmcloud fn action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. Verifique que se devuelve el mensaje `hello` en la salida.
    Salida de ejemplo:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. Puede actualizar el plugin de {{site.data.keyword.openwhisk_short}} ejecutando el mandato siguiente:
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

Puede utilizar la CLI de {{site.data.keyword.openwhisk_short}} para:

* Ejecutar sus fragmentos de código, o acciones, en {{site.data.keyword.openwhisk_short}}. Consulte
[Creación e invocación de acciones](./openwhisk_actions.html).
* Usar desencadenantes y reglas para permitir a sus acciones responder ante sucesos. Consulte
[Creación de desencadenantes y reglas](./openwhisk_triggers_rules.html).
* Aprender cómo los paquetes agrupan acciones y configurar orígenes de sucesos externos. Consulte [Creación y uso de paquetes](./openwhisk_packages.html).
* Explorar el catálogo de paquetes y mejorar sus aplicaciones con servicios externos, como un [origen de sucesos de {{site.data.keyword.cloudant}}](./openwhisk_cloudant.html).

Para obtener una lista de mandatos para el plugin de {{site.data.keyword.openwhisk_short}}, ejecute `ibmcloud fn` sin argumentos.
{: tip}

## Uso de servicios de acciones
{: #binding_services}

{{site.data.keyword.openwhisk_short}} proporciona el mandato `service bind` para poner las credenciales de servicio de {{site.data.keyword.Bluemix_notm}} a disposición del código en el tiempo de ejecución. A continuación, puede utilizar el mandato `service bind` para enlazar cualquier servicio de {{site.data.keyword.Bluemix_notm}} con cualquier acción definida en {{site.data.keyword.openwhisk_short}}.

Para conocer los pasos detallados sobre cómo utilizar los servicios de acciones, consulte [Enlace de servicios a acciones](./binding_services.html).

## Configuración de la CLI de {{site.data.keyword.openwhisk_short}} para utilizar un proxy HTTPS
{: #cli_https_proxy}

La CLI de {{site.data.keyword.openwhisk_short}} se puede configurar para utilizar un proxy HTTPS. Para configurar un proxy HTTPS, se debe crear una variable de entorno llamada `HTTPS_PROXY`. La variable se debe establecer en la dirección del proxy HTTPS y el puerto correspondiente utilizando el siguiente formato:
`{PROXY IP}:{PROXY PORT}`.

## Conmutación a diferentes regiones, organizaciones y espacios
{: #region_info}

Si ya ha iniciado la sesión, puede ejecutar el mandato `ibmcloud target` en la CLI de {{site.data.keyword.Bluemix_notm}} para cambiar regiones, organizaciones y espacios.

{{site.data.keyword.openwhisk_short}} está disponible en las regiones de EE.UU. sur y Reino Unido de {{site.data.keyword.Bluemix_notm}}. Para cambiar de región, utilice el mandato `ibmcloud target`. Por ejemplo, para cambiar a la región de Reino Unido y al espacio `staging` de esa región:
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

Puede crear espacios para manejar los despliegues de preproducción (transferencia) y producción creando espacios para cada uno. La creación de espacios permite a {{site.data.keyword.openwhisk_short}} tener dos espacios de nombres distintos definidos para el usuario. Ejecute [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) para crear más espacios bajo su organización, como "staging" y "production":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} tiene restricciones sobre los nombres de espacios de nombres. Para obtener más información, consulte el apartado sobre [Detalles y límites del sistema](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities).
{: tip}

## Migración de la CLI de OpenWhisk al plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Con la introducción del plugin de CLI de {{site.data.keyword.openwhisk_short}}, la CLI autónoma de OpenWhisk ya no es necesaria.

### Sintaxis de mandatos
{: #command_syntax}

Todos los mandatos `wsk`, excepto el mandato `wsk bluemix login`, que ya no es necesario, funcionan del mismo modo mediante el mandato `ibmcloud fn`.  Todos los argumentos y las opciones de mandato son los mismos.

### Host y autenticación de API
{: #api_authentication}

En la CLI de OpenWhisk, se debía configurar la clave de API de autenticación y el host de API.
Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario que configure explícitamente la clave de API y el host de API. En su lugar, puede iniciar la sesión con `ibmcloud login` y seleccionar la región y el espacio de nombres como destino mediante el mandato `ibmcloud target`. Tras iniciar la sesión, todos los mandatos empiezan por `ibmcloud fn`.

Si necesita utilizar la clave de API de autenticación de {{site.data.keyword.openwhisk_short}} en un cliente HTTP externo, como cURL o Postman, puede recuperarla con los mandatos siguientes:

Para obtener la clave de API actual:
```
ibmcloud fn property get --auth
```
{: pre}

Para obtener el host de API actual:
```
ibmcloud fn property get --apihost
```
{: pre}

La clave de API es específica por región, organización y espacio seleccionados como destino en el plugin de CLI de {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticación de pasarela de API
{: #apigw_authentication}

La CLI de OpenWhisk requería que ejecute `wsk bluemix login` para poder configurar la autorización de pasarela de API para la gestión de las API utilizando el mandato `wsk api`. Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario ejecutar `wsk bluemix login`. En su lugar, cuando utilice el mandato `ibmcloud login` para iniciar sesión en {{site.data.keyword.Bluemix_notm}}, el plugin de {{site.data.keyword.openwhisk}} utilizará automáticamente su información de destino e inicio de sesión actual. Ahora puede gestionar las API utilizando el mandato `ibmcloud fn api`.

### Migración de scripts de despliegue
{: #migrating_deploy_scripts}

Si tiene scripts que utilizan la CLI de OpenWhisk con el binario de `wsk`, todos los mandatos funcionan de la misma manera mediante el mandato `ibmcloud fn`. Puede modificar los scripts para utilizar el plugin de CLI de {{site.data.keyword.Bluemix_notm}} o crear un alias o un derivador para que los mandatos actuales que utilizan `wsk` se conviertan en `ibmcloud fn`. Los mandatos `ibmcloud login` e `ibmcloud target` de la CLI de {{site.data.keyword.Bluemix_notm}} funcionan de modo desatendido. Con la modalidad desatendida, puede configurar el entorno antes de ejecutar mandatos `ibmcloud fn` para desplegar y gestionar las entidades de {{site.data.keyword.openwhisk_short}}.

## Historial de versiones
{: #version_history}

Un registro histórico de versiones que muestra los puntos destacados y las correcciones de errores.

1.0.18 (20-06-2018)
* Arreglo para desenlazar instancias de servicio suministradas por el usuario.
* Mejoras en el rendimiento.

1.0.17 (12-06-2018)
* Adición de soporte para enlazar (`ibmcloud wsk service bind`) y desenlazar (`ibmcloud wsk service unbind`) instancias de servicio suministradas por el usuario creadas con el mandato `ibmcloud cf create-user-provided-service`.

1.0.16 (24-05-2018)
* Mejoras y arreglos de errores menores.

1.0.15 (21-05-2018)
* Mejoras y arreglos de errores menores.

1.0.14 (17-05-2018)
* Habilitación del soporte para el carácter `&` en nombres de organizaciones y de espacios.

1.0.13 (07-05-2018)
* Mejoras en la gestión de errores y arreglos de errores menores.

1.0.12 (30-04-2018)
* Actualizaciones en el SDK de {{site.data.keyword.Bluemix_notm}} para mantener la compatibilidad con la CLI de `bx`.

1.0.11 (23-04-2018)
* Mejoras y arreglos de errores menores.

1.0.10 (09-04-2018)
* Adición de la nueva opción `--web-secure` al mandato `ibmcloud wsk action create|update` para proteger los puntos finales de acciones web.
* Arreglo del parámetro [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237) de back-to-back path.

1.0.9 (16-03-2018)
* Habilitación del soporte para el enlace de servicios en el nivel de paquete.

1.0.8 (22-02-2018)
* Habilitación del soporte para el enlace de servicios IAM.

1.0.7 (02-02-2018)
* `ibmcloud wsk api` ahora acepta parámetros de vía de acceso, como `/api/{id}`. Para obtener información, consulte [Pasarela de API](./openwhisk_apigateway.html).
* Se ha restaurado el soporte de proxy.
* Se ha eliminado `swift:3`.

1.0.6 (30-01-2018)
* Arreglo de error para el mandato `ibmcloud wsk service bind` para las acciones de dentro de un paquete.
