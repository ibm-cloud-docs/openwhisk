---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: functions compared, architecture, limitless, functions

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


# Architettura FaaS (Function as a Service)
{: #faas}

{{site.data.keyword.openwhisk}} offre OpenWhisk in un ambiente senza server e altamente scalabile. Puoi confrontare l'architettura senza server e il calcolo a basso costo di {{site.data.keyword.openwhisk_short}} con altri modelli di architettura.
{: shortdesc}

## Perché usare un'architettura senza server?

- Nessun sovraccarico dell'infrastruttura
- Manutenzione minima
- Convenienza
- Facile scalabilità
- Rapidità


## Confronto tra le architetture OpenWhisk
{: #faas_architecture}

Vengono confrontate le seguenti architetture OpenWhisk:

1. **FaaS (Function as a Service)** su [{{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk){: external}. IBM è l'unico fornitore ad offrire OpenWhisk gestito. Una buona introduzione al modello di programmazione senza server utilizzando una piattaforma FaaS è disponibile nel [blog di Martin Fowler](https://martinfowler.com/articles/serverless.html){: external} e puoi consultare i [casi di utilizzo](/docs/openwhisk?topic=cloud-functions-use_cases) per eseguire OpenWhisk con una progettazione senza server.

2. **IaaS (Infrastructure as a Service)** con RYO (Roll Your Own) OpenWhisk. Puoi scaricare OpenWhisk da Apache Incubation Project ed eseguirlo su [{{site.data.keyword.cloud_notm}} IaaS](https://cloud.ibm.com/catalog?category=compute){: external}.

3. **PaaS (Platform as a Service)** come runtime dell'applicazione gestito. Un buon esempio è il runtime [Liberty for Java](https://cloud.ibm.com/catalog/starters/liberty-for-java){: external} che è gestito dall'implementazione di {{site.data.keyword.cloud_notm}} Foundry.

4. **CaaS (Container as a Service)** come ambiente del contenitore gestito. Un buon esempio è [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started#container_index).

5. **IaaS (Infrastructure as a Service)** con il runtime Java EE. Un buon esempio è la [VM WebSphere Application Server su {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog/services/websphere-application-server){: external}.

La seguente tabella confronta gli elementi di ciascuna architettura dal punto di vista di uno sviluppatore che sta creando e gestendo le applicazioni:


| Argomento | (1) FaaS su {{site.data.keyword.openwhisk_short}} | (2) IaaS con RYO OpenWhisk | (3) PaaS | (4) CaaS | (5) IaaS con Java EE |
| --- | --- | --- | --- | --- | --- |
|	Unità di applicazione	|	Funzione singola (normalmente un piccolo blocco di codice in JavaScript, in Swift o nel contenitore Docker) - può essere inferiore a 1 Kb ma anche più grande. Di solito non più di pochi Kb.	|	Come la colonna (1)	|	Dipende dal runtime utilizzato. Un file EAR o WAR, o altro bundle di applicazioni specifico per il linguaggio, di solito relativamente grande - Kb o anche Mb con molti servizi in un bundle, ma può essere piccolo come un singolo servizio.	|	Il contenitore Docker è l'unità di distribuzione.	|	VM con server delle applicazioni con il file EAR o WAR e altre dipendenze - solitamente con dimensioni in Gb.	|
|	Footprint della risorsa	|	L'utente finale non paga o si preoccupa della memoria, della CPU o di altre risorse. Sebbene l'azione abbia qualche footprint, l'utente non se ne deve preoccupare	|	Alto. L'utente finale deve prima eseguire il provisioning dell'ambiente IaaS e solo successivamente installare e configurare OpenWhisk su di esso	|	Piccolo. L'utente finale paga per la memoria e la CPU delle applicazioni in esecuzione, ma non paga nulla per le applicazioni che non sono in esecuzione	|	Da piccolo a medio	|	Alto. L'utente finale deve pagare per l'archiviazione su disco, la memoria, le CPU e, eventualmente, altri componenti quando l'applicazione è in esecuzione. Quando viene arrestata, vengono addebitati solo i costi di archiviazione	|
|	Installazione e configurazione	|	Nessuna richiesta	|	Difficile - tutto eseguito dall'utente finale	|	Nessuna richiesta	|	Moderata - Hardware, rete, SO, strumenti di gestione contenitore forniti dal fornitore di CaaS, immagini, connettività e istanze dall'utente finale	|	Difficile - Hardware, rete, SO, installazione iniziale di Java EE forniti dal fornitore, ulteriore configurazione, clustering e scalabilità dall'utente finale	|
|	Tempo di provisioning	|	Millisecondi	|	Vedi le colonne (4) e (5)	|	Minuti	|	Minuti	|	Ore	|
|	Amministrazione in corso	|	Nessuno	|	Difficile	|	Nessuno	|	Moderato	|	Difficile	|
|	Ridimensionamento elastico	|	Ogni azione è sempre istantaneamente e intrinsecamente ridimensionata a seconda del carico. Non è necessario eseguire il provisioning di VM o di altre risorse in anticipo	|	Non fornito - l'utente finale deve fornire la capacità di calcolo su IaaS e gestire il ridimensionamento delle VM. Una volta ridimensionate le VM, OpenWhisk ridimensiona automaticamente le azioni, ma il provisioning delle risorse deve già essere stato eseguito	|	Ridimensionamento automatico ma lento. Durante l'aumento del carico, gli utenti potrebbero attendere il completamento dell'azione di ridimensionamento per diversi minuti. Il ridimensionamento automatico richiede un'attenta ottimizzazione	|	Ridimensionamento automatico ma lento. Durante l'aumento del carico, gli utenti potrebbero attendere il completamento dell'azione di ridimensionamento per diversi minuti. Il ridimensionamento automatico richiede un'attenta ottimizzazione	|	Non fornito	|
|	Pianificazione della capacità	|	Non necessario. FaaS fornisce automaticamente la capacità necessaria	|	È necessario eseguire il provisioning di una capacità sufficiente in anticipo o eseguirne lo script	|	È necessaria qualche pianificazione della capacità, ma viene fornito un aumento automatico della capacità	|	È necessaria qualche pianificazione della capacità, ma viene fornito un aumento automatico della capacità	|	È necessario eseguire in modo statico il provisioning di una capacità sufficiente per gestire il carico di lavoro massimo	|
|	Connessioni persistenti e stato	|	Limitato - può mantenere una connessione persistente, tranne nei casi di memorizzazione del contenitore nella cache. Generalmente lo stato deve essere mantenuto in una risorsa esterna	|	Come la colonna (1)	|	Supportato: può mantenere un socket o una connessione aperta per lunghi periodi, può memorizzare lo stato in memoria tra le chiamate	|	Supportato: può mantenere un socket o una connessione aperta per lunghi periodi, può memorizzare lo stato in memoria tra le chiamate	|	Supportato: può mantenere un socket o una connessione aperta per lunghi periodi, può memorizzare lo stato in memoria tra le chiamate	|
|	Manutenzione	|	Nessuna - L'intero stack è gestito da IBM.	|	Significativa - a seconda dell'ambiente di destinazione, l'utente deve eseguire il provisioning di hardware, rete, SO, archiviazione, DB, installare e gestire OpenWhisk e così via.	|	Nessuna - L'intero stack è gestito dal fornitore.	|	Significativa - L'utente deve creare e gestire immagini personalizzate, distribuire e gestire contenitori, connessioni tra contenitori e così via.	|	Significativa - L'utente deve assegnare VM, gestire e ridimensionare i server Java EE singolarmente.	|
|	Elevata disponibilità (HA) e Ripristino di emergenza (DR)	|	Intrinseco / nessun costo aggiuntivo	|	RYO (Roll your own) 	|	Disponibile a un costo aggiuntivo	|	I contenitori non riusciti possono essere riavviati automaticamente	|	Disponibile a un costo aggiuntivo, semiautomatico. È possibile impostare il failover automatico delle VM	|
|	Sicurezza	|	Fornita dal fornitore	|	RYO (Roll your own)	|	Combinazione di RYO fornita dal fornitore	|	Combinazione di RYO fornita dal fornitore	|	RYO (Roll your own)	|
|	Velocità sviluppatore	|	Massima	|	Massima	|	Massima	|	Media	|	Lenta	|
|	Utilizzo delle risorse (risorse inattive che devono ancora essere pagate)	|	Le risorse non sono mai inattive poiché vengono richiamate solo su richiesta. Quando il carico di lavoro è assente, non si verifica alcun costo o assegnazione delle risorse.	|	Poiché questa opzione utilizza IaaS o CaaS, si applicano considerazioni simili come nelle colonne (4) e (5)	|	Alcune risorse possono essere inattive e il ridimensionamento automatico aiuta a eliminare tali risorse. Deve essere presente sempre un certo numero di istanze in esecuzione e possono essere utilizzate con meno del 50% della loro capacità. Le istanze arrestate non costano nulla	|	Simile alla colonna (3)	|	Alcune risorse possono essere inattive, ma il ridimensionamento automatico non è supportato. Deve essere presente sempre un certo numero di istanze in esecuzione e possono essere utilizzate con meno del 50% della loro capacità. Le istanza arrestate possono sostenere il costo di archiviazione	|
|	Maturità	|	Maturità anticipata	|	Maturità anticipata	|	Maturità anticipata	|	Maturità moderata	|	Maturità elevata	|
|	Limiti delle risorse	|	[Esistono alcuni limiti](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)	|	Dipende dalle risorse assegnate	|	No	|	No	|	No	|
|	Latenza per i servizi utilizzati raramente	|	Le richieste rare possono vedere inizialmente un tempo di risposta di diversi secondi, ma rimangono nell'intervallo ms per le richieste successive	|	Dipende	|	Bassa	|	Bassa	|	Bassa - supponendo che il sistema abbia risorse sufficienti	|
|	Tipo di punto ottimale dell'applicazione	|	Elaborazione evento, IoT, backend mobile, microservizi. Sicuramente non per le applicazioni monolitiche. Vedi [Casi di utilizzo](/docs/openwhisk?topic=cloud-functions-use_cases)	|	Come la colonna (1), ma quando l'utente vuole eseguire su un cloud non IBM o in loco.	|	Applicazioni web con carico di lavoro 24x7, servizi con stato che devono mantenere la connessione aperta per lunghi periodi di tempo. Può essere utilizzato per eseguire microservizi o applicazioni monolitiche	|	Ideale per le applicazioni di microservizi.	|	Applicazioni aziendali tradizionali migrate dall'istallazione in loco al cloud. Ideale per le applicazioni monolitiche	|
|	Addebito della granularità e fatturazione	|	[Per blocchi di 100 millisecondi](https://cloud.ibm.com/openwhisk/learn/pricing){: external}	|	Dipende dall'implementazione - Se vengono utilizzati IaaS o CaaS, si applicano considerazioni simili - Vedi le colonne (4) e (5)	|	Solitamente addebito all'ora (raramente al minuto) per un bundle di risorse (CPU + memoria + spazio su disco)	|	Simile alla colonna (3)	|	Simile alla colonna (3)	|
|	Costo totale di proprietà	|	Per il suo punto ottimale, è probabile che le applicazioni costino un ordine di grandezza inferiore alle alternative. Poiché le risorse vengono ridimensionate automaticamente, non si verifica l'over-provisioning.	|	Per le distribuzioni cloud, è probabile che sia più costoso di FaaS OpenWhisk, ma la distribuzione in loco può essere più economica rispetto alle architetture tradizionali	|	Relativamente basso - l'utente non ha bisogno di eseguire il provisioning o di gestire risorse e può concentrarsi sullo sviluppo di applicazioni. Qualche livello di over-provisioning rispetto all'architettura senza server	|	Moderato - L'utente deve eseguire il provisioning e gestire i contenitori e l'applicazione e potrebbe vedere un certo livello di over-provisioning rispetto all'architettura senza server o PaaS	|	Relativamente alto - Considerando che la migrazione di applicazioni legacy nel modello nativo del cloud potrebbe essere proibitivamente costosa, questa può essere una scelta praticabile ed economica per tali applicazioni.	|

## Considerazioni sui costi
{: #faas_cost}

L'infrastruttura per i tuoi ambienti di test, preparazione, test di carico e altro può essere costosa. Ci vuole tempo per configurarli e, dato che di solito funzionano 24 ore su 24, 7 giorni su 7, sono spesso sottoutilizzati e consumano grandi quantità di capacità. Utilizzando un'architettura senza server, i costi per un numero qualsiasi di ambienti vengono generati in base al carico anziché al numero di ambienti definiti.
{: shortdesc}

Per stimare i costi per un'applicazione senza server, puoi utilizzare il [calcolatore dei prezzi ](https://cloud.ibm.com/openwhisk/learn/pricing){: external}.

### Capacità illimitata
{: #faas_capacity}

Nelle architetture tradizionali, ogni servizio consuma la quantità di capacità assegnata e ti viene addebitato il consumo della capacità. L'architettura senza server di {{site.data.keyword.openwhisk_short}} riduce la restrizione sulla granularità dell'architettura dei tuoi microservizi.

Quando non è in uso, {{site.data.keyword.openwhisk_short}} non costa nulla. Il codice viene eseguito quando c'è una chiamata HTTP, una modifica dello stato del database o un altro tipo di evento che attiva l'esecuzione del tuo codice. Ti viene addebitata una fattura in base a ogni millisecondo del tempo di esecuzione arrotondato per eccesso a 100 ms e non all'ora di utilizzo della VM, indipendentemente dal fatto che la VM fosse operativa o meno. Poiché paghi solo quando gli eventi vengono consumati e non in base al numero di ambienti, puoi suddividere la tua applicazione in 100, 1000 o anche più microservizi.

### Esegui le azioni in qualsiasi regione
{: #faas_region}

Nelle architetture tradizionali, il codice deve essere in esecuzione in una determinata regione ed è necessario anche pagare l'infrastruttura per tale regione. Con {{site.data.keyword.openwhisk_short}}, le azioni possono essere distribuite e rese disponibili per l'esecuzione in qualsiasi regione senza costi aggiuntivi. Puoi aumentare la disponibilità e la resilienza del tuo codice senza le restrizioni di costo tradizionali.

### Ridondanza in base alla progettazione
{: #faas_redundancy}

Nelle architetture tradizionali, le applicazioni devono essere ridondanti. Con {{site.data.keyword.openwhisk_short}}, i processi non devono essere altamente disponibili (HA) perché le applicazioni senza server sono prive di stato e guidate dagli eventi di richiesta in base alla progettazione. Eliminando la necessità di creare esplicitamente la ridondanza, la natura senza stato delle applicazioni senza server può ridurre significativamente i costi di infrastruttura.




