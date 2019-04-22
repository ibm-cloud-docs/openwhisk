---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: text to speech, watson, cognitive, converting, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
# Paquete Watson: Text to Speech
{: #openwhisk_catalog_watson_texttospeech}

Este paquete preinstalado no está disponible en la región de Tokio. Consulte el paquete [Text to Speech](/docs/openwhisk?topic=cloud-functions-text-to-speech-package) instalable que utiliza autenticación de IAM. 
{: tip}

El paquete `/whisk.system/watson-textToSpeech` ofrece una forma cómoda de invocar API Watson para convertir el texto a voz.
{: shortdesc}

El paquete incluye las acciones siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | paquete | usuario, contraseña | Paquete para convertir texto en habla |
| `/whisk.system/watson-textToSpeech/textToSpeech` | acción | payload, voice, accept, encoding, username, password | Convertir texto en audio |

**Nota**: el paquete `/whisk.system/watson` está en desuso, incluida la acción `/whisk.system/watson/textToSpeech`. Consulte el [paquete {{site.data.keyword.texttospeechshort}} instalable](/docs/openwhisk?topic=cloud-functions-text-to-speech-package) en su lugar.

## Configuración del paquete Watson Text to Speech en {{site.data.keyword.Bluemix_notm}}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, se crean automáticamente los enlaces de paquete para sus instancias de servicio de {{site.data.keyword.Bluemix_notm}} Watson.

1. Cree una instancia de servicio de Watson Text to Speech en el [panel de control](http://cloud.ibm.com) de {{site.data.keyword.Bluemix_notm}}.

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
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Liste los paquetes para ver que se ha creado el enlace de paquete:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Salida de ejemplo:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## Configuración de un paquete Watson Text to Speech fuera de {{site.data.keyword.Bluemix_notm}}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}} o si quiere configurar Watson Text to Speech fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para el servicio de Watson Text to Speech. Necesita el nombre de usuario del servicio de Watson Text to Speech y la contraseña.

Cree un enlace de paquete configurado para el servicio de Watson Speech to Text.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Conversión de texto a habla

La acción `/whisk.system/watson-textToSpeech/textToSpeech` convierte texto en un texto hablado. Los parámetros son según se indica a continuación:

- `username`: el nombre de usuario de la API de Watson.
- `password`: contraseña de la API de Watson.
- `payload`: texto que se debe convertir en habla.
- `voice`: voz de la persona que habla.
- `accept`: formato del archivo de audio.
- `encoding`: codificación de los datos binarios del habla.

Invoque la acción **textToSpeech** en el enlace del paquete para convertir el texto.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

Salida de ejemplo:
```
{
  "payload": "<base64 encoding of a .wav file>"
}
```
{: screen}
