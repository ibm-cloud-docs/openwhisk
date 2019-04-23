---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Laufzeiten

## Aktionslaufzeiten
Aktionen können in diversen Programmiersprachen codiert und ausgeführt werden (z. B. Javascript, Python usw.). Die verfügbaren Laufzeitumgebungen werden in den folgenden Abschnitten gezeigt. 

Die folgenden Links geben eine JSON-Antwort zurück, die die verfügbaren Laufzeiten für IBM Cloud-Funktionen in den einzelnen Regionen anzeigt. 

Der Abschnitt `runtimes` der Antwort enthält den Satz von verfügbaren Laufzeiten. 

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

Der Abschnitt `image` enthält den Namen des Laufzeit-Image unter [DockerHub](https://hub.docker.com/) und das verwendete Tag.  

Die folgenden Beispiele verweisen auf die Images `ibmfunctions/action-nodejs-v10` und `openwhisk/nodejs6action`. Die Tags können Versionsnummern wie `1.9.0` oder die Kurzform eines Git-Commit-Hashwerts wie `b99d71e` sein. 

Beispiele für Imagefelder. 
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- Aktionslaufzeiten werden regelmäßig aktualisiert. Diese Aktualisierungen umfassen Sicherheitskorrekturen und Nebenversionsaktualisierungen für die Pakete in den Laufzeiten. Nebenversionsaktualisierungen können zu Unterbrechungen der Abwärtskompatibilität führen. Laufzeitaktualisierungen können sich negativ auf Ihre Aktionen auswirken. Es gibt keine automatische Migration auf eine neuere Laufzeit desselben Typs. 
- Aktionen, die in veralteten Laufzeiten ausgeführt werden, können erst erfolgreich abgeschlossen werden, wenn die Laufzeit auf eine unterstützte Version aktualisiert wird. Suchen Sie im Rahmen der Fehlerbehebung für eine fehlgeschlagene Aktion
in der Abfrageantwort nach `deprecated=true`, um zu ermitteln, ob eine Laufzeitumgebung veraltet ist. Informationen zum Aktualisieren der Laufzeit finden Sie unter [Aktionslaufzeit ändern](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime). 

## JavaScript-Laufzeitumgebungen
{: #openwhisk_ref_javascript_environments}

JavaScript-Aktionen können in Node.js Version 8 oder 10 ausgeführt werden.  

Node.js Version 6 ist die Standardversion, aber seit 6. Dezember 2018 veraltet. Wenn Sie eine JavaScript-Aktion weiterhin verwenden möchten, müssen Sie auf Node.js Version 8 oder 10 aktualisieren.
{: deprecated}

### Umgebung mit Node.js Version 10 und IBM SDKs
{: #openwhisk_ref_javascript_environments_10}
Die Umgebung mit Node.js Version 10 wird verwendet, wenn beim Erstellen oder Aktualisieren einer Aktion das Flag `--kind` explizit mit dem Wert `nodejs:10` angegeben wird. 

#### Migration von `nodejs:8` auf `nodejs:10`
- Das NPM-Paket `ibm_db` ist in `nodejs:10` nicht verfügbar. Das Paket `ibm_db` bietet keine Unterstützung für Node.js 10. Sie können den Fortschritt in der [Ausgabe ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541) nachverfolgen. 
- Das NPM-Paket `cloudant` ist in `nodejs:10` nicht verfügbar, das Paket ist veraltet, Sie müssen das offizielle NPM-Paket [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) Version 3.0.0 verwenden, wenn Sie das nodejs-Modul importieren (d. h. `require('@cloudant/cloudant')`). Außerdem [gibt Version 3.x nur Promises zurück ](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x). 
- Das NPM-Paket `cradle` ist in `nodejs:10` nicht verfügbar. 
- Das NPM-Paket `log4js` ist in `nodejs:10` nicht verfügbar. Sie können das Problem unter [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805) nachverfolgen. 
- Das NPM-Paket `watson-developer-cloud` ist in `nodejs:10` nicht verfügbar. Sie können den Fortschritt bezüglich der neuen Version in diesem Problem nachverfolgen: [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780). 

Detaillierte Informationen zur Laufzeitumgebung mit Node.js Version 10 finden Sie in der Datei [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md). 

### Umgebung mit Node.js Version 8 und IBM SDKs
{: #openwhisk_ref_javascript_environments_8}
Die Umgebung mit Node.js Version 8 wird verwendet, wenn beim Erstellen oder Aktualisieren einer Aktion das Flag `--kind` explizit mit dem Wert `nodejs:8` angegeben wird. 

Node.js Version 8 befindet sich im Wartungsmodus und ist bis Dezember 2019 verfügbar. Weitere Informationen finden Sie im [Node.js-Releaseplan](https://github.com/nodejs/Release).
{: deprecated}
 
Detaillierte Informationen zur Laufzeitumgebung mit Node.js Version 8 finden Sie in der Datei [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md). 

### Umgebung mit Node.js Version 6 (veraltet)
{: #openwhisk_ref_javascript_environments_6}
Node.js Version 6 ist die Standardversion, sie ist aber veraltet. Wenn Sie eine JavaScript-Aktion weiterhin verwenden möchten, müssen Sie auf Node.js Version 8 oder 10 aktualisieren.
{: deprecated}

Detaillierte Informationen zur Laufzeitumgebung mit Node.js Version 6 finden Sie in der Datei [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md). 

## Python-Laufzeitumgebungen
{: #openwhisk_ref_python_environments}

OpenWhisk unterstützt die Ausführung von Python-Aktionen mit zwei verschiedenen Laufzeitversionen.

### Python 3.7-Aktionen (auf Basis von Debian Stretch)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7-Aktionen werden mit Python 3.7.x ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den Parameter `--kind python:3.7` in der `wsk`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren.


Die Laufzeit enthält neben den Standardbibliotheken von Python 3.7 auch SDK-Pakete für IBM Cloud-Services, die zur Verwendung durch Python-Aktionen verfügbar sind. 

Detaillierte Informationen zur Laufzeitumgebung mit Python 3.7 finden Sie in der Datei [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md). 

### Python 3.6-Aktionen (auf Basis von Debian Jessie)
{: #openwhisk_ref_python_environments_3.6}

Python 3-Aktionen werden mit Python 3.6.x ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den Parameter `--kind python:3.6` in der `wsk`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren.


Die Laufzeit enthält neben den Standardbibliotheken von Python 3.6 auch SDK-Pakete für IBM Cloud-Services, die zur Verwendung durch Python-Aktionen verfügbar sind.

Detaillierte Informationen zur Laufzeitumgebung mit Python 3.6 finden Sie in der Datei [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md). 

### Python 2-Aktionen

Python 2-Aktionen werden mit Python 2.7.15 ausgeführt, sofern Sie nicht das Flag `--kind` beim Erstellen oder Aktualisieren einer Aktion angeben. Wenn Sie diese Laufzeit explizit auswählen wollen, verwenden Sie das Flag `--kind python:2`.

Wenn Sie Python-Aktionen mit 'virtualenv' erstellen, verwenden Sie das Docker-Image `openwhisk/python2action`.
Die folgenden Pakete sind neben der Standardbibliothek von Python 2.7 zur Verwendung durch Python 2-Aktionen verfügbar:

Detaillierte Informationen zur Laufzeitumgebung mit Python 2 finden Sie in der Datei [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md). 

## Swift-Aktionen
{: #swift-actions}

Swift 3.1.1- und 4.1-Laufzeiten sind veraltet und bis 28. Februar 2019 verfügbar.
Starten Sie alle neuen Aktionen mit der oder migrieren Sie alle vorhandenen Aktionen zur Swift 4.2-Laufzeit mithilfe von `swift:4.2` und dem neuen Kompilierprozess.
{: tip}

### Swift 3
Swift 3-Aktionen werden mit Swift 3.1.1 (`--kind swift:3.1.1`) ausgeführt. Geben Sie immer `--kind swift:3.1.1` an, da vorherige Swift-Versionen nicht unterstützt werden.

Sie müssen alle Swift-Aktionen zur Verwendung der Art `swift:3.1.1` migrieren. Geben Sie als bewährtes Verfahren immer die bestimmte Art an, wenn Sie Aktionen erstellen oder aktualisieren.
{: tip}

Swift 3.1.1-Aktionen können die folgenden Pakete verwenden, wenn eine einzelne Swift-Quellendatei verwendet wird:
- KituraNet Version 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON Version 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK Version 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4-Aktionen können mithilfe von Swift 4.1 oder 4.2 unter Verwendung von `--kind swift:4.1` bzw. `--kind swift:4.2` ausgeführt werden.
Der Standardwert von `--kind swift:default` ist Swift 4.2. 

Swift 4.x-Aktionslaufzeiten betten keine Pakete ein; befolgen Sie die Anweisungen für [gepackte Swift-Aktionen](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable), um Abhängigkeiten mit einer Datei 'Package.swift' hinzuzufügen. 

Swift 4.1-Aktionen können die folgenden Pakete verwenden, wenn eine einzelne Swift-Quellendatei verwendet wird:
- Watson Developer Cloud SDK Version 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

Swift 4.2-Aktionen können die folgenden Pakete verwenden, wenn eine einzelne Swift-Quellendatei verwendet wird:
- Watson Developer Cloud SDK Version 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 3.1.1 auf Swift 4.1 migrieren

#### SwiftyJSON mit einer einzelnen Quellenaktionsdatei
Wenn Sie eine `swift:3.1.1`-Aktion haben, die nicht kompiliert ist  und das Paket **SwiftyJSON** verwendet, müssen Sie Ihre Aktion vorkompilieren und die Version von SwiftyJSON angeben, die Sie für die `swift:4.2`-Aktionsart verwenden wollen. Wenn Sie mit Swift 4.1 beginnen, stehen für die Verwaltung von JSON-Daten Verbesserungen zur Verfügung. 

## PHP-Aktionen
{: #openwhisk_ref_php}

PHP 7.1 und 7.2 sind ab dem 11. Januar 2019 veraltet. Wenn Sie eine PHP-Aktion weiterhin verwenden möchten, aktualisieren Sie auf PHP 7.3.
{: deprecated}

PHP-Aktionen werden mit PHP 7.3.0 ausgeführt. Zur Verwendung dieser Laufzeit geben Sie den Parameter `--kind php:7.3` in der `wsk`-Befehlszeilenschnittstelle an, wenn Sie eine Aktion erstellen oder aktualisieren.
Dies ist das Standardverhalten, wenn Sie eine Aktion mit einer Datei erstellen, die die Erweiterung `.php` hat.

Die PHP 7.1- und 7.2-Laufzeiten sind veraltet. Migrieren Sie alle Aktionen zu PHP 7.3, um die Latenzzeit zu verbessern und die End-to-End-Ausführung zu beschleunigen. 

Die folgenden PHP-Erweiterungen sind zusätzlich zu den Standarderweiterungen verfügbar:

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Docker-Aktionen
{: #openwhisk_ref_docker}

Docker-Aktionen werden in einer vom Benutzer bereitgestellten Binärdatei in einem Docker-Container ausgeführt. Die Binärdatei wird in einem Docker-Image auf der Basis von [python:3.6-alpine](https://hub.docker.com/r/library/python) ausgeführt, sodass die Binärdatei mit dieser Distribution kompatibel sein muss. 

Das Docker-Gerüst (Skeleton) ist eine bequeme Methode, OpenWhisk-kompatible Docker-Images zu erstellen. Sie können das Gerüst mit dem CLI-Plug-in-Befehl `ibmcloud fn sdk install docker` installieren. 

Das Hauptbinärprogramm muss sich in `/action/exec` im Container befinden. Die ausführbare Datei empfängt die Eingabeargumente von einer einzelnen Befehlszeilenargumentzeichenfolge, die als `JSON`-Objekt deserialisiert werden kann. Es muss ein Ergebnis über `stdout` in Form einer einzeiligen Zeichenfolge aus serialisierten `JSON`-Daten zurückgeben.

Sie können darüber hinaus auch Kompilierungsschritte oder Abhängigkeiten einbeziehen, indem Sie die `Dockerfile` ändern, die in `dockerSkeleton` enthalten ist.
