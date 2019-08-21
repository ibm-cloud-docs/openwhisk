---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: use cases, microservices, web apps, iot, serverless, cognitive, serverless, functions

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


# Häufige Anwendungsfälle
{: #use_cases}

Das von {{site.data.keyword.openwhisk_short}} angebotene Ausführungsmodell unterstützt verschiedene Anwendungsfälle. In den folgenden Abschnitten werden typische Beispiele beschrieben. Eine ausführlichere Beschreibung der serverunabhängigen Architektur (Serverless Architecture), Beispielanwendungsfälle, eine Erörterung der Vor- und Nachteile sowie bewährte Verfahren zur Implementierung finden Sie in dem hervorragenden [Artikel von Mike Roberts im Blog von Martin Fowler (englisch)](https://martinfowler.com/articles/serverless.html){: external}.
{: shortdesc}

## Microservices
{: #use_cases_microservices}

Trotz ihrer Vorteile bleiben auf Microservices basierende Lösungen in ihrer Erstellung mit gängigen Cloudtechnologien schwierig, da sie häufig die Steuerung einer komplexen Toolchain sowie separate Pipelines für Buildoperationen und Betrieb erfordern. Insbesondere kleine und agile Teams verschwenden sehr viel Zeit für komplexe Probleme der Infrastruktur und des Betriebs, wie zum Beispiel Fehlertoleranz, Lastverteilung, automatische Skalierung und Protokollierung. Solche Teams wünschen sich eine Möglichkeit, optimierten und wertsteigernden Code mit ihren bekannten und bevorzugten Programmiersprachen und mit Programmiersprachen, die am besten zur Lösung bestimmter Probleme geeignet sind, entwickeln zu können.

Durch seinen modularen und inhärent skalierbaren Aufbau eignet sich {{site.data.keyword.openwhisk_short}} ideal für die Implementierung differenzierter Teile von Logik in Aktionen. {{site.data.keyword.openwhisk_short}}-Aktionen sind voneinander unabhängig und können in einer Reihe verschiedener Sprachen, die von {{site.data.keyword.openwhisk_short}} unterstützt werden, implementiert werden und auf verschiedene Back-End-Systeme zugreifen. Jede Aktion kann unabhängig bereitgestellt und verwaltet werden und lässt sich unabhängig von anderen Aktionen skalieren. Die Interkonnektivität zwischen Aktionen wird durch {{site.data.keyword.openwhisk_short}} in Form von Regeln, Sequenzen und Namenskonventionen realisiert. Dieser Typ von Umgebung bietet gute Voraussetzungen für Anwendungen, die auf Microservices basieren, gute Voraussetzungen.

Ein weiteres wichtiges Argument für {{site.data.keyword.openwhisk_short}} ist der Systemaufwand bei einer Disaster-Recovery-Konfiguration. Betrachten Sie Mikroservices mit PaaS oder CaaS im Vergleich zur Verwendung von {{site.data.keyword.openwhisk_short}}. Nehmen Sie einmal an, dass Sie 10 Mikroservices haben, die Container oder Cloud Foundry-Laufzeiten verwenden. Dies entspricht 10 kontinuierlich ausgeführten und fakturierbaren Prozessen in einer einzigen Verfügbarkeitszone, 20 Prozessen bei Ausführung in zwei Verfügbarkeitszonen und 40 Prozessen bei Ausführung in zwei Regionen mit jeweils zwei Zonen. Um dasselbe Ziel mit {{site.data.keyword.openwhisk_short}} zu erreichen, können Sie die Mikroservices in beliebig vielen Verfügbarkeitszonen und Regionen ausführen, ohne dass hierdurch zusätzliche Kosten entstehen. 

## Web-Apps
{: #use_cases_webapps}

Aufgrund der ereignisgesteuerten Spezifik von {{site.data.keyword.openwhisk_short}} bietet das Produkt einige Vorteile für Anwendungen mit Benutzerinteraktion, bei denen die HTTP-Anforderungen, die aus dem Browser des Benutzers stammen, als Ereignisse dienen. {{site.data.keyword.openwhisk_short}}-Anwendungen verwenden Rechenkapazität und werden nur dann in Rechnung gestellt, wenn sie Benutzeranforderungen bedienen. Es gibt weder ein Idle Standby noch einen Wartemodus. Dieses Feature macht {{site.data.keyword.openwhisk_short}} deutlich kostengünstiger als traditionelle Container oder Cloud Foundry-Anwendungen. Bei beiden dieser Tools tritt Leerlaufzeit auf, wenn sie auf eingehende Benutzeranforderungen warten, und diese gesamte Leerlaufzeit wird Ihnen in Rechnung gestellt. 

Mit {{site.data.keyword.openwhisk_short}} können vollständige Webanwendungen erstellt und ausgeführt werden. Die Kombination von serverunabhängigen APIs mit der Bereitstellung statischer Dateien für Siteressourcen, wie zum Beispiel HTML-, JavaScript- und CSS-Dateien, ermöglicht die Erstellung vollständig serverunabhängiger Webanwendungen. Das Einfache am Betrieb einer gehosteten {{site.data.keyword.openwhisk_short}}-Umgebung besteht darin, überhaupt nichts selbst betreiben zu müssen. Dass {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.cloud_notm}} gehostet wird, ist ein großer Vorteil im Vergleich zu Betrieb einer traditionellen Serverlaufzeit wie Node.js Express in eigener Regie.

## IoT (Internet der Dinge)
{: #use_cases_iot}

IoT-Szenarios sind ihrer Spezifik nach häufig sensorgesteuert. Zum Beispiel könnte eine Aktion in {{site.data.keyword.openwhisk_short}} ausgelöst werden, wenn es erforderlich ist, auf einen Sensor zu reagieren, der eine bestimmte Temperatur überschreitet. IoT-Interaktionen sind in der Regel statusunabhängig mit dem Potenzial für sehr hohe Belastungen im Fall größerer Ereignisse (Naturkatastrophen, schwerwiegende Wetterereignisse, Verkehrsstaus, usw.). Dies macht ein elastisches System erforderlich, dessen normale Auslastung möglicherweise gering ist, das sich jedoch sehr schnell mit vorhersagbarer Antwortzeit skalieren lassen muss. Daher ist die Fähigkeit wünschenswert, zahlreiche Simultanereignisse ohne Vorwarnung an das System verarbeiten zu können. Es ist schwierig, ein System unter Verwendung traditioneller Serverarchitekturen aufzubauen, das diese Anforderungen erfüllt. Solche Systeme verfügen entweder nicht über die entsprechende Leistung, um Lastspitzen des Datenverkehrs auffangen zu können, oder sind mit solchen Überkapazitäten ausgestattet, dass sie extrem hohe Kosten verursachen. 

Es ist möglich, IoT-Anwendungen zu implementieren, die traditionelle Serverarchitekturen nutzen. In vielen Fällen erfordert die Kombination verschiedener Services und Datenbridges eine sehr hohe Leistung und flexible Pipelines. Dies beginnt bei IoT-Geräten, setzt sich über Cloudspeicher fort und reicht bis hin zu einer Analyseplattform. Bridges, die häufig vorkonfiguriert sind, bieten nicht die Programmierbarkeit, die zur Implementierung und Feinabstimmung einer bestimmten Lösungsarchitektur benötigt wird. Angesichts der Auswahl an Pipelines und der fehlenden Standardisierung im Bereich der Datenfusion im Allgemeinen und im IoT-Bereich im Besonderen, erfordert die Pipeline in vielen Fällen eine angepasste Datenumsetzung. Diese angepasste Datenumsetzung gilt für die Formatkonvertierung, die Filterung und die Erweiterungen. {{site.data.keyword.openwhisk_short}} ist ein hervorragendes Tool zur Implementierung einer solchen Transformation in einer 'serverunabhängigen' Weise, bei der die angepasste Logik in einer vollständig verwalteten und elastischen Cloudplattform bereitgestellt wird.

Betrachten Sie das folgende Beispiel für eine IoT-Anwendung, die {{site.data.keyword.openwhisk_short}}, Node-RED, Cognitive und andere Services verwendet: [Serverunabhängige Transformation von bewegten IoT-Daten mit {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c){: external}. 

![Beispiel für die Architektur einer IoT-Lösung](images/IoT_solution_architecture_example.png)

## API-Back-End
{: #use_cases_backend}

Serverunabhängige IT-Plattformen bieten Entwicklern eine schnelle Möglichkeit zum Erstellen von APIs ohne Server. {{site.data.keyword.openwhisk_short}} unterstützt die automatische Generierung von REST-APIs für Aktionen. Das [{{site.data.keyword.openwhisk_short}}-Feature](/docs/openwhisk?topic=cloud-functions-apigateway) kann eine Aktion mit anderen HTTP-Methoden als POST und ohne den Berechtigungs-API-Schlüssel der Aktion über das {{site.data.keyword.openwhisk_short}}-API-Gateway aufrufen. Diese Fähigkeit ist nicht nur zur Bereitstellung von APIs für externe Konsumenten, sondern auch zur Erstellung von Mikroserviceanwendungen hilfreich.

Außerdem können {{site.data.keyword.openwhisk_short}}-Aktionen mit einem API-Management-Tool Ihrer Wahl verbunden werden (z. B. [IBM API Connect](https://www.ibm.com/cloud/api-connect){: external} oder ein anderes Tool). Ähnlich wie für andere Anwendungsfälle gelten auch hier alle Gesichtspunkte der Skalierbarkeit und anderer Aspekte der Servicequalität. 

[Emoting](https://github.com/IBM-Cloud/openwhisk-emoting){: external} ist eine Beispielapp, die {{site.data.keyword.openwhisk_short}}-Aktionen durch eine REST-API verwendet.

Schauen Sie sich das folgende Beispiel an, das eine Beschreibung der [Verwendung einer serverunabhängigen Plattform als API-Back-End](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples){: external} enthält.

## Mobile-Back-End
{: #use_cases_mobile}

Viele mobile Anwendungen erfordern serverseitige Logik. Entwickler mobiler Anwendungen besitzen jedoch für gewöhnlich keine Erfahrungen mit der Verwaltung serverseitiger Logik und konzentrieren sich eher auf die App, die auf dem Gerät ausgeführt wird. Dieses Entwicklungsziel lässt sich mithilfe von {{site.data.keyword.openwhisk_short}} als serverseitigem Back-End leicht und gut umsetzen. Darüber hinaus bietet die integrierte Unterstützung für Swift auf der Serverseite den Entwicklern die Möglichkeit, von vorhandenen iOS-Programmierkenntnissen erneut zu profitieren. Da mobile Anwendungen oft unvorhersagbare Auslastungsmuster aufweisen, können Sie eine {{site.data.keyword.openwhisk_short}}-Lösung wie {{site.data.keyword.cloud}} nutzen. Diese Lösung lässt sich praktisch für alle Auslastungsanforderungen skalieren, ohne dass Ressourcen im Voraus bereitgestellt werden müssen.

## Datenverarbeitung
{: #use_cases_data}

Angesichts der gegenwärtig verfügbaren Datenmengen erfordert die Anwendungsentwicklung die Fähigkeit, neue Daten zu verarbeiten und potenziell darauf zu reagieren. Diese Anforderung schließt die Verarbeitung sowohl strukturierter Datenbanksätze als auch nicht strukturierter Dokumente, Bilder und Videos mit ein. {{site.data.keyword.openwhisk_short}} kann durch vom System bereitgestellte oder angepasste Feeds konfiguriert werden, um auf Änderungen in Daten zu reagieren und automatisch Aktionen an den eingehenden Datenfeeds ausführen. Aktionen können programmiert werden, um Änderungen zu verarbeiten, Datenformate umzusetzen, Nachrichten zu senden und zu empfangen, andere Aktionen aufzurufen sowie verschiedene Datenspeicher zu aktualisieren. Unterstützte Datenspeicher sind SQL-basierte relationale Datenbanken, speicherinterne Datengitter, NoSQL-Datenbanken, Dateien, Nachrichtenbroker und verschiedene andere Systeme. {{site.data.keyword.openwhisk_short}}-Regeln und -Sequenzen bieten die Flexibilität, Änderungen in der Verarbeitungspipeline ganz ohne Programmierungsaufwand, sondern durch einfache Konfigurationsänderungen, vornehmen zu können. Dank der Datenspeicheroptionen und des niedrigen Verwaltungsaufwands ist ein {{site.data.keyword.openwhisk_short}}-basiertes System ein höchst agiles System, das sich problemlos an neue Anforderungen anpassen lässt. 

## Cognitive
{: #use_cases_cognitive}

Kognitive Technologien (Cognitive Computing) lassen sich effektiv mit {{site.data.keyword.openwhisk_short}} zu leistungsfähigen Anwendungen kombinieren. Zum Beispiel können IBM Alchemy API und Watson Visual Recognition zusammen mit {{site.data.keyword.openwhisk_short}} eingesetzt werden, um automatisch nützliche Informationen aus Videos zu extrahieren, ohne diese ansehen zu müssen. Diese Technologie ist die 'kognitive' Erweiterung des bereits erläuterten Anwendungsfalls für die [Datenverarbeitung](#use_cases_data). Eine weitere sinnvolle Einsatzmöglichkeit von {{site.data.keyword.openwhisk_short}} ist die Implementierung einer Botfunktion in Kombination mit kognitiven Services.

Eine Beispielanwendung mit dem Namen [Dark Vision](https://github.com/IBM-cloud/openwhisk-darkvisionapp){: external} wird bereitgestellt und leistet genau dies. In dieser Anwendung lädt der Benutzer ein Video oder Bild mit der Webanwendung Dark Vision hoch, die es in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank speichert. Wenn das Video hochgeladen ist, erkennt {{site.data.keyword.openwhisk_short}} durch die Empfangsbereitschaft für {{site.data.keyword.cloudant_short_notm}}-Änderungen (Auslöser) das neue Video. {{site.data.keyword.openwhisk_short}} löst anschließend die Aktion der Videoextraktion aus. Während der Ausführung generiert der Extraktor Rahmen (Bilder) und speichert diese in {{site.data.keyword.cloudant_short_notm}}. Die Rahmen werden anschließend mit Watson Visual Recognition verarbeitet und die Ergebnisse in derselben {{site.data.keyword.cloudant_short_notm}}-Datenbank gespeichert. Die Ergebnisse können mithilfe der Webanwendung Dark Vision oder einer iOS-Anwendung angezeigt werden. Neben {{site.data.keyword.cloudant_short_notm}} kann {{site.data.keyword.cos_full_notm}} verwendet werden. In diesem Fall werden Video- und Bildmetadaten in {{site.data.keyword.cloudant_short_notm}} gespeichert, während die Mediendateien in {{site.data.keyword.cos_full_notm}} gespeichert werden.

## Ereignisverarbeitung mit Kafka oder {{site.data.keyword.messagehub}}
{: #use_cases_events}

{{site.data.keyword.openwhisk_short}} eignet sich ideal zur Kombination mit Kafka, mit dem Service von {{site.data.keyword.messagehub_full}} (Kafka-basiert) und mit anderen Nachrichtensystemen. Die ereignisgesteuerte Funktionsweise dieser Systeme erfordert eine ereignisgesteuerte Laufzeit zur Verarbeitung von Nachrichten. Die Laufzeit kann Geschäftslogik auf diese Nachrichten anwenden und eben diese Funktionalität wird durch {{site.data.keyword.openwhisk_short}} mithilfe von Feeds, Auslösern und Aktionen bereitgestellt. Kafka und {{site.data.keyword.messagehub}} werden häufig für hohe und unvorhersehbare Auslastungsvolumen genutzt und setzen voraus, dass Konsumenten dieser Nachrichten praktisch auf Zuruf skalierbar sind. Dies ist wiederum ein optimaler Einsatzbereich für {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} verfügt über integrierte Funktionen zur Verarbeitung von Nachrichten und zur Veröffentlichung von Nachrichten, die im Paket [Event Streams](/docs/openwhisk?topic=cloud-functions-pkg_event_streams) bereitgestellt werden. 



