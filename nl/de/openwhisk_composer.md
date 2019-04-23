---

copyright:
years: 2017, 2019
lastupdated: "2019-03-08"

keywords: composer, openwhisk, compositions, sequence, branch

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}

# Aktionskompositionen mit Composer erstellen
{: #openwhisk_composer}

{{site.data.keyword.openwhisk}} unterstützt jetzt Composer for Apache OpenWhisk als technische Vorschau. Composer erweitert Apache OpenWhisk-Sequenzen um zusätzliche Kombinatoren ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md), [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)), mit denen Sie komplexere Abläufe wie bedingte Verzweigungen, Fehlerbehandlung und Schleifen erstellen können. Die vollständige Dokumentation und technische Spezifikationen sind im [Composer-Git-Repository](https://github.com/apache/incubator-openwhisk-composer) verfügbar. 

Sie können auch das Open-Source-Projekt [Kui](https://github.com/ibm/kui) verwenden, um das Erstellen, Bereitstellen und Visualisieren Ihres Composer-Quellcodes zu vereinfachen. Weitere Informationen zur Verwendung von Kui mit Composer finden Sie unter [Kui in Github](https://github.com/ibm/kui).
{: note}

## Composer-Bibliothek für JavaScript oder Python 3 installieren
{: #install_composer}

Sie können Composer mit Aktionen verwenden, die in allen Sprachen geschrieben wurden, aber Sie drücken die Komposition entweder in JavaScript oder in Python 3 aus. Nach der Installation können Sie die Composer-Befehle `compose/pycompose` und `deploy/pydeploy` verwenden, um eine [Komposition zu konfigurieren und auszuführen](#run).
{: shortdesc}

**Für JavaScript**:
1. Installieren Sie das [Node.js-Paket](https://github.com/apache/incubator-openwhisk-composer) der Composer-Bibliothek mithilfe des Node-Paketmanagers. 

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Bestätigen Sie, dass Sie die Bibliothek installiert haben, indem Sie die Hilfe für die Composer-Befehle ausführen. 

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    Beispielausgabe:
    ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Für Python 3**:
Installieren Sie die Bibliothek [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python) mithilfe von `pip3`. 

1.  Klonen Sie das GitHub-Repository 'Composer for Python 3'.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  Navigieren Sie zum Composer-Verzeichnis.
    ```
    cd composer-python
    ```
    {: pre}
3.  Installieren Sie die Composer-Bibliothek. Schließen Sie den Punkt (`.`) ein, sodass der Befehl in dem Verzeichnis sucht, in dem Sie sich befinden.
    ```
    pip3 install -e .
    ```
    {: pre}
4.  Bestätigen Sie, dass Sie die Bibliothek installiert haben, indem Sie die Hilfe für die Composer-Befehle ausführen.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    Beispielausgabe:
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## Kompositionen in IBM Cloud Functions konfigurieren und ausführen
{: #run}

Sie können die JavaScript- oder Python 3-Composer-Bibliotheken verwenden, um Ihre Kompositionen in {{site.data.keyword.openwhisk}} zu erstellen. Verwenden Sie `compose` oder `pycompose`, um Ihren Kompositionsquellcode zu kompilieren, und verwenden Sie dann `deploy` oder `pydeploy`, um die Komposition in {{site.data.keyword.openwhisk}} bereitzustellen. Nachdem Sie die Komposition konfiguriert haben, können Sie sie in {{site.data.keyword.openwhisk}} ausführen.
{: shortdesc}

**Vorbereitende Schritte**:
Standardmäßig verwenden Bereitstellungen die in `~/.wskprops` festgelegten Werte. Überschreiben Sie die Standardeinstellung, indem Sie zwei Parameter als Eingabe für den Composer-Befehl `deploy` oder `pydeploy` festlegen. 

1.  Legen Sie für den API-Host den {{site.data.keyword.openwhisk}}-Endpunkt fest.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  Fügen Sie Ihren Authentifizierungsschlüssel für die Befehlszeilenschnittstelle `wsk` hinzu.
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**So führen Sie eine Komposition aus**: 

1.  Erstellen Sie Composer-Quellcode mit den NodeJS- oder Python 3-Bibliotheken. Erstellen Sie z. B. eine Datei `demo.js`. 
2.  Kompilieren Sie den Composer-Quellcode in einer JSON-Datei. 
    *   In JavaScript:
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   In Python 3:
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  Stellen Sie den Code in {{site.data.keyword.openwhisk}} bereit. 
    *   In JavaScript: Schließen Sie das Flag `-w` ein, um jede vorhandene Bereitstellung namens `demo` zu überschreiben.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   In Python 3: Schließen Sie das Flag `-w` ein, um jede vorhandene Bereitstellung namens `demo` zu überschreiben.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  Führen Sie die Komposition auf dieselbe Weise aus, wie Sie [andere Aktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_triggers) in {{site.data.keyword.openwhisk}} aufrufen.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} führt den Code aus, der als spezielle Art von Aktion bereitgestellt wird. Weitere Details finden Sie in der Dokumentation zu [Conductoraktionen](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md). 

## Sequenzen mit Composer erweitern
{: #extending}

Apache OpenWhisk ermöglicht das Verketten von Funktionen in einer `Sequenz`, wo die Ausgabe einer Aktion die Eingabe der nächsten Aktion ist. 

### Sequenzen ohne Composer
{: #sequences-without-composer}
Sie können zwei Funktionen namens `action1` und `action2` in {{site.data.keyword.openwhisk_short}} verketten: 

`ibmcloud fn action create --sequence mysequence action1 action2`.

Das Ergebnis dieses Befehls ist eine Funktion namens `mysequence`, die ein Komposition von `action1` und `action2` ist. Sie können `mysequence` auf dieselbe Weise verwenden wie alle anderen Funktionen in OpenWhisk. 

### Sequenzen mit Composer
{: #sequences-with-composer}
In Composer können Sie umfassendere Sequenzen angeben, unter Verwendung von Quellcode statt über die Befehlszeile. 

Für JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

Für Python 3:
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="Einfache Sequenz" alt="Sequenz mit zwei Aktionen" style="width:250px; border-style: none"/></br>
_Abbildung 1. Sequenz mit zwei Aktionen_

Sie sind nicht darauf beschränkt, Funktionen in Composer zu verketten. Composer umfasst eine Familie von [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md)- oder [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md)-basierten Kombinatoren, die die Ausdruckskraft von Sequenzen verbessern. In den folgenden Abschnitten sehen Sie einige gängige Beispiele. 

### Fehlerbehandlung
{: #error-handling}
Sie können Fehlerbehandlung zu einer Sequenz hinzufügen, indem Sie `try-catch-finally`-Blocks verwenden. In diesem Beispiel schließen Sie die Sequenz in einen Versuch ein. Der Code `handleError` wird ausgeführt, wenn eine der Aktionen einen Fehler zurückgibt. 

Für JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

Für Python 3:
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Versuchssequenz" alt="Sequenz mit Fehlerbehandlung" style="width:400px; border-style: none"/></br>
_Abbildung 2. Sequenz mit Fehlerbehandlung_

### Bedingte Verzweigung
{: #conditional-branch}
Sie können eine verzweigte Sequenz mithilfe von `if-then-else` erstellen. In diesem Beispiel wird `if-then-else` veranschaulicht. `action1` sollte einen booleschen Wert zurückgeben. Bei `true` wird `action2` ausgeführt, andernfalls wird `action3` ausgeführt. Beachten Sie, dass `action3` optional ist und für `if-then` weggelassen werden kann. 

Für JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

Für Python 3:
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="If-Sequenz" alt="Sequenz mit bedingter Verzweigung" style="width:250px; border-style: none"/></br>
_Abbildung 3. Sequenz mit bedingter Verzweigung_

### Schleifen
{: #loop}
Sie können in Composer Schleifenkonstrukte erstellen. In diesem Beispiel wird `action2` ausgeführt, solange für `action1` der Wert `true` zurückgegeben wird. Der Composer schränkt die Gesamtzahl von Schritten ein, die Sie in einer zusammengesetzten Sequenz ausführen können. Der aktuelle Grenzwert ist 20. 

Für JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

Für Python 3:
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="While-Sequenz" alt="Sequenz mit While-Schleife" style="width:250px; border-style: none"/></br>
_Abbildung 4. Sequenz mit `While`-Schleife_

### Inlinedefinition von Aktionen
{: #inline-def}
Sie können Aktionen im Kompositionscode selbst definieren. In diesem Beispiel erstellen Sie mithilfe von `composer.action()` die Aktionsdefinition in der Komposition namens `hello`. 

Für JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

Für Python 3:
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="While-Sequenz" alt="Sequenz mit Inlineaktionsdefinition" style="width:250px; border-style: none"/></br>
_Abbildung 5. Sequenz mit Inlineaktionsdefinition_

## Andere Kombinatordefinitionen verwenden
{: #combinator-def}
Eine vollständige Liste von Kombinatordefinitionen finden Sie in der Dokumentation zu Composer in Apache OpenWhisk ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) oder [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)). 
