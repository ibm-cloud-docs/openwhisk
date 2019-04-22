---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

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

[Serverless Framework](https://serverless.com/) est une infrastructure open source pour construction d'applications sans serveur. A l'aide d'un simple fichier manifeste, les développeurs peuvent définir des fonctions sans serveur, les connecter à des sources d'événements et déclarer les services de cloud qui sont requis par leur application. L'infrastructure gère le déploiement de ces applications sans serveur vers les fournisseurs de cloud. Elle permet également aux développeurs de surveiller des services en environnement de production, de diffuser des mises à jour et d'aider au débogage de problèmes. Elle dispose aussi d'un écosystème dynamique de plug-ins tiers venant étendre les fonctionnalités de l'infrastructure. OpenWhisk est la technologie sur laquelle repose Serverless Framework.
{:shortdesc}

OpenWhisk a [son propre plug-in de fournisseur pour Serverless Framework](https://github.com/serverless/serverless-openwhisk). Les développeurs utilisant l'infrastructure Serverless Framework peuvent choisir de déployer leurs applications vers n'importe quelle instance de la plateforme OpenWhisk (hébergée sur {{site.data.keyword.Bluemix_notm}}, un autre cloud ou un registre privé). La prise en charge de fournisseurs divers signifie également que le portage d'applications entre plateformes est beaucoup plus facile et que les développeurs peuvent développer des applications sans serveur multi-cloud.

## Guide d'initiation
{: #openwhisk_goserverless_starting}

Document officiel de l'infrastructure Serverless Framework : [Getting Started Guide for OpenWhisk](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/).
* Couvre l'installation, le flux de travaux de développement, les pratiques recommandées, les instructions pas à pas pour la construction et le déploiement d'une application OpenWhisk fonctionnelle, et plus.

Regardez la vidéo [The Serverless Framework and OpenWhisk](https://youtu.be/GJY10W98Itc) décrivant comment utiliser l'infrastructure Serverless Framework avec le plug-in de fournisseur OpenWhisk.

## Documentation
{: #openwhisk_goserverless_docs}

[Cliquez ici](https://serverless.com/framework/docs/providers/openwhisk/) pour accéder à la documentation la plus récente sur l'utilisation d'OpenWhisk avec Serverless Framework.

## Exemples
{: #openwhisk_goserverless_samples}
[Les exemples Serverless Framework dans le référentiel GitHub](https://github.com/serverless/examples) illustrent comment utiliser OpenWhisk pour construire des API HTTP, des planificateurs périodiques, des fonctions de chaînage, et plus.
