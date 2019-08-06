---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, functions, packages

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


# Text to Speech
{: #pkg_text_to_speech}

## Opciones de paquete

| Paquete | Disponibilidad | Descripción |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | Preinstalado (No disponible en Tokio) | Paquete para convertir texto en habla |
| [`text-to-speech-v1`](#text_to_speech_ins) | Instalable | Trabajar con el servicio {{site.data.keyword.texttospeechshort}}. |

## Watson Text to Speech
{: #text_to_speech}

Este paquete preinstalado no está disponible en la región de Tokio. Consulte el paquete instalable [Text to Speech](#text_to_speech_ins) utilizando la autenticación IAM.
{: tip}

El paquete `/whisk.system/watson-textToSpeech` ofrece una forma cómoda de invocar API Watson para convertir el texto a voz.
{: shortdesc}

El paquete incluye las acciones siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Paquete | `username`, `password` | Paquete para convertir texto en habla. |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Acción | `payload`, `voice`, `accept`, `encoding`, `username`, `password` | Convertir texto en audio. |

El paquete `/whisk.system/watson` está en desuso, incluida la acción `/whisk.system/watson/textToSpeech`. Consulte el [paquete {{site.data.keyword.texttospeechshort}} instalable](#text_to_speech_ins) en su lugar.
{: deprecated}

### Configuración del paquete de {{site.data.keyword.texttospeechshort}} en {{site.data.keyword.cloud_notm}}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.cloud_notm}}, se crean automáticamente los enlaces de paquete para sus instancias de servicio de {{site.data.keyword.cloud_notm}} Watson.

1. Cree una instancia de servicio de {{site.data.keyword.texttospeechshort}} en su panel de control de {{site.data.keyword.cloud_notm}} [](https://cloud.ibm.com){: external}. Asegúrese de recordar el nombre de la instancia de servicio y la organización y el espacio de
{{site.data.keyword.cloud_notm}} en el que se encuentra.

2. Actualice los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para la instancia de servicio de Watson que ha creado.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  created bindings:
  Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Liste los paquetes para ver que se ha creado el enlace de paquete.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Resultado de ejemplo**
  ```
  packages
  /myOrg_mySpace/Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### Configuración de un paquete de {{site.data.keyword.texttospeechshort}} fuera de {{site.data.keyword.cloud_notm}}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.cloud_notm}} o si quiere configurar {{site.data.keyword.texttospeechshort}} fuera de {{site.data.keyword.cloud_notm}}, debe crear manualmente un enlace de paquete para el servicio {{site.data.keyword.texttospeechshort}}. Necesita el nombre de usuario del servicio {{site.data.keyword.texttospeechshort}} y la contraseña.

Cree un enlace de paquete configurado para el servicio {{site.data.keyword.texttospeechshort}}.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username <username> -p password <password>
```
{: pre}

### Conversión de texto a habla

La acción `/whisk.system/watson-textToSpeech/textToSpeech` convierte texto en un texto hablado. Se da soporte a los parámetros siguientes.

| `username` | El nombre de usuario de la API de Watson. |
| `password` | La contraseña de la API de Watson. |
| `payload` | El texto que se debe convertir en habla. |
| `voice` | La voz de la persona que habla. |
| `accept` | El formato del archivo de voz. |
| `encoding` | Codificación de los datos binarios del habla. |

Pruebe la acción `textToSpeech` en el enlace del paquete para convertir el texto.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

**Resultado de ejemplo**
```
{
  "payload": "<base64 encoding of a .wav file>"
}
```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

El servicio instalable {{site.data.keyword.texttospeechfull}} proporciona API con funcionalidades de síntesis de voz de IBM para sintetizar texto en un habla natural en diversos idiomas, dialectos y voces.
{:shortdesc}

El servicio da soporte a una voz masculina o a una voz femenina como mínimo, a veces a ambas, para cada idioma. El audio se transmite de vuelta al cliente con un retardo mínimo. Para obtener más información sobre el servicio, consulte la [documentación de IBM Cloud](/docs/services/text-to-speech?topic=text-to-speech-about).

El paquete {{site.data.keyword.texttospeechshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.texttospeechshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html){: external} | Paquete | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Trabajar con el servicio {{site.data.keyword.texttospeechshort}}. |
| [`get-voice`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `voice`, `customization_id` | Obtener una voz. |
| [`list-voices`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listar voces. |
| [`synthesize`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `accept`, `voice`, `customization_id` | Sintetizar audio. |
| [`get-pronunciation`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `voice`, `format`, `customization_id` | Obtener pronunciación. |
| [`create-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `language`, `description` | Crear un modelo personalizado. |
| [`delete-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Suprimir un modelo personalizado. |
| [`get-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Obtener un modelo personalizado. |
| [`list-voice-models`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models){: external} | Acción | `username`,`password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Listar modelos personalizados. |
| [`update-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `name`, `description`, `words` | Actualizar un modelo personalizado. |
| [`add-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word`, `translation`, `part_of_speech` | Añadir una palabra personalizada. |
| [`add-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words`  | Añadir palabras personalizadas. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Suprimir una palabra personalizada. |
| [`get-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Obtener una palabra personalizada. |
| [`list-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listar palabras personalizadas. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data){: external} | Acción | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Suprimir datos etiquetados. |

### Creación de una instancia de servicio de {{site.data.keyword.texttospeechshort}}
{: #service_instance_texttospeech}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.texttospeechshort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.texttospeechshort}}](https://cloud.ibm.com/catalog/services/text_to_speech){: external}.
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

### Instalación del paquete {{site.data.keyword.texttospeechshort}}
{: #install_texttospeech}

Una vez tenga una instancia de servicio de {{site.data.keyword.texttospeechshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.texttospeechshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

**Antes de empezar**
[Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar el paquete de {{site.data.keyword.texttospeechshort}}, ejecute los mandatos siguientes.

1. Clone el repositorio del paquete {{site.data.keyword.texttospeechshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
    ```
    {: pre}

3. Verifique que el paquete se ha añadido a la lista de paquetes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Resultado**
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

    **Resultado de ejemplo**
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-speech-v1 parameters
    ```
    {: pre}

    **Resultado de ejemplo**
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

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a la [página Crear](https://cloud.ibm.com/openwhisk/create){: external}.

2. Seleccione el espacio de nombres en el que desea instalar el paquete utilizando el menú desplegable de espacios de nombres.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes de **Watson**.

5. Pulse sobre el paquete **Text To Speech**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina `text-to-speech-v1`.

7. Para utilizar las acciones del paquete `text-to-speech-v1`, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos [4 y 6 de las instrucciones de la CLI](#texttospeech_cli).
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario.

  Debe completar los pasos siguientes con cada acción que desee utilizar.
  {: note}

    1. Pulse sobre una acción del paquete `text-to-speech-v1` que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo parámetro. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.texttospeechshort}}
{: #usage_texttospeech}

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


