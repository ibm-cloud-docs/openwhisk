---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# CLI autónoma de {{site.data.keyword.openwhisk_short}}

**En 9 de marzo de 2018, la descarga de la CLI autónoma de {{site.data.keyword.openwhisk_short}} dejará de estar disponible. Para seguir gestionando entidades de {{site.data.keyword.openwhisk_short}} tras esta fecha, utilice el [plugin de CLI de {{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) para la CLI de {{site.data.keyword.Bluemix}}.**

La interfaz de línea de mandatos **wsk** distribuida de {{site.data.keyword.openwhisk}} permite la gestión de todas las entidades de {{site.data.keyword.openwhisk_short}}. 
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* Configuración de la clave de autenticación de {{site.data.keyword.openwhisk_short}}
  * No es necesario acceder a la consola de {{site.data.keyword.openwhisk_short}} para obtener la clave de autenticación
  * Cuando se cambia de región, organización y espacio, las claves de autenticación se generan automáticamente
* Renovación de una clave de autenticación de {{site.data.keyword.openwhisk_short}} caducada
* Actualización de la CLI a una versión posterior


Para las siguientes tareas de gestión, es necesario que utilice el plugin.

* Gestión de API
  * Configuración de la señal de acceso de API GW
  * Renovación de una señal de acceso caducada de API GW

## Configuración de la CLI de {{site.data.keyword.openwhisk_short}} 
{: #openwhisk_cli_setup}

Puede utilizar la interfaz de línea de mandatos (CLI) de {{site.data.keyword.openwhisk_short}} para configurar su espacio de nombres y clave de autorización.
Acceda a [Configurar CLI](https://console.bluemix.net/openwhisk/cli?loadWsk=true) y
siga las instrucciones para instalarlo.

Primero, configure las dos propiedades siguientes:

1. **Host de API** (nombre o dirección IP) del despliegue de {{site.data.keyword.openwhisk_short}} que desea utilizar.
2. **Clave de autorización** (nombre de usuario y contraseña) que le garantiza acceso a la API de {{site.data.keyword.openwhisk_short}}.

Hay dos regiones de {{site.data.keyword.Bluemix_notm}} disponibles que requieren su propio host de API exclusivo y una clave de autorización.

* EE.UU. Sur
  * Host de API: `openwhisk.ng.bluemix.net`

* Reino Unido
  * Host de API: `openwhisk.eu-gb.bluemix.net`

Ejecute el siguiente mandato para definir el host de API para la región de {{site.data.keyword.Bluemix_notm}} deseada:

EE. UU. Sur:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Reino Unido:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Si necesita cambiar de región, debe volver a configurar la CLI con el host de API y la clave de autorización, ya que la clave de autorización es específica por región.
{: tip}

Los artefactos (por ejemplo, acciones, reglas, paquetes) son también específicos de la región. Por lo tanto, si utiliza el mismo artefacto en varias regiones, debe desplegarlo en cada región deseada.

Si conoce su clave de autorización, puede configurar la CLI para que la utilice. 

Ejecute el siguiente mandato para definir la clave de autorización:

```
wsk property set --auth <authorization_key>
```
{: pre}

**Consejo:** la CLI de {{site.data.keyword.openwhisk_short}} guarda las propiedades definidas en `~/.wskprops` de forma predeterminada. La ubicación de este archivo se puede modificar mediante la variable de entorno `WSK_CONFIG_FILE`. 

Para comprobar la configuración de la CLI, intente [crear y ejecutar una acción](./index.html#openwhisk_start_hello_world).

## Uso de la CLI de {{site.data.keyword.openwhisk_short}}
{: #using_openwhisk_cli}

Una vez que el entorno esté configurado, puede utilizar la CLI de {{site.data.keyword.openwhisk_short}} para realizar las tareas siguientes:

* Ejecutar sus fragmentos de código, o acciones, en {{site.data.keyword.openwhisk_short}}. Consulte [Crear e invocar acciones](./openwhisk_actions.html).
* Usar desencadenantes y reglas para permitir a sus acciones responder ante sucesos. Consulte
[Creación de desencadenantes y reglas](./openwhisk_triggers_rules.html).
* Aprender cómo los paquetes agrupan acciones y configurar orígenes de sucesos externos. Consulte [Uso y creación de paquetes](./openwhisk_packages.html).
* Explorar el catálogo de paquetes y mejorar sus aplicaciones con servicios externos como un [origen de sucesos de Cloudant](./openwhisk_cloudant.html). Consulte [Paquetes de {{site.data.keyword.openwhisk_short}} preinstalados](./openwhisk_catalog.html).

## Configure la CLI para utilizar un proxy HTTPS
{: #cli_https_proxy}

La CLI se puede configurar para utilizar un proxy HTTPS. Para configurar un proxy HTTPS, se debe crear una variable de entorno llamada `HTTPS_PROXY`. La variable se debe establecer en la dirección del proxy HTTPS y el puerto correspondiente utilizando el siguiente formato:
`{PROXY IP}:{PROXY PORT}`.
