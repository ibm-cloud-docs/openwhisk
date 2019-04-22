---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: personality insights, watson, cognitive, serverless, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Paquete {{site.data.keyword.personalityinsightsshort}}

El servicio {{site.data.keyword.personalityinsightsfull}} permite a las aplicaciones obtener información de los medios sociales, los datos empresariales u otras comunicaciones digitales. El servicio utiliza análisis lingüístico para inferir las características de personalidad intrínsecas de las personas, incluidas las del Modelo de los cinco grandes de psicología, necesidades y valores a partir de comunicaciones digitales como, por ejemplo, correos electrónicos, mensajes de texto, tweets y publicaciones en foros.
{: shortdesc}

El servicio puede inferir automáticamente, desde redes sociales potencialmente ruidosas, retratos de individuos que reflejan sus características de personalidad. El servicio puede inferir las preferencias de consumo con base a los resultados de sus análisis y, a partir de contenido JSON que se marca temporalmente, informar de su comportamiento a lo largo del tiempo.
* Para obtener información sobre el significado de los modelos que utiliza el servicio y para describir las características de personalidad, consulte [Modelos de personalidad](https://cloud.ibm.com/docs/services/personality-insights/models.html).
* Para obtener información sobre el significado de las preferencias de consumo, consulte [Preferencias de consumo](https://cloud.ibm.com/docs/services/personality-insights/preferences.html).

**Nota:** La solicitud de creación de registros está inhabilitada para el servicio {{site.data.keyword.personalityinsightsshort}}. El servicio ni registra ni conserva datos de las solicitudes y respuestas, independientemente de como se configura la cabecera de solicitud `X-Watson-Learning-Opt-Out`.

El paquete {{site.data.keyword.personalityinsightsshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.personalityinsightsshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.personalityinsightsshort}} V3. |
| [profile](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Obtener un perfil. |
| [profile-as-csv](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    content,     content_type,     content_language,     accept_language,     raw_scores,     csv_headers,     consumption_preferences,  | Obtener un perfil como un archivo CSV. |

## Creación de una instancia de servicio de {{site.data.keyword.personalityinsightsshort}}
{: #service_instance_insights}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.personalityinsightsshort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.personalityinsightsshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/personality_insights).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.personalityinsightsshort}}
{: #install_insights}

Una vez tenga una instancia de servicio de {{site.data.keyword.personalityinsightsshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.personalityinsightsshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar el paquete {{site.data.keyword.personalityinsightsshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.personalityinsightsshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/personality-insights-v3/manifest.yaml
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
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.personalityinsightsshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind personality_insights personality-insights-v3
    ```
    {: pre}

    Dependiendo de la región en la que haya creado la instancia de servicio, es posible que la instancia de servicio tenga un nombre distinto porque es un servicio de IAM. Si el mandato anterior falla, utilice el nombre de servicio siguiente para el mandato bind:
    ```
    ibmcloud fn service bind personality-insights personality-insights-v3
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.personalityinsightsshort}}.
    ```
    ibmcloud fn package get personality-insights-v3 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desee instalar el paquete. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes de **Watson**.

5. Pulse el paquete **Personality Insights**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **personality-insights-v3**.

7. Para utilizar las acciones del paquete **personality-insights-v3**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **personality-insights-v3** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.personalityinsightsshort}}
{: #usage_insights}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas las acciones necesitarán un parámetro de versión en el formato AAAA-MM-DD. Cuando la API se cambie de forma que no se compatible con versiones anteriores, se ofrecerá una nueva fecha de versión. Para obtener más información, consulte la [Referencia de API](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning).

Las funciones de este paquete utilizan la versión actual de Personality Insights, 2017-10-13. Pruebe la acción `profile`.
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "Puede
intentar escribir un resumen sobre sí mismo, pero debe tener al menos 100 palabras. Este resumen
puede ser cualquier texto y probablemente el servicio de información sobre la personalidad probablemente
no devolverá nada interesante. El servicio utiliza análisis lingüístico para inferir las características de personalidad intrínsecas de las personas, incluidas las del Modelo de los cinco grandes de psicología, necesidades y valores a partir de comunicaciones digitales como, por ejemplo, correos electrónicos, mensajes de texto, tweets y publicaciones en foros. El servicio puede inferir automáticamente, desde redes sociales potencialmente ruidosas, retratos de individuos que reflejan sus características de personalidad. El servicio puede inferir las preferencias de consumo con base a los resultados de sus análisis y, a partir de contenido JSON que se marca temporalmente, informar de su comportamiento a lo largo del tiempo."
```
{: pre}
