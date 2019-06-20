---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

keywords: serverless, framework, getting started

subcollection: cloud-functions

---





{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Serverless Framework
{: #openwhisk_goserverless}

[Serverless Framework](https://serverless.com/) è un framework open-source per la creazione di applicazioni senza server. Utilizzando un semplice file manifest, gli sviluppatori possono definire funzioni senza server, collegarle alle origini eventi e dichiarare i servizi cloud necessari alla loro applicazione. Il framework gestisce la distribuzione di queste applicazioni senza server ai provider cloud. Inoltre, consente agli sviluppatori di monitorare i servizi in produzione, presentare aggiornamenti e risolvere problemi di debug. Ha anche un vivace ecosistema di plug-in di terze parti per estendere la funzionalità del framework. OpenWhisk è la tecnologia che guida il Serverless Framework.
{:shortdesc}

OpenWhisk ha [il suo proprio plug-in del provider per Serverless Framework](https://github.com/serverless/serverless-openwhisk). Gli sviluppatori che utilizzano il Serverless Framework possono scegliere di distribuire le proprie applicazioni su qualsiasi istanza della piattaforma OpenWhisk (ospitata su {{site.data.keyword.Bluemix_notm}}, cloud o privato). Il supporto di più provider significa anche che lo spostamento di un'applicazione tra le piattaforme è molto più semplice, consentendo agli sviluppatori di sviluppare applicazioni senza server multi-cloud.

## Introduzione
{: #openwhisk_goserverless_starting}

[Guida introduttiva ufficiale di Serverless Framework per OpenWhisk](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/).
* Una guida che descrive l'installazione, il flusso di lavoro di sviluppo, le procedure consigliate, i passi dettagliati per la creazione e la distribuzione di un'applicazione OpenWhisk funzionante e altro ancora.

Guarda [The Serverless Framework and OpenWhisk](https://youtu.be/GJY10W98Itc), un video che spiega come utilizzare il Serverless Framework con il plug-in del provider OpenWhisk.

## Documentazione
{: #openwhisk_goserverless_docs}

La documentazione più recente su come utilizzare OpenWhisk con il Serverless Framework [è disponibile qui](https://serverless.com/framework/docs/providers/openwhisk/).

## Esempi
{: #openwhisk_goserverless_samples}
[Il repository GitHub di esempi Serverless Framework](https://github.com/serverless/examples) ora presenta OpenWhisk che mostra come creare API HTTP, scheduler basati su cron, funzioni di concatenamento e molto altro.

