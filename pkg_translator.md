---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-08"

keywords: watson, translator, cognitive, translating text, language

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Translator
{: #openwhisk_catalog_watson_translator}

The `/whisk.system/watson-translator` package offers a convenient way to call Watson APIs to translate.
{: shortdesc}
{: #language-translator-package}

The package includes the following actions.

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | package | username, password | Package for text translation and language identification  |
| `/whisk.system/watson-translator/translator` | action | payload, translateFrom, translateTo, translateParam, username, password | Translate text |
| `/whisk.system/watson-translator/languageId` | action | payload, username, password | Identify language |



## Setting up the Watson Translator package in {{site.data.keyword.Bluemix_notm}}

If you're using {{site.data.keyword.openwhisk}} from {{site.data.keyword.Bluemix_notm}}, the package bindings are automatically created for your {{site.data.keyword.Bluemix_notm}} Watson service instances.

1. Create a Watson Translator service instance in your {{site.data.keyword.Bluemix_notm}} [dashboard](http://cloud.ibm.com). Be sure to remember the name of the service instance and the {{site.data.keyword.Bluemix_notm}} organization and space you're in.

2. Refresh the packages in your namespace. The refresh automatically creates a package binding for the Watson service instance that you created.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Example output:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
  ```
  {: screen}

  List packages to see that the package binding was created:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## Setting up a Watson Translator package outside {{site.data.keyword.Bluemix_notm}}

If you're not using {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} or if you want to set up your Watson Translator outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your Watson Translator service. You need the Watson Translator service user name, and password.

- Create a package binding that is configured for your Watson Translator service.
  ```
  ibmcloud fn package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## Translating text

The `/whisk.system/watson-translator/translator` action translates text from one language to another. The parameters are as follows:

- `username`: The Watson API user name.
- `password`: The Watson API password.
- `payload`: The text to be translated.
- `translateParam`: The input parameter that indicates the text to translate. For example, if `translateParam=payload`, then the value of the `payload` parameter that is passed to the action is translated.
- `translateFrom`: A two-digit code of the source language.
- `translateTo`: A two-digit code of the target language.

Invoke the **translator** action in your package binding to translate some text from English to French.
```
ibmcloud fn action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

Example output:
```
{
    "payload": "Ciel bleu a venir"
}
```
{: screen}

## Identifying the language of some text

The `/whisk.system/watson-translator/languageId` action identifies the language of some text. The parameters are as follows:

- `username`: The Watson API user name.
- `password`: The Watson API password.
- `payload`: The text to identify.

Invoke the **languageId** action in your package binding to identify the language.
```
ibmcloud fn action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

Example output:
```
{
  "payload": "Ciel bleu a venir",
  "language": "fr",
  "confidence": 0.710906
}
```
{: screen}
