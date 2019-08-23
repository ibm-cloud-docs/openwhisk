---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, functions, serverless, javascript, node, node.js

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



# Aktionen erstellen
{: #actions}

Sie können eine Aktion erstellen, bei der es sich um eine Funktion der höchsten Ebene handelt, die ein JSON-Objekt zurückgibt. Sie können Aktionen in einem Paket kombinieren, um die Verwaltung Ihrer Aktionen zu vereinfachen.
{: shortdesc}

Vorbereitende Schritte: Um eine Aktion zu erstellen, muss Ihr Quellcode bestimmte Anforderungen erfüllen. Wenn Sie z. B. eine Aktion aus Code erstellen möchten, der in mehreren Dateien enthalten ist, müssen Sie Ihren Code in einer Datei zusammenführen und packen, bevor Sie die Aktion erstellen. Ausführliche Informationen zu den Voraussetzungen für die einzelnen Laufzeitkomponenten finden Sie unter [App-Code für serverunabhängigen Betrieb vorbereiten](/docs/openwhisk?topic=cloud-functions-prep).


## Aktionen über die CLI erstellen
{: #actions_cli}

1. Erstellen Sie eine Aktion.
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  **Beispiel**
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  **Beispielausgabe**

  ```
  ok: created action hello
  ```
  {: screen}

  Tipps:
  - Um Kosten zu sparen, können Sie Grenzwerte festlegen.
      - Um eine Begrenzung für die Speicherbelegung festzulegen, fügen Sie `--memory VALUE` in den Befehl 'create' ein, wobei der Wert in Megabyte angegeben wird.
      - Um ein Zeitlimit festzulegen, fügen Sie `--timeout VALUE` in den Befehl 'create' ein, wobei der Wert in Millisekunden angegeben wird.
  - Wenn Sie Ihren Code als Docker-Image gepackt haben, fügen Sie `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` anstelle des lokalen Pfads zu Ihrer App und des Flags '--kind' in den Befehl 'create' ein. Verwenden Sie Ihre Images möglichst ohne den Tag `latest`. Wenn der Tag `latest` verwendet wird, wird das Image mit diesem Tag verwendet, das aber möglicherweise nicht immer das zuletzt erstellte Image ist.

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  
2. Überprüfen Sie, ob die Aktion in Ihrer Aktionsliste aufgeführt wird.

  ```
  ibmcloud fn action list
  ```
  {: pre}

  **Beispielausgabe**

  ```
  actions
  hello       private
  ```
  {: screen}


## Apps oder Laufzeiten in Aktionen aktualisieren
{: #actions_update}

Sie können den Befehl 'update' immer dann ausführen, wenn Sie den Code in Ihrer App aktualisieren oder eine Migration auf eine neuere Version einer Laufzeit durchführen müssen. Da sich Node.js Version 8 beispielsweise im Wartungsmodus befindet, möchten Sie die Laufzeit möglicherweise in Node.js 10 ändern.

Wenn Sie eine Migration auf eine neue Laufzeitversion durchführen, müssen Sie unter Umständen den Code in Ihrer App ändern, damit er der neuen Laufzeitversion entspricht. In den meisten Fällen sind die Laufzeitversionen kompatibel.
{: tip}

1. Aktualisieren Sie Ihre App lokal.

2. Wenn Sie Ihre App als Docker-Image gepackt haben, laden Sie das aktuellste Image auf Docker Hub hoch. Dies ermöglicht dem System das Extrahieren Ihres neuen Docker-Images bei der nächsten Ausführung des Codes für Ihre Aktion. Wenn ein Container ausgeführt wird, der eine Vorgängerversion Ihres Docker-Image verwendet, verwenden alle neuen Aufrufe weiterhin dieses Image. Sie müssen den Befehl 'update' ausführen, damit neue Aufrufe auf dem neuen Image ausgeführt werden. 

3. Aktualisieren Sie eine Aktion und schließen Sie den lokalen Pfad zu Ihrer App oder dem Docker-Image ein.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    **Beispiel**

    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    **Beispielausgabe**

    ```
    ok: updated action hello
    ```
    {: screen}

    Wenn Sie Ihren Code als Docker-Image gepackt haben, fügen Sie in Ihren Befehl 'create' `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG` anstelle des Pfads zu der lokalen App und des Flags `--kind` ein. Verwenden Sie Ihre Images möglichst ohne den Tag `latest`. Wenn der Tag `latest` verwendet wird, wird das Image mit diesem Tag verwendet, das aber möglicherweise nicht immer das zuletzt erstellte Image ist. 

      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## Parameter an Aktionen binden
{: #actions_params}

Sie können Parameter an Aktionen binden, um Standardparameter festzulegen. Gebundene Parameter dienen als Standardparameter für Aktionen, es sei denn, beim Aufruf werden Parameter angegeben.
{: shortdesc}

Bevor Sie beginnen, müssen Sie die [Aktion erstellen](#actions_cli).

So binden Sie die Parameter:

1. Aktualisieren Sie eine Aktion und binden Sie die Standardparameter an die Aktion.

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **Beispiel**

    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    **Beispielausgabe**

    ```
    ok: updated action MyApp
    ```
    {: screen}

    Wenn Sie Ihre nicht zum Service gehörenden Berechtigungsnachweisparameter ändern, werden durch einen Befehl `action update` mit neuen Parametern alle Parameter entfernt, die zurzeit vorhanden sind, jedoch nicht in dem Befehl `action update` angegeben werden. Beispiel: Wenn Sie `action update -p key1 new-value -p key2 new-value` ausführen, aber andere festgelegte Parameter auslassen, sind diese Parameter nach dem Aktualisieren der Aktion nicht mehr vorhanden. Alle Services, die an die Aktion gebunden wurden, werden ebenfalls entfernt. Wenn Sie einen Service gebunden haben, müssen Sie erneut [die Services an Ihre Aktion binden](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Stellen Sie sicher, dass die Parameter an die Aktion gebunden wurden.

    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    **Beispielausgabe**

    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

Optional: Um die Parameter zu löschen, die zuvor gebunden waren, aktualisieren Sie die Aktion, ohne Parameter zu einzufügen.

```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## Aktionen miteinander zu Aktionssequenzen verketten
{: #actions_seq}

Sie können eine Aktion erstellen, die eine Sequenz von Aktionen miteinander verkettet. Dabei wird das Ergebnis einer Aktion als Argument an die nächste Aktion übergeben.
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

Parameter, die zwischen den Aktionen in der Sequenz übergeben werden, sind (mit Ausnahme der Standardparameter) explizit. Daher sind die Parameter, die an die Aktionssequenz übergeben werden, nur für die erste Aktion in der Sequenz verfügbar. Das Ergebnis der ersten Aktion in der Sequenz wird zum JSON-Eingabeobjekt für die zweite Aktion in der Sequenz usw. Das Objekt enthält keine Parameter, die ursprünglich an die Sequenz übergeben wurden, es sei denn, die erste Aktion schließt sie explizit in ihr Ergebnis ein. Die Eingabeparameter für eine Aktion werden mit den Standardparametern der Aktion zusammengeführt. Erstere haben Vorrang und überschreiben alle übereinstimmenden Standardparameter.

Eine Sequenz besitzt kein Gesamtzeitlimit, das jenseits der Zeitlimits für die einzelnen Aktionen innerhalb der Aktionssequenz gilt. Da es sich bei einer Sequenz um eine Hintereinanderschaltung oder Aneinanderkettung von Operationen ähnlich einer Pipeline handelt, bewirkt ein Fehler in einer Aktion die Unterbrechung dieser Pipeline. Wenn eine Aktion das Zeitlimit überschreitet, wird die gesamte Sequenz mit diesem Fehler beendet.

Wenn Sie als Nächstes eine Regel erstellen oder die Aktionen aufrufen, verwenden Sie den Namen der Sequenz.


## Aktionen packen
{: #actions_pkgs}

In {{site.data.keyword.openwhisk}} können Sie Pakete verwenden, um eine Gruppe zusammengehöriger Aktionen und Feeds zu bündeln und diese zur gemeinsamen Nutzung mit anderen Benutzern bereitzustellen. Pakete bieten außerdem die Möglichkeit, Parameter über alle Entitäten in dem Paket hinweg gemeinsam zu nutzen.
{: shortdesc}

Ein Paket kann *Aktionen* und *Feeds* enthalten.
- Eine Aktion ist ein Abschnitt Code, der in {{site.data.keyword.openwhisk_short}} ausgeführt wird. Zum Beispiel enthält das {{site.data.keyword.cloudant}}-Paket Aktionen zum Lesen und Schreiben von Datensätzen in einer {{site.data.keyword.cloudant_short_notm}}-Datenbank.
- Ein Feed dient zum Konfigurieren einer externen Ereignisquelle, sodass diese Auslöserereignisse aktiviert. Das Paket für Alarme enthält zum Beispiel einen Feed, der einen Auslöser mit einer angegebenen Häufigkeit anwenden kann.


1. Erstellen Sie ein Paket.

  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. Rufen Sie eine Zusammenfassung des Pakets ab. Beachten Sie, dass das Paket leer ist.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Beispielausgabe**

  ```
  package /myNamespace/custom
  ```
  {: screen}

4. Erstellen Sie eine Aktion und fügen Sie sie in das Paket ein. Zum Erstellen einer Aktion in einem Paket müssen Sie dem Aktionsnamen einen Paketnamen als Präfix voranstellen. Eine Verschachtelung von Paketen ist nicht zulässig. Ein Paket kann nur Aktionen, jedoch kein anderes Paket enthalten.

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. Rufen Sie eine Zusammenfassung des Pakets ab.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Beispielausgabe**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## Parameter an Pakete binden
{: #actions_pkgs_params}

Sie können Standardparameter für alle Entitäten in einem Paket festlegen, indem Sie Parameter auf Paketebene festlegen, die von allen Aktionen in dem Paket übernommen werden.

Gebundene Parameter dienen als Standardparameter für Aktionen im Paket, es sei denn, Folgendes gilt:

- Die Aktion selbst hat einen Standardparameter.
- Die Aktion hat einen Parameter, der zur Zeit des Aufrufs bereitgestellt wird.

Bevor Sie beginnen, müssen Sie ein Paket erstellen, das mindestens eine Aktion enthält.

1. Aktualisieren Sie ein Paket und binden Sie den Standardparameter an das Paket.

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    **Beispiel**

    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    **Beispielausgabe**

    ```
    ok: updated package MyApp
    ```
    {: screen}

    Wenn Sie Ihre nicht zum Service gehörenden Berechtigungsnachweisparameter ändern, werden durch einen Befehl `package update` mit neuen Parametern alle Parameter entfernt, die zurzeit vorhanden sind, jedoch nicht in dem Befehl `package update` angegeben werden. Beispiel: Wenn Sie `package update -p key1 new-value -p key2 new-value` ausführen, aber andere festgelegte Parameter auslassen, sind diese Parameter nach dem Aktualisieren des Pakets nicht mehr vorhanden. Alle Services, die an das Paket gebunden wurden, werden ebenfalls entfernt. Nachdem Sie andere Parameter aktualisiert haben, müssen Sie wieder [Services an Ihr Paket binden](/docs/openwhisk?topic=cloud-functions-services).
    {: tip}

3. Stellen Sie sicher, dass die Parameter an das Paket gebunden wurden.

    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    **Beispielausgabe**

    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. Stellen Sie sicher, dass die Parameter von dem Paket übernommen wurden.

    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    **Beispielausgabe**

    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## Pakete von Aktionen gemeinsam nutzen
{: #actions_pkgs_share}

Nachdem die Aktionen und Feeds, die ein Paket bilden, auf Fehler geprüft und getestet worden sind, kann das Paket zur gemeinsamen Nutzung durch alle {{site.data.keyword.openwhisk_short}}-Benutzer bereitgestellt werden. Durch die gemeinsame Nutzung haben Benutzer die Möglichkeit, das Paket zu binden, Aktionen in dem Paket aufzurufen sowie {{site.data.keyword.openwhisk_short}}-Regeln zu verfassen und Aktionssequenzen zu erstellen. Aktionen und Feeds in einem gemeinsam genutzten Paket sind _öffentlich_. Wenn ein Paket privat ist, ist auch sein gesamter Inhalt privat.
{: shortdesc}

1. Stellen Sie das Paket zur gemeinsamen Nutzung für alle Benutzer bereit. 

  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. Zeigen Sie die Eigenschaft `publish` des Pakets an, um zu prüfen, ob sie jetzt den Wert 'true' hat.

  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  **Beispielausgabe**

  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. Rufen Sie eine Beschreibung des Pakets ab, um den vollständig qualifizierten Namen des Pakets für andere Benutzer bereitzustellen, damit sie das Paket binden oder darin enthaltene Aktionen aufrufen können. Der vollständig qualifizierte Name schließt den Namensbereich ein (in diesem Beispiel der Namensbereich `myNamespace`).

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Beispielausgabe**

  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## Umgebungsvariablen für Aktionen
{: #actions_envvars}

Die Aktionsumgebung enthält mehrere Umgebungsvariablen, die für die aktive Aktion spezifisch sind. Auf die Eigenschaften kann in der Systemumgebung für alle unterstützten Laufzeiten zugegriffen werden. Mit diesen Eigenschaften können Aktionen programmgestützt über die REST-API mit Assets arbeiten oder einen internen Alarm auslösen, wenn die Aktion kurz davor ist, das zugeteilte Zeitbudget aufzubrauchen.
{: shortdesc}

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| `__OW_API_HOST` | Der API-Host für die Bereitstellung, die diese Aktion ausführt. |
| `__OW_API_KEY` | Der API-Schlüssel für das Subjekt, das die Aktion aufruft. Diese Variable wird nur für klassische CF-basierte Namensbereiche bereitgestellt. |
| `__OW_NAMESPACE` | Die Namensbereichs-ID (GUID). Bei klassischen CF-basierten Namensbereichen wird diese ID aus dem Organisations- und Bereichsnamen erstellt. |
| `__OW_NAMESPACE_CRN` | Der Cloudressourcenname (Cloud Resource Name, [CRN](/docs/overview?topic=overview-crn)) des Namensbereichs. Der CRN ist nur für Namensbereiche verfügbar, die für IAM aktiviert sind.
| `__OW_ACTION_NAME` | Der vollständig qualifizierte Name der Aktion, die ausgeführt wird. |
| `__OW_IAM_NAMESPACE_API_KEY` | Der API-Schlüssel für Namensbereiche, die für IAM aktiviert sind. Informationen zur Verwendung finden Sie in [Zugriffsrichtlinien festlegen](/docs/openwhisk?topic=cloud-functions-namespaces#namespace-access). |
| `__OW_IAM_API_URL` | Der Serviceendpunkt, der für IAM-Operationen wie den Abruf eines Tokens vom API-Schlüssel verwendet wird. Diese Variable ist nur für Namensbereiche verfügbar, die für IAM aktiviert sind.
|
| `__OW_ACTIVATION_ID` | Die Aktivierungs-ID für die Instanz dieser in der Ausführung befindlichen Aktion. |
| `__OW_DEADLINE` | Die näherungsweise berechnete Zeit (in Millisekunden im Epochenformat), zu der diese Aktion ihr gesamtes Kontingent für die Dauer aufgebraucht hat. |

### Aktionsumgebungsvariablen in Ihre App integrieren
{: #actions_envvars_app}

Wenn Sie die Werte für eine Aktion anzeigen möchten, schließen Sie die Anzeige der Werte in Ihren App-Code ein und geben Sie sie in den Ergebnissen aus.

**Beispiel für Python**
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

Nachdem Sie den Code in einer Aktion aktualisiert und aktiviert haben, enthält das Ergebnis den vollständig qualifizierten Namen für die Aktion.
```bash
"response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
                "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
{: screen}



