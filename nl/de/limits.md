---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions, functions

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


# Systemdetails und Grenzwerte
{: #limits}

Die folgenden Abschnitte enthalten technische Details zum {{site.data.keyword.openwhisk}}-System und Informationen zu Grenzwerten.
{: shortdesc}

## Systembegrenzungen
{: #limits_syslimits}

### Aktionen
{: #limits_actions}

{{site.data.keyword.openwhisk_short}} unterliegt einigen wenigen Systembegrenzungen, wie zum Beispiel in Bezug auf die Speicherkapazität, die eine Aktion verwenden kann, oder auf die zulässige Anzahl von Aktionsaufrufen pro Minute.

In der folgenden Tabelle sind die Standardbegrenzungen für Aktionen aufgeführt.

| Begrenzung | Beschreibung | Standardwert | Min | Max |
| --- | ---| --- | --- | --- |
| `codeSize` | Die maximale Codegröße für eine Aktion ist 48 MB. Verwenden Sie für JavaScript-Aktionen ein Tool zum Verketten des gesamten Quellcodes einschließlich der Abhängigkeiten in einer einzelnen Bundledatei. Diese Begrenzung ist festgelegt und kann nicht geändert werden. | 48 | 1 | 48 | 
| `concurrent`| Die Anzahl der Aktivierungen, die für einen Namensbereich entweder ausgeführt oder in die Warteschlange für die Ausführung gestellt werden, kann 1000 nicht überschreiten. Dieser Begrenzungswert ist festgelegt, kann jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Anweisungen zur Erhöhung dieser Begrenzung finden Sie in [Festgelegte Begrenzungen erhöhen](/docs/openwhisk?topic=cloud-functions-limits#limits_increase). | 1000 | 1 | 1000* |
| `logs`| Die Protokollbegrenzung N liegt im Bereich von [0 MB..10 MB] und wird pro Aktion festgelegt. Ein Benutzer kann die Begrenzung des Aktionsprotokolls ändern, wenn eine Aktion erstellt oder aktualisiert wird. Protokolle, die die festgelegte Begrenzung überschreiten, werden abgeschnitten, sodass neue Protokolleinträge ignoriert werden. Ferner wird als letzte Ausgabe der Aktivierung eine Warnung hinzugefügt, um darauf hinzuweisen, dass die Aktivierung die festgelegte Protokollbegrenzung überschritten hat. | 10 | 0 | 10 |
| `memory` | Die Speicherbegrenzung M liegt im Bereich von [128 MB..2048 MB] und wird pro Aktion in MB festgelegt. Ein Benutzer kann die Speicherbegrenzung beim Erstellen der Aktion ändern. Ein Container kann nicht mehr Speicher nutzen, als durch die Begrenzung zugelassen wird. | 256 | 128 | 2048 |
| `minuteRate` | Pro Namensbereich können nicht mehr als N Aktivierungen pro Minute übergeben werden. Die Begrenzung N der Rate ist auf 5000 festgelegt und begrenzt die Anzahl von Aktionsaufrufen in Fenstern von 1 Minute. Ein CLI- oder API-Aufruf, der diese Begrenzung überschreitet, empfängt einen Fehlercode, der dem HTTP-Statuscode `429: TOO MANY REQUESTS` entspricht. Dieser Begrenzungswert ist festgelegt, kann jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Anweisungen zur Erhöhung dieser Begrenzung finden Sie in [Festgelegte Begrenzungen erhöhen](#limits_increase). | 5000 | 1 | 5000* | 
| `openulimit` | Die maximale Anzahl geöffneter Dateien für eine Aktion beträgt 1024 (bezieht sich auf feste und veränderliche Begrenzungen). Diese Begrenzung ist festgelegt und kann nicht geändert werden. Wenn eine Aktion aufgerufen wird, verwendet der Docker-Ausführungsbefehl das Argument `--ulimit nofile=1024:1024`, um den Wert für `openulimit` festzulegen. Weitere Informationen finden Sie in der Referenzdokumentation zum [Docker-Ausführungsbefehl (run)](https://docs.docker.com/engine/reference/commandline/run/){: external}. | 1024 | 0 | 1024 | 
| `parameters` | Die maximale Größe der Parameter, die angehängt werden können (in MB). Die Größenbegrenzung für alle Parameter bei der Erstellung oder Aktualisierung einer Aktion, eines Pakets oder eines Auslösers ist 5 MB. Eine Entität mit Parametern, die die Begrenzung überschreiten, wird bei der Erstellung oder Aktualisierung zurückgewiesen. Diese Begrenzung ist festgelegt und kann nicht geändert werden. | 5 | 0 | 5 | 
| `proculimit` | Die maximale Anzahl der für den Aktionscontainer verfügbaren Prozesse ist 1024. Diese Begrenzung ist festgelegt und kann nicht geändert werden. Wenn eine Aktion aufgerufen wird, verwendet der Docker-Ausführungsbefehl das Argument `--pids-limit 1024`, um den Wert für `proculimit` festzulegen. Weitere Informationen finden Sie in der Referenzdokumentation zum [Docker-Ausführungsbefehl (run)](https://docs.docker.com/engine/reference/commandline/run/){: external}. | 1024 | 0 | 1024 | 
| `result` | Die maximale Ausgabegröße des Ergebnisses eines Aktionsaufrufs in MB. Diese Begrenzung ist festgelegt und kann nicht geändert werden. | 5 | 0 | 5 | 
| `sequenceMaxActions` | Die maximale Anzahl von Aktionen, die eine Sequenz umfasst. Diese Begrenzung ist festgelegt und kann nicht geändert werden. | 50 | 0 | 50* | 
| `timeout` | Das Zeitlimit N liegt im Bereich von [100 ms..600000 ms] und wird pro Aktion in Millisekunden festgelegt. Ein Benutzer kann das Zeitlimit ändern, wenn eine Aktion erstellt wird. Ein Container, der länger als N Millisekunden aktiv ist, wird beendet. | 60000 | 100 | 600000 | 

### Festgelegte Begrenzungen erhöhen
{: #limits_increase}

Begrenzungswerte, die mit einem Stern (*) enden, sind festgelegt, können jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Wenn Sie den Begrenzungswert erhöhen möchten, wenden Sie sich an IBM Support, indem Sie ein Ticket direkt über die IBM [{{site.data.keyword.openwhisk_short}}-Webkonsole](https://cloud.ibm.com/openwhisk){: external} öffnen.
  1. Wählen Sie **Support** aus.
  2. Wählen Sie **Ticket hinzufügen** im Dropdown-Menü aus.
  3. Wählen Sie **Technisch** als Tickettyp aus.
  4. Wählen Sie **Funktionen** als technischen Bereich der Unterstützung aus.

### Auslöser
{: #limits_triggers}

Auslöser unterliegen einer Aktivierungsrate pro Minute (wie in der folgenden Tabelle angegeben).

| Begrenzung | Beschreibung | Standardwert | Min | Max |
| --- | --- | --- | --- | --- |
| `minuteRate` | Die Begrenzung N der Rate ist auf 5000 festgelegt und begrenzt die Anzahl von Auslösern, die ein Benutzer in Fenstern von 1 Minute aktivieren kann. Ein Benutzer kann die Auslöserbegrenzung nicht ändern, wenn ein Auslöser erstellt wird. Ein CLI- oder API-Aufruf, der diese Begrenzung überschreitet, empfängt einen Fehlercode, der dem HTTP-Statuscode `429: TOO MANY REQUESTS` entspricht. Dieser Begrenzungswert ist festgelegt, kann jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Ausführliche Anweisungen zur Erhöhung dieser Begrenzung finden Sie im Abschnitt [Festgelegte Begrenzungen erhöhen](#limits_triggersfixed). | 5000* | 5000* | 5000* |

### Festgelegte Begrenzungen erhöhen
{: #limits_triggersfixed}

Begrenzungswerte, die mit einem Stern (*) enden, sind festgelegt, können jedoch erhöht werden, wenn eine Kosten-Nutzen-Analyse höhere Sicherheitsbegrenzungswerte rechtfertigt. Wenn Sie den Begrenzungswert erhöhen möchten, wenden Sie sich an IBM Support, indem Sie ein Ticket direkt über die IBM [{{site.data.keyword.openwhisk_short}}-Webkonsole](https://cloud.ibm.com/openwhisk){: external} öffnen. 
  1. Wählen Sie **Support** aus.
  2. Wählen Sie **Ticket hinzufügen** im Dropdown-Menü aus.
  3. Wählen Sie **Technisch** als Tickettyp aus.
  4. Wählen Sie **Funktionen** als technischen Bereich der Unterstützung aus.


## {{site.data.keyword.openwhisk_short}}-Entitäten
{: #limits_entities_ov}

### Namensbereiche und Pakete
{: #limits_namespaces}

Aktionen, Auslöser (triggers) und Regeln von {{site.data.keyword.openwhisk_short}} gehören in einen Namensbereich und manchmal in ein Paket.

Pakete können Aktionen und Feeds enthalten. Ein Paket kann kein anderes Paket enthalten, sodass eine Paketverschachtelung nicht zulässig ist. Darüber hinaus müssen Entitäten nicht in einem Paket enthalten sein.

Sie können neue IAM-basierte Namensbereiche durch Ausführen von `ibmcloud fn namespace create` erstellen.  Cloud Foundry-basierte Namensbereiche werden aus einer Kombination aus Organisations- und Bereichsname erstellt. Wenn z. B. die Organisation `user@email.com` und der Bereich `dev` als Ziel festgelegt sind, dann ist damit der {{site.data.keyword.openwhisk_short}} Cloud Foundry-basierte Namensbereich `user@email.com_dev` gemeint. 

Der Namensbereich `/whisk.system` ist für Entitäten reserviert, die mit dem {{site.data.keyword.openwhisk_short}}-System verteilt werden.

Das [Serverless Framework](https://serverless.com/) wird für IAM-basierte Namensbereiche nicht unterstützt.
{: note}


### Vollständig qualifizierte Namen
{: #limits_fullnames}

Der vollständig qualifizierte Name einer Entität sieht wie folgt aus:
`/namespaceName/[packageName]/entityName`. Beachten Sie, dass das Zeichen `/` zum Abgrenzen von Namensbereichen, Paketen und Entitäten verwendet wird. Darüber hinaus muss Namensbereichen das Zeichen `/` als Präfix vorangestellt werden.

Aus Gründen des Komforts kann der Namensbereich weggelassen werden, wenn es sich um den Standardnamensbereich des Benutzers handelt. Betrachten Sie zum Beispiel einen Benutzer mit dem Standardnamensbereich `/myOrg`. Die folgenden Beispiele zeigen die vollständig qualifizierten Namen einer Reihe von Entitäten und ihre Aliasnamen.

| Vollständig qualifizierter Name | Alias | Namensbereich | Paket | Name |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Sie können dieses Benennungsschema zum Beispiel bei Verwendung der {{site.data.keyword.openwhisk_short}}-CLI sowie an anderen Stellen verwenden.

Präziser formuliert, muss der Name dem folgenden regulären Ausdruck (in Java-Metazeichensyntax) entsprechen: `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Aktionssemantik
{: #limits_semantics}

In den folgenden Abschnitten werden Details zu {{site.data.keyword.openwhisk_short}}-Aktionen beschrieben.

### Statusunabhängigkeit
{: #limits_stateless}

Aktionsimplementierungen sind statusunabhängig oder *idempotent*. Das System setzt diese Eigenschaft zwar nicht durch, jedoch ist nicht garantiert, dass ein Status, der von einer Aktion verwaltet wird, über Aufrufe hinweg verfügbar ist.

Darüber hinaus könnten mehrere Instanziierungen einer Aktion mit jeweils eigenem Status vorhanden sein. Ein Aktionsaufruf könnte an irgendeine dieser Instanziierungen gesendet werden.

### Aufrufeingabe und Aufrufausgabe
{: #limits_invocationio}

Die Eingabe für eine Aktion und die Ausgabe aus einer Aktion ist ein Wörterverzeichnis mit Schlüssel/Wert-Paaren. Der Schlüssel ist eine Zeichenfolge und der Wert ein gültiger JSON-Wert.

### Aufrufreihenfolge von Aktionen
{: #limits_ordering}

Aufrufe einer Aktion werden nicht geordnet. Wenn der Benutzer eine Aktion zweimal über die Befehlszeile oder die REST-API aufruft, ist es möglich, dass der zweite Aufruf vor dem ersten ausgeführt wird. Wenn die Aktionen Nebeneffekte haben, werden diese möglicherweise in irgendeiner Reihenfolge beobachtet.

Außerdem ist nicht garantiert, dass Aktionen automatisch ausgeführt werden. Zwei Aktionen können gleichzeitig ausgeführt werden, sodass ihre Nebeneffekte verzahnt auftreten. OpenWhisk garantiert kein bestimmtes Konsistenzmodell für Nebeneffekte bei gleichzeitiger Ausführung. 

### Aktionsausführungen
{: #limits_exec}

Wenn eine Aufrufanforderung empfangen wird, zeichnet das System die Anforderung auf und sendet eine Aktivierung.

Das System gibt eine Aktivierungs-ID zurück (bei einem nicht blockierenden Aufruf), die bestätigt, dass der Aufruf empfangen wurde.
Bei einem Netzausfall oder bei anderen Fehlern, die auftreten, bevor Sie eine HTTP-Antwort erhalten, ist es möglich, dass die Anforderung von {{site.data.keyword.openwhisk_short}} empfangen und verarbeitet wurde.

Das System versucht, die Aktion einmal aufzurufen, was zu einem der folgenden vier Ergebnisse führt:

| Ergebnis | Beschreibung |
| --- | --- |
| `success` | Der Aktionsaufruf wurde erfolgreich ausgeführt. |
| `application error` | Der Aktionsaufruf war erfolgreich, jedoch hat die Aktion absichtlich einen Fehlerwert zurückzugeben, zum Beispiel weil eine Vorbedingung für die Argumente nicht erfüllt war. |
| `action developer error` | Die Aktion wurde aufgerufen, aber abnormal beendet, zum Beispiel weil die Aktion eine Ausnahmebedingung nicht erkannt hat oder weil ein Syntaxfehler vorhanden war. |
| `whisk internal error` | Das System konnte die Aktion nicht aufrufen.
Das Ergebnis wird im Feld `status` des Aktivierungsdatensatzes wie im folgenden Abschnitt dokumentiert aufgezeichnet. |

Jeder Aufruf, der erfolgreich empfangen wurde und der dem Benutzer möglicherweise in Rechnung gestellt wird, erhält einen Aktivierungsdatensatz.

Wenn das Ergebnis *action developer error* ist, wird die Aktion vielleicht teilweise ausgeführt und generiert extern sichtbare Nebeneffekte. Es liegt in der Verantwortung des Benutzers, zu prüfen, ob solche Nebeneffekte aufgetreten sind, und eine Retry-Logik auszuführen. Bestimmte whisk-interne Fehler (*whisk internal errors*) weisen darauf hin, dass die Ausführung einer Aktion gestartet wird, jedoch fehlschlägt, bevor die Aktion die Beendigung feststellt.

## Aktivierungsdatensatz
{: #limits_activation}

Jeder Aktionsaufruf und jeder aktivierte Auslöser hat einen Aktivierungsdatensatz zur Folge.

Ein Aktivierungsdatensatz enthält die folgenden Felder:

| Feld | Beschreibung
| --- | --- |
| `activationId` | Die Aktivierungs-ID. |
| `start` und `end` | Zeitmarken, die den Start und das Ende der Aktivierung aufzeichnen. Die Werte haben das [UNIX-Zeitformat](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15){: external}. |
| `namespace` und `name` | Der Namensbereich und der Name der Entität. |
| `logs` | Ein Array von Zeichenfolgen mit den Protokollen, die durch die Aktion während ihrer Aktivierung generiert wurden. Jedes Array-Element entspricht einer Zeilenausgabe an `stdout` oder `stderr` durch die Aktion und enthält die Zeit und den Datenstrom der Protokollausgabe. Die Struktur ist wie folgt: `TIMESTAMP STREAM: LOG_OUTPUT`. |
| `response` | Ein Wörterverzeichnis, in dem die Schlüssel `success`, `status` und `result` definiert werden. `status`: Das Aktivierungsergebnis, das einen der folgenden Werte haben kann: "success", "application error", "action developer error", "whisk internal error". `success`: Hat nur dann den Wert `true`, wenn der Status `success` lautet. |
| `result` | Ein Wörterverzeichnis, das das Aktivierungsergebnis enthält. Wenn die Aktivierung erfolgreich war, enthält das Ergebnis den Wert, der von der Aktion zurückgegeben wurde. Wenn die Aktivierung nicht erfolgreich war, enthält `result` den Schlüssel `error` und im Regelfall eine Erläuterung des Fehlers. |




