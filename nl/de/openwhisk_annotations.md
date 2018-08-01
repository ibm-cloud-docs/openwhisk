---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Annotationen
{: #openwhisk_annotations}

{{site.data.keyword.openwhisk}}-Aktionen, -Auslöser, -Regeln und -Pakete (die insgesamt als Assets bezeichnet werden) können mit `Annotationen` versehen werden. Annotationen werden ebenso wie Parameter mit einem `Schlüssel`, der einen Namen definiert, und mit einem `Wert`, der den Wert definiert, an Assets angehängt. Annotationen können bequem über die Befehlszeilenschnittstelle (CLI) mit dem Flag `--annotation` oder kurz `-a` festgelegt werden.
{: shortdesc}

Zweck: Annotationen wurden in {{site.data.keyword.openWhisk_short}} hinzugefügt, um das Experimentieren ohne Änderungen an dem zugrunde liegenden Assetschema zu ermöglichen. Bis zum Zeitpunkt der Abfassung dieses Dokuments war ganz bewusst nicht definiert worden, welche `Annotationen` zulässig sein sollen. Da sich die Verwendung von Annotationen inzwischen immer weiter ausbreitet, um semantische Änderungen zu vermitteln, ist es wichtig, Annotationen zu dokumentieren.

Die häufigste Verwendung von Annotationen bis heute dient dem Zweck, Aktionen und Pakete zu dokumentieren. Viele der Pakete im {{site.data.keyword.openwhisk_short}}-Katalog haben Annotationen, wie zum Beispiel die folgenden: eine Beschreibung der Funktionalität, die durch die Aktionen eines Pakets bereitgestellt werden, die Parameter, die beim Binden des Pakets zu verwenden sind, die Aufrufparameter oder eine Angabe, ob ein Parameter ein geheimer Schlüssel ("secret") wie ein Kennwort ist oder nicht. Annotationen werden nach Bedarf entwickelt, zum Beispiel um eine Benutzerschnittstellenintegration (UI-Integration) zu ermöglichen.

Das folgende Beispiel zeigt einen Satz von Annotationen für eine Aktion `echo`, der die Eingabeargumente unverändert (Beispiel: `function main(args) { return args }`) zurückgibt. Diese Aktion kann zur Protokollierung von Eingabeparametern zum Beispiel im Zusammenhang mit einer Sequenz oder Regel verwendet werden.
```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

Annotationen, die **Pakete** beschreiben, sind zum Beispiel:

- `description`: Eine prägnante Beschreibung des Pakets.
- `parameters`: Ein Array, das Parameter beschreibt, deren Gültigkeitsbereich das Paket ist.

Annotationen, die **Aktionen** beschreiben, sind zum Beispiel:

- `description`: Eine prägnante Beschreibung der Aktion.
- `parameters`: Ein Array, das Aktionen beschreibt, die zur Ausführung der Aktion erforderlich sind.
- `sampleInput`: Ein Beispiel für das Eingabeschema mit typischen Werten.
- `sampleOutput`: Ein Beispiel für das Ausgabeschema, in der Regel für die Beispieleingabe (`sampleInput`).

Annotationen, die **Parameter** beschreiben, sind zum Beispiel:

- `name`: Der Name des Parameters.
- `description`: Eine prägnante Beschreibung des Parameters.
- `doclink`: Ein Link zu weiterer Dokumentation für den Parameter (nützlich für OAuth-Tokens).
- `required`: 'True' für erforderliche Parameter, 'false' für optionale Parameter.
- `bindTime`: 'True', wenn der Parameter beim Binden eines Pakets angegeben wird.
- `type`: Der Typ des Parameters, einer der folgenden Werte: `password`, `array` (jedoch weiter gefasst verwendbar).

Die Annotationen werden _nicht_ geprüft. Daher ist es zum Beispiel zwar denkbar, durch die Annotationen zu erschließen, ob die Zusammensetzung zweier Aktionen zu einer Sequenz zulässig ist, aber das System tut dies bisher nicht.

## Spezielle Annotationen für Webaktionen
{: #annotations-specific-to-web-actions}

Die API-Kerndefinitionen wurden in letzter Zeit um neue Funktionen erweitert. Um Pakete und Aktionen an diesen Funktionen beteiligen zu können, wurden die folgenden neuen Annotationen eingeführt, die semantische Bedeutung haben. Diese Annotationen müssen explizit auf `true` gesetzt werden, um Wirkung zu haben. Durch Ändern des Werts von `true` in `false` wird das angehängte Asset von der neuen API ausgeschlossen. Die Annotationen haben im System ansonsten keine Bedeutung. Sehen Sie sich die folgenden Annotationen an:

- `web-export`: Gilt nur für eine Aktion. Wenn angegeben, macht diese Annotation die entsprechende Aktion für REST-Aufrufe _ohne_ Authentifizierung zugänglich. Solche Aktionen werden als [_Webaktionen_](openwhisk_webactions.html) bezeichnet, da sie die Verwendung von OpenWhisk-Aktionen zum Beispiel in einem Browser ermöglichen. Es ist wichtig zu beachten, dass der _Eigner_ der Webaktion die Kosten für die Ausführung der Webaktion im System trägt. Das heißt mit anderen Worten, dass der _Eigner_ der Aktion auch Eigner des Aktivierungsdatensatzes ist.
- `final`: Gilt nur für eine Aktion. Sie macht alle Aktionsparameter, die bereits definiert sind, unveränderbar. Ein Parameter einer Aktion, die die Annotation hat, kann nicht durch Aufrufparameter überschrieben werden, wenn der Parameterwert durch das umschließende Paket oder die Aktionsdefinition definiert wurde.
- `raw-http`: Gilt nur für eine Aktion bei Vorhandensein einer Annotation `web-export`. Ist diese Annotation vorhanden, werden die Abfrage- und Hauptteilparameter der HTTP-Anforderung als reservierte Eigenschaften an die Aktion übergeben.
- `web-custom-options`: Wenn diese Annotation angegeben ist, ermöglicht sie es einer Webaktion auf OPTIONS-Anforderungen mit angepassten Headern zu antworten. Andernfalls gilt eine [CORS-Standardantwort](openwhisk_webactions.html#options-requests).
- `require-whisk-auth`: Diese Annotation schützt die Webaktion, sodass sie nur von Anforderungen aufgerufen wird, die die entsprechenden Authentifizierungsnachweise bereitstellen. Durch Angabe eines booleschen Werts wird gesteuert, ob der Wert für die Basisauthentifizierung der Anforderung (d. h. der Whisk-Authentifizierungsschlüssel) authentifiziert wird. Der Wert `true` authentifiziert die Berechtigungsnachweise, und der Wert `false` ruft die Aktion ohne Authentifizierung auf. Bei Festlegung auf eine Zahl oder eine Zeichenfolge muss dieser Wert mit dem Headerwert `X-Require-Whisk-Auth` der Anforderung übereinstimmen. 
In beiden Fällen ist es wichtig zu beachten, dass der _Eigner_ der Webaktion die Kosten für die Ausführung der Webaktion im System trägt (d. h., dem _Eigner_ der Aktion ist auch der Aktivierungsdatensatz zugeordnet).

## Spezielle Annotationen für Aktivierungen

Das System kann Aktivierungsdatensätze mit den folgenden Annotationen versehen:

- `path`: Der vollständig qualifizierte Pfadname der Aktion, die die Aktivierung generiert hat. Beachten Sie, dass in dem Fall, dass diese Aktivierung das Ergebnis einer Aktion war, sich der Pfad auf das übergeordnete Paket bezieht.
- `kind`: Die Art der ausgeführten Aktion und eine der unterstützten OpenWhisk-Laufzeittypen.
- `limits`: Die Zeit-, Speicher- und Protokollbegrenzungen, denen diese Aktivierung unterliegt.

Für Aktivierungen, die sich auf Sequenzen beziehen, generiert das System die folgenden Annotationen:

- `topmost`: Diese Annotation ist nur für eine ganz äußere Sequenzaktion vorhanden.
- `causedBy`: Diese Annotation ist nur für Aktionen vorhanden, die in einer Sequenz enthalten sind.

Schließlich zeichnen Aktivierungen auch die folgenden Elemente auf, um Leistungstransparenz sicherzustellen:

- `waitTime`: Die Zeit, die für das Warten im internen OpenWhisk-System aufgewendet wurde. Dabei handelt es sich groß um den Zeitraum zwischen dem Zeitpunkt, zu dem der Controller die Aktivierungsanforderung empfängt, und dem Zeitpunkt, zu dem ein Aufrufer einen Container für die Aktion bereitgestellt. Dieser Wert ist gegenwärtig nur für Aktivierungen vorhanden, die nicht zu Sequenzen gehören. Für Sequenzen kann diese Information aus dem Datensatz der Sequenzaktivierung `topmost` abgeleitet werden.
- `initTime`: Die Zeit, die für die Initialisierung der Funktion aufgewendet wurde. Wenn dieser Wert vorhanden ist, war für die Aktion eine Initialisierung erforderlich und dieser Wert stellt einen Kaltstart dar. Bei einer Warmaktivierung wird die Initialisierung übersprungen, sodass die Annotation nicht generiert wird.

Ein Beispiel für diese Annotationen, wie sie in einem Aktivierungsdatensatz angezeigt werden, ist nachfolgend dargestellt.

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}
