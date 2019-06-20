---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# risoluzione dei problemi
{: #troubleshooting}

Se hai dei problemi quando utilizzi {{site.data.keyword.openwhisk}}, tieni presenti queste tecniche per la risoluzione dei problemi e su come ottenere supporto.
{: shortdesc}



## Azione con errori
{: #ts_action_fails}

{: tsSymptoms}
L'azione è in errore.

{: tsCauses}
Gli errori dell'applicazione possono essere dovuti all'uso di un runtime obsoleto. L'azione non può essere completata correttamente finché il runtime non viene aggiornato a uno supportato.

{: tsResolve}
Per vedere quale runtime viene utilizzato per l'azione, esegui `ibmcloud fn action get ACTION_NAME` e verifica `deprecated=true` nella risposta della query. Se il runtime è obsoleto, [aggiorna il runtime](/docs/openwhisk?topic=cloud-functions-actions#actions_update).


<br />


## Raggiungimento dei limiti di sistema
{: #ts_limit_reached}

{: tsSymptoms}
La funzione non viene eseguita correttamente e potrebbe generare un messaggio quale `{"error":"signal:killed"}`.

{: tsCauses}
Tutte le funzioni hanno [limiti di sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) minimi e massimi, quali un utilizzo massimo della memoria o un timeout.

{: tsResolve}
Alcuni limiti possono essere innalzati se un caso di business può giustificare valori limite di sicurezza più elevati. Per aumentare il valore limite, contatta il supporto IBM aprendo un ticket direttamente dalla console Web di IBM Cloud Functions.

1. Seleziona **Supporto**.
2. Seleziona **Aggiungi ticket** dal menu a discesa.
3. Seleziona **Tecnico** per il tipo di ticket.
4. Seleziona **Functions** per l'area tecnica di supporto.


<br />


## Come ottenere aiuto e supporto
{: #gettinghelp}

Hai ancora problemi con la tua funzione?
{: shortdesc}

-   Per vedere se {{site.data.keyword.Bluemix_notm}} è disponibile, [controlla la {{site.data.keyword.Bluemix_notm}} pagina sugli stati ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/status?selected=status).
-   Controlla i forum per vedere se altri utenti hanno riscontrato lo stesso problema. Se utilizzi i forum per porre una domanda, contrassegnala con una tag, in modo che venga vista dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.
    -   Se hai domande tecniche sullo sviluppo di funzioni con{{site.data.keyword.openwhisk}}, pubblica la tua domanda in [Stack Overflow ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/search?q=ibm-cloud-functions) e contrassegnala con la tag `ibm-cloud-functions`.
    -   Per domande sul servizio e istruzioni introduttive, utilizza il forum [IBM Developer Answers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix). Includile tag `ibm-cloud` e `functions`.
    Per ulteriori dettagli sull'utilizzo dei forum, vedi [Come ottenere supporto](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Contatta il supporto IBM aprendo un caso. Per informazioni su come aprire un caso di supporto IBM o sui livelli di supporto e sulla gravità dei casi, consulta [Come contattare il supporto](/docs/get-support?topic=get-support-getting-customer-support).
Quando segnali un problema, includi il tuo ID attivazione. Per ottenerne uno, esegui `ibmcloud fn activation list`.
{: tip}
