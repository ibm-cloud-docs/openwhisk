---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

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


# Instalación de la CLI y el plugin
{: #cli_install}

{{site.data.keyword.openwhisk}} ofrece un potente plugin para la CLI de {{site.data.keyword.cloud_notm}} que permite la gestión completa del sistema {{site.data.keyword.openwhisk_short}}. Puede utilizar el plugin de CLI de {{site.data.keyword.openwhisk_short}} para gestionar los fragmentos de código en las acciones, crear desencadenantes y reglas para permitir que las acciones respondan a sucesos, y empaquetar acciones en paquetes.
{:shortdesc}


## Configuración de la CLI de {{site.data.keyword.cloud_notm}}
{: #cli_setup}

**Antes de empezar**

Debe crear una [cuenta de {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.

Descargue e instale la CLI de {{site.data.keyword.cloud_notm}} e inicie la sesión.
{: shortdesc}

1. Descargue e instale la [CLI de {{site.data.keyword.cloud_notm}}](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Inicie la sesión en la CLI de {{site.data.keyword.cloud_notm}}. Para especificar una región de {{site.data.keyword.cloud_notm}}, [incluya el punto final de API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

3. Siga las indicaciones para seleccionar la cuenta de {{site.data.keyword.cloud_notm}}.

4. Obtenga una lista de grupos de recursos. 

```
ibmcloud resource groups
```
{: pre}

**Resultado de ejemplo**

```
Retrieving all resource groups under account <account_name> as email@ibm.com...
OK
Name      ID                                 Default Group   State   
default   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
test      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. Opcional: Establecer como destino un grupo de recursos que no sea el valor predeterminado ejecutando el mandato siguiente.
```
ibmcloud target -g <resource_group>
```
{: pre}


**Resultado de ejemplo**

```
Targeted resource group <resource_group>
```
{: screen}

## Configuración del plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cli_plugin_setup}

Para trabajar con {{site.data.keyword.openwhisk_short}}, descargue e instale el plugin de CLI.
{: shortdesc}

Puede utilizar el plug-in de CLI de {{site.data.keyword.openwhisk_short}} para realizar las tareas siguientes.

* Ejecutar sus fragmentos de código, o acciones, en {{site.data.keyword.openwhisk_short}}. Consulte [Creación e invocación de acciones](/docs/openwhisk?topic=cloud-functions-actions).
* Crear desencadenantes y reglas para permitir a sus acciones responder ante sucesos. Consulte [Creación de desencadenantes y reglas](/docs/openwhisk?topic=cloud-functions-triggers).
* Empaquetar acciones y configurar orígenes de sucesos externos. Consulte [Creación y uso de paquetes](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Explorar el catálogo de paquetes y mejorar sus aplicaciones con servicios externos. Consulte [Adición de {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services).

Complete los pasos siguientes para instalar el Explorar el plugin de CLI de {{site.data.keyword.openwhisk_short}}

1. Instale el plug-in de {{site.data.keyword.openwhisk_short}}.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Verifique que esté instalado el plug-in de CLI.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  **Resultado**
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. Tras iniciar la sesión, todos los mandatos de {{site.data.keyword.openwhisk_short}} empiezan por `ibmcloud fn`. Para ver todo lo que puede hacer con el plugin de {{site.data.keyword.openwhisk_short}}, ejecute `ibmcloud fn` sin argumentos.
  ```
  ibmcloud fn
  ```
  {: pre}




## Establecer como destino espacios de nombres de {{site.data.keyword.openwhisk_short}}
{: #cli_regions}
De forma predeterminada, {{site.data.keyword.openwhisk_short}} utiliza [espacios de nombres habilitados para IAM](/docs/iam?topic=iam-iamoverview){: external}. Ya no se pueden crear espacios de nombres basados en Cloud Foundry.
{: important}

### Crear o establecer como destino un espacio de nombres.
Para obtener una lista de los espacios de nombres de {{site.data.keyword.openwhisk_short}}, ejecute `ibmcloud fn namespace list`.

#### Cree un espacio de nombres habilitado para IAM.
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**Respuesta**
  ```
  ok: created namespace <namespace_name>
  ```
  {: screen}


#### Establecer como destino un espacio de nombres habilitado para IAM. 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**Respuesta**
  ```
  ok: whisk namespace set to <namespace_name>
  ```
  {: screen}
  
#### Establecer como destino un espacio de nombres basado en Cloud Foundry. 
  
Puede utilizar los distintivos `-o` y `-s` para dirigirse a un `org` y `space` específicos, o puede seguir las indicaciones.

* Para establecer como destino un espacio de nombres de Cloud Foundy, incluya los nombres `org` y `space` en el mandato `target`.

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Establezca Cloud Foundry como destino y siga las indicaciones para seleccionar un `org` y un `space`.

```
ibmcloud target --cf
```
{: pre}


**Respuesta**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>
                        
  API endpoint:      https://cloud.ibm.com   
  Region:            us-south   
  User:              <email>   
  Account:           (<account_id>) <-> <account>   
  Resource group:    default   
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)   
  Org:               <org_name>   
  Space:             <space_name>  
  ```
  {: screen} 





#### Opcional: Creación de espacios de nombres para despliegues de transición y producción.

Puede crear espacios de nombres habilitados para IAM para manejar sus despliegues de preproducción (transferencia) y de producción de {{site.data.keyword.openwhisk_short}} creando espacios de nombres para cada uno de ellos. Ejecute [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) para crear más espacios de nombres en su organización, como por ejemplo "staging" (transferencia) y "production" (producción):

Cree un espacio de nombres de transferencia.
```
ibmcloud fn namespace create staging
```
{: pre}

Cree un espacio de nombres de producción.
```
ibmcloud fn namespace create production
```
{: pre}

{{site.data.keyword.openwhisk_short}} tiene restricciones sobre los nombres de espacios de nombres. Para obtener más información, consulte la documentación sobre [Detalles y límites del sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}


## Configuración de la CLI de {{site.data.keyword.openwhisk_short}} para utilizar un proxy HTTPS
{: #cli_proxy}

La CLI de {{site.data.keyword.openwhisk_short}} se puede configurar para utilizar un proxy HTTPS. Para configurar un proxy HTTPS, se debe crear una variable de entorno llamada `HTTPS_PROXY`. La variable se debe establecer en la dirección del proxy HTTPS y el puerto correspondiente utilizando el siguiente formato: `{PROXY IP}:{PROXY PORT}`.

Si se cambia el nombre de `org` o de `space`, se crea un nuevo espacio de nombres basado en el nuevo nombre. Las entidades del espacio de nombres antiguo no serán visibles en el nuevo espacio de nombres y se pueden suprimir.
{: important}


## Migración de la CLI de OpenWhisk al plugin de CLI de {{site.data.keyword.openwhisk_short}}
{: #cli_migrate}

Ahora puede utilizar el plugin de CLI de {{site.data.keyword.openwhisk_short}} para interactuar con las entidades de {{site.data.keyword.openwhisk_short}}. Aunque puede seguir usando la CLI de OpenWhisk autónoma, no tiene las características más recientes admitidas por {{site.data.keyword.openwhisk_short}}, como espacios de nombres basados en IAM y `enlace de servicios`.
{: shortdesc}


### Sintaxis de mandatos
{: #cli_syntax}

Todas las opciones y argumentos de mandato de los mandatos del plugin de CLI de Cloud Functions son las mismas que las opciones de [la CLI autónoma de OpenWhisk](https://github.com/apache/incubator-openwhisk-cli){: external}. Pero, tenga en cuenta las diferencias siguientes.

* El plugin de {{site.data.keyword.openwhisk}} utiliza automáticamente su información de destino e inicio de sesión actual.
* Los mandatos de `wsk` se ejecutan ahora como `ibmcloud fn`.
* El mandato `wsk ibmcloud login` ya no se necesita. Puede iniciar sesión utilizando `ibmcloud login`.
* Puede gestionar las API utilizando `ibmcloud fn api`.

Para obtener más información, consulte la [Referencia de CLI de {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli).

### Host y autenticación de API
{: #cli_api_auth}

Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario que configure explícitamente la clave de API y el host de API. En su lugar, puede iniciar la sesión con `ibmcloud login`. Puede establecer como destino un espacio de nombres habilitado para IAM ejecutando `ibmcloud fn property set --namespace <namespace_name>` o un espacio de nombres basado en Cloud Foundry ejecutando `ibmcloud target --cf`. Tras iniciar la sesión, todos los mandatos empiezan por `ibmcloud fn`.


Si necesita utilizar la clave de API de autenticación de {{site.data.keyword.openwhisk_short}} en un cliente HTTP externo, como cURL o Postman, puede recuperarla con los mandatos siguientes.

Obtenga la clave de API actual ejecutando el mandato siguiente.
```
ibmcloud fn property get --auth
```
{: pre}

Obtenga el host de API actual ejecutando el mandato siguiente.
```
ibmcloud fn property get --apihost
```
{: pre}

La clave de API es específica por región, organización y espacio seleccionados como destino en el plugin de CLI de {{site.data.keyword.openwhisk_short}}.
{: tip}


### Autenticación de pasarela de API
{: #cli_apigw_authentication}

La CLI de OpenWhisk requería que ejecute `wsk ibmcloud login` para poder configurar la autorización de pasarela de API para la gestión de las API utilizando el mandato `wsk api`. Con el plugin de CLI de {{site.data.keyword.openwhisk_short}}, no es necesario ejecutar `wsk ibmcloud login`. En su lugar, cuando utilice el mandato `ibmcloud login` para iniciar sesión en {{site.data.keyword.cloud_notm}}, el plugin de {{site.data.keyword.openwhisk}} utilizará automáticamente su información de destino e inicio de sesión actual. Ahora puede gestionar las API utilizando el mandato `ibmcloud fn api`.


### Migración de scripts de despliegue
{: #cli_migrating_deploy_scripts}

Si tiene scripts que utilizan la CLI de OpenWhisk con los mandatos `wsk`, todos los mandatos funcionan igual utilizando el mandato `ibmcloud fn`. Puede modificar los scripts para utilizar el plugin de CLI de {{site.data.keyword.cloud_notm}} o crear un alias o un derivador para que los mandatos actuales que utilizan `wsk` se conviertan en `ibmcloud fn`. Los mandatos `ibmcloud login` e `ibmcloud target` de la CLI de {{site.data.keyword.cloud_notm}} funcionan de modo desatendido. Con la modalidad desatendida, puede configurar el entorno antes de ejecutar mandatos `ibmcloud fn` para desplegar y gestionar las entidades de {{site.data.keyword.openwhisk_short}}.

## Historial de versiones de CLI
{: #cli_versions}

Un registro histórico de versiones que muestra los puntos destacados y las correcciones de errores.

v1.0.30 (03 de abril de 2019)
* Se ha mejorado la gestión del `enlace de servicios` de IAM y los servicios basados en organización y espacio.
* Se ha añadido un arreglo para el manejo del punto final de API https://cloud.ibm.com.

v1.0.29 (06 de febrero de 2019)
* Se han añadido los mandatos `deploy` y `undeploy` para desplegar y anular el despliegue de una recopilación de entidades de Functions a través de un archivo de manifiesto. Para obtener más información, consulte la documentación de [Despliegue](/docs/openwhisk?topic=cloud-functions-deploy#deploy).

v1.0.28 (21 de enero de 2019)
* Se ha añadido un mensaje de error cuando `update|delete|get namespace name` existe varias veces.

v1.0.27 (11 de diciembre de 2018)
* Se han añadido arreglos para `namespace get`.
* Se ha añadido un arreglo para `--save-as` cuando una acción es una acción de caja negra.

v1.0.26 (30 de noviembre de 2018)
* Se ha habilitado `fn property get --auth` para devolver correctamente la clave de autenticación en un entorno nuevo.

v1.0.25 (23 de noviembre de 2018)
* Se ha mejorado la visualización del resultado de los mensajes de error.
* Se ha añadido un arreglo de `fn namespace get` para mostrar correctamente las propiedades del espacio de nombres.

1.0.23 (15 de octubre de 2018)
* Se ha añadido soporte para el reconocimiento del código de una acción ruby (`.rb`).

1.0.22 (20 de agosto de 2018)
* Se ha añadido soporte para la región us-east.

1.0.21 (01 de agosto de 2018)
* Los alias `fn` y `functions` ahora se pueden utilizar para mandatos de {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>` e `ibmcloud fn <command>`. También puede seguir utilizando `ibmcloud wsk <command>`.

1.0.19 (02 de julio de 2018)
* Mejoras y arreglos de errores menores.

1.0.18 (20 de junio de 2018)
* Se ha añadido un arreglo para anular el enlace de instancias de servicio proporcionadas por el usuario.
* Mejoras en el rendimiento.

1.0.17 (12 de junio de 2018)
* Se ha añadido soporte para enlazar (`ibmcloud wsk service bind`) y desenlazar (`ibmcloud wsk service unbind`) instancias de servicio suministradas por el usuario creadas con el mandato `ibmcloud cf create-user-provided-service`.

1.0.16 (24 de mayo de 2018)
* Mejoras y arreglos de errores menores.

1.0.15 (21 de mayo de 2018)
* Mejoras y arreglos de errores menores.

1.0.14 (17 de mayo de 2018)
* Se ha habilitado el soporte para el carácter `&` en nombres de organizaciones y de espacios.

1.0.13 (07 de mayo de 2018)
* Mejoras en la gestión de errores y arreglos de errores menores.

1.0.12 (30 de abril de 2018)
* Actualizaciones en el SDK de {{site.data.keyword.cloud_notm}} para mantener la compatibilidad con la CLI de `bx`.

1.0.11 (23 de abril de 2018)
* Mejoras y arreglos de errores menores.

1.0.10 (09 de abril de 2018)
* Se ha añadido una nueva opción `--web-secure` al mandato `ibmcloud wsk action create|update` para proteger los puntos finales de acciones web.
* Se ha arreglado el parámetro [defect](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external} de vía de acceso back-to-back.

1.0.9 (16 de marzo de 2018)
* Se ha habilitado el soporte para el enlace de servicios en el nivel de paquete.

1.0.8 (22 de febrero de 2018)
* Se ha habilitado el soporte para el enlace de servicios IAM.

1.0.7 (02 de febrero de 2018)
* Se ha actualizado `ibmcloud wsk api` para que acepte parámetros de vía de acceso como `/api/{id}`. Para obtener más información, consulte [Pasarela de API](/docs/openwhisk?topic=cloud-functions-apigateway).
* Se ha restaurado el soporte de proxy.
* Se ha eliminado `swift:3`.

1.0.6 (30 de enero de 2018)
* Se ha arreglado un error con el mandato `ibmcloud wsk service bind` para las acciones que hay dentro de un paquete.



