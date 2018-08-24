---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Häufige Anwendungsfälle
{: #openwhisk_common_use_cases}

Das von {{site.data.keyword.openwhisk_short}} angebotene Ausführungsmodell unterstützt verschiedene Anwendungsfälle. In den folgenden Abschnitten werden typische Beispiele beschrieben. Eine ausführlichere Beschreibung der serverunabhängigen Architektur (Serverless Architecture), Beispielanwendungsfälle, eine Erörterung der Vor- und Nachteile sowie bewährte Verfahren zur Implementierung finden Sie in dem hervorragenden [Artikel von Mike Roberts im Blog von Martin Fowler (englisch)](https://martinfowler.com/articles/serverless.html).
{: shortdesc}

## Microservices
{: #openwhisk_common_use_cases_microservices}

Trotz ihrer Vorteile bleiben auf Microservices basierende Lösungen in ihrer Erstellung mit gängigen Cloudtechnologien schwierig, da sie häufig die Steuerung einer komplexen Toolchain sowie separate Pipelines für Buildoperationen und Betrieb erfordern. Insbesondere kleine und agile Teams wenden sehr viel Zeit für Fragen der komplexen Infrastruktur und des Betriebs, wie zum Beispiel Fehlertoleranz, Lastverteilung, automatische Skalierung und Protokollierung, auf. Solche Teams wünschen sich eine Möglichkeit, optimierten und wertsteigernden Code mit ihren bekannten und bevorzugten Programmiersprachen und mit Programmiersprachen, die am besten zur Lösung bestimmter Probleme geeignet sind, entwickeln zu können.

Durch seinen modularen und inhärent skalierbaren Aufbau eignet sich {{site.data.keyword.openwhisk_short}} ideal für die Implementierung differenzierter Teile von Logik in Aktionen. {{site.data.keyword.openwhisk_short}}-Aktionen sind voneinander unabhängig und können in einer Reihe verschiedener Sprachen, die von {{site.data.keyword.openwhisk_short}} unterstützt werden, implementiert werden und auf verschiedene Back-End-Systeme zugreifen. Jede Aktion kann unabhängig bereitgestellt und verwaltet werden und lässt sich unabhängig von anderen Aktionen skalieren. Die Interkonnektivität zwischen Aktionen wird durch {{site.data.keyword.openwhisk_short}} in Form von Regeln, Sequenzen und Namenskonventionen realisiert. Dieser Typ von Umgebung bietet gute Voraussetzungen für Anwendungen, die auf Microservices basieren, gute Voraussetzungen.

Ein weiteres wichtiges Argument für {{site.data.keyword.openwhisk_short}} ist der Systemaufwand bei einer Disaster-Recovery-Konfiguration. Betrachten Sie zum Beispiel Mikroservices mit PaaS oder CaaS im Vergleich zur Verwendung von {{site.data.keyword.openwhisk_short}}. Nehmen Sie einmal an, dass Sie 10 Mikroservices haben, die Container oder CloudFoundry-Laufzeiten verwenden. Dies entspricht 10 kontinuierlich ausgeführten und fakturierbaren Prozessen in einer einzelnen Verfügbarkeitszone, 20 Prozessen bei Ausführung in zwei Verfügbarkeitszonen und 40 Prozessen bei Ausführung in zwei Regionen mit jeweils zwei Zonen. Um dasselbe Ziel mit {{site.data.keyword.openwhisk_short}} zu erreichen, können Sie sie Mikroservices in so vielen Verfügbarkeitszonen und Regionen ausführen, wie Sie möchten, ohne dass hierdurch zusätzliche Kosten entstehen.

[Logistics Wizard](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/) ist eine auf Unternehmen abgestimmte Beispielanwendung, die {{site.data.keyword.openwhisk_short}} und CloudFoundry verwendet, um eine 12-Faktor-Anwendung zu erstellen. In dieser intelligenten Supply-Chain-Management-Lösung wird eine Umgebung simuliert, die ein ERP-System ausführt. Dieses ERP-System wird hierbei durch Anwendungen erweitert, die die Sichtbarkeit und geschäftliche Flexibilität der Supply-Chain-Manager verbessern.

## Web-Apps
{: #openwhisk_common_use_cases_webapps}

Aufgrund der ereignisgesteuerten Spezifik von {{site.data.keyword.openwhisk_short}} bietet das Produkt einige Vorteile für Anwendungen mit Benutzerinteraktion, bei denen die HTTP-Anforderungen, die aus dem Browser des Benutzers stammen, als Ereignisse dienen. {{site.data.keyword.openwhisk_short}}-Anwendungen verwenden Rechenkapazität und werden nur dann in Rechnung gestellt, wenn sie Benutzeranforderungen bedienen. Es gibt weder ein Idle Standby noch einen Wartemodus. Dieses Feature macht {{site.data.keyword.openwhisk_short}} deutlich günstiger als konventionelle Container oder Cloud Foundry-Anwendungen. Diese können beide einen Großteil der Zeit im Leerlauf auf eingehende Benutzeranforderungen warten und diese gesamte 'Ruhezeit' wird auch in Rechnung gestellt.

Mit {{site.data.keyword.openwhisk_short}} können vollständige Webanwendungen erstellt und ausgeführt werden. Die Kombination von serverunabhängigen APIs mit der Bereitstellung statischer Dateien für Siteressourcen, wie zum Beispiel HTML-, JavaScript- und CSS-Dateien, ermöglicht die Erstellung vollständig serverunabhängiger Webanwendungen. Das Einfache am Betrieb einer gehosteten {{site.data.keyword.openwhisk_short}}-Umgebung besteht darin, überhaupt nichts selbst betreiben zu müssen. Dass {{site.data.keyword.openwhisk_short}} in {{site.data.keyword.Bluemix_notm}} gehostet wird, ist ein großer Vorteil im Vergleich zu Betrieb einer traditionellen Serverlaufzeit wie Node.js Express in eigener Regie.

Die folgenden Beispiele zeigen, wie {{site.data.keyword.openwhisk_short}} zum Erstellen einer Web-App verwendet werden kann:
- [Webaktionen: Serverunabhängige Web-Apps mit {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [{{site.data.keyword.openwhisk_short}}-Anwendung mit Benutzerinteraktion mit {{site.data.keyword.Bluemix_notm}} und Node.js erstellen](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [Serverunabhängige HTTP-Handler mit {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT (Internet der Dinge)
{: #openwhisk_common_use_cases_iot}

IoT-Szenarios sind ihrer Spezifik nach häufig sensorgesteuert. Zum Beispiel könnte eine Aktion in {{site.data.keyword.openwhisk_short}} ausgelöst werden, wenn es erforderlich ist, auf einen Sensor zu reagieren, der eine bestimmte Temperatur überschreitet. IoT-Interaktionen sind in der Regel statusunabhängig mit dem Potenzial für sehr hohe Belastungen im Fall größerer Ereignisse (Naturkatastrophen, schwerwiegende Wetterereignisse, Verkehrsstaus, usw.). Dies macht ein elastisches System erforderlich, dessen normale Auslastung möglicherweise gering ist, das sich jedoch sehr schnell mit vorhersagbarer Antwortzeit skalieren lassen muss. Daher ist die Fähigkeit wünschenswert, zahlreiche Simultanereignisse ohne Vorwarnung an das System verarbeiten zu können. Es ist schwierig, ein System unter Verwendung traditioneller Serverarchitekturen aufzubauen, das diese Anforderungen erfüllt. Solche Systeme verfügen entweder nicht über die entsprechende Leistung, um Lastspitzen des Datenverkehrs auffangen zu können, oder sind mit solchen Überkapazitäten ausgestattet, dass sie extrem hohe Kosten verursachen.

Es ist möglich, IoT-Anwendungen zu implementieren, die traditionelle Serverarchitekturen nutzen. In vielen Fällen erfordert die Kombination verschiedener Services und Datenbridges eine sehr hohe Leistung und flexible Pipelines. Dies beginnt bei IoT-Geräten, setzt sich über Cloudspeicher fort und reicht bis hin zu einer Analyseplattform. Bridges, die häufig vorkonfiguriert sind, bieten nicht die Programmierbarkeit, die zur Implementierung und Feinabstimmung der Architektur einer bestimmten Lösung wünschenswert ist. Angesichts der Auswahl an Pipelines und der fehlenden Standardisierung im Bereich der Datenfusion im Allgemeinen und im IoT-Bereich im Besonderen, erfordert die Pipeline in vielen Fällen eine angepasste Datenumsetzung. Diese angepasste Datenumsetzung gilt für die Formatkonvertierung, die Filterung und die Erweiterungen. {{site.data.keyword.openwhisk_short}} ist ein hervorragendes Tool zur Implementierung einer solchen Transformation in einer 'serverunabhängigen' Weise, bei der die angepasste Logik in einer vollständig verwalteten und elastischen Cloudplattform bereitgestellt wird.

Betrachten Sie das folgende Beispiel für eine IoT-Anwendung, die {{site.data.keyword.openwhisk_short}}, NodeRed, Cognitive und andere Services verwendet: [Serverunabhängige Transformation von bewegten IoT-Daten mit {{site.data.keyword.openwhisk_short}}](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c#.akt3ocjdt).

![Beispiel für die Architektur einer IoT-Lösung](images/IoT_solution_architecture_example.png)

## API-Back-End
{: #openwhisk_api_backend}

Serverunabhängige IT-Plattformen bieten Entwicklern eine schnelle Möglichkeit zum Erstellen von APIs ohne Server. {{site.data.keyword.openwhisk_short}} unterstützt die automatische Generierung von REST-APIs für Aktionen. Die [experimentelle Funktion](./openwhisk_apigateway.html) von {{site.data.keyword.openwhisk_short}} ermöglicht Ihnen den Aufruf einer Aktion mit anderen HTTP-Methoden als POST und ohne den Berechtigungs-API-Schlüssel der Aktion über das {{site.data.keyword.openwhisk_short}}-API-Gateway. Diese Fähigkeit ist nicht nur zur Bereitstellung von APIs für externe Konsumenten, sondern auch zur Erstellung von Mikroserviceanwendungen hilfreich.

Außerdem können {{site.data.keyword.openwhisk_short}}-Aktionen mit einem API-Management-Tool Ihrer Wahl verbunden werden (z. B. [IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) oder ein anderes Tool). Ähnlich wie für andere Anwendungsfälle gelten auch hier alle Gesichtspunkte der Skalierbarkeit und anderer Aspekte der Servicequalität (QoS).

[Emoting](https://github.com/l2fprod/openwhisk-emoting) ist eine Beispielapp, die {{site.data.keyword.openwhisk_short}}-Aktionen durch eine REST-API verwendet.

Schauen Sie sich das folgende Beispiel an, das eine Beschreibung der [Verwendung einer serverunabhängigen Plattform als API-Back-End](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples) enthält.

## Mobile-Back-End
{: #openwhisk_common_use_cases_mobile}

Viele mobile Anwendungen erfordern serverseitige Logik. Entwickler mobiler Anwendungen besitzen jedoch für gewöhnlich keine Erfahrungen mit der Verwaltung serverseitiger Logik und konzentrieren sich eher auf die App, die auf dem Gerät ausgeführt wird. Dieses Entwicklungsziel lässt sich mithilfe von {{site.data.keyword.openwhisk_short}} als serverseitigem Back-End leicht und gut umsetzen. Darüber hinaus bietet die integrierte Unterstützung für Swift auf der Serverseite den Entwicklern die Möglichkeit, von vorhandenen iOS-Programmierkenntnissen erneut zu profitieren. Da mobile Anwendungen oft unvorhersagbare Auslastungsmuster aufweisen, können Sie eine {{site.data.keyword.openwhisk_short}}-Lösung wie {{site.data.keyword.Bluemix}} nutzen. Diese Lösung lässt sich praktisch für alle Auslastungsanforderungen skalieren, ohne dass Ressourcen im Voraus bereitgestellt werden müssen.

[Skylink](https://github.com/IBM-Bluemix/skylink) ist eine Beispielanwendung, mit der Sie eine Flugdrohne über iPad mit der IBM Cloud verbinden und unter Verwendung von {{site.data.keyword.openwhisk_short}}, IBM {{site.data.keyword.cloudant_short_notm}}, IBM Watson und Alchemy Vision die Bilder quasi in Echtzeit analysieren können.

[BluePic](https://github.com/IBM-Swift/BluePic) ist eine Anwendung für die gemeinsame Nutzung von Fotos und Bildern, mit der Sie Fotos aufnehmen und mit anderen BluePic-Benutzern teilen können. Diese Anwendung veranschaulicht, wie eine Kitura-basierte und in Swift geschriebene Serveranwendung, die {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.cloudant_short_notm}} und {{site.data.keyword.cos_full_notm}} nutzt, in einer mobilen iOS 10-Anwendung für Bilddaten verwendet wird. AlchemyAPI wird auch in der {{site.data.keyword.openwhisk_short}}-Sequenz eingesetzt, um das Bild zu analysieren, Texttags basierend auf dem Inhalt des Bildes zu extrahieren und schließlich eine Push-Benachrichtigung an den Benutzer zu senden.

## Datenverarbeitung
{: #data-processing}

Angesichts der gegenwärtig verfügbaren Datenmengen erfordert die Anwendungsentwicklung die Fähigkeit, neue Daten zu verarbeiten und potenziell darauf zu reagieren. Diese Anforderung schließt die Verarbeitung sowohl strukturierter Datenbanksätze als auch nicht strukturierter Dokumente, Bilder und Videos mit ein. {{site.data.keyword.openwhisk_short}} kann durch vom System bereitgestellte oder angepasste Feeds konfiguriert werden, um auf Änderungen in Daten zu reagieren und automatisch Aktionen an den eingehenden Datenfeeds ausführen. Aktionen können programmiert werden, um Änderungen zu verarbeiten, Datenformate umzusetzen, Nachrichten zu senden und zu empfangen, andere Aktionen aufzurufen sowie verschiedene Datenspeicher zu aktualisieren. Unterstützte Datenspeicher sind SQL-basierte relationale Datenbanken, speicherinterne Datengitter, NoSQL-Datenbanken, Dateien, Nachrichtenbroker und verschiedene andere Systeme. {{site.data.keyword.openwhisk_short}}-Regeln und -Sequenzen bieten die Flexibilität, Änderungen in der Verarbeitungspipeline ganz ohne Programmierungsaufwand, sondern durch einfache Konfigurationsänderungen, vornehmen zu können. Die Datenspeicheroptionen und der niedrige Verwaltungsaufwand machen ein {{site.data.keyword.openwhisk_short}}-basiertes System zu einem höchst agilen und problemlos an sich ändernde Anforderungen anpassbaren System.

Das [OpenChecks](https://github.com/krook/openchecks)-Projekt ist eine Art Machbarkeitsnachweis, der belegt, wie {{site.data.keyword.openwhisk_short}} zur Verarbeitung von Scheckeinreichungen bei einem Bankkonto unter Verwendung optischer Zeichenerkennung verwendet werden kann. Es basiert auf dem öffentlichen {{site.data.keyword.openwhisk_short}}-Service von {{site.data.keyword.Bluemix_notm}} und stützt sich auf {{site.data.keyword.cloudant}} und {{site.data.keyword.cos_full_notm}}. Bei lokaler Nutzung (On-Premises) könnte es CouchDB und OpenStack Swift verwenden. Weitere Speicherservices sind unter anderem FileNet oder Cleversafe. Tesseract stellt die OCR-Bibliothek bereit.

## Cognitive
{: #openwhisk_common_use_cases_cognitive}

Kognitive Technologien (Cognitive Computing) lassen sich effektiv mit {{site.data.keyword.openwhisk_short}} zu leistungsfähigen Anwendungen kombinieren. Zum Beispiel können IBM Alchemy API und Watson Visual Recognition zusammen mit {{site.data.keyword.openwhisk_short}} eingesetzt werden, um automatisch nützliche Informationen aus Videos zu extrahieren, ohne diese ansehen zu müssen. Diese Technologie ist die 'kognitive' Erweiterung des bereits erläuterten Anwendungsfalls für die [Datenverarbeitung](#data-processing). Eine weitere sinnvolle Einsatzmöglichkeit von {{site.data.keyword.openwhisk_short}} ist die Implementierung einer Botfunktion in Kombination mit kognitiven Services.

Eine Beispielanwendung mit dem Namen [Dark Vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp) wird bereitgestellt und leistet genau dies. In dieser Anwendung lädt der Benutzer ein Video oder Bild mit der Webanwendung Dark Vision hoch, die es in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank speichert. Wenn das Video hochgeladen ist, erkennt {{site.data.keyword.openwhisk_short}} durch die Empfangsbereitschaft für {{site.data.keyword.cloudant_short_notm}}-Änderungen (Auslöser) das neue Video. {{site.data.keyword.openwhisk_short}} löst anschließend die Aktion der Videoextraktion aus. Während der Ausführung generiert der Extraktor Rahmen (Bilder) und speichert diese in {{site.data.keyword.cloudant_short_notm}}. Die Rahmen werden anschließend mit Watson Visual Recognition verarbeitet und die Ergebnisse in derselben {{site.data.keyword.cloudant_short_notm}}-Datenbank gespeichert. Die Ergebnisse können mithilfe der Webanwendung Dark Vision oder einer iOS-Anwendung angezeigt werden. Neben {{site.data.keyword.cloudant_short_notm}} kann {{site.data.keyword.cos_full_notm}} verwendet werden. In diesem Fall werden Video- und Bildmetadaten in {{site.data.keyword.cloudant_short_notm}} gespeichert, während die Mediendateien in {{site.data.keyword.cos_short}} gespeichert werden.

Es ist ein [Beispiel für eine iOS-Swift-Anwendung](https://github.com/gconan/BluemixMobileServicesDemoApp) verfügbar, die {{site.data.keyword.openwhisk_short}}, IBM Mobile Analytics und Watson verwendet, um Klänge zu analysieren und an einen Slack-Kanal zu senden.

## Ereignisverarbeitung mit Kafka oder {{site.data.keyword.messagehub}}
{: #openwhisk_event_processing}

{{site.data.keyword.openwhisk_short}} eignet sich ideal zur Kombination mit Kafka, mit dem Service von {{site.data.keyword.messagehub_full}} (Kafka-basiert) und mit anderen Nachrichtensystemen. Die ereignisgesteuerte Funktionsweise dieser Systeme erfordert eine ereignisgesteuerte Laufzeit zur Verarbeitung von Nachrichten. Die Laufzeit kann Geschäftslogik auf diese Nachrichten anwenden und eben diese Funktionalität wird durch {{site.data.keyword.openwhisk_short}} mithilfe von Feeds, Auslösern und Aktionen bereitgestellt. Kafka und {{site.data.keyword.messagehub}} werden häufig für hohe und unvorhersehbare Auslastungsvolumen genutzt und setzen voraus, dass Konsumenten dieser Nachrichten praktisch auf Zuruf skalierbar sind. Dies ist wiederum ein optimaler Einsatzbereich für {{site.data.keyword.openwhisk_short}}. {{site.data.keyword.openwhisk_short}} verfügt über eine integrierte Funktionalität zur Verarbeitung von Nachrichten und zur Veröffentlichung von Nachrichten, die im Paket [openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka) bereitgestellt werden.

Eine [Beispielanwendung, die das Ereignisverarbeitungsszenario implementiert](https://github.com/IBM/openwhisk-data-processing-message-hub) wird mit {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.messagehub}} und Kafka bereitgestellt.
