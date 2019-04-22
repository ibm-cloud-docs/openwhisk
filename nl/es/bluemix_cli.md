---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuración del plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}


{{site.data.keyword.openwhisk}} ofrece un potente plugin para la CLI de {{site.data.keyword.Bluemix_notm}} que permite la gestión completa del sistema {{site.data.keyword.openwhisk_short}}. Puede utilizar el plugin de CLI de {{site.data.keyword.openwhisk_short}} para gestionar los fragmentos de código en las acciones, crear desencadenantes y reglas para permitir que las acciones respondan a sucesos, y empaquetar acciones en paquetes.
{:shortdesc}

Ahora puede utilizar el alias `fn` en los mandatos del plugin {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>`
{: tip}

## Configuración de la CLI de {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Descargue e instale la CLI de {{site.data.keyword.Bluemix_notm}} e inicie la sesión.
{: shortdesc}

1. Descargue e instale la
[CLI de {{site.data.keyword.Bluemix_notm}}](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Para especificar una región de IBM Cloud, [incluya el punto final de API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

  Puede especificar la organización y el espacio en el inicio de sesión para omitir sus solicitudes utilizando los distintivos siguientes: `ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. Si no ha especificado una organización ni un espacio, complete las solicitudes que siguen al mandato de inicio de sesión.


## Configuración del plug-in de {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Para trabajar con {{site.data.keyword.openwhisk_short}}, descargue e instale el plugin de CLI.
{: shortdesc}

Puede utilizar el plugin para:

* Ejecutar sus fragmentos de código, o acciones, en {{site.data.keyword.openwhisk_short}}. Consulte
[Creación e invocación de acciones](/docs/openwhisk?topic=cloud-functions-openwhisk_actions).
* Usar desencadenantes y reglas para permitir a sus acciones responder ante sucesos. Consulte
[Creación de desencadenantes y reglas](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers).
* Aprender cómo los paquetes agrupan acciones y configurar orígenes de sucesos externos. Consulte [Creación y uso de paquetes](/docs/openwhisk?topic=cloud-functions-openwhisk_packages).
* Explorar el catálogo de paquetes y mejorar sus aplicaciones con servicios externos, como un [origen de sucesos de {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

Para ver todo lo que puede hacer con el plugin de {{site.data.keyword.openwhisk_short}}, ejecute `ibmcloud fn` sin argumentos.
{: tip}

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

¿Ya tiene el plugin pero necesita actualizar? Ejecute `ibmcloud plugin update cloud-functions`.
{:tip}



## Uso de servicios de acciones
{: #binding_services_cli}

{{site.data.keyword.openwhisk_short}} proporciona el mandato `service bind` para poner las credenciales de servicio de {{site.data.keyword.Bluemix_notm}} a disposición del código en el tiempo de ejecución. A continuación, puede utilizar el mandato `service bind` para enlazar cualquier servicio de {{site.data.keyword.Bluemix_notm}} con cualquier acción definida en {{site.data.keyword.openwhisk_short}}.

Para conocer los pasos detallados sobre cómo utilizar los servicios de acciones, consulte [Enlace de servicios a acciones](/docs/openwhisk?topic=cloud-functions-binding_services).


## Configuración de la CLI de {{site.data.keyword.openwhisk_short}} para utilizar un proxy HTTPS
{: #cli_https_proxy}

La CLI de {{site.data.keyword.openwhisk_short}} se puede configurar para utilizar un proxy HTTPS. Para configurar un proxy HTTPS, se debe crear una variable de entorno llamada `HTTPS_PROXY`. La variable se debe establecer en la dirección del proxy HTTPS y el puerto correspondiente utilizando el siguiente formato:
`{PROXY IP}:{PROXY PORT}`.



## Conmutación a diferentes regiones, organizaciones y espacios
{: #region_info}

Si ya ha iniciado la sesión, puede ejecutar el mandato `ibmcloud target` en la CLI de {{site.data.keyword.Bluemix_notm}} para cambiar regiones, organizaciones y espacios.


Para crear y gestionar entidades, debe hacer referencia a un espacio de nombres. El espacio de nombres predeterminado, que se puede indicar con un guión bajo (`_`) en algunas situaciones, corresponde al espacio de nombres basado en Cloud Foundry al que se hace referencia actualmente.

Puede crear espacios para manejar los despliegues de preproducción (transferencia) y producción creando espacios para cada uno. La creación de espacios permite a {{site.data.keyword.openwhisk_short}} tener dos espacios de nombres distintos definidos para el usuario. Ejecute [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create) para crear más espacios bajo su organización, como "staging" y "production":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

{{site.data.keyword.openwhisk_short}} tiene restricciones sobre los nombres de espacios de nombres. Para obtener más información, consulte el apartado sobre [Detalles y límites del sistema](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_entities).
{: tip}

**Aviso**: al cambiar el nombre de la organización o el espacio, se crea un nuevo espacio de nombres basado en el nombre modificado. Las entidades del espacio de nombres antiguo no serán visibles en el nuevo espacio de nombres y se pueden suprimir.


## Migración de la CLI de OpenWhisk al plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Ahora puede utilizar el plugin de CLI de {{site.data.keyword.openwhisk_short}} para interactuar con las entidades de
{{site.data.keyword.openwhisk_short}}. Aunque puede seguir usando la CLI de OpenWhisk autónoma, no tendrá las características más recientes admitidas por {{site.data.keyword.openwhisk_short}}, como espacios de nombres basados en IAM y `enlace de servicios`.
{: shortdesc}

### Sintaxis de mandatos
{: #command_syntax}

Todos los mandatos `wsk`, excepto el mandato `wsk bluemix login`, que ya no es necesario, funcionan del mismo modo mediante el mandato `ibmcloud fn`. Todas las opciones y argumentos de mandato de los mandatos del plugin de CLI de Cloud Functions son las mismas que las de los mandatos de la CLI autónoma de OpenWhisk. Para obtener más información, consulte
[Proyecto de CLI de Apache OpenWhisk ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-cli).

### Host y autenticación de API
{: #api_authentication}

En la CLI de OpenWhisk, se debía configurar la clave de API de autenticación y el host de API. Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario que configure explícitamente la clave de API y el host de API. En su lugar, puede iniciar la sesión con `ibmcloud login` y seleccionar la región y el espacio de nombres como destino mediante el mandato `ibmcloud target`. Tras iniciar la sesión, todos los mandatos empiezan por `ibmcloud fn`.


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





## Historial de versiones de CLI
{: #version_history}

Un registro histórico de versiones que muestra los puntos destacados y las correcciones de errores.

v1.0.30 (03-04-2019)
* Se ha mejorado la gestión del `enlace de servicios` de IAM y los servicios basados en organización y espacio.
* Se ha añadido un arreglo para el manejo del punto final de API https://cloud.ibm.com.

v1.0.29 (06-02-2019)
* Se han añadido los mandatos `deploy` y `undeploy` para desplegar y anular el despliegue de una recopilación de entidades de Functions a través de un archivo de manifiesto. Para obtener más información, consulte la documentación de
[Despliegue](/docs/openwhisk?topic=cloud-functions-deploy#deploy).

v1.0.28 (21-01-2019)
* Se ha añadido un mensaje de error cuando `update|delete|get namespace name` existe varias veces.

v1.0.27 (11-12-2018)
* Se han añadido arreglos para `namespace get`.
* Se ha añadido un arreglo para `--save-as` cuando una acción es una acción de caja negra.
* Se ha añadido el distintivo `--concurrency` para los mandatos action create y action update.

v1.0.26 (30-11-2018)
* Se ha habilitado `fn property get --auth` para devolver correctamente la clave de autenticación en un entorno nuevo.

v1.0.25 (23-11-2018)
* Se ha mejorado la visualización del resultado de los mensajes de error.
* Se ha añadido un arreglo de `fn namespace get` para mostrar correctamente las propiedades del espacio de nombres.

1.0.23 (15-10-2018)
* Se ha añadido soporte para el reconocimiento del código de una acción ruby (.rb).

1.0.22 (20-08-2018)
* Se ha añadido soporte para la región us-east.

1.0.21 (2018-08-01)
* Los alias `fn` y `functions` ahora se pueden utilizar para mandatos de {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>` e `ibmcloud fn <command>`. También puede seguir utilizando `ibmcloud wsk <command>`.

1.0.19 (2018-07-02)
* Mejoras y arreglos de errores menores.

1.0.18 (20-06-2018)
* Se ha añadido un arreglo para anular el enlace de instancias de servicio proporcionadas por el usuario.
* Mejoras en el rendimiento.

1.0.17 (12-06-2018)
* Se ha añadido soporte para enlazar (`ibmcloud wsk service bind`) y desenlazar (`ibmcloud wsk service unbind`) instancias de servicio suministradas por el usuario creadas con el mandato `ibmcloud cf create-user-provided-service`.

1.0.16 (24-05-2018)
* Mejoras y arreglos de errores menores.

1.0.15 (21-05-2018)
* Mejoras y arreglos de errores menores.

1.0.14 (17-05-2018)
* Se ha habilitado el soporte para el carácter `&` en nombres de organizaciones y de espacios.

1.0.13 (07-05-2018)
* Mejoras en la gestión de errores y arreglos de errores menores.

1.0.12 (30-04-2018)
* Actualizaciones en el SDK de {{site.data.keyword.Bluemix_notm}} para mantener la compatibilidad con la CLI de `bx`.

1.0.11 (23-04-2018)
* Mejoras y arreglos de errores menores.

1.0.10 (09-04-2018)
* Se ha añadido una nueva opción `--web-secure` al mandato `ibmcloud wsk action create|update` para proteger los puntos finales de acciones web.
* Se ha arreglado el parámetro
[defect](https://github.com/apache/incubator-openwhisk-cli/issues/237) de vía de acceso back-to-back.

1.0.9 (16-03-2018)
* Se ha habilitado el soporte para el enlace de servicios en el nivel de paquete.

1.0.8 (22-02-2018)
* Se ha habilitado el soporte para el enlace de servicios IAM.

1.0.7 (02-02-2018)
* Se ha actualizado `ibmcloud wsk api` para que acepte parámetros de vía de acceso como `/api/{id}`. Para obtener información, consulte [Pasarela de API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway).
* Se ha restaurado el soporte de proxy.
* Se ha eliminado `swift:3`.

1.0.6 (30-01-2018)
* Se ha arreglado un error con el mandato `ibmcloud wsk service bind` para las acciones que hay dentro de un paquete.
