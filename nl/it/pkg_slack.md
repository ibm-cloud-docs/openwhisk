---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: slack package, token-based, api

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

# Slack
{: #pkg_slack}

Il pacchetto `/whisk.system/slack` preinstallato offre una soluzione pratica per utilizzare le [API Slack](https://api.slack.com/).
{: shortdesc}

Il pacchetto include le seguenti azioni:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/slack` | pacchetto | url, channel, username | Interagire con la API Slack |
| `/whisk.system/slack/post` | azione | text, url, channel, username | Pubblicare un messaggio in un canale Slack |

Si consiglia di effettuare la creazione di un bind di pacchetto con i valori `username`, `url` e `channel`. Con il bind, non dovrai specificare i valori ogni volta che richiami l'azione nel pacchetto.

## Pubblicazione di un messaggio in un canale Slack

L'azione `/whisk.system/slack/post` pubblica un messaggio in un canale Slack specificato. I parametri sono i seguenti:

- `url`: l'URL del webhook Slack.
- `channel`: il canale Slack nel quale pubblicare il messaggio.
- `username`: il nome con il quale pubblicare il messaggio.
- `text`: un messaggio da pubblicare.
- `token`: (facoltativo) un [token di accesso ](https://api.slack.com/tokens) Slack.

Il seguente esempio mostra come configurare Slack, creare un bind di pacchetto e pubblicare un messaggio in un canale.

1. Configura un [webhook in entrata](https://api.slack.com/incoming-webhooks) Slack per il tuo team.

  Dopo che Slack è stato configurato, ottieni un URL webhook simile a `https://hooks.slack.com/services/aaaaaaaaa/bbbbbbbbb/cccccccccccccccccccccccc`. Il webhook sarà necessario nel prossimo passo.

2. Crea un bind di pacchetto con le tue credenziali Slack, il canale in cui vuoi pubblicare il messaggio e il nome utente con il quale vuoi farlo.
  ```
  ibmcloud fn package bind /whisk.system/slack mySlack \
    --param url "https://hooks.slack.com/services/..." \
    --param username "Bob" \
    --param channel "#MySlackChannel"
  ```
  {: pre}

3. Richiama l'azione **post** nel tuo bind di pacchetto per pubblicare un messaggio nel canale Slack.
  ```
  ibmcloud fn action invoke mySlack/post --blocking --result \
    --param text "Hello from OpenWhisk!"
  ```
  {: pre}

## Utilizzo dell'API basata sul token Slack

Se preferisci, puoi utilizzare l'API basata sul token Slack anziché l'API webhook. Se così fosse, devi passare un parametro `token` che contiene il tuo [token di accesso](https://api.slack.com/tokens) Slack. Puoi quindi utilizzare qualsiasi [metodo API Slack](https://api.slack.com/methods) come parametro `url`. Ad esempio, per pubblicare un messaggio, puoi utilizzare il valore di parametro `url` [slack.postMessage](https://api.slack.com/methods/chat.postMessage).

