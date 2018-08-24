---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Mit Parametern arbeiten

Hier erfahren Sie, wie Sie Parameter bei Paketen und Aktionen für die Bereitstellung festlegen und wie Parameter beim Aufruf an Aktionen übergeben werden können. Sie können auch eine Datei verwenden, um Parameter zu speichern und den Dateinamen an die Aktion zu übergeben, anstatt jeden Parameter einzeln in der Befehlszeile anzugeben.
{: shortdesc}

Bei serverunabhängigen Aktionen werden Daten bereitgestellt, indem Parameter zu den Aktionen hinzugefügt werden, die als Argument für die wichtigste serverunabhängige Funktion deklariert werden. Alle Daten werden auf diese Weise empfangen und die Werte können auf verschiedene Arten festgelegt werden. Die erste Option ist das Angeben von Parametern, wenn eine Aktion oder ein Paket erstellt (oder aktualisiert) wird. Diese Option ist nützlich für Daten, die bei jeder Ausführung gleich bleiben, vergleichbar mit Umgebungsvariablen auf anderen Plattformen, oder für Standardwerte, die beim Aufruf überschrieben werden können. Die zweite Option besteht darin, Parameter zu anzugeben, wenn die Aktion aufgerufen wird, wodurch alle zuvor festgelegten Parameter überschrieben werden.

