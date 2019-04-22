---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: watson, translator, cognitive, translating text, language

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Paquete Watson: Translator
{: #openwhisk_catalog_watson_translator}

Este paquete preinstalado ya no está disponible en ninguna región. Consulte el paquete [Language Translator](/docs/openwhisk?topic=cloud-functions-language-translator-package) instalable que utiliza la nueva API V3 con autenticación de IAM. 
{: tip}

El paquete `/whisk.system/watson-translator` ofrece una forma cómoda de invocar API Watson a convertir.
{: shortdesc}

El paquete incluye las acciones siguientes.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | paquete | usuario, contraseña | Paquete para la traducción de texto e identificación de idioma  |
| `/whisk.system/watson-translator/translator` | acción | payload, translateFrom, translateTo, translateParam, username, password | Traducir texto |
| `/whisk.system/watson-translator/languageId` | acción | payload, username, password | Identificar idioma |

**Nota**: el paquete `/whisk.system/watson` está en desuso, incluidas las acciones `/whisk.system/watson/translate` y `/whisk.system/watson/languageId`. Consulte el [paquete {{site.data.keyword.languagetranslatorshort}} instalable](/docs/openwhisk?topic=cloud-functions-language-translator-package) en su lugar.

## Configuración del paquete de Watson Translator en {{site.data.keyword.Bluemix_notm}}

Si utiliza {{site.data.keyword.openwhisk}} desde {{site.data.keyword.Bluemix_notm}}, se crean automáticamente los enlaces de paquete para sus instancias de servicio de {{site.data.keyword.Bluemix_notm}} Watson.

1. Cree una instancia de servicio de Watson Translator en su [panel de control](http://cloud.ibm.com) de {{site.data.keyword.Bluemix_notm}}. Asegúrese de recordar el nombre de la instancia de servicio y la organización y el espacio de
{{site.data.keyword.Bluemix_notm}} en el que se encuentra.

2. Actualice los paquetes de su espacio de nombres. La renovación crea automáticamente un enlace de paquete para la instancia de servicio de Watson que ha creado.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Salida de ejemplo:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
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
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## Configuración de un paquete de Watson Translator fuera de {{site.data.keyword.Bluemix_notm}}

Si no utiliza {{site.data.keyword.openwhisk_short}} en {{site.data.keyword.Bluemix_notm}} o si quiere configurar Watson Translator fuera de {{site.data.keyword.Bluemix_notm}}, debe crear manualmente un enlace de paquete para el servicio de Watson Translator. Necesita el nombre de usuario del servicio de Watson Translator y la contraseña.

- Cree un enlace de paquete configurado para el servicio de Watson Translator.
  ```
  ibmcloud fn package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## Traducción de texto

La acción `/whisk.system/watson-translator/translator` traduce texto de un idioma a otro. Los parámetros son según se indica a continuación:

- `username`: el nombre de usuario de la API de Watson.
- `password`: contraseña de la API de Watson.
- `payload`: el texto que se debe traducir.
- `translateParam`: el parámetro de entrada que indica el texto a traducir. Por ejemplo, si es `translateParam=payload`, el valor del parámetro `payload` que se pasa a la acción, se traduce.
- `translateFrom`: un código de dos dígitos del lenguaje origen.
- `translateTo`: un código de dos dígitos del idioma de destino.

Invoque la acción **translator** en su enlace de paquete para traducir algún texto de inglés a español.
```
ibmcloud fn action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

Salida de ejemplo:
```
{
    "payload": "Ciel bleu a venir"
}
```
{: screen}

## Identificación del idioma de un texto

La acción `/whisk.system/watson-translator/languageId` identifica el idioma de algún texto. Los parámetros son según se indica a continuación:

- `username`: el nombre de usuario de la API de Watson.
- `password`: contraseña de la API de Watson.
- `payload`: el texto a identificar.

Invocar la acción **languageId** en su enlace de paquete para identificar el idioma.
```
ibmcloud fn action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

Salida de ejemplo:
```
{
  "payload": "Ciel bleu a venir",
  "language": "fr",
  "confidence": 0.710906
}
```
{: screen}
