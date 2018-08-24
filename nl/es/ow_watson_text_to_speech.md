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

# Paquete {{site.data.keyword.texttospeechshort}}

El servicio {{site.data.keyword.texttospeechfull}} proporciona API que utilizan las funcionalidades de síntesis de voz de IBM para sintetizar texto en un habla natural en distintos idiomas, dialectos y voces.
{:shortdesc}

El servicio da soporte a una voz masculina o a una voz femenina como mínimo, a veces a ambas, para cada idioma. El audio se transmite de vuelta al cliente con un retardo mínimo. Para obtener más información sobre el servicio, consulte la [documentación de IBM Cloud](https://console.bluemix.net/docs/services/text-to-speech/index.html).

El paquete {{site.data.keyword.texttospeechshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.texttospeechshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.texttospeechshort}}. |
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    voice,     customization_id,  | Obtener una voz. |
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Listar voces. |
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text,     accept,     voice,     customization_id,  | Sintetizar audio. |
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,     voice,     format,     customization_id,  | Obtener pronunciación. |
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, language, description,  | Crear un modelo personalizado. |
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Suprimir un modelo personalizado. |
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Obtener un modelo personalizado. |
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Listar modelos personalizados. |
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    name, description, words,  | Actualizar un modelo personalizado. |
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,    translation, part_of_speech,  | Añadir una palabra personalizada. |
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  | Añadir palabras personalizadas. |
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | Suprimir una palabra personalizada. |
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,  | Obtener una palabra personalizada. |
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Listar palabras personalizadas. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Suprimir datos etiquetados. |

## Creación de una instancia de servicio de {{site.data.keyword.texttospeechshort}}
{: #service_instance}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.texttospeechshort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.texttospeechshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/text_to_speech).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.texttospeechshort}}
{: #install}

Una vez tenga una instancia de servicio de {{site.data.keyword.texttospeechshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.texttospeechshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

Antes de empezar:
  1. [Instale el plugin {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Instale el [mandato `wskdeploy` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) y añada el binario descargado a su PATH.

Para instalar el paquete {{site.data.keyword.texttospeechshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.texttospeechshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    wskdeploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.texttospeechshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind text_to_speech text-to-speech-v1
    ```
    {: pre}

    Dependiendo de la región en la que haya creado la instancia de servicio, es posible que la instancia de servicio tenga un nombre distinto porque es un servicio de IAM. Si el mandato anterior falla, utilice el nombre de servicio siguiente para el mandato bind:
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "text_to_speech": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #texttospeech_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desea instalar el paquete {{site.data.keyword.cos_short}}. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de Paquetes de **Watson**.

5. Pulse en el paquete **Text To Speech**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones y puede buscar el nuevo paquete, que se denomina **text-to-speech-v1**.

7. Para utilizar las acciones en el paquete **text-to-speech-v1**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba. 
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse en una Acción del paquete **text-to-speech-v1** que desee utilizar. Se abre la página de detalles de dicha acción. 
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**. 
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.texttospeechshort}}
{: #usage}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Pruebe la acción `list-voices`.
```
ibmcloud fn action invoke text-to-speech-v1/list-voices -b
```
{: pre}
