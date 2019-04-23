---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Mit Parametern arbeiten

Bei serverunabhängigen Aktionen werden Daten bereitgestellt, indem Parameter zu den Aktionen hinzugefügt werden. Parameter werden als Argument für die wichtigste serverunabhängige Funktion deklariert.
{: shortdesc}

Sie können Werte für Parameter auf zwei Arten angeben: 
* **Parameter während des Aufrufs an eine Aktion übergeben**: Stellen Sie Parameter bereit, wenn die Aktion aufgerufen wird, entweder über CLI-Flags oder über eine Datei. Parameter, die beim Aufruf angegeben werden, überschreiben alle Standardparameter, die zuvor festgelegt wurden. 
* **Parameter an eine Aktion oder ein Paket binden**: Legen Sie Standardparameter fest, wenn eine Aktion oder ein Paket erstellt oder aktualisiert wird. Diese Option ist nützlich für Daten, die bei jeder Ausführung gleich bleiben, vergleichbar mit Umgebungsvariablen auf anderen Plattformen, oder für Standardwerte, die beim Aufruf überschrieben werden können.

## Parameter beim Aufruf an eine Aktion übergeben
{: #pass-params-action}

Beim Aufruf können Parameter an eine Aktion übergeben werden.

1. Speichern Sie den folgenden Code in einer Datei mit dem Namen `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Erstellen Sie die Aktion `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Wenn Sie diese Aktion zuvor verwendet haben, stellen Sie sicher, dass alle zuvor festgelegten Parameter gelöscht werden, indem Sie sie aktualisieren.
```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. Rufen Sie die Aktion auf, indem Sie die Parameter `name` und `place` übergeben.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **Hinweis**: Sie können stattdessen eine Datei mit JSON-formatierten Parametern übergeben.
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

5. Sie können auch Parameter in einem strukturierten Objekt an Ihre Aktion übergeben. Aktualisieren Sie die Aktion `hello` zum Beispiel wie folgt:
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    Jetzt erwartet die Aktion, dass ein einzelner Parameter `person` die Felder `name` und `place` aufweist. 

6. Rufen Sie die Aktion mit einem einfachen Parameter `person` auf, der ein gültiges JSON-Objekt ist.
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

## Parameter an eine Aktion binden
{: #default-params-action}

Aktionen können mit mehreren benannten Parametern aufgerufen werden. Die Basisaktion `hello` erwartet zwei Parameter: den Namen (`name`) einer Person und den Ort (`place`), an dem sie sich aufhält. 

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

Anstatt nun jedes Mal alle Parameter an eine Aktion zu übergeben, können Sie Standardparameter an die Aktion binden. Die folgenden Schritte demonstrieren, wie Sie den Parameter `place` an die Basisaktion `hello` binden, sodass für die Aktion standardmäßig der Ort 'Kansas' angegeben ist. 

1. Speichern Sie den folgenden Code in einer Datei mit dem Namen `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Erstellen Sie die Aktion `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Aktualisieren Sie die Aktion, um Parameterwerte mithilfe des Flags `--param` und einem Schlüssel/Wert-Paar zu binden. 

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **Hinweis**: Sie können stattdessen eine Datei mit JSON-formatierten Parametern übergeben.
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    Wenn Sie Ihre nicht zum Service gehörenden Berechtigungsnachweisparameter ändern, werden durch einen Befehl `action update` mit neuen Parametern alle Parameter entfernt, die zurzeit vorhanden sind, jedoch nicht in dem Befehl `action update` angegeben werden. Beispiel: Wenn Sie `action update -p key1 new-value -p key2 new-value` ausführen, aber andere festgelegte Parameter auslassen, sind diese Parameter nach dem Aktualisieren der Aktion nicht mehr vorhanden. Alle Services, die an die Aktion gebunden wurden, werden ebenfalls entfernt. Nachdem Sie andere Parameter aktualisiert haben, müssen Sie wieder [Services an Ihre Aktion binden](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

4. Rufen Sie die Aktion auf, indem Sie nur den Parameter `name` übergeben.
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

    Da Sie den Parameter `place` beim Aufruf der Aktion nicht angegeben haben, wird der Wert des gebundenen Standardparameters, `Kansas`, verwendet. 

5. Gebundene Parameter können durch eine entsprechende Angabe eines Parameterwerts im Aufruf überschrieben werden. Rufen Sie die Aktion auf, indem Sie sowohl `name` als auch `place` übergeben.
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

## Parameter an ein Paket binden
{: #default-params-package}

Standardparameter können auch auf Paketebene festgelegt werden. Gebundene Parameter dienen als Standardparameter für Aktionen im Paket, es sei denn, Folgendes gilt: 

- Die Aktion selbst hat einen Standardparameter. 
- Die Aktion hat einen Parameter, der zur Zeit des Aufrufs bereitgestellt wird. 

Im folgenden Beispiel wird der Standardparameter `name` im Paket `MyApp` festgelegt und eine Aktion angezeigt, die ihn verwendet.

1. Erstellen Sie ein Paket und binden Sie den Standardparameter `name` an das Paket.
```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. Speichern Sie den folgenden Code in einer Datei mit dem Namen `helloworld.js`. 

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. Erstellen Sie die Aktion im Paket `MyApp`.
```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    Wenn Sie Ihre nicht zum Service gehörenden Berechtigungsnachweisparameter ändern, werden durch einen Befehl `action update` mit neuen Parametern alle Parameter entfernt, die zurzeit vorhanden sind, jedoch nicht in dem Befehl `action update` angegeben werden. Beispiel: Wenn Sie `action update -p key1 new-value -p key2 new-value` ausführen, aber andere festgelegte Parameter auslassen, sind diese Parameter nach dem Aktualisieren der Aktion nicht mehr vorhanden. Alle Services, die an die Aktion gebunden wurden, werden ebenfalls entfernt. Nachdem Sie andere Parameter aktualisiert haben, müssen Sie wieder [Services an Ihre Aktion binden](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

3. Rufen Sie die Aktion ohne Übergabe von Parametern auf.
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

    Der Standardparameter, der an das Paket gebunden ist, wird verwendet.
