---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Aktionen verwalten
{: #managing_actions}
{: #openwhisk_managing}

Sie können Aktionen verwalten, indem Sie die Aktionsausgabe überwachen, bestimmte Informationen zu einer Aktion abrufen oder Aktionen löschen.
{: shortdec}

## Aktionen abrufen
{: #getting-actions}

Nachdem Sie eine Aktion erstellt haben, können Sie weitere Informationen zu den Aktionsdetails abrufen und die Aktionen in Ihrem Namensbereich auflisten.
{: shortdesc}

So listen Sie alle von Ihnen erstellten Aktionen auf:
```
ibmcloud fn action list
```
{: pre}

Mit dem Erstellen weiterer Aktionen kann es sinnvoll sein, zusammengehörige Aktionen in [Paketen](/docs/openwhisk?topic=cloud-functions-openwhisk_packages) zu gruppieren. Ihre Liste von Aktionen können Sie wie folgt nach Aktionen in nur einem bestimmten Paket filtern:
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

Metadaten, die bestimmte Aktionen beschreiben, können Sie wie folgt abrufen:

```
ibmcloud fn action get hello
```
{: pre}

Beispielausgabe:
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

<table>
<caption>Informationen zu der Ausgabe des Befehls <code>action get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu der Ausgabe des Befehls <code>action get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Der Namensbereich, in dem die Ausführung dieser Aktion erfolgt.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Der Name der Aktion.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>Die semantische Version der Aktion.</td>
</tr>
<tr>
<td><code>exec</code></td>
<td><ul><li><code>kind</code>: Die Art von Aktion. Folgende Werte sind gültig: nodejs:6, nodejs:8, php:7.1, python:3, python-jessie:3, swift:3.1.1, swift:4.1, java, blackbox, sequence.</li>
<li><code>code</code>: Der Javascript- oder Swift-Code, der ausgeführt werden soll, wenn 'kind' den Wert 'nodejs' oder 'swift' hat.</li>
<li><code>components</code>: Die Aktionen in der Sequenz, wenn 'kind' den Wert 'sequence' hat. Die Aktionen werden der Reihenfolge nach aufgelistet.</li>
<li><code>image</code>: Der Name des Container-Image, wenn 'kind' den Wert 'blackbox' hat.</li>
<li><code>init</code>: Optionale Referenz auf eine komprimierte Datei, wenn 'kind' den Wert 'nodejs' hat.</li>
<li><code>binary</code>: Gibt an, ob die Aktion als ausführbare Binärdatei kompiliert ist.</li></ul></td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotationen für diese Aktion. Eine Liste der gültigen Annotationen enthalten die Referenzinformationen in [Annotationen für Aktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#action) und [Annotationen für Webaktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#annotations-specific-to-web-actions).</td>
</tr>
<tr>
<td><code>limits</code></td>
<td><ul><li><code>timeout</code>: Das für die Aktion in Millisekunden festgelegte Zeitlimit, nach dessen Verstreichen die Aktion beendet wird. Standardwert: 6000</li>
<li><code>memory</code>: Die maximale Hauptspeichergrenzwert in MB, der für die Aktion festgelegt ist. Standardwert: 256</li>
<li><code>logs</code>: Das für die Aktion festgelegte maximale Protokollgrößenlimit in MB. Standardwert: 10</li></ul></td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Gibt an, ob die Aktion öffentlich veröffentlicht wird.</td>
</tr>
</tbody></table>

## Aktivierungsdetails anzeigen
{: #activation_details}

{{site.data.keyword.openwhisk_short}}-Aktionen können von anderen Benutzern, als Reaktion auf verschiedene Ereignisse oder als Teil einer Aktionssequenz aufgerufen werden. Wann immer eine Aktion aufgerufen wird, wird für diesen Aufruf ein Aktivierungsdatensatz erstellt. Wenn Sie Informationen zum Ergebnis des Aktionsaufrufs wünschen, können Sie Details zu Aktivierungen abrufen.

So rufen Sie alle Aktivierungsdatensätze (Aktivierungs-IDs) in einem Namensbereich ab:
```
ibmcloud fn activation list
```
{: pre}

So rufen Sie Details zu einem bestimmten Aktivierungsdatensatz ab, der aus einem Aktionsaufruf hervorgegangen ist:
```
ibmcloud fn activation get <activation_ID>
```
{: pre}

Beispielausgabe:
```
ok: got activation c2b36969fbe94562b36969fbe9856215
{
    "namespace": "myNamespace",
    "name": "hello",
    "version": "0.0.1",
    "subject": "user@email.com",
    "activationId": "c2b36969fbe94562b36969fbe9856215",
    "start": 1532456307768,
    "end": 1532456309838,
    "duration": 2070,
    "response": {
        "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "done": true
  }
    },
    "logs": [],
    "annotations": [
        {
            "key": "path",
            "value": "myNamespace/hello"
        },
        {
            "key": "waitTime",
            "value": 50
        },
        {
            "key": "kind",
    "value": "nodejs:6"
        },
        {
            "key": "limits",
    "value": {
                "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
        },
        {
            "key": "initTime",
            "value": 53
        }
    ],
    "publish": false
}
```
{: screen}

<table>
<caption>Informationen zu der Ausgabe des Befehls <code>activation get</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Symbol 'Idee'"/> Informationen zu der Ausgabe des Befehls <code>activation get</code></th>
</thead>
<tbody>
<tr>
<td><code>namespace</code></td>
<td>Der Namensbereich, in dem sich diese Aktivierung befindet. Dieser Namensbereich kann ein anderer sein als der, in dem die Aktion ausgeführt wird.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Der Name der Aktion.</td>
</tr>
<tr>
<td><code>version</code></td>
<td>Die semantische Version der Aktion.</td>
</tr>
<tr>
<td><code>subject</code></td>
<td>Das Benutzerkonto, mit dem das Element aktiviert wurde.</td>
</tr>
<tr>
<td><code>activationId</code></td>
<td>ID dieses Aktivierungsdatensatzes.</td>
</tr>
<tr>
<td><code>start</code></td>
<td>Zeit, zu der die Aktivierung begann.</td>
</tr>
<tr>
<td><code>end</code></td>
<td>Zeit, zu der die Aktivierung abgeschlossen wurde.</td>
</tr>
<tr>
<td><code>duration</code></td>
<td>Die Zeit (in Millisekunden), die benötigt wurde, um die Aktivierung vollständig auszuführen.</td>
</tr>
<tr>
<td><code>response</code></td>
<td><ul><li><code>status</code>: Der Exitstatus der Aktivierung.</li>
<li><code>statusCode</code>: Der Statuscode oder, wenn die Aktion fehlerhaft war, der HTTP-Fehlercode.</li>
<li><code>success</code>: Gibt an, ob die Aktion erfolgreich ausgeführt wurde.</li>
<li><code>result</code>: Der Rückgabewert von der Aktivierung.</li>
</ul></td>
</tr>
<tr>
<td><code>logs</code></td>
<td>Protokolle für diese Aktivierung.</td>
</tr>
<tr>
<td><code>annotations</code></td>
<td>Annotationen für diese Aktion. Eine Liste der gültigen Annotationen enthalten die Referenzinformationen zu [Annotationen](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#activation).</td>
</tr>
<tr>
<td><code>publish</code></td>
<td>Gibt an, ob die Aktion öffentlich veröffentlicht wird.</td>
</tr>
</tbody></table>

## In der Aktionskomponente auf Aktionsmetadaten zugreifen
{: #accessing-action-metadata-within-the-action-body}

Die Aktionsumgebung enthält mehrere Eigenschaften, die für die aktive Aktion spezifisch sind. Mit diesen Eigenschaften kann die Aktion programmgestützt über die REST-API mit OpenWhisk-Assets arbeiten oder einen internen Alarm auslösen, wenn die Aktion kurz davor ist, das zugeteilte Zeitbudget aufzubrauchen. Auf die Eigenschaften kann in der Systemumgebung für alle unterstützten Laufzeiten zugegriffen werden: Node.js, Python, Swift, Java und Docker bei Verwendung des Docker-Gerüsts für OpenWhisk.

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| `__OW_API_HOST` | Der API-Host für die OpenWhisk-Bereitstellung, die diese Aktion ausführt. |
| `__OW_API_KEY` | Der API-Schlüssel für das Subjekt, das die Aktion aufruft. Dieser Schlüssel kann ein eingeschränkter API-Schlüssel sein und ist nicht angegeben, es sei denn, er wird explizit angefordert. Siehe [Annotationen](/docs/openwhisk?topic=cloud-functions-openwhisk_annotations#openwhisk_annotations). |
| `__OW_NAMESPACE` | Der Namensbereich für die Aktivierung. Dieser Namensbereich kann ein anderer als der Namensbereich für die Aktion sein. |
| `__OW_ACTION_NAME` | Der vollständig qualifizierte Name der Aktion, die ausgeführt wird. |
| `__OW_ACTIVATION_ID` | Die Aktivierungs-ID für die Instanz dieser in der Ausführung befindlichen Aktion. |
| `__OW_DEADLINE` | Die näherungsweise berechnete Zeit (in Millisekunden in Epochenformat), zu der diese Aktion ihr gesamtes Kontingent für die Dauer aufgebraucht haben wird. |

## Aktions-URL abrufen
{: #get-action-url}

Eine Aktion kann über die REST-Schnittstelle durch eine HTTPS-Anforderung aufgerufen werden.
{: shortdesc}

So rufen Sie eine Aktions-URL ab:
```
ibmcloud fn action get actionName --url
```
{: pre}

Beispielausgabe für Standardaktionen:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Beispielausgabe für [Webaktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions):
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Hinweis:** Bei Standardaktionen ist eine Authentifizierung erforderlich, wenn der Aufruf über eine HTTPS-Anforderung erfolgt. Weitere Informationen zu Aktionsaufrufen über die REST-Schnittstelle finden Sie in der [REST-API-Referenz](https://cloud.ibm.com/apidocs/functions).

## Aktionscode speichern
{: #save-action}

Es ist möglich, Code, der einer vorhandenen Aktion zugeordnet ist, abzurufen und lokal zu speichern. Code kann für alle Aktionen mit Ausnahme von Sequenzen und Docker-Aktionen gespeichert werden.
{: shortdesc}

Speichern Sie den Aktionscode unter einem Dateinamen, der dem Namen einer vorhandenen Aktion im aktuellen Arbeitsverzeichnis entspricht.
```
ibmcloud fn action get actionName --save
```
{: pre}

Es wird eine Dateierweiterung verwendet, die der Art ('kind') der verwendeten Aktion entspricht. Bei Aktionscode, der sich in einer ZIP-Datei befindet, wird die Erweiterung .zip verwendet. Beispielausgabe:
```
ok: saved action code to /absolutePath/currentDirectory/actionName.js
```
{: screen}

Wenn Sie das Flag `--save-as` verwenden, können Sie stattdessen einen angepassten Dateipfad, einen angepassten Dateinamen und eine angepasste Erweiterung angeben.
```
ibmcloud fn action get actionName --save-as codeFile.js
```
{: pre}

Beispielausgabe:
```
ok: saved action code to /absolutePath/currentDirectory/codeFile.js
```
{: screen}

## Aktionsprotokolle überwachen
{: #monitor-action-output}

{{site.data.keyword.openwhisk_short}}-Aktionen können von anderen Benutzern, als Reaktion auf verschiedene Ereignisse oder als Teil einer Aktionssequenz aufgerufen werden. Um Informationen darüber zu erhalten, wann Aktionen aufgerufen wurden und was die Ausgabe war, kann es nützlich sein, die Aktionsprotokolle zu überwachen.

Sie können die Ausgabe von Aktionen, wenn sie aufgerufen werden, über die {{site.data.keyword.openwhisk_short}}-CLI beobachten.

1. Starten Sie eine Sendeaufrufschleife, die ununterbrochen prüft, ob Protokolle von Aktivierungen vorhanden sind.
    ```
    ibmcloud fn activation poll
    ```
    {: pre}

2. Wechseln Sie zu einem anderen Fenster und rufen Sie eine Aktion auf.
    ```
    ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
    ```
    {: screen}

3. Im Polling-Fenster wird das Aktivierungsprotokoll angezeigt.
    ```
    Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
    ```
    {: screen}
    Gegebenenfalls werden auch die Protokolle für Aktionen, die für Sie in {{site.data.keyword.openwhisk_short}} ausgeführt werden, in Echtzeit angezeigt.
    
## Aktionslaufzeit ändern
{: #changing-action-runtime}

Sie können die Art der Laufzeit (`kind`) ändern, um zu einer neueren Version der Aktionslaufzeit zu migrieren. Da sich Node.js Version 8 beispielsweise im Wartungsmodus befindet, möchten Sie die Laufzeit möglicherweise in Node.js 10 ändern. Führen Sie die folgenden Schritte aus, um eine Aktionslaufzeit zu ändern. **Hinweis:** Möglicherweise müssen Sie den Code in `actionName.js` ändern, um mit der neuen Laufzeitversion kompatibel zu sein. Dies hängt davon ab, ob solche Änderungen von dem Laufzeitswitch benötigt werden. In den meisten Fällen sind die Laufzeitversionen kompatibel. 

1. Speichern Sie den Aktionscode in einer Datei. 

  ```
  ibmcloud fn action get actionName --save
  ```
  {: pre}

2. Aktualisieren Sie die Aktion, indem Sie die neue Laufzeit angeben. 

  ```
  ibmcloud fn action update actionName actionName.js --kind nodejs:10
  ```
  {: pre}

Eine Liste der verfügbaren Laufzeiten finden Sie unter [Laufzeiten](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)

## Umfangreiche Aktionen verwalten
{: #large-app-support}

Die maximale Codegröße für eine Aktion ist 48 MB. Anwendungen, die viele Drittanbietermodule, native Bibliotheken oder externe Tools enthalten, erreichen diesen Grenzwert gegebenenfalls. Wenn Sie eine Aktion mit einem ZIP- oder JAR-Paket mit einer Größe von über 48 MB erstellen, müssen Sie das Laufzeitimage um Abhängigkeiten erweitern und anschließend eine einzelne Quellendatei oder ein Archiv mit einer Größe von weniger als 48 MB verwenden.

Wenn zum Beispiel eine Docker-Laufzeit erstellt wird, die erforderliche gemeinsam genutzte Bibliotheken einschließt, müssen eventuelle Abhängigkeiten nicht in der Archivdatei enthalten sein. Private Quellendateien können weiterhin in das Archiv eingebunden und während der Ausführung eingefügt werden.

## Aktionen löschen
{: #deleting-actions}

Sie können eine Bereinigung durchführen, indem Sie Aktionen löschen, die nicht mehr verwendet werden sollen.

1. Löschen Sie eine Aktion.
    ```
    ibmcloud fn action delete hello
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: deleted hello
    ```
    {: screen}

2. Überprüfen Sie, ob die Aktion nicht mehr in der Liste der Aktionen angezeigt wird.
    ```
    ibmcloud fn action list
    ```
    {: pre}

    Beispielausgabe:
    ```
    actions
    ```
    {: screen}
