---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: pacchetto Translator
{: #openwhisk_catalog_watson_translator}

Il pacchetto `/whisk.system/watson-translator` offre una soluzione pratica per richiamare le diverse API Watson per la traduzione.
{: shortdesc}

Il pacchetto include le seguenti azioni.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | pacchetto | username, password | Pacchetto per la traduzione del testo e per l'identificazione della lingua  |
| `/whisk.system/watson-translator/translator` | azione | payload, translateFrom, translateTo, translateParam, username, password | Tradurre il testo |
| `/whisk.system/watson-translator/languageId` | azione | payload, username, password | Identificare la lingua |

**Nota**: il pacchetto `/whisk.system/watson` è obsoleto, incluse le azioni `/whisk.system/watson/translate` e `/whisk.system/watson/languageId`.

## Configurazione del pacchetto Watson Translator in {{site.data.keyword.Bluemix_notm}}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.Bluemix_notm}}, vengono creati automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.Bluemix_notm}} Watson.

1. Crea un'istanza del servizio Watson Translator nel tuo [dashboard](http://console.bluemix.net) {{site.data.keyword.Bluemix_notm}}. Assicurati di ricordare il nome dell'istanza del servizio e dell'organizzazione e dello spazio {{site.data.keyword.Bluemix_notm}} in cui ti trovi.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio Watson da te creata.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Output di esempio:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
  ```
  {: screen}

  Elenca i pacchetti per verificare che il bind di pacchetto sia stato creato:
  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## Configurazione di un pacchetto Watson Translator all'esterno di {{site.data.keyword.Bluemix_notm}}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} o se vuoi configurare Watson Translator all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio Watson Translator. Ti servono il nome utente e la password del servizio Watson Translator.

- Crea un bind di pacchetto configurato per il tuo servizio Watson Translator.
  ```
  ibmcloud wsk package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## Traduzione del testo

L'azione `/whisk.system/watson-translator/translator` traduce il testo da una lingua a un'altra. I parametri sono i seguenti:

- `username`: il nome utente dell'API Watson.
- `password`: la password dell'API Watson.
- `payload`: il testo da tradurre.
- `translateParam`: il parametro di input che indica il testo da tradurre. Ad esempio, se `translateParam=payload`, viene tradotto il valore del parametro `payload` che viene passato all'azione.
- `translateFrom`: un codice a due cifre della lingua di origine.
- `translateTo`: un codice a due cifre della lingua di destinazione.

Richiama l'azione **translator** nel tuo bind di pacchetto per tradurre un testo dall'inglese al francese.
```
ibmcloud wsk action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

Output di esempio:
```
{
    "payload": "Ciel bleu a venir"
  }
```
{: screen}

## Identificazione della lingua di un testo

L'azione `/whisk.system/watson-translator/languageId` identifica la lingua di un testo. I parametri sono i seguenti:

- `username`: il nome utente dell'API Watson.
- `password`: la password dell'API Watson.
- `payload`: il testo da identificare.

Richiama l'azione **languageId** nel tuo bind di pacchetto per identificare la lingua.
```
ibmcloud wsk action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

Output di esempio:
```
{
  "payload": "Ciel bleu a venir",
    "language": "fr",
    "confidence": 0.710906
  }
```
{: screen}
