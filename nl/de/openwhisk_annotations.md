---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: annotations, annotate, package, parameters, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Annotationen
{: #openwhisk_annotations}

Aktionen, Auslöser, Regeln und Pakete von {{site.data.keyword.openwhisk}}, die in ihrer Gesamtheit als Entitäten bezeichnet werden, können Annotationen einschließen.
{: shortdesc}

Annotationen sind wie Parameter an Entitäten angehängt. Annotationen bestehen aus einem Element `key` (Schlüssel), das einen Namen definiert, und einem Element `value` (Wert), das den Wert definiert. Am häufigsten werden Annotationen zum Dokumentieren von Aktionen und Paketen verwendet. Viele der Pakete im {{site.data.keyword.openwhisk_short}}-Katalog sind mit Annotationen versehen, wie zum eine Beschreibung der Funktionalität, die anhand der Aktionen eines Pakets bereitgestellt werden, die Parameter, die beim Binden des Pakets zu verwenden sind, die Aufrufparameter oder die Angabe, ob es sich bei einem Parameter um einen geheimen Schlüssel ('secret') handelt. Annotationen werden nach Bedarf entwickelt, zum Beispiel um eine Benutzerschnittstellenintegration (UI-Integration) zu ermöglichen.

Sie können eine Entität aus der Befehlszeilenschnittstelle (CLI) mithilfe des Flags `--annotation` oder des Flags `-a` dokumentieren.

## Annotationen für Aktionen
{: #action}

Die folgenden Annotationen beschreiben Aktionen:

- `description`: Eine prägnante Beschreibung der Aktion.
- `parameters`: Ein Array, das Aktionen beschreibt, die zur Ausführung der Aktion erforderlich sind.
- `sampleInput`: Ein Beispiel für das Eingabeschema mit typischen Werten.
- `sampleOutput`: Ein Beispiel für das Ausgabeschema, in der Regel für die Beispieleingabe (`sampleInput`).



Der folgende Code stellt einen Beispielsatz von Annotationen für eine Aktion `echo` dar, die ihre Eingabeargumente unverändert zurückgibt. Diese Aktion kann zur Protokollierung von Eingabeparametern zum Beispiel im Zusammenhang mit einer Sequenz oder Regel verwendet werden.

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Annotationen für Webaktionen
{: #annotations-specific-to-web-actions}

Für die folgenden Annotationen für Webaktionen muss explizit der Wert `true` festgelegt werden, um die API-Interaktivität zu aktivieren:

- `web-export`: Bei Anwendung auf eine Aktion wird aus dieser Aktion eine [Webaktion](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions). Die Aktion wird für REST-Aufrufe ohne Authentifizierung zugänglich, sodass Benutzer über einen Browser auf die Aktionen zugreifen können.
    * **Hinweis**: Der Eigner der Webaktion trägt die Kosten für ihre Ausführung im System. Anders ausgedrückt bedeutet das, dass der Eigner der Aktion auch Eigner des Aktivierungsdatensatzes ist.
- `final`: Bei Anwendung auf eine Aktion werden alle zuvor definierten Aktionsparameter unveränderlich. Die Parameter können nicht durch Parameter überschrieben werden, die während des Aufrufs angegeben werden.
- `raw-http`: Bei Anwendung auf eine Aktion, die mit der Annotation `web-export` versehen ist, werden die Abfrage- und Hauptteilparameter der HTTP-Anforderung als reservierte Eigenschaften an die Aktion übergeben.
- `web-custom-options`: Ermöglicht einer Webaktion, mit angepassten Headern auf OPTIONS-Anforderungen zu antworten. Andernfalls wird eine [Standard-CORS-Antwort](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#options-requests) angewendet.
- `require-whisk-auth`: Die Webaktion kann nur von Anforderungen aufgerufen werden, die entsprechende Authentifizierungsnachweise bereitstellen.
    * Durch Angabe eines booleschen Werts wird gesteuert, ob der Wert für die Basisauthentifizierung der Anforderung authentifiziert ist. Der Wert `true` authentifiziert die Berechtigungsnachweise, und der Wert `false` ruft die Aktion ohne Authentifizierung auf.
    * Bei Festlegung einer Zahl oder Zeichenfolge muss dieser Wert mit dem Headerwert `X-Require-Whisk-Auth` der Anforderung übereinstimmen.
    * **Hinweis**: Der Eigner der Webaktion trägt die Kosten für ihre Ausführung im System. Anders ausgedrückt bedeutet das, dass der Eigner der Aktion auch Eigner des Aktivierungsdatensatzes ist.

## Annotationen für Pakete
{: #package}

Die folgenden Annotationen beschreiben Pakete:

- `description`: Eine prägnante Beschreibung des Pakets.
- `parameters`: Ein Array, das Parameter beschreibt, deren Gültigkeitsbereich das Paket ist.

## Annotationen für Parameter
{: #parameter}

Die folgenden Annotationen beschreiben Parameter:

- `name`: Der Name des Parameters.
- `description`: Eine prägnante Beschreibung des Parameters.
- `doclink`: Ein Link zu weiterer Dokumentation für den Parameter (nützlich für OAuth-Tokens).
- `required`: 'True' für erforderliche Parameter, 'false' für optionale Parameter.
- `bindTime`: 'True', wenn der Parameter beim Binden eines Pakets angegeben wird.
- `type`: Der Typ des Parameters, einer der folgenden Werte: `password`, `array` (jedoch weiter gefasst verwendbar).

## Annotation für Aktivierungen
{: #activation}

Sie können Aktivierungsdatensätze mit den folgenden Annotationen dokumentieren:

- `path`: Der vollständig qualifizierte Pfadname der Aktion, die die Aktivierung generiert hat. Beachten Sie, dass in dem Fall, dass diese Aktivierung das Ergebnis einer Aktion war, sich der Pfad auf das übergeordnete Paket bezieht.
- `kind`: Die Art der ausgeführten Aktion und eine der unterstützten OpenWhisk-Laufzeittypen.
- `limits`: Die Zeit-, Speicher- und Protokollbegrenzungen, denen diese Aktivierung unterliegt.

Für Aktivierungen, die sich auf Sequenzen beziehen, generiert das System die folgenden Annotationen:

- `topmost`: Diese Annotation ist nur für eine ganz äußere Sequenzaktion vorhanden.
- `causedBy`: Diese Annotation ist nur für Aktionen vorhanden, die in einer Sequenz enthalten sind.

Um Leistungstransparenz bereitzustellen, zeichnen Aktivierungen auch die folgenden Elemente auf:

- `waitTime`: Die Zeit, die für das Warten im internen OpenWhisk-System aufgewendet wurde. Dabei handelt es sich groß um den Zeitraum zwischen dem Zeitpunkt, zu dem der Controller die Aktivierungsanforderung empfängt, und dem Zeitpunkt, zu dem ein Aufrufer einen Container für die Aktion bereitgestellt. Dieser Wert ist gegenwärtig nur für Aktivierungen vorhanden, die nicht zu Sequenzen gehören. Für Sequenzen kann diese Information aus dem Datensatz der Sequenzaktivierung `topmost` abgeleitet werden.
- `initTime`: Die Zeit, die für die Initialisierung der Funktion aufgewendet wurde. Wenn dieser Wert vorhanden ist, war für die Aktion eine Initialisierung erforderlich und dieser Wert stellt einen Kaltstart dar. Bei einer Warmaktivierung wird die Initialisierung übersprungen, sodass die Annotation nicht generiert wird.

Das folgende Beispiel zeigt, wie diese Annotationen in einem Aktivierungsdatensatz angezeigt werden könnten:

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
