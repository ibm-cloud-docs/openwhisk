---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilizzo del pacchetto GitHub
{: #openwhisk_catalog_github}

Il pacchetto `/whisk.system/github` offre una soluzione pratica per utilizzare le [API GitHub](https://developer.github.com/).
{: shortdesc}

Il pacchetto include il seguente feed:

| Entità | Tipo | Parametri | Descrizione |
| --- | --- | --- | --- |
| `/whisk.system/github` | pacchetto | username, repository, accessToken | Interagire con la API GitHub|
| `/whisk.system/github/webhook` | feed | events, username, repository, accessToken | Attivare gli eventi di trigger in caso di attività GitHub |

Si consiglia di effettuare la creazione di un bind di pacchetto con i valori `username`, `repository` e `accessToken`.  Con il bind, non dovrai specificare i valori ogni volta che usi il feed nel pacchetto.

## Attivazione di un evento trigger con l'attività GitHub

Il feed `/whisk.system/github/webhook` configura un servizio per attivare un trigger in caso di attività in uno specifico repository GitHub. I parametri sono i seguenti:

- `username`: il nome utente del repository GitHub.
- `repository`: il repository GitHub.
- `accessToken`: il tuo token di accesso personale GitHub. Quando [crei il tuo token](https://github.com/settings/tokens), assicurati di selezionare gli ambiti **repo:status** e **public_repo**. Inoltre, assicurati di non avere alcun webhook già definito per il tuo repository.
- `events`: il [tipo di evento GitHub](https://developer.github.com/v3/activity/events/types/) a cui sei interessato.

Nel seguente esempio, viene creato un trigger che si attiva ogni volta che c'è un nuovo commit in un repository GitHub.

1. Genera un [token di accesso personale](https://github.com/settings/tokens) GitHub.
  
  Il token di accesso verrà utilizzato nel prossimo passo.
  
2. Crea un bind di pacchetto configurato per il tuo repository GitHub e con il tuo token di accesso.
  
  ```
  wsk package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}
  
3. Crea un trigger per il tipo di evento `push` GitHub utilizzando il tuo feed `myGit/webhook`.
  
  ```
  wsk trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}
  
  Un commit al repository GitHub mediante `git push` comporta l'attivazione del trigger da parte del webhook. Se una regola corrisponde al trigger, viene richiamata l'azione associata.
  L'azione riceve il payload del webhook GitHub come parametro di input. Ogni evento webhook Github ha uno schema JSON simile, ma è un oggetto payload univoco che è determinato dal proprio tipo di evento.
  Per ulteriori informazioni sul contenuto del payload, consulta la documentazione API [GitHub events and payload](https://developer.github.com/v3/activity/events/types/).
  
