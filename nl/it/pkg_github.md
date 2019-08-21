---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: github, actions, trigger, event, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# GitHub
{: #pkg_github}

Questo pacchetto pre-installato non è disponibile nella regione Tokyo.
{: tip}

Il pacchetto `/whisk.system/github` offre una soluzione pratica per utilizzare le [API GitHub](https://developer.github.com/){: external}.
{: shortdesc}

Il pacchetto GitHub include le seguenti entità.

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/github` | Pacchetto | `username`, `repository`, `accessToken` | Interagire con l'API GitHub. |
| `/whisk.system/github/webhook` | Feed | `events`, `username`, `repository`, `accessToken` | Attivare gli eventi trigger in caso di attività GitHub. |

Si consiglia di effettuare la creazione di un bind di pacchetto con i valori `username`, `repository` e `accessToken`.  Con il bind, non dovrai specificare i valori ogni volta che usi il feed nel pacchetto.

## Attivazione di un evento trigger in caso di attività GitHub

Il feed `/whisk.system/github/webhook` configura un servizio per attivare un trigger in caso di attività in uno specifico repository GitHub. I parametri sono i seguenti.

| Parametro | Descrizione |
| --- | --- |
| `username` | Il nome utente del repository GitHub. |
| `repository` | Il repository GitHub. |
| `accessToken` | Il tuo token di accesso personale GitHub. Quando [crei il tuo token, assicurati di selezionare gli ambiti `repo:status` e `public_repo`. Inoltre, assicurati di non avere alcun webhook già definito per il tuo repository. |
| `events` | Il [tipo di evento GitHub ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.github.com/v3/activity/events/types/) a cui sei interessato. |

Nel seguente esempio, viene creato un trigger che si attiva ogni volta che c'è un nuovo commit a un repository GitHub.

1. Genera un token di accesso personale GitHub. Passa a **GitHub.com** > **Settings** > **Personal access tokens** per generare un token. Il token di accesso verrà utilizzato nel prossimo passo.

2. Crea un bind di pacchetto configurato per il tuo repository GitHub e con il tuo token di accesso.
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. Crea un trigger per il tipo di evento `push` di GitHub utilizzando il tuo feed `myGit/webhook`.
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  Un commit al repository GitHub mediante `git push` comporta l'attivazione del trigger da parte del webhook. Se una regola corrisponde al trigger, viene richiamata l'azione associata. L'azione riceve il payload del webhook GitHub come parametro di input. Ogni evento webhook Github ha uno schema JSON simile, ma è un oggetto payload univoco che è determinato dal proprio tipo di evento. Per ulteriori informazioni sul contenuto del payload, consulta la documentazione API [GitHub events and payload](https://developer.github.com/v3/activity/events/types/){: external}.



