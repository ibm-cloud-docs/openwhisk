---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Casi di utilizzo comuni
{: #openwhisk_common_use_cases}

Il modello di esecuzione offerto da {{site.data.keyword.openwhisk_short}} supporta vari casi di utilizzo. Le seguenti sezioni includono alcuni esempi tipici. Per una discussione più dettagliata sull'architettura Senza server, i casi di utilizzo di esempio, i pro e i contro e le procedure consigliate per l'implementazione, leggi l'eccellente [articolo di Mike Roberts sul blog di Martin Fowler](https://martinfowler.com/articles/serverless.html).
{: shortdesc}

## Microservizi
{: #openwhisk_common_use_cases_microservices}

Nonostante la loro utilità, le soluzioni basate sui microservizi rimangono difficili da creare con le tradizionali tecnologie cloud, che spesso richiedono il controllo di una toolchain complessa e la separazione delle pipeline operative e di creazione. I piccoli team dinamici trascorrono troppo tempo con complessità infrastrutturali e operative come, ad esempio, tolleranza agli errori, bilanciamento del carico, ridimensionamento automatico e registrazione. Questi team vogliono un modo per sviluppare un codice semplificato e a valore aggiunto con linguaggi di programmazione che già conoscono, prediligono e più adatti a risolvere problemi particolari.

La natura modulare e intrinsecamente scalabile di {{site.data.keyword.openwhisk_short}} lo rende ideale per implementare parti di logica granulari nelle azioni. Le azioni {{site.data.keyword.openwhisk_short}} sono indipendenti l'una dall'altra e possono essere implementate utilizzando i diversi linguaggi supportati da {{site.data.keyword.openwhisk_short}} e accedere a vari sistemi di backend. Ogni azione può essere distribuita e gestita in modo indipendente e viene ridimensionata indipendentemente dalle altre azioni. L'interconnettività tra le azioni è fornita da {{site.data.keyword.openwhisk_short}} sotto forma di regole, sequenze e convenzioni di denominazione. Questo tipo di ambiente è di buon auspicio per le applicazioni basate sui microservizi.

Un altro argomento importante a favore di {{site.data.keyword.openwhisk_short}} è il costo di un sistema in una configurazione di ripristino di emergenza. Confronta i microservizi PaaS o CaaS rispetto all'utilizzo di {{site.data.keyword.openwhisk_short}} supponendo che tu abbia 10 microservizi, che utilizzando contenitori o runtime CloudFoundry. Questo confronto equivale a 10 processi in continua esecuzione e fatturabili in una singola zona di disponibilità, 20 quando eseguiti su 2 zone di disponibilità e 40 quando eseguiti in due regioni con due zone ciascuna. Per raggiungere lo stesso obiettivo con {{site.data.keyword.openwhisk_short}}, puoi eseguirli in tutte le zone di disponibilità e regioni che desideri senza dover pagare un centesimo in più.

[Logistics Wizard](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/) è un'applicazione di esempio a livello aziendale che utilizza {{site.data.keyword.openwhisk_short}} e CloudFoundry per creare un'applicazione a 12 fattori. È una soluzione intelligente per la gestione della catena di fornitura che mira a simulare un ambiente che esegue un sistema ERP. Amplia questo sistema ERP con le applicazioni per migliorare la visibilità e l'agilità dei gestori della catena di fornitura.

## Applicazioni web
{: #openwhisk_common_use_cases_webapps}

Data la natura guidata dagli eventi di {{site.data.keyword.openwhisk_short}}, offre numerosi vantaggi per le applicazioni rivolte agli utenti, poiché le richieste HTTP provenienti dal browser dell'utente fungono da eventi. Le applicazioni {{site.data.keyword.openwhisk_short}} utilizzano la capacità di calcolo e vengono fatturate solo quando completano le richieste degli utenti. Lo standby inattivo o la modalità di attesa sono inesistenti. Questa funzione rende {{site.data.keyword.openwhisk_short}} notevolmente meno costoso rispetto ai contenitori tradizionali o alle applicazioni CloudFoundry. Entrambi possono restare la maggior parte del tempo inattivi, in attesa di richieste utente in entrata e fatturati per tutto questo tempo di “sospensione”.

È possibile creare ed eseguire un'applicazione web completa con {{site.data.keyword.openwhisk_short}}. La combinazione di API senza server con l'hosting di file statici per risorse del sito come HTML, JavaScript e CSS, significa che puoi creare applicazioni web interamente senza server. La semplicità di funzionamento di un ambiente {{site.data.keyword.openwhisk_short}} ospitato consiste nel non dover effettuare alcuna operazione. Poiché {{site.data.keyword.openwhisk_short}} è ospitato su {{site.data.keyword.Bluemix_notm}}, è un grande vantaggio rispetto all'installazione e alla gestione di un runtime Node.js Express o altri runtime di server tradizionali.

Vedi i seguenti esempi su come utilizzare {{site.data.keyword.openwhisk_short}} per creare un'applicazione web:
- [Web actions: Serverless Web Apps with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [Build a user-facing {{site.data.keyword.openwhisk_short}} application with {{site.data.keyword.Bluemix_notm}} and Node.js](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [Serverless HTTP handlers with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT
{: #openwhisk_common_use_cases_iot}

Gli scenari dell'Internet delle cose sono spesso intrinsecamente guidati dai sensori. Ad esempio, un'azione potrebbe essere attivata in {{site.data.keyword.openwhisk_short}} se vi è la necessità di reagire di fronte a un sensore che supera una determinata temperatura. Le interazioni IoT sono di solito senza stato con un potenziale carico elevato in eventi spontanei importanti come disastri naturali, tempeste meteorologiche significative o ingorghi. Si crea la necessità di un sistema elastico in cui il normale carico di lavoro potrebbe essere ridotto, ma deve ridimensionarsi rapidamente con un tempo di risposta prevedibile. Quindi è preferibile la capacità di gestire molti eventi simultanei senza preavviso al sistema. È difficile creare un sistema che soddisfi questi requisiti utilizzando architetture server tradizionali, in quanto tendono a essere sottodimensionate e incapaci di gestire picchi di carico nel traffico o potrebbero essere sottoposte a un provisioning eccessivo ed estremamente costose.

È possibile implementare applicazioni IoT che utilizzano architetture server tradizionali. Tuttavia, in molti casi la combinazione di servizi e bridge di dati diversi richiede pipeline flessibili e dalle prestazioni elevate. A partire dai dispositivi IoT all'archiviazione cloud e una piattaforma di analisi. Spesso i bridge preconfigurati non hanno la programmabilità desiderata per implementare e ottimizzare una particolare architettura di soluzione. Data la varietà di pipeline e la mancanza di standardizzazione sulla fusione dei dati in generale (su IoT in particolare), è comune vedere ambienti in cui la pipeline richiede la trasformazione personalizzata dei dati. Queste trasformazioni di dati personalizzate si applicano alla conversione del formato, al filtraggio o all'ampliamento. {{site.data.keyword.openwhisk_short}} è uno strumento eccellente per implementare tale trasformazione, in modo "senza server", in cui la logica personalizzata è ospitata su una piattaforma cloud completamente gestita ed elastica.

Guarda la seguente applicazione IoT di esempio che utilizza{{site.data.keyword.openwhisk_short}}, NodeRed, Cognitive e altri servizi: [Serverless transformation of IoT data-in-motion with {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c#.akt3ocjdt).

![Esempio di architettura della soluzione IoT](images/IoT_solution_architecture_example.png)

## Backend API
{: #openwhisk_api_backend}

Le piattaforme di calcolo senza server offrono agli sviluppatori un modo rapido per creare API senza server. {{site.data.keyword.openwhisk_short}} supporta la generazione automatica dell'API REST per le azioni. La [funzione sperimentale](./openwhisk_apigateway.html) di {{site.data.keyword.openwhisk_short}} può richiamare un'azione con metodi HTTP diversi da POST e senza la chiave API di autorizzazione dell'azione tramite il Gateway API {{site.data.keyword.openwhisk_short}}. Questa capacità è utile non solo per esporre le API ai clienti esterni, ma anche per creare applicazioni di microservizi.

Inoltre, le azioni {{site.data.keyword.openwhisk_short}} possono essere connesse a uno strumento di gestione API a scelta (come [IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) o altri). Analogamente ad altri casi di utilizzo, si applicano tutte le considerazioni sulla scalabilità e altri QoS (Qualities of Services).

[Emoting](https://github.com/l2fprod/openwhisk-emoting) è un'applicazione di esempio che utilizza le azioni {{site.data.keyword.openwhisk_short}} tramite un'API REST.

Vedi il seguente esempio che include una discussione sull'[utilizzo di Serverless come backend API](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples).

## Backend mobile
{: #openwhisk_common_use_cases_mobile}

Numerose applicazioni mobili richiedono una logica lato server. Tuttavia, gli sviluppatori mobili di solito non hanno esperienza nella gestione della logica lato server e preferiscono concentrarsi sull'applicazione in esecuzione sul dispositivo. Questo obiettivo di sviluppo è facilmente ottenibile utilizzando {{site.data.keyword.openwhisk_short}} come backend lato server ed è una buona soluzione. Inoltre, il supporto integrato per Swift lato server consente agli sviluppatori di riutilizzare le proprie competenze di programmazione iOS esistenti. Poiché le applicazioni mobili hanno spesso modelli di carico imprevedibili, potresti voler utilizzare una soluzione {{site.data.keyword.openwhisk_short}} come {{site.data.keyword.Bluemix}}. Questa soluzione può ridimensionarsi per soddisfare praticamente qualsiasi richiesta di carico di lavoro senza la necessità di fornire risorse in anticipo.

[Skylink](https://github.com/IBM-Bluemix/skylink) è un'applicazione di esempio che collega mediante un iPad un aeromobile drone all'IBM Cloud con un'analisi delle immagini quasi in tempo reale che sfrutta {{site.data.keyword.openwhisk_short}}, IBM {{site.data.keyword.cloudant_short_notm}}, IBM Watson e Alchemy Vision.

[BluePic](https://github.com/IBM-Swift/BluePic) è un'applicazione per la condivisione di foto e immagini che può essere utilizzata per scattare foto e condividerle con altri utenti BluePic. Questa applicazione mostra come sfruttare, in un'applicazione mobile iOS 10, un'applicazione server basata su Kitura che è scritta in Swift e utilizza {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.cos_full_notm}} per i dati di immagine. Nella sequenza {{site.data.keyword.openwhisk_short}} viene anche utilizzato AlchemyAPI per analizzare l'immagine, estrarre tag di testo in base al contenuto dell'immagine e infine inviare una notifica di push all'utente.

## Elaborazione dati
{: #data-processing}

Data la quantità di dati attualmente disponibile, lo sviluppo di applicazioni richiede la capacità di elaborare nuovi dati e, potenzialmente, rispondere agli stessi. Questa necessità include il bisogno di elaborare record di database strutturati e documenti non strutturati, immagini o video. {{site.data.keyword.openwhisk_short}} può essere configurato in base ai feed forniti dal sistema o personalizzati per reagire alle modifiche nei dati ed eseguire automaticamente le azioni sui feed di dati in entrata. Le azioni possono essere programmate per elaborare le modifiche, trasformare i formati dei dati, inviare e ricevere messaggi, richiamare altre azioni e aggiornare vari archivi di dati. Gli archivi di dati supportati includono database relazionali basati su SQL, griglie di dati in memoria, database NoSQL, file, broker di messaggistica e diversi altri sistemi. Le regole e le sequenze di {{site.data.keyword.openwhisk_short}} offrono flessibilità per apportare modifiche alla pipeline di elaborazione senza programmazione e vengono eseguite tramite semplici aggiornamenti di configurazione. Le opzioni dell'archivio di dati e il basso sovraccarico amministrativo rendono un sistema basato su {{site.data.keyword.openwhisk_short}} estremamente agile e facilmente adattabile ai mutevoli requisiti.

Il progetto [OpenChecks](https://github.com/krook/openchecks) è un'installazione di prova che mostra come è possibile utilizzare {{site.data.keyword.openwhisk_short}} per elaborare il deposito di assegni su un conto bancario utilizzando il riconoscimento ottico dei caratteri. È costruito sul servizio pubblico {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.openwhisk_short}} e si basa su {{site.data.keyword.cloudant}} e {{site.data.keyword.cos_full_notm}}. Per le installazioni in loco, può utilizzare CouchDB e OpenStack Swift. Altri servizi di archiviazione possono includere FileNet o Cleversafe. Tesseract fornisce la libreria OCR.

## Cognitive
{: #openwhisk_common_use_cases_cognitive}

Le tecnologie cognitive possono essere efficacemente combinate con {{site.data.keyword.openwhisk_short}} per creare potenti applicazioni. Ad esempio, IBM Alchemy API e Watson Visual Recognition possono essere utilizzati con {{site.data.keyword.openwhisk_short}} per estrarre automaticamente informazioni utili dai video senza doverli guardare. Questa tecnologia è l'estensione “cognitiva” del caso di utilizzo [Elaborazione dati](#data-processing) che è stato discusso in precedenza. Un altro buon uso di {{site.data.keyword.openwhisk_short}} è l'implementazione della funzione Bot combinata con i servizi cognitivi.

Viene fornita un'applicazione di esempio, [Dark vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp), che fa proprio questo. In questa applicazione l'utente carica un video o un'immagine utilizzando l'applicazione web Dark Vision, che li memorizza in un database {{site.data.keyword.cloudant_short_notm}}. Una volta caricato il video, {{site.data.keyword.openwhisk_short}} rileva il nuovo video ascoltando le modifiche (trigger) di {{site.data.keyword.cloudant_short_notm}}. {{site.data.keyword.openwhisk_short}} attiva quindi l'azione dell'estrattore video. Durante la sua esecuzione, l'estrattore produce dei frame (immagini) e li memorizza in {{site.data.keyword.cloudant_short_notm}}. I frame vengono quindi elaborati con Watson Visual Recognition e i risultati vengono memorizzati nello stesso database {{site.data.keyword.cloudant_short_notm}}. I risultati possono essere visualizzati utilizzando l'applicazione web Dark Vision o un'applicazione iOS. {{site.data.keyword.cos_full_notm}} può essere utilizzato in aggiunta a {{site.data.keyword.cloudant_short_notm}}, dove i metadati dei video e delle immagini vengono memorizzati in {{site.data.keyword.cloudant_short_notm}} e i file multimediali vengono memorizzati in {{site.data.keyword.cos_short}}.

È disponibile un'[applicazione iOS Swift di esempio](https://github.com/gconan/BluemixMobileServicesDemoApp) che utilizza {{site.data.keyword.openwhisk_short}}, IBM Mobile Analytics e Watson per l'analisi del tono e la pubblicazione in un canale Slack.

## Elaborazione di eventi con Kafka o {{site.data.keyword.messagehub}}
{: #openwhisk_event_processing}

{{site.data.keyword.openwhisk_short}} è ideale per l'utilizzo con Kafka, {{site.data.keyword.messagehub_full}} (basato su Kafka) e altri sistemi di messaggistica. La natura guidata da eventi di tali sistemi richiede un runtime basato sugli eventi per elaborare i messaggi. Il runtime può applicare la logica di business a questi messaggi, che è esattamente ciò che fornisce {{site.data.keyword.openwhisk_short}}, con i suoi feed, trigger e azioni. Kafka e {{site.data.keyword.messagehub}} sono spesso utilizzati per volumi di carichi di lavoro elevati e imprevedibili e richiedono che i consumatori di tali messaggi siano scalabili in qualsiasi momento. Questa situazione è, ancora una volta, un punto ottimale per {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} ha funzionalità integrate per consumare e pubblicare i messaggi forniti nel pacchetto [openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka).

Un'[applicazione di esempio che implementa lo scenario di elaborazione eventi](https://github.com/IBM/openwhisk-data-processing-message-hub) viene fornita con {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.messagehub}} e Kafka.
