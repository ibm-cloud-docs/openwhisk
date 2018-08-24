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

# Paquete {{site.data.keyword.speechtotextshort}}

El servicio {{site.data.keyword.speechtotextfull}} proporciona API que utilizan las funcionalidades de reconocimiento de habla de IBM para crear transcripciones de audio hablado.
{:shortdesc}

El servicio puede transcribir el habla de diversos idiomas y formatos de audio. Además de una transcripción básica, el servicio puede producir información detallada sobre muchos aspectos del audio. Para la mayoría de los idiomas, el servicio da soporte a dos tasas de muestreo, banda ancha y banda estrecha. Devuelve todo el contenido de la respuesta JSON en el conjunto de caracteres UTF-8. Para obtener más información sobre el servicio, consulte la [documentación de IBM Cloud](https://console.bluemix.net/docs/services/speech-to-text/index.html).

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

## Creación de una instancia de servicio de {{site.data.keyword.speechtotextshort}}
{: #service_instance}

Antes de instalar el paquete, debe crear una instancia de servicio y las credenciales de servicio de {{site.data.keyword.speechtotextshort}}.
{: shortdesc}

1. [Cree una instancia de servicio de {{site.data.keyword.speechtotextshort}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/catalog/services/speech_to_text).
2. Cuando se crea una instancia de servicio, se generan las credenciales de forma automática.

## Instalación del paquete {{site.data.keyword.speechtotextshort}}
{: #install}

Una vez tenga una instancia de servicio de {{site.data.keyword.speechtotextshort}}, utilice la CLI de {{site.data.keyword.openwhisk}} para instalar el paquete {{site.data.keyword.speechtotextshort}} en su espacio de nombres.
{: shortdesc}

### Instalación desde la CLI de {{site.data.keyword.openwhisk_short}}
{: #speechtotext_cli}

Antes de empezar:
  1. [Instale el plugin {{site.data.keyword.openwhisk_short}} para la CLI de {{site.data.keyword.Bluemix_notm}}](bluemix_cli.html#cloudfunctions_cli).
  2. Instale el [mandato `wskdeploy` ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) y añada el binario descargado a su PATH.

Para instalar el paquete {{site.data.keyword.speechtotextshort}}:

1. Clone el repositorio del paquete {{site.data.keyword.speechtotextshort}}.
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Despliegue el paquete.
    ```
    wskdeploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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

1. En la consola de {{site.data.keyword.openwhisk_short}}, vaya a [Crear página ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/openwhisk/create).

2. Con la ayuda de las listas **Cloud Foundry Org** y **Cloud Foundry Space**, seleccione el espacio de nombres en el que desea instalar el paquete {{site.data.keyword.cos_short}}. Los espacios de nombres se forman combinando los nombres de espacios y organizaciones.

3. Pulse **Instalar paquetes**.

4. Pulse el grupo de Paquetes de **Watson**.

5. Pulse el paquete **Speech To Text**.

5. Pulse **Instalar**.

6. Una vez que se haya instalado el paquete, se le redirigirá a la página Acciones y puede buscar el nuevo paquete, que se denomina **speech-to-text-v1**.

7. Para utilizar las acciones en el paquete **speech-to-text-v1**, debe enlazar las credenciales de servicio con las acciones.
  * Para enlazar las credenciales de servicio con todas las acciones del paquete, siga los pasos 5 y 6 en las instrucciones de la CLI listadas más arriba. 
  * Para enlazar las credenciales de servicio con acciones individuales, realice los pasos siguientes en la interfaz de usuario. **Nota**: Debe completar los pasos siguientes con cada acción que desee utilizar.
    1. Pulse en una Acción del paquete **speech-to-text-v1** que desee utilizar. Se abre la página de detalles de dicha acción. 
    2. En la navegación del lado izquierdo, pulse en la sección **Parámetros**. 
    3. Especifique un nuevo **parámetro**. Para la clave, especifique `__bx_creds`. Para el valor, pegue en el objeto JSON de credenciales de servicio de la instancia de servicio que ha creado anteriormente.

## Utilización del paquete {{site.data.keyword.speechtotextshort}}
{: #usage}

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
