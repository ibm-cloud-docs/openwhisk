---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Paquete {{site.data.keyword.nlushort}}

El servicio {{site.data.keyword.nlufull}} sirve para analizar varias características del contenido de texto de acuerdo con su tamaño.
{: shortdesc}

Proporcione un texto, HTML sin procesar o un URL y {{site.data.keyword.nlushort}} ofrecerá resultados de las características que solicite. De forma predeterminada el servicio borra el contenido HTML antes del análisis, de modo que los resultados podrán ignorar la mayoría de los anuncios y otros contenidos no deseados. Puede crear <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">modelos personalizados </a> con Watson Knowledge Studio que se pueden utilizar para detectar entidades y relaciones personalizadas en Natural Language Understanding.

El paquete {{site.data.keyword.nlushort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.nlushort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  |  Trabajar con el servicio {{site.data.keyword.nlushort}}. |
| [analyze](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   features, text, html, url, clean, xpath, fallback_to_raw, return_analyzed_text, language, limit_text_characters,  | Analizar texto, HTML o una página web pública |
| [delete-model](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Suprimir un modelo |
| [list-models](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Listar modelos |

## Creación de una instancia de servicio de {{site.data.keyword.nlushort}}
{: #service_instance}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.nlushort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.nlushort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/natural-language-understanding).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.nlushort}}
{: #install}

Una vez tenga una instancia de servicio de {{site.data.keyword.nlushort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.nlushort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #nlus_cli}

Antes de empezar:
  1. [Instale el plugin {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Instale el [mandato `wskdeploy` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) y añada el binario descargado a su PATH.

Para instalar el paquete {{site.data.keyword.nlushort}}:

1. Clone el repositorio del paquete {{site.data.keyword.nlushort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    wskdeploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
    ```
    {: pre}

3. Verifique que el paquete se ha añadido a la lista de paquetes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Salida:
    ```
    packages
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.nlushort}} que creó para el paquete.
    ```
    ibmcloud fn service bind natural-language-understanding natural-language-understanding-v1
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.nlushort}}.
    ```
    ibmcloud fn package get natural-language-understanding-v1 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "natural-language-understanding": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #nlus_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desea instalar el paquete {{site.data.keyword.cos_short}}. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de Paquetes de **Watson**.

5. Pulse el paquete **Natural Language Understanding**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones y puede buscar el nuevo paquete, que se denomina ** natural-language-entendimiento-v1 **.

7. Para utilizar las acciones en el paquete **natural-language-understanding-v1**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba. 
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse en una Acción del paquete **natural-language-understanding-v1** que desee utilizar. Se abre la página de detalles de dicha acción. 
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**. 
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.nlushort}}
{: #usage}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones necesitarán un parámetro de versión en el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning).

Las funciones de este paquete utilizan la versión actual de Natural Language Understanding, 2018-03-16. Pruebe la acción `list-models`.
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}
