---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: tone analyzer, functions, serverless, watson

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Paquete {{site.data.keyword.toneanalyzershort}}

El servicio {{site.data.keyword.toneanalyzerfull}} utiliza el análisis lingüístico para detectar tonos emocionales y de lenguaje en texto escrito.
{:shortdesc}

El servicio puede analizar el tono a nivel de documento y de frase. Puede utilizar el servicio para comprender cómo se perciben sus comunicaciones por escrito y mejorar el tono de sus comunicaciones. Las empresas pueden utilizar el servicio para obtener el tono de las comunicaciones de sus clientes y responder a cada cliente adecuadamente, o para comprender y mejorar las conversaciones con los clientes en general.

**Nota:** La solicitud de creación de registros está inhabilitada para el servicio Tone Analyzer. El servicio ni registra ni retiene datos de las solicitudes y respuestas, independientemente de cómo esté configurada la cabecera de solicitud `X-Watson-Learning-Opt-Out`.

El paquete {{site.data.keyword.toneanalyzershort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.toneanalyzershort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.toneanalyzershort}}. |
| [tone](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    tone_input,     content_type,     sentences,     tones,     content_language,     accept_language,  | Analizar el tono general. |
| [tone-chat](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   utterances,     content_language,     accept_language,  | Analizar el tono de fidelización del cliente. |

## Creación de una instancia de servicio de {{site.data.keyword.toneanalyzershort}}
{: #service_instance_tone}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.toneanalyzershort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/tone_analyzer).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.toneanalyzershort}}
{: #install_tone}

Una vez tenga una instancia de servicio de {{site.data.keyword.toneanalyzershort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.toneanalyzershort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #toneanalyzer_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar el paquete {{site.data.keyword.toneanalyzershort}}:

1. Clone el repositorio del paquete {{site.data.keyword.toneanalyzershort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.toneanalyzershort}} que creó para el paquete.
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    Dependiendo de la región en la que haya creado la instancia de servicio, es posible que la instancia de servicio tenga un nombre distinto porque es un servicio de IAM. Si el mandato anterior falla, utilice el nombre de servicio siguiente para el mandato bind:
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}
    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.toneanalyzershort}}.
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #toneanalyzer_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a la [página Crear ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desee instalar el paquete. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes de **Watson**.

5. Pulse el paquete **Tone Analyzer**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **tone-analyzer-v3**.

7. Para utilizar las acciones del paquete **tone-analyzer-v3**, debe enlazar las credenciales de servicio a la acción.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **tone-analyzer-v3** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.toneanalyzershort}}
{: #usage_tone}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones necesitarán un parámetro de versión en el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning).

Las funciones de este paquete utilizan la versión actual de Tone Analyzer, 2017-09-21. Pruebe la acción `tone`.
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}
