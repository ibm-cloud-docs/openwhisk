---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: pacchetto Speech to Text
{: #openwhisk_catalog_watson_texttospeech}

Il pacchetto `/whisk.system/watson-speechToText` offre una soluzione pratica per richiamare le diverse API Watson per la conversione da voce a testo.
{: shortdesc}

Il pacchetto include le seguenti azioni.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | pacchetto | username, password | Pacchetto per convertire la voce in testo |
| `/whisk.system/watson-speechToText/speechToText` | azione | payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out | Convertire audio in testo |

**Nota**: il pacchetto `/whisk.system/watson` è obsoleto, inclusa l'azione `/whisk.system/watson/speechToText`.

## Configurazione del pacchetto Watson Speech to Text in {{site.data.keyword.Bluemix_notm}}

Se utilizzi {{site.data.keyword.openwhisk}} da {{site.data.keyword.Bluemix_notm}}, vengono creati automaticamente i bind di pacchetto per le tue istanze del servizio {{site.data.keyword.Bluemix_notm}} Watson.

1. Crea un'istanza del servizio Watson Speech to Text nel tuo [dashboard](http://console.ng.Bluemix.net) {{site.data.keyword.Bluemix_notm}}.

  Assicurati di ricordare il nome dell'istanza del servizio e dell'organizzazione e dello spazio {{site.data.keyword.Bluemix_notm}} in cui ti trovi.

2. Aggiorna i pacchetti nel tuo spazio dei nomi. L'aggiornamento crea automaticamente un bind di pacchetto per l'istanza del servizio Watson da te creata.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Output di esempio:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  Elenca i pacchetti per verificare che il bind sia stato creato:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Output di esempio:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

## Configurazione di un pacchetto Watson Speech to Text all'esterno di {{site.data.keyword.Bluemix_notm}}

Se non utilizzi {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} o se vuoi configurare Watson Speech to Text all'esterno di {{site.data.keyword.Bluemix_notm}}, devi creare manualmente un bind di pacchetto per il tuo servizio Watson Speech to Text. Ti servono il nome utente e la password del servizio Watson Speech to Text.

Crea un bind di pacchetto configurato per il tuo servizio Watson Speech to Text.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Conversione da voce a testo

L'azione `/whisk.system/watson-speechToText/speechToText` converte l'audio in testo. I parametri sono i seguenti:

- `username`: il nome utente dell'API Watson.
- `password`: la password dell'API Watson.
- `payload`: i dati binari audio codificati da convertire in testo.
- `content_type`: il tipo MIME dell'audio.
- `encoding`: la codifica dei dati binari del file audio.
- `continuous`: indica se vengono restituiti più risultati finali che rappresentano frasi consecutive separate da pause lunghe.
- `inactivity_timeout`: il tempo, in secondi, dopo il quale viene chiusa la connessione, se nell'audio presentato viene rilevato solo silenzio.
- `interim_results`: indica se il servizio deve restituire risultati intermedi.
- `keywords`: un elenco di parole chiave da individuare nell'audio.
- `keywords_threshold`: un valore di attendibilità corrispondente al limite inferiore per l'individuazione di una parola chiave.
- `max_alternatives`: il numero massimo di trascrizioni alternative da restituire.
- `model`: l'identificativo del modello da utilizzare per la richiesta di riconoscimento.
- `timestamps`: indica se l'allineamento temporale viene restituito per ciascuna parola.
- `watson-token`: fornisce un token di autenticazione per il servizio come alternativa all'immissione delle credenziali del servizio.
- `word_alternatives_threshold`: un valore di attendibilità corrispondente al limite inferiore per l'individuazione di un'ipotesi come possibile parola alternativa.
- `word_confidence`: indica se per ciascuna parola deve essere restituita una misura di attendibilità compresa nell'intervallo da 0 a 1.
- `X-Watson-Learning-Opt-Out`: indica se annullare la selezione della raccolta dati per la chiamata.

Richiama l'azione **speechToText** nel tuo bind di pacchetto per convertire l'audio codificato.
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

Output di esempio:
```
{
  "data": "Hello Watson"
  }
```
{: screen}
