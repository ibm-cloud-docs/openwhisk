---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation du package Watson Translator
{: #openwhisk_catalog_watson_translator}

Le package `/whisk.system/watson-translator` permet
d'appeler diverses API Watson pour la traduction.
{: shortdesc}

Le package comprend les actions suivantes :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | Package | username, password | Package pour la traduction de texte et l'identification de la langue  |
| `/whisk.system/watson-translator/translator` | Action | payload, translateFrom, translateTo, translateParam, username, password | Traduire le texte |
| `/whisk.system/watson-translator/languageId` | Action | payload, username, password | Identifier la langue |

**Remarque** : le package `/whisk.system/watson` est obsolète, y compris les actions `/whisk.system/watson/translate` et `/whisk.system/watson/languageId`.

## Configuration du package Watson Translator dans {{site.data.keyword.Bluemix_notm}}

Si vous utilisez OpenWhisk depuis {{site.data.keyword.Bluemix_notm}}, OpenWhisk crée automatiquement des liaisons de package pour vos instances de service {{site.data.keyword.Bluemix_notm}} Watson. 

1. Créez une instance de service Watson Translator dans votre [tableau de bord](http://console.ng.Bluemix.net) {{site.data.keyword.Bluemix_notm}}. 
  
  Mémorisez le nom de l'instance de service ainsi que l'organisation et l'espace {{site.data.keyword.Bluemix_notm}} dans lesquels vous vous
trouvez.
  
2. Actualisez les packages dans votre espace de nom. L'actualisation
crée automatiquement une liaison de package pour l'instance de service Watson
que vous avez créée.
  ```
  wsk package refresh
  ```
  {: pre}
  
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
  ```
  
  ```
  wsk package list
  ```
  {: pre}
  
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  
  
## Configuration d'un package Watson Translator en dehors de {{site.data.keyword.Bluemix_notm}}

Si vous n'utilisez pas OpenWhisk dans {{site.data.keyword.Bluemix_notm}} ou si vous voulez configurer
Watson Translator en dehors de {{site.data.keyword.Bluemix_notm}}, vous devez créer une liaison de package
manuellement pour votre service Watson Translator. Vous avez besoin du nom
d'utilisateur du service et du mot de passe du service Watson
Translator.

- Créez une liaison de package configurée pour votre service Watson
Translator.

  ```
  wsk package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
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

- Appelez l'action `translator` dans votre liaison de package pour traduire un texte anglais en français.
  ```
  wsk action invoke myWatsonTranslator/translator \
  --blocking --result \
  --param payload "Blue skies ahead" --param translateFrom "en" \
  --param translateTo "fr"
  ```
  {: pre}
  
  ```json
  {
      "payload": "Ciel bleu a venir"
  }
  ```
  
  
## Identification de la langue d'un texte

L'action `/whisk.system/watson-translator/languageId` identifie la langue d'un texte. Les paramètres sont les suivants :

- `username` : nom d'utilisateur de l'API Watson.
- `password` : mot de passe de l'API Watson.
- `payload` : texte à identifier.

- Appelez l'action `languageId` dans votre liaison de package pour identifier la langue.
  ```
  wsk action invoke myWatsonTranslator/languageId \
  --blocking --result \
  --param payload "Ciel bleu a venir"
  ```
  {: pre}
  
  ```json
  {
    "payload": "Ciel bleu a venir",
    "language": "fr",
    "confidence": 0.710906
  }
  ```
  
