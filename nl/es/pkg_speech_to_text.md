---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

keywords: speech to text, watson, package, cognitive,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}

# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## Paquetes

| Nombre | Disponibilidad | Descripción |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | Instalable | Funciona con el servicio {{site.data.keyword.speechtotextshort}} V1. |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | Preinstalado (No disponible en Tokio) | Llama a las API de Watson para convertir el discurso en texto |


## Servicio {{site.data.keyword.speechtotextshort}}
{: #speech_to_text}

El servicio instalable {{site.data.keyword.speechtotextfull}} proporciona una [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) que utiliza las funcionalidades de reconocimiento de habla de IBM para crear transcripciones de audio hablado.
{:shortdesc}

El servicio puede transcribir el habla de diversos idiomas y formatos de audio. Además de una transcripción básica, el servicio puede producir información detallada sobre muchos aspectos del audio. Para la mayoría de los idiomas, el servicio da soporte a dos tasas de muestreo, banda ancha y banda estrecha. Devuelve todo el contenido de la respuesta JSON en el conjunto de caracteres UTF-8. Para obtener más información sobre el servicio, consulte la [documentación de IBM&reg; Cloud](/docs/services/speech-to-text?topic=speech-to-text-about).

El paquete {{site.data.keyword.speechtotextshort}} contiene las siguientes entidades. Puede encontrar más información en la referencia de {{site.data.keyword.speechtotextshort}} API pulsando en el nombre de entidad.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html) | paquete | username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabajar con el servicio {{site.data.keyword.speechtotextshort}} V1. |
| [get-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    model_id,  | Obtener un modelo. |
| [list-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Listar modelos. |
| [recognize-sessionless](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    audio,     content_type,     model,     customization_id,     acoustic_customization_id,     base_model_version,     customization_weight,     inactivity_timeout,     keywords,     keywords_threshold,     max_alternatives,     word_alternatives_threshold,     word_confidence,     timestamps,     profanity_filter,     smart_formatting,     speaker_labels,  | Reconocer audio (sin sesión). |
| [check-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    id,  | Comprobar un trabajo. |
| [check-jobs](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url, | Comprobar trabajos. |
| [create-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    audio,     content_type,     model,     callback_url,     events,     user_token,     results_ttl,     customization_id,     acoustic_customization_id,     base_model_version,     customization_weight,     inactivity_timeout,     keywords,     keywords_threshold,     max_alternatives,     word_alternatives_threshold,     word_confidence,     timestamps,     profanity_filter,     smart_formatting,     speaker_labels,  | Crear un trabajo. |
| [delete-job](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    id,  | Suprimir un trabajo. |
| [register-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    callback_url,     user_secret,  | Registrar una devolución de llamada. |
| [unregister-callback](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    callback_url,  | Desregistrar una devolución de llamada. |
| [create-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, base_model_name, dialect, description,  | Crear un modelo de idioma personalizado. |
| [delete-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Suprimir un modelo de idioma personalizado. |
| [get-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Obtener un modelo de idioma personalizado. |
| [list-language-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Listar modelos de idioma personalizados. |
| [reset-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Restablecer un modelo de idioma personalizado. |
| [train-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_type_to_add,     customization_weight,  | Entrenar un modelo de idioma personalizado. |
| [upgrade-language-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Actualizar un modelo de idioma personalizado. |
| [add-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,     corpus_file,     allow_overwrite,  | Añadir un corpus. |
| [delete-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,  | Suprimir un corpus. |
| [get-corpus](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     corpus_name,  | Obtener un corpus. |
| [list-corpora](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Listar corpus. |
| [add-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,    word, sounds_like, display_as,  | Añadir una palabra personalizada. |
| [add-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    words,  | Añadir palabras personalizadas. |
| [delete-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,  | Suprimir una palabra personalizada. |
| [get-word](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_name,  | Obtener una palabra personalizada. |
| [list-words](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word_type,     sort,  | Listar palabras personalizadas. |
| [create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, base_model_name, description,  | Crear un modelo acústico personalizado. |
| [delete-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Suprimir un modelo acústico personalizado. |
| [get-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Obtener un modelo acústico personalizado. |
| [list-acoustic-models](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    language,  | Listar modelos acústicos personalizados. |
| [reset-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Restablecer un modelo acústico personalizado. |
| [train-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     custom_language_model_id,  | Entrenar un modelo acústico personalizado. |
| [upgrade-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     custom_language_model_id,  | Actualizar un modelo acústico personalizado. |
| [add-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,     audio_resource,     content_type,     contained_content_type,     allow_overwrite,  | Añadir un recurso de audio. |
| [delete-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,  | Suprimir un recurso de audio. |
| [get-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     audio_name,  | Obtener un recurso de audio. |
| [list-audio](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,  | Listar recursos de audio. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data) | acción |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customer_id,  | Suprimir datos etiquetados. |

### Creación de una instancia de servicio de {{site.data.keyword.speechtotextshort}}
{: #service_instance_speechtotext}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.speechtotextshort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.speechtotextshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/speech_to_text).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

### Instalación del paquete {{site.data.keyword.speechtotextshort}}
{: #install_speechtotext}

Una vez tenga una instancia de servicio de {{site.data.keyword.speechtotextshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.speechtotextshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #speechtotext_cli}

Antes de empezar:
  1. [Instale el plugin de {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar el paquete {{site.data.keyword.speechtotextshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.speechtotextshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. Enlace a las credenciales de la instancia de {{site.data.keyword.speechtotextshort}} que creó para el paquete.
    ```
    ibmcloud fn service bind speech_to_text speech-to-text-v1
    ```
    {: pre}

    Dependiendo de la región en la que haya creado la instancia de servicio, es posible que la instancia de servicio tenga un nombre distinto porque es un servicio de IAM. Si el mandato anterior falla, utilice el nombre de servicio siguiente para el mandato bind:
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    Salida de ejemplo:
    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. Verifique que el paquete esté configurado con sus credenciales de la instancia de servicio de {{site.data.keyword.speechtotextshort}}.
    ```
    ibmcloud fn package get speech-to-text-v1 parameters
    ```
    {: pre}

    Salida de ejemplo:
    ```
    ok: got package speech-to-text-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalación desde la interfaz de usuario de {{site.data.keyword.openwhisk_short}}
{: #speechtotext_ui}

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desee instalar el paquete. 

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de paquetes de **Watson**.

5. Pulse sobre el paquete **Speech To Text**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones donde podrá buscar su nuevo paquete, que se denomina **speech-to-text-v1**.

7. Para utilizar las acciones del paquete **speech-to-text-v1**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba.
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse sobre una acción del paquete **speech-to-text-v1** que desee utilizar. Se abrirá la página de detalles de dicha acción.
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**.
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

### Utilización del paquete {{site.data.keyword.speechtotextshort}}
{: #usage_speechtotext}

Para utilizar las acciones de este paquete, ejecute los mandatos en el formato siguiente:

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Pruebe la acción `list-models`.
```
ibmcloud fn action invoke speech-to-text-v1/list-models -b
```
{: pre}

## Watson: Speech to Text
{: #preinstall_speechtotext}

Este paquete preinstalado no está disponible en la región de Tokio. Consulte el paquete [Speech to Text](#install_speechtotext) instalable que utiliza autenticación de IAM.
{: tip}

El paquete `/whisk.system/watson-speechToText` ofrece una forma cómoda de invocar API Watson para convertir la voz a texto.
{: shortdesc}

El paquete incluye las acciones siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | paquete | usuario, contraseña | Paquete para convertir habla en texto |
| `/whisk.system/watson-speechToText/speechToText` | acción | payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out | Convertir audio en texto |

**Nota**: el paquete `/whisk.system/watson` está en desuso, incluida la acción `/whisk.system/watson/speechToText`. Consulte el [paquete {{site.data.keyword.speechtotextshort}}](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud) en su lugar.

### Configuración del paquete Watson Speech to Text en {{site.data.keyword.Bluemix_notm}}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, se crean automáticamente los enlaces de paquete para sus instancias de servicio de {{site.data.keyword.Bluemix_notm}} Watson.

1. Cree una instancia de servicio de Watson Speech to Text en el [panel de control](http://cloud.ibm.com) de {{site.data.keyword.Bluemix_notm}}.

  Asegúrese de recordar el nombre de la instancia de servicio y la organización y el espacio de
{{site.data.keyword.Bluemix_notm}} en el que se encuentra.

2. Actualice los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para la instancia de servicio de Watson que ha creado.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Salida de ejemplo:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  Liste los paquetes para ver que se ha creado el enlace:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### Configuración de un paquete Watson Speech to Text fuera de {{site.data.keyword.Bluemix_notm}}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}} o si quiere configurar Watson Speech to Text fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para el servicio de Watson Speech to Text. Necesita el nombre de usuario del servicio de Watson Speech to Text y la contraseña.

Cree un enlace de paquete configurado para el servicio de Watson Speech to Text.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### Conversión de habla a texto

La acción `/whisk.system/watson-speechToText/speechToText` convierte el audio en texto. Los parámetros son según se indica a continuación:

- `username`: el nombre de usuario de la API de Watson.
- `password`: contraseña de la API de Watson.
- `payload`: datos binarios codificados del habla para convertir en texto.
- `content_type`: tipo MIME del audio.
- `encoding`: codificación de los datos binarios del habla.
- `continuous`: indica si se devuelven varios resultados finales que representan frases consecutivas separadas por pausas prolongadas.
- `inactivity_timeout`: tiempo, en segundos, después del cual, si solo se detecta silencio en el audio enviado, la conexión se cierra.
- `interim_results`: indica si el servicio debe devolver resultados temporales.
- `keywords`: lista de palabras clave para detectar en el audio.
- `keywords_threshold`: valor de confianza que se encuentra en el límite inferior para detectar una palabra clave.
- `max_alternatives`: número máximo de transcripciones alternativas que deben devolverse.
- `model`: identificador del modelo que debe utilizarse para la solicitud de reconocimiento.
- `timestamps`: indica si se devuelve la alineación de tiempo para cada palabra.
- `watson-token`: proporciona un elemento de autenticación para el servicio como alternativa para proporcionar credenciales del servicio.
- `word_alternatives_threshold`: valor de confianza que se encuentra en el límite inferior para identificar una hipótesis como posible alternativa de palabra.
- `word_confidence`: indica si se devuelve para cada palabra una medida de confianza entre 0 y 1.
- `X-Watson-Learning-Opt-Out`: indica si se debe renunciar a la recopilación de datos para la llamada.

Invoque la acción **speechToText** en el enlace de paquete para convertir el audio codificado.
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

Salida de ejemplo:
```
{
  "data": "Hello Watson"
}
```
{: screen}

