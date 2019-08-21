---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# Serverunabhängige Apps planen
{: #plan}

Im Folgenden finden Sie Informationen zu den Entscheidungen, die Sie im Laufe des Prozesses treffen müssen, bevor Sie mit dem Erstellen Ihrer Funktion beginnen.
{: shortdesc}

## Laufzeitunterstützung prüfen
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}} stellt [Standardlaufzeitumgebungen](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes) zur Ausführung des Codes bereit. Da {{site.data.keyword.openwhisk_short}} aber serverunabhängig arbeitet, unterliegen Sie keinen Einschränkungen, was die verwendbaren Laufzeiten angeht. Sie können angepasste Laufzeiten erstellen, indem Sie ein eigenes [angepasstes Docker-Image](/docs/openwhisk?topic=cloud-functions-actions#actions-docker) erstellen, um Ihren Code zu packen.
{: shortdesc}



## Codearchitektur anpassen
{: #plan_architect}

Möglicherweise müssen Sie einige Anpassungen an Ihrem vorhandenen Code vornehmen, damit er in {{site.data.keyword.openwhisk}} ausgeführt werden kann. Wenn Sie Ihren Code noch nicht geschrieben haben, behalten Sie diese Aspekte bei der Codeentwicklung im Auge.
{: shortdesc}

1. Teilen Sie Ihren Code in Komponenten auf.

    Wenn Sie bereits eine App haben, die Sie für den serverunabhängigen Betrieb ändern wollen, müssen Sie überlegen, wie Sie die App in kleinere Teile zerlegen können. Jede Funktion enthält eine Reihe von Auslösern für den Code, den Sie ausführen möchten. Wenn z. B. ein Problem bei GitHub erstellt wird, führen Sie diesen JavaScript-Code aus. Wenn Ihre App mehrere dieser Ereignisse oder Aktionen enthält, sollten Sie sie in verschiedene Funktionen trennen.

2. Verwenden Sie {{site.data.keyword.cloud_notm}}-Services anstelle von Frameworks.

    Anstatt Frameworks zu verwenden, um bestimmte Leistungsmerkmale zur Laufzeit in Ihrer Funktion zur Verfügung zu stellen, können Sie {{site.data.keyword.cloud}}-Services verwenden. Viele allgemeine Tasks, die durch Frameworks ermöglicht werden, sind als Services in {{site.data.keyword.cloud}} verfügbar.
    {: shortdesc}

    Verwenden Sie zur Authentifizierung beispielsweise {{site.data.keyword.appid_full}} anstelle eines Frameworks. Wenn Sie einen externen Dateispeicher benötigen, versuchen Sie es mit {{site.data.keyword.cos_full}}.

    Wenn die Funktionalität, die Sie integrieren möchten, nicht als Service unter {{site.data.keyword.cloud}} verfügbar ist, können Sie diese Funktionalität auch mit Feeds und APIs integrieren, ohne dass Frameworks erforderlich sind.

3. [Prüfen Sie, ob Ihr Code die Systemgrenzwerte einhält.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) 

    Ihr Code muss unter Einhaltung der Systemgrenzwerte ausgeführt werden können. Wenn Ihr gepackter Code beispielsweise größer als 48 MB ist, müssen Sie ihn möglicherweise in kleinere Teile zerlegen oder als Docker-Image packen.

    Anwendungen, die viele Drittanbietermodule, native Bibliotheken oder externe Tools enthalten, erreichen diesen Grenzwert gegebenenfalls. Wenn Sie eine Aktion mit einem ZIP- oder JAR-Paket mit einer Größe von über 48 MB erstellen, müssen Sie das Laufzeitimage um Abhängigkeiten erweitern. Anschließend müssen Sie eine einzelne Quellendatei oder ein Archiv mit einer Größe von weniger als 48 MB verwenden. Wenn zum Beispiel eine Docker-Laufzeit erstellt wird, die erforderliche gemeinsam genutzte Bibliotheken einschließt, müssen eventuelle Abhängigkeiten nicht in der Archivdatei enthalten sein. Private Quellendateien können weiterhin in das Archiv eingebunden und während der Ausführung eingefügt werden.

4. Bestimmen Sie die Parameter, die in Ihren Code injiziert werden müssen.

    Bei serverunabhängigen Aktionen werden Daten bereitgestellt, indem Parameter zu den Aktionen hinzugefügt werden. Parameter werden als Argument für die wichtigste serverunabhängige Funktion deklariert. In der Regel sind diese Parameter Berechtigungsnachweise für einen Service, aber es kann sich auch um beliebige andere Parameter handeln, die Ihren Code mit verschiedenen Auslösern verwendbar machen.

5. [Überprüfen Sie die strukturellen Anforderungen für Ihren Code, der in einer Funktion verwendet werden soll.](/docs/openwhisk?topic=cloud-functions-prep) 

    Unabhängig davon, ob Sie bereits eine App haben oder erst ein Script entwickeln wollen, sind an Ihrem Code wahrscheinlich gewissen Anpassungen nötig, damit er von {{site.data.keyword.openwhisk}} genutzt werden kann. Der Code selbst muss einige strukturelle Anforderungen erfüllen, wie z. B. die Eingabeparameter und die Ausgabeergebnisse. Zudem muss er unter Umständen in eine einzelne Datei gepackt werden, die alle zugehörigen Abhängigkeiten enthält. 







## Ereignisquelle bestimmen
{: #plan_source}

Betrachten Sie Ihre Funktion im Hinblick auf das Ereignis, das die Ausführung des Codes auslösen soll. Vielleicht möchten Sie, dass der Code bei jedem Commit für ein GitHub-Repository ausgeführt wird. Oder vielleicht möchten Sie, dass Ihr Code jedes Mal ausgeführt wird, wenn eine Aktualisierung in einer Cloudant-Datenbank erfolgt.
{: shortdesc}

Nachdem Sie ein Ereignis ausgewählt haben, das Ihre Funktion auslösen soll, überprüfen Sie die Pakete, die bereits verfügbar sind. Möglicherweise können Sie eines dieser Pakete zur Vereinfachung Ihrer Funktionsentwicklung verwenden. Ist dies nicht der Fall, können Sie ein eigenes Paket für Ihre Ereignisquelle erstellen.

Möglicherweise müssen Sie auf der Basis Ihrer Ereignisauswahl bestimmte Überarbeitungen am eigenen vorhandenen Code vornehmen.


## Bereitstellungen in verschiedenen Umgebungen organisieren
{: #plan_environments}

Entscheiden Sie, wie Sie Ihre Funktionen über verschiedene Umgebungen hinweg (z. B. Entwicklungs-, Staging- und Produktionsumgebungen) bereitstellen möchten.
{: shortdesc}

Da {{site.data.keyword.openwhisk_short}} ein Cloud Foundry-basierter Service ist, können Sie die Bereitstellung Ihrer Funktionen in den Cloud Foundry-Organisationen und -Bereichen verwalten, die für Sie in {{site.data.keyword.cloud_notm}} zur Verfügung gestellt werden. Bei der Verwaltung Ihrer Funktionen über alle diese Umgebungen hinweg empfiehlt es sich möglicherweise, eine Organisation pro Funktion zu erstellen. Erstellen Sie anschließend einen Bereich für jede Umgebung, die Sie benötigen. Stattdessen können Sie auch eine Organisation für jede Umgebung verwenden und einen Bereich pro Funktion erstellen. Ganz gleich, wie Sie Ihre Organisationen und Bereiche gestalten: Wählen Sie eine Struktur, die es Ihnen ermöglicht, Ihre Funktionsentitäten effektiv zu verwalten. 

Sie haben auch die Möglichkeit, [Namensbereiche](/docs/openwhisk?topic=cloud-functions-namespaces) zum Isolieren der Ressourcen zu verwenden. Jeder {{site.data.keyword.cloud_notm}}-Bereich enthält standardmäßig einen Open Whisk-Namensbereich. Sie können Entitäten wie Aktionen oder Auslöser in einem Namensbereich gruppieren und anschließend IAM-Richtlinien (Identity and Access Management) erstellen, um Benutzerberechtigungen für diese Gruppe zu verwalten.




