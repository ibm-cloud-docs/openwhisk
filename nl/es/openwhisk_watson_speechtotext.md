---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Paquete Watson: Speech to Text
{: #openwhisk_catalog_watson_texttospeech}

El paquete `/whisk.system/watson-speechToText` ofrece una forma cómoda de invocar API Watson para convertir la voz a texto.
{: shortdesc}

El paquete incluye las acciones siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | paquete | usuario, contraseña | Paquete para convertir habla en texto |
| `/whisk.system/watson-speechToText/speechToText` | acción | payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out | Convertir audio en texto |

**Nota**: el paquete `/whisk.system/watson` está en desuso, incluida la acción `/whisk.system/watson/speechToText`.

## Configuración del paquete Watson Speech to Text en {{site.data.keyword.Bluemix_notm}}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, se crean automáticamente los enlaces de paquete para sus instancias de servicio de {{site.data.keyword.Bluemix_notm}} Watson.

1. Cree una instancia de servicio de Watson Speech to Text en el [panel de control](http://console.ng.Bluemix.net) de {{site.data.keyword.Bluemix_notm}}.

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

## Configuración de un paquete Watson Speech to Text fuera de {{site.data.keyword.Bluemix_notm}}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}} o si quiere configurar Watson Speech to Text fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para el servicio de Watson Speech to Text. Necesita el nombre de usuario del servicio de Watson Speech to Text y la contraseña.

Cree un enlace de paquete configurado para el servicio de Watson Speech to Text.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Conversión de habla a texto

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
