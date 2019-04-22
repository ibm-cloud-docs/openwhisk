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

# Package Watson : Translator
{: #openwhisk_catalog_watson_translator}

Ce package préinstallé n'est plus disponible dans aucune région. Veuillez consulter le package installable [Language Translator](/docs/openwhisk?topic=cloud-functions-language-translator-package) en utilisant la nouvelle API V3 avec l'authentification IAM.
{: tip}

Le package `/whisk.system/watson-translator` permet d'appeler diverses API Watson pour la traduction.
{: shortdesc}

Le package inclut les actions ci-dessous.

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | package | username, password | Package pour la traduction de texte et l'identification de la langue  |
| `/whisk.system/watson-translator/translator` | action | payload, translateFrom, translateTo, translateParam, username, password | Traduire le texte |
| `/whisk.system/watson-translator/languageId` | action | payload, username, password | Identifier la langue |

**Remarque** : Le package
`/whisk.system/watson` est obsolète, y compris les actions
`/whisk.system/watson/translate` et `/whisk.system/watson/languageId`. Consultez plutôt le [package installable {{site.data.keyword.languagetranslatorshort}}](/docs/openwhisk?topic=cloud-functions-language-translator-package). 

## Configuration du package Watson Translator dans {{site.data.keyword.Bluemix_notm}}

Si vous utilisez {{site.data.keyword.openwhisk}} depuis {{site.data.keyword.Bluemix_notm}}, les liaisons de package sont automatiquement créées pour vos instances de service {{site.data.keyword.Bluemix_notm}} Watson.

1. Créez une instance de service Watson Translator dans votre [tableau de bord](http://cloud.ibm.com) {{site.data.keyword.Bluemix_notm}}. Mémorisez le nom de l'instance de service ainsi que l'organisation et l'espace {{site.data.keyword.Bluemix_notm}} dans lesquels vous vous trouvez.

2. Actualisez les packages dans votre espace de nom. L'actualisation crée automatiquement une liaison de package pour l'instance de service Watson que vous avez créée.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Exemple de sortie :
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
  ```
  {: screen}

  Affichez la liste des packages pour vérifier que la liaison de package a bien été créée :
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemple de sortie :
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## Configuration d'un package Watson Translator en dehors d'{{site.data.keyword.Bluemix_notm}}

Si vous n'utilisez pas {{site.data.keyword.openwhisk_short}} dans {{site.data.keyword.Bluemix_notm}} ou si vous voulez configurer Watson Translator en dehors d'{{site.data.keyword.Bluemix_notm}}, vous devez créer une liaison de package manuellement pour votre service Watson Translator. Vous avez besoin du nom d'utilisateur du service et du mot de passe du service Watson Translator.

- Créez une liaison de package configurée pour votre service Watson Translator.
  ```
  ibmcloud fn package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## Traduction de texte

L'action `/whisk.system/watson-translator/translator` traduit un texte d'une langue vers une autre. Les paramètres sont les suivants :

- `username` : nom d'utilisateur de l'API Watson.
- `password` : mot de passe de l'API Watson.
- `payload` : texte à traduire.
- `translateParam`: paramètre d'entrée indiquant le texte à traduire. Par exemple, si `translateParam=payload`, la valeur du paramètre `payload` qui est transmise à l'action est traduite.
- `translateFrom` : code à deux chiffres de la langue source.
- `translateTo` : code à deux chiffres de la langue cible.

Appelez l'action **translator** dans votre liaison de package pour traduire du texte anglais en français.
```
ibmcloud fn action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

Exemple de sortie :
```
{
    "payload": "Ciel bleu a venir"
}
```
{: screen}

## Identification de la langue d'un texte

L'action `/whisk.system/watson-translator/languageId` identifie la langue d'un texte. Les paramètres sont les suivants :

- `username` : nom d'utilisateur de l'API Watson.
- `password` : mot de passe de l'API Watson.
- `payload` : texte à identifier.

Appelez l'action **languageId** dans votre liaison de package pour identifier la langue.
```
ibmcloud fn action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

Exemple de sortie :
```
{
  "payload": "Ciel bleu à venir",
    "language": "fr",
    "confidence": 0.710906
  }
```
{: screen}