## Parameter beim Aufruf an eine Aktion übergeben
{: #pass-params-action}

Beim Aufruf können Parameter an eine Aktion übergeben werden. Die bereitgestellten Beispiele verwenden JavaScript, aber alle anderen Sprachen funktionieren auf die gleiche Weise. Detaillierte Beispiele finden Sie in den folgenden Abschnitten zu [Javascript-Aktionen](./openwhisk_actions.html#creating-and-invoking-javascript-actions), [Swift-Aktionen](./openwhisk_actions.html#creating-swift-actions), [Python-Aktionen](./openwhisk_actions.html#creating-python-actions), [Java-Aktionen](./openwhisk_actions.html#creating-java-actions), [PHP-Aktionen](./openwhisk_actions.html#creating-php-actions), [Docker-Aktionen](./openwhisk_actions.html#creating-docker-actions) oder [Go-Aktionen](./openwhisk_actions.html#creating-go-actions).

1. Verwenden Sie Parameter in der Aktion. Erstellen Sie zum Beispiel eine Datei mit dem Namen **hello.js** und dem folgenden Inhalt:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Die Eingabeparameter werden in Form eines JSON-Objektparameters an die Funktion **main** übergeben. Beachten Sie, wie die Parameter `name` und `place` in diesem Beispiel aus dem Objekt `params` abgerufen werden.

2. Aktualisieren Sie die Aktion **hello** so, dass sie verwendet werden kann:
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

  Wenn Sie Ihre nicht zum Service gehörenden Berechtigungsnachweisparameter ändern, werden durch einen Befehl `action update` mit neuen Parametern alle Parameter entfernt, die zurzeit vorhanden sind, jedoch nicht in dem Befehl `action update` angegeben werden. Beispiel: Wenn Sie `action update -p key1 new-value -p key2 new-value` ausführen, aber andere festgelegte Parameter auslassen, sind diese Parameter nach dem Aktualisieren der Aktion nicht mehr vorhanden. Alle Services, die an die Aktion gebunden wurden, werden ebenfalls entfernt. Nachdem Sie andere Parameter aktualisiert haben, müssen Sie wieder [Services an Ihre Aktion binden](./binding_services.html).
  {: tip}

3. Parameter können in der Befehlszeile explizit angegeben oder [in einer Datei angegeben](./parameters.html#using-parameter-files) werden, die die gewünschten Parameter enthält.

  Zur Übergabe von Parametern direkt über die Befehlszeile geben Sie ein Schlüssel/Wert-Paar für das Flag `--param` an:
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **Antwort:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Beachten Sie die Verwendung der Option `--result`: Sie bewirkt einen blockierenden Aufruf, bei dem die Befehlszeilenschnittstelle auf den Abschluss der Aktivierung wartet und dann nur das Ergebnis anzeigt. Aus Gründen des Bedienungskomforts kann diese Option ohne die Option `--blocking` verwendet werden, die automatisch abgeleitet wird.

  Darüber hinaus gilt: Wenn die in der Befehlszeile angegebenen Parameterwerte gültige JSON-Werte sind, dann werden sie analysiert und als strukturiertes Objekt an Ihre Aktion gesendet.

  Aktualisieren Sie die Aktion **hello** zum Beispiel wie folgt:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Jetzt erwartet die Aktion, dass ein einzelner Parameter `person` die Felder `name` und `place` aufweist.

  Rufen Sie als Nächstes die Aktion mit einem einzelnen Parameter `person` auf, der ein gültiger JSON-Wert ist, wie im folgenden Beispiel:
  ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **Antwort:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Das Ergebnis ist dasselbe, weil die CLI automatisch den Parameter `person` in das strukturierte Objekt analysiert, das jetzt von der Aktion erwartet wird.

## Standardparameter für eine Aktion festlegen
{: #default-params-action}

Aktionen können mit mehreren benannten Parameter aufgerufen werden. Die Aktion **hello** aus dem vorherigen Beispiel erwartet beispielsweise zwei Parameter: den Namen (*name*) einer Person und den Ort (*place*), aus dem sie kommt.

Anstatt nun jedes Mal alle Parameter an eine Aktion zu übergeben, können Sie bestimmte Parameter binden. Im folgenden Beispiel wird der Parameter *place* gebunden, sodass die Aktion mit dem Standardwert "Kansas" arbeitet:

1. Aktualisieren Sie die Aktion mit der Option `--param`, um Parameterwerte zu binden, oder durch die Übergabe einer Datei mit den Parametern an `--param-file`. (Beispiele für die Verwendung von Dateien finden Sie im Abschnitt [Parameterdateien verwenden](./parameters.html#using-parameter-files).

  Zur expliziten Angabe von Standardparametern über die Befehlszeile übergeben Sie ein Schlüssel/Wert-Paar an das Flag `param`:
  ```
  ibmcloud fn action update hello --param place Kansas
  ```
  {: pre}

2. Rufen Sie die Aktion auf, indem Sie diesmal nur den Parameter `name` übergeben.
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Sie stellen fest, dass Sie den Parameter "place" beim Aufruf der Aktion nicht angeben mussten. Gebundene Parameter können weiterhin durch eine entsprechende Angabe eines Parameterwerts im Aufruf überschrieben werden.

3. Rufen Sie die Aktion auf, indem Sie die Werte `name` und `place` übergeben und die Ausgabe beobachten:

  Rufen Sie die Aktion mit dem Flag `--param` auf:
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  Beispielausgabe:
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  Die Parameter für eine Aktion, die bei ihrer Erstellung oder Aktualisierung festgelegt wurden, werden immer durch einen Parameter überschrieben, der direkt beim Aufruf angegeben wird.
  {: tip}

## Standardparameter für ein Paket festlegen
{: #default-params-package}

Parameter können auf Paketebene festgelegt werden und dienen als Standardparameter für Aktionen, _es sei denn_:

- Die Aktion hat selbst einen Standardparameter.
- Die Aktion verfügt über einen Parameter, der beim Aufruf angegeben wird. Dieser hat stets "Priorität", wenn mehr als ein Parameter verfügbar ist.

Im folgenden Beispiel wird der Standardparameter `name` im Paket **MyApp** festgelegt und eine Aktion angezeigt, die ihn verwendet.

1. Erstellen Sie ein Paket mit einem eingestellten Parameter:

  ```
  ibmcloud fn package update MyApp --param name World
  ```
  {: pre}

2. Erstellen Sie eine Aktion im Paket **MyApp**:
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Erstellen Sie die Aktion:
  ```
  ibmcloud fn action update MyApp/hello hello.js
  ```
  {: pre}

3. Rufen Sie die Aktion auf und beobachten Sie den Standardpaketparameter bei seiner Verwendung:
  ```
  ibmcloud fn action invoke --result MyApp/hello
  ```
  {: pre}

  Beispielausgabe:
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## Parameterdateien verwenden
{: #using-parameter-files}

Sie können Parameter in eine Datei im JSON-Format einfügen und anschließend die Parameter übergeben, indem Sie den Dateinamen mit dem Flag `--param-file` angeben. Diese Methode kann für die Erstellung (oder Aktualisierung) von Paketen und Aktionen und während des Aktionsaufrufs verwendet werden.

1. Nehmen Sie als Beispiel **hello** von weiter oben, indem Sie `hello.js` mit folgendem Inhalt verwenden:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Aktualisieren Sie die Aktion mit dem geänderten Inhalt von `hello.js`:

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. Erstellen Sie eine Parameterdatei mit dem Namen `parameters.json`, die JSON-formatierte Parameter enthält:

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. Verwenden Sie den Dateinamen `parameters.json`, wenn Sie die Aktion **hello** aufrufen, und beobachten Sie die Ausgabe:

  ```
  ibmcloud fn action invoke --result hello --param-file parameters.json
  ```

  Beispielausgabe:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
