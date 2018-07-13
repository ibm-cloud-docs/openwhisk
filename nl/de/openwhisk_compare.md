---

copyright:
  years: 2016, 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Function as a Service (FaaS) im Vergleich
{: #openwhisk_faas_compared}

{{site.data.keyword.openwhisk}} stellt OpenWhisk in einer hoch skalierbaren, serverunabhängigen Umgebung bereit. Sie können die serverunabhängige Architektur und die kosteneffektive Datenverarbeitung von {{site.data.keyword.openwhisk_short}} mit anderen Architekturmodellen vergleichen.
{: shortdesc}

## Vergleich von OpenWhisk-Architekturen
{: #architecture_comparison}

Die folgenden OpenWhisk-Architekturen werden verglichen:

1. **Function as a Service (FaaS)** unter [{{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk). IBM ist der einzige Anbieter für verwaltetes OpenWhisk. Eine gute Einführung in das serverunabhängige Programmiermodell mit einer FaaS-Plattform finden Sie unter [Martin Fowlers Blog](https://martinfowler.com/articles/serverless.html) und es stehen [Anwendungsfälle](./openwhisk_use_cases.html) für die Ausführung von OpenWhisk mit einem serverunabhängigen Design zur Verfügung.

2. **Infrastructure as a Service (IaaS)** mit OpenWhisk Roll Your Own (RYO). Sie können OpenWhisk von Apache Incubation Project herunterladen und in [{{site.data.keyword.Bluemix_notm}} IaaS](https://console.ng.bluemix.net/catalog/?category=devices) ausführen.

3. **Platform as a Service (PaaS)** als verwaltete Anwendungslaufzeit. Ein gutes Beispiel ist die [Liberty for Java](https://console.ng.bluemix.net/catalog/starters/liberty-for-java)-Laufzeit, die von der {{site.data.keyword.Bluemix_notm}} Foundry-Implementierung verwaltet wird.

4. **Container as a Service (CaaS)** als verwaltete Containerumgebung. Ein gutes Beispiel ist [{{site.data.keyword.containerlong_notm}}](/docs/containers/container_index.html#container_index).

5. **Infrastructure as a Service (IaaS)** mit Java EE-Laufzeit. Ein gutes Beispiel ist [WebSphere Application Server VM on {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/catalog/services/websphere-application-server).

In der folgenden Tabelle werden die Elemente der einzelnen Architekturen aus der Perspektive eines Entwicklers verglichen, der Anwendungen erstellt und betreibt:


| Thema | (1) FaaS unter {{site.data.keyword.openwhisk_short}} | (2) IaaS mit OpenWhisk RYO | (3) PaaS | (4) CaaS | (5) IaaS mit Java EE |
| --- | --- | --- | --- | --- | --- |
|	Anwendungseinheit	|	Einzelne Funktion (in der Regel ein kleiner Codeblock in einem JavaScript-, Swift- oder Docker-Container) - kann kleiner als ein KB oder auch größer sein. Meist nicht mehr als wenige KB.	|	Wie Spalte (1)	|	Hängt von der verwendeten Laufzeit ab. Eine EAR- oder WAR-Datei oder ein anderes sprachspezifisches Anwendungsbundle, meist relativ groß - KB oder sogar MB mit vielen Services in einem Bundle, oder aber auch nur so groß wie einzelner Service.	|	Docker-Container ist die Bereitstellungseinheit.	|	VM mit App-Server mit EAR- oder WAR-Datei und anderen Abhängigkeiten - Größe meist im GB-Bereich.	|
|	Ressourcenbedarf	|	Endbenutzer braucht Hauptspeicher, CPU-Kapazität oder andere Ressourcen nicht zu bezahlen und nicht dafür zu sorgen. Obwohl die Aktion einen Bedarf hat, muss sich der Benutzer nicht darum kümmern.	|	Hoch. Endbenutzer muss zuerst eine IaaS-Umgebung bereitstellen und anschließend OpenWhisk darin installieren und konfigurieren.	|	Gering. Endbenutzer bezahlen Speicher und CPU für die Ausführung von Apps, jedoch nichts für Apps, die nicht ausgeführt werden.	|	Gering bis mittel.	|	Hoch. Endbenutzer muss für Plattenspeicher, Hauptspeicher, CPUs und mögliche andere Komponenten zahlen, wenn die App ausgeführt wird. Wenn sie gestoppt ist, entstehen nur Speicherkosten.	|
|	Installation und Einrichtung	|	Keine erforderlich.	|	Aufwendig - erfolgt alles durch Endbenutzer.	|	Keine erforderlich.	|	Moderat - Hardware, Vernetzung, Betriebssystem, Tools für Container-Management durch CaaS-Anbieter bereitgestellt; Images, Konnektivität und Instanzen durch Endbenutzer.	|	Aufwendig - Hardware, Vernetzung, Betriebssystem, Java EE-Erstinstallation durch Anbieter; zusätzliche Konfiguration, Clustering, Skalierung durch Endbenutzer.	|
|	Bereitstellungszeit	|	Millisekunden	|	Siehe Spalten (4) und (5).	|	Minuten	|	Minuten	|	Stunden	|
|	Fortlaufende Verwaltung	|	Keine	|	Aufwendig	|	Keine	|	Moderat	|	Aufwendig	|
|	Elastisches Skalieren	|	Jede Aktion wird immer sofort und inhärent abhängig von der Last skaliert. Es ist nicht erforderlich, VMs oder andere Ressource im Voraus bereitzustellen.	|	Nicht bereitgestellt - Endbenutzer muss Rechnerkapazität auf IaaS-Plattform bereitstellen und die Skalierung von VMs verwalten. Wenn VMs skaliert wurden, skaliert OpenWhisk Aktionen automatisch, jedoch muss die Ressource im Voraus bereitgestellt werden.	|	Automatische, jedoch langsame Skalierung. In Phasen erhöhter Auslastung müssen Benutzer möglicherweise mehrere Minuten auf die Ausführung der Skalieraktion warten. Automatisches Skalieren erfordert sorgfältige Optimierung.	|	Automatische, jedoch langsame Skalierung. In Phasen erhöhter Auslastung müssen Benutzer möglicherweise mehrere Minuten auf die Ausführung der Skalieraktion warten. Automatisches Skalieren erfordert sorgfältige Optimierung.	|	Nicht bereitgestellt.	|
|	Kapazitätsplanung	|	Nicht erforderlich. FaaS stellt automatisch die erforderliche Kapazität bereit.	|	Bereitstellung ausreichender Kapazität im Voraus erforderlich oder durch ein Script.	|	Einige Kapazitätsplanung erforderlich, aber eine automatische Funktion zur Kapazitätserhöhung wird bereitgestellt.	|	Einige Kapazitätsplanung erforderlich, aber eine automatische Funktion zur Kapazitätserhöhung wird bereitgestellt.	|	Statische Bereitstellung ausreichender Kapazität zum Auffangen von Spitzenlasten erforderlich.	|
|	Persistente Verbindungen und Status	|	Begrenzt - kann keine persistente Verbindung außer beim Container-Caching aufrechterhalten. Allgemein muss der Status in einer externen Ressource verwaltet werden.	|	Wie Spalte (1)	|	Wird unterstützt - kann ein offenes Socket oder eine Verbindung über lange Zeit behalten, kann den Status zwischen Aufrufen im Arbeitsspeicher speichern.	|	Wird unterstützt - kann ein offenes Socket oder eine Verbindung über lange Zeit behalten, kann den Status zwischen Aufrufen im Arbeitsspeicher speichern.	|	Wird unterstützt - kann ein offenes Socket oder eine Verbindung über lange Zeit behalten, kann den Status zwischen Aufrufen im Arbeitsspeicher speichern.	|
|	Wartung	|	Keine - gesamter Stack wird von IBM verwaltet.	|	Erheblich - abhängig von der Zielumgebung muss der Benutzer für Hardware, Vernetzung, Betriebssysteme, Speicher, Datenbankressourcen, Installation und Wartung von OpenWhisk usw. sorgen.	|	Keine - gesamter Stack wird vom Anbieter verwaltet.	|	Erheblich - Benutzer muss angepasste Images erstellen und verwalten, Container bereitstellen und verwalten, Verbindungen zwischen Containern verwalten usw.	|	Erheblich - Benutzer muss VMs zuordnen, Java EE-Server einzeln verwalten und skalieren.	|
|	High Availability (HA - Hochverfügbarkeit) und Disaster Recovery (DR)	|	Inhärent / keine Zusatzkosten	|	Roll Your Own (RYO) 	|	Gegen Zusatzgebühr verfügbar	|	Ausgefallene Container können automatisch erneut gestartet werden.	|	Gegen Zusatzgebühr verfügbar, halbautomatisch. VMs können automatisch von anderen Ressourcen übernommen werden (Failover).	|
|	Sicherheit	|	Vom Anbieter bereitgestellt	|	Roll Your Own (RYO)	|	Mischung aus RYO und Anbieterleistung	|	Mischung aus RYO und Anbieterleistung	|	Roll Your Own (RYO)	|
|	Geschwindigkeit für Entwickler	|	Am höchsten	|	Am höchsten	|	Am höchsten	|	Durchschnittlich	|	Langsam	|
|	Auslastung von Ressourcen (inaktive Ressourcen, für die gezahlt werden muss)	|	Ressourcen sind nie inaktiv, da sie nur auf Anforderung aufgerufen werden. Wenn keine Auslastung vorhanden ist, treten keine Kosten oder Ressourcenzuordnungen auf.	|	Da diese Option mit IaaS oder CaaS arbeitet, gilt Ähnliches wie in den Spalten (4) und (5).	|	Einige Ressourcen können inaktiv sein und automatisches Skalieren hilft beim Vermeiden inaktiver Ressourcen. Eine Reihe aktiver Instanzen muss immer vorhanden sein und wird wahrscheinlich zu weniger als 50% ihrer Kapazität ausgelastet. Gestoppte Instanzen kosten nichts.	|	Ähnlich wie Spalte (3)	|	Einige Ressourcen können inaktiv sein, jedoch wird automatisches Skalieren nicht unterstützt. Eine Reihe aktiver Instanzen muss immer vorhanden sein und wird wahrscheinlich zu weniger als 50% ihrer Kapazität ausgelastet. Für gestoppte Instanzen können Kosten für Speicher anfallen.	|
|	Reife	|	Früher Reifegrad.	|	Früher Reifegrad.	|	Früher Reifegrad.	|	Mittlerer Reifegrad.	|	Hoch ausgereift.	|
|	Ressourcengrenzen	|	[Einige Begrenzungen gelten.](./openwhisk_reference.html#openwhisk_syslimits)	|	Abhängig von zugeordneten Ressourcen.	|	Nein	|	Nein	|	Nein	|
|	Latenz für selten genutzte Services	|	Seltene Anforderungen können zu Anfang mit einigen Sekunden Antwortzeit verbunden sein, was sich jedoch im Millisekundenbereich für nachfolgende Anforderungen bewegt.	|	Jeweils abhängig.	|	Niedrig.	|	Niedrig.	|	Niedrig - sofern das System über ausreichend Ressourcen verfügt	|
|	Optimaler Anwendungstyp	|	Ereignisverarbeitung, Internet der Dinge, Mobile-Back-End, Microservices. Definitiv nicht für monolithische Anwendungen. Siehe [Anwendungsfälle](./openwhisk_use_cases.html).	|	Wie in Spalte (1), jedoch wenn der Benutzer in einer anderen Cloud als IBM Cloud oder lokal (On-Premises) arbeiten möchte.	|	Webanwendungen mit 24x7-Auslastung, statusabhängige Services, die die Verbindung über lange Zeiträume offen halten müssen. Geeignet zur Ausführung von Microservices oder monolithischen Anwendungen.	|	Ideal geeignet für Microservice-Anwendungen.	|	Traditionelle Unternehmensanwendungen, die aus lokalen Umgebungen in die Cloud migriert wurden. Ideal geeignet für monolithische Anwendungen.	|
|	Gebührenaufteilung und Abrechnung	|	[Pro Block von 100 Millisekunden](https://console.ng.bluemix.net/openwhisk/learn/pricing).	|	Abhängig von der Implementierung - Wenn IaaS oder CaaS verwendet werden, gilt Ähnliches - siehe Spalten (4) und (5).	|	In der Regel Gebühr auf Stundenbasis (selten pro Minute) für ein Ressourcenpaket (CPU + Arbeitsspeicher + Plattenspeicher).	|	Ähnlich wie Spalte (3)	|	Ähnlich wie Spalte (3)	|
|	Anschaffungs- und Betriebskosten (TCO)	|	Bei Verwendung für optimal geeignete Anwendungen liegt die Größenordnung der Gebühren sehr wahrscheinlich unter der der Alternativen. Da Ressourcen automatisch skaliert werden, erfolgt keine Überbereitstellung.	|	Für Cloudbereitstellungen ist es wahrscheinlich teurer als OpenWhisk FaaS, jedoch kann eine lokale Bereitstellung (On-Premises) jedoch günstiger als traditionelle Architekturen sein.	|	Relativ gering - Der Benutzer muss keine Ressourcen bereitstellen oder verwalten und kann sich auf seine Anwendungsentwicklung konzentrieren. Im Vergleich zu Serverless ist eine gewisse Überbereitstellung möglich.	|	Moderat - Der Benutzer muss Container und Anwendung bereitstellen und verwalten und es ist im Vergleich zu Serverless oder PaaS eine gewisse Überbereitstellung möglich.	|	Relativ hoch - In Anbetracht der möglicherweise sehr hohen Kosten für die Migration der traditionellen Anwendungen auf ein natives Cloudmodell kann dies ein durchaus gangbarer und ökonomischer Weg für die betreffenden Apps sein.	|

## Kostenaspekte
{: #cost_considerations}

Die Infrastruktur für die Umgebungen, die Sie für Tests, Staging, Belastungstests usw. benötigen, kann hohe Kosten verursachen. Es ist zeitaufwendig, sie einzurichten, und da sie normalerweise rund um die Uhr in Betrieb sein müssen, sind sie in der Regel nicht ausgelastet, binden aber viel Kapazität. Wenn Sie eine serverunabhängige Architektur verwenden, werden die Kosten für eine beliebige Anzahl von Umgebungen auf der Basis der tatsächlichen Auslastung und nicht der Anzahl der definierten Umgebungen generiert.
{: shortdesc}

Zum Berechnen der Kosten für eine serverunabhängige Anwendung können Sie den [Preisrechner ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/openwhisk/learn/pricing) verwenden.

### Sehr hohe Kapazität
{: #limitless_capacity}

In traditionellen Architekturen verbraucht jeder Service die Kapazität, die ihm zugeordnet ist, und es wird Ihnen die verbrauchte Kapazität in Rechnung gestellt. Die serverunabhängige Architektur von {{site.data.keyword.openwhisk_short}} mindert die Einschränkungen für die Granularität Ihrer Mikroservice-Architektur.

Wenn Sie {{site.data.keyword.openwhisk_short}} nicht nutzen, entstehen Ihnen keine Kosten. Ihr Code wird immer ausgeführt, wenn ein HTTP-Aufruf, eine Änderung eines Datenbankstatus oder ein anderer Typ von Ereignis eingeht, der die Ausführung Ihres Codes auslöst. Die Ausführungszeit wird Ihnen in Millisekunden (aufgerundet auf die nächsten 100 ms) in Rechnung gestellt und nicht pro Stunde VM-Nutzung und ungeachtet dessen, ob VM nützliche Operationen ausgeführt hat oder nicht. Da Sie nur zahlen, wenn Ereignisse konsumiert werden, und nicht anhand der Anzahl Ihrer Umgebungen, können Sie Ihre App in 100, 1000 oder mehr Mikroservices aufteilen.

### Aktionen in einer beliebigen Region ausführen
{: #actions_region}

In traditionellen Architekturen muss der Code in jeder Region ausgeführt werden, in der er angewendet werden soll, und die Infrastruktur für diese Region muss ebenfalls bezahlt werden. Bei {{site.data.keyword.openwhisk_short}} können Aktionen in jeder beliebigen Region implementiert und verfügbar gemacht werden, ohne dass zusätzliche Kosten entstehen. So können Sie die Verfügbarkeit und Ausfallsicherheit Ihres Codes ohne die traditionell damit verbundenen Kosten verbessern.

### Redundanz gemäß Design
{: #redundancy_design}

In traditionellen Architekturen müssen Apps redundant sein. Bei {{site.data.keyword.openwhisk_short}} müssen Prozesse nicht hoch verfügbar sein, da serverunabhängige Apps aufgrund Ihres Designs statusunabhängig und anforderungs-/ereignisgesteuert sind. Da nicht explizit für Redundanz gesorgt werden muss, kann die Statusunabhängigkeit serverunabhängiger Apps die Infrastrukturkosten deutlich reduzieren.
