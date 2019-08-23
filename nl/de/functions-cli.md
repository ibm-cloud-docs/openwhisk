---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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




# {{site.data.keyword.openwhisk_short}}-Befehlszeilenschnittstelle
{: #functions-cli}

Mit den im Folgenden aufgeführten Befehlen können Sie die Entitäten verwalten, aus denen sich Ihre Funktionen zusammensetzen.
{: shortdec}

<br />

## 'action'-Befehle
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

Erstellen Sie eine Aktion.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>Der Name der Aktion. Wenn Sie die Aktion in ein Paket aufnehmen möchten, geben Sie den Namen im Format `PACKAGE_NAME`/`ACTION_NAME` ein. Dieser Wert ist erforderlich. </dd>

   <dt>`APP_FILE`</dt>
   <dd>Der Pfad zu der App-Datei oder dem Paket, die/das als Aktion ausgeführt werden soll. Diese Option ist erforderlich.</dd>
   
   <dt>`--copy`</dt>
   <dd>Die Aktion wird als Name einer vorhandenen Aktion behandelt.</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>Der Docker Hub-Benutzername und der Name des Docker-Images in Docker Hub, mit denen die Aktion ausgeführt werden soll. Dieses Flag wird benötigt, um Aktionen aus Docker-Images zu erstellen.</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>Die Laufzeit für Ihre App. Dieses Flag ist optional. Wenn keine Angabe für `VALUE` erfolgt, wird die Standardversion für die erkannte Laufzeit verwendet.
     Mögliche Werte (`VALUES`) für die Option `--kind`.
     <table>
  <tr>
    <th>Sprache</th>
    <th>ID des Typs</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (Standard), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (Standard)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (Standard)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (Standard)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (Standard)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (Standard)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (Standard)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (Standard)</td>
  </tr>
  <tr>
    <td>Andere Sprachen werden durch die Verwendung von Docker-Aktionen unterstützt.</td>
  </tr>
</table>
{: caption="Tabelle 1. Unterstützte Laufzeiten" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>Die maximale Protokollgröße in MB für die Aktion. Der Standardwert ist 10 MB.</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>Wenn die Eingangsmethode der Aktion nicht `main` ist, geben Sie den angepassten Namen an. Dieses Flag ist erforderlich, wenn die Eingangsmethode nicht `main` ist. Bei einigen Laufzeiten (wie z. B. Java) muss der Name die vollständig qualifizierte Methode sein. </dd>

   <dt>`--native`</dt>
   <dd>Sie können das Argument `--native` als Kurzform für `--docker openwhisk/dockerskeleton` verwenden. Mit diesem Argument können Sie eine ausführbare Datei erstellen und bereitstellen, die innerhalb des standardmäßigen Docker-SDK für Aktionen ausgeführt wird.
       <ol><li>Wenn Sie ein Docker-Image erstellen, wird eine ausführbare Datei innerhalb des Containers unter `/action/exec` erstellt. Kopieren Sie die Datei `/action/exec` in Ihr lokales Dateisystem und komprimieren Sie sie in die Datei `exec.zip`. </li>
       <li>Erstellen Sie eine Docker-Aktion, die die ausführbare Datei als Initialisierungsdaten empfängt. Hierbei ersetzt das Argument `--native` das Argument `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>Erstellen Sie eine Aktionssequenz und geben Sie die Namen der zugehörigen Aktionen an. Trennen Sie die Aktionsnamen (`ACTION_NAMEs`) durch Kommas. </dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>Das Zeitlimit (`LIMIT`) in Millisekunden. Der Standardwert ist 60000 Millisekunden. Wenn das Zeitlimit erreicht ist, wird die Aktion beendet.</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>Behandelt die Aktion als Webaktion, als unaufbereitete HTTP-Webaktion oder als Standardaktion. Geben Sie `yes` oder `true` für eine Webaktion, `raw` für eine unaufbereitete HTTP-Webaktion oder `no` oder `false` für eine Standardaktion an. Geben Sie zum Schützen Ihrer Webaktion auch die Option `--web-secure` an. </dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>Schützt die Webaktion. Die Angabe `VALUE` für `SECRET` kann `true`, `false` oder eine beliebige Zeichenfolge sein. Diese Option kann nur zusammen mit der Option `--web` verwendet werden.</dd>
   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

Sie können Ihren Namensbereich bereinigen, indem Sie Aktionen löschen, die Sie nicht mehr verwenden wollen.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

Sie können Metadaten abrufen, die eine bestimmte Aktion beschreiben.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Der Name einer Aktion. Dieser Wert ist erforderlich.</dd>

   <dt>`--save`</dt>
   <dd>Sie können den Code, der zu einer vorhandenen Aktion gehört, abrufen und lokal speichern. Eine Ausnahme bilden dabei Sequenzen und Docker-Aktionen. `FILENAME` entspricht dem Namen einer vorhandenen Aktion im aktuellen Arbeitsverzeichnis und die Dateierweiterung entspricht der Art der Aktion. Beispielsweise wird für Aktionscode, der sich in einer Archivdatei befindet, die Erweiterung '.zip' verwendet. Dieses Flag ist optional.</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>Speichert den Code für Aktionen in einer Datei mit angepasstem Namen, wenn Sie einen Dateipfad, `FILENAME` und die Erweiterung angeben. Dieses Flag ist optional.</dd>

  <dt>`--summary`</dt>
  <dd>Ruft eine Zusammenfassung der Aktionsdetails ab. Parameter mit dem Präfix "*" werden gebunden. Parameter mit dem Präfix "**" werden gebunden und endgültig festgelegt. Dieses Flag ist optional.</dd>

  <dt>`--url`</dt>
  <dd>Ruft nur die URL für die Aktion ab. Dieses Flag ist optional.</dd>
   </dl>

<br />**Beispiel**

```
ibmcloud fn action get hello
```
{: pre}

**Ausgabe**
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
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

Sie können eine Aktion ausführen, um sie zu testen.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Der Name der Aktion. Dieser Wert ist erforderlich. </dd>

   <dt>`--blocking, -b`</dt>
   <dd>Blockierende Aufrufe verwenden einen Typ von Anforderung/Antwort und warten, bis das Aktivierungsergebnis verfügbar ist. Der Wartezeitraum beträgt 60 Sekunden oder entspricht dem [Zeitlimit (`LIMIT` `VALUE`](/docs/openwhisk?topic=cloud-functions-limits)) der Aktion, je nachdem, welcher Wert geringer ist. Dieses Flag ist optional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

   <dt>`--result, -r`</dt>
   <dd>Das Ergebnis des App-Codes wird als Ausgabe des Befehls angezeigt. Wenn diese Option nicht angegeben wird, wird die Aktivierungs-ID angezeigt. Der Aufruf blockiert, wenn diese Option angegeben wird. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `ibmcloud fn action list`
{: #cli_action_list}

Sie können alle Aktionen, die Sie erstellt haben, oder eine bestimmte Anzahl von Aktionen auflisten.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Der Name eines Pakets mit Aktionen. Dieser Wert ist optional. Wenn keine Angabe gemacht wird, werden alle Aktionen aufgelistet.</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>Listet eine angegebene Anzahl von Aktionen auf. Standardmäßig werden 30 Aktionen aufgelistet.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen Aktionen nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert.</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>Schließt eine angegebene Anzahl der zuletzt erstellten Aktionen aus dem Ergebnis aus.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

Sie können eine Aktion oder die App in einer Aktion aktualisieren.

Wenn Sie Parameter für ein Paket, eine Aktion oder einen Auslöser aktualisieren, müssen Sie alle zuvor erstellten Parameter angeben. Andernfalls werden die zuvor erstellten Parameter entfernt. Bei Paketen werden alle Services, die an das Paket gebunden wurden, ebenfalls entfernt. Daher müssen Sie die [Services erneut an Ihr Paket binden](/docs/openwhisk?topic=cloud-functions-services), nachdem Sie andere Parameter aktualisiert haben.
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Befehlsoptionen**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Der Name der Aktion. Wenn Sie die Aktion in ein Paket aufnehmen möchten, geben Sie den Namen im Format `PACKAGE_NAME`/`ACTION_NAME` ein. Dieser Wert ist erforderlich. </dd>

  <dt>`APP_FILE`</dt>
  <dd>Der Pfad zu der App-Datei oder dem Paket, die/das als Aktion ausgeführt werden soll. Diese Option ist erforderlich, wenn Sie Ihre App in der Aktion aktualisieren möchten.</dd>

  <dt>`--copy`</dt>
  <dd>Die Aktion wird als Name einer vorhandenen Aktion behandelt.</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>Der Docker Hub-Benutzername und der Name des Docker-Images in Docker Hub, mit denen die Aktion ausgeführt werden soll. Dieses Flag wird benötigt, um Aktionen aus Docker-Images zu erstellen.</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>Die Laufzeit für Ihre App. Dieses Flag ist optional. Wenn keine Angabe für VALUE erfolgt, wird die Standardversion für die erkannte Laufzeit verwendet.
    Mögliche Werte für die Option `--kind`.
    <table>
  <tr>
    <th>Sprache</th>
    <th>ID des Typs</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (Standard), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (Standard)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (Standard)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (Standard)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (Standard)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (Standard)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (Standard)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (Standard)</td>
  </tr>
  <tr>
    <td>Andere Sprachen werden durch die Verwendung von Docker-Aktionen unterstützt.</td>
  </tr>
</table>
{: caption="Tabelle 1. Unterstützte Laufzeiten" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>Die maximale Protokollgröße in MB für die Aktion. Der Standardwert ist 10 MB.</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>Wenn die Eingangsmethode der Aktion nicht `main` ist, geben Sie den angepassten Namen an. Dieses Flag ist erforderlich, wenn die Eingangsmethode nicht `main` ist. Bei einigen Laufzeiten (wie z. B. Java) muss der Name die vollständig qualifizierte Methode sein. </dd>

  <dt>`--native`</dt>
  <dd>Sie können das Argument `--native` als Kurzform für `--docker openwhisk/dockerskeleton` verwenden. Mit dem Argument können Sie eine ausführbare Datei erstellen und bereitstellen, die innerhalb des standardmäßigen Docker-SDK für Aktionen ausgeführt wird.
       <ol><li>Wenn Sie ein Docker-Image erstellen, wird eine ausführbare Datei innerhalb des Containers unter `/action/exec` erstellt. Kopieren Sie die Datei `/action/exec` in Ihr lokales Dateisystem und komprimieren Sie sie in die Datei `exec.zip`. </li>
      <li>Erstellen Sie eine Docker-Aktion, die die ausführbare Datei als Initialisierungsdaten empfängt. Hierbei ersetzt das Argument `--native` das Argument `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>Erstellt eine Aktionssequenz durch Angabe des Namens der zugehörigen Aktionen.</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>Das Zeitlimit in Millisekunden. Der Standardwert ist 60000 Millisekunden. Wenn das Zeitlimit erreicht ist, wird die Aktion beendet.</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>Behandelt die Aktion als Webaktion, als unaufbereitete HTTP-Webaktion oder als Standardaktion. Geben Sie `yes` oder `true` für eine Webaktion, `raw` für eine unaufbereitete HTTP-Webaktion oder `no` oder `false` für eine Standardaktion an. Geben Sie zum Schützen Ihrer Webaktion auch die Option `--web-secure` an. </dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>Schützt die Webaktion. Die Angabe `VALUE` für `SECRET` kann `true`, `false` oder eine beliebige Zeichenfolge sein. Diese Option kann nur zusammen mit der Option `--web` verwendet werden.</dd>
  </dl>

<br />**Beispiel**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## 'activation'-Befehle
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

Sie können Metadaten abrufen, die eine bestimmte Aktivierung beschreiben.

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**Befehlsoptionen**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>Die ID für eine bestimmte Aktivierung. Verwenden Sie `ibmcloud fn activation list`, um eine Liste der verfügbaren IDs abzurufen. Dieser Wert ist erforderlich, es sei denn, die Option `--last` oder `-l` wird angegeben.</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>Ein Feld in den Metadaten, von dem Informationen angezeigt werden sollen. Wenn Sie z. B. das Protokollfeld anzeigen möchten, führen Sie `ibmcloud fn activation get ACTIVATION_ID logs` aus. Dieser Wert ist optional.</dd>

  <dt>`--last, -l`</dt>
  <dd>Zeigt die Metadaten für die letzte Aktivierung an. Dieses Flag ist optional.</dd>

  <dt>`--summary, -s`</dt>
  <dd>Zeigt die Ergebnisantwort nur für die Aktivierungsdetails an. Dieses Flag ist optional.</dd>
  </dl>


<br />**Beispiel**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list`
{: #cli_activation_list}

Sie können alle Aktivierungs-IDs für alle Aktionen in einem Paket auflisten.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**Befehlsoptionen**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>Zeigt die vollständige Aktivierungsbeschreibung an. </dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Listet eine angegebene Anzahl von Aktionen auf. Der Standardwert sind 30 Aktivierungen und das Maximum sind 200 Aktivierungen.</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>Listet die Aktivierungen auf, die seit dem angegebenen Datum erstellt wurden. Die Dauer wird in Millisekunden seit dem 1. Januar 1970 gemessen. Beispiel: `1560371263` ist der 12. Juni 2019 08:27:43 (UTC). </dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Schließt eine angegebene Anzahl der letzten Aktivierungen aus dem Ergebnis aus.</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>Listet die Aktivierungen auf, die vor dem angegebenen Datum erstellt wurden. Die Dauer wird in Millisekunden seit dem 1. Januar 1970 gemessen. Beispiel: `1560371263` ist der 12. Juni 2019 08:27:43 (UTC). </dd>
  </dl>

<br />**Beispiel**
```
ibmcloud fn activation list
```
{: pre}

**Ausgabe**
```
activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

Ruft Protokolle für eine bestimmte Aktivierung ab.

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**Befehlsoptionen**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>Die ID für eine bestimmte Aktivierung. Verwenden Sie `ibmcloud fn activation list`, um eine Liste der verfügbaren IDs abzurufen. Dieser Wert ist erforderlich, es sei denn, die Option `--last` oder `-l` wird angegeben.</dd>

  <dt>`--last, -l`</dt>
  <dd>Zeigt die Protokolle für die letzte Aktivierung an. Dieses Flag ist optional.</dd>

  <dt>`--strip, -r`</dt>
  <dd>Zeigt nur die Protokollnachricht an; die Zeitmarke und die Datenstrominformationen werden ausgeschlossen. Dieses Flag ist optional.</dd>
  </dl>

<br />**Beispiel**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

Zeigt eine Live-Liste (Streaming) der Aktivierungen für eine Aktion oder einen Namensbereich an. Sie können `STRG+C` drücken, um das Polling zu beenden. 

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**Befehlsoptionen**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>Ein Namensbereich, der mit / beginnt. Fragt Aktivierungen für einen Namensbereich, eine Aktion oder einen Bereich ab. Dieser Wert ist optional. Wenn kein Namensbereich und keine Aktion angegeben wird, wird der Bereich abgefragt.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Fragt Aktivierungen für einen Namensbereich, eine Aktion oder einen Bereich ab. Dieser Wert ist optional. Wenn kein Namensbereich und keine Aktion angegeben wird, wird der Bereich abgefragt.</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>Führt das Polling von Aktivierungen für eine angegebene Anzahl von Sekunden aus und beendet dann das Programm. Dieses Flag ist optional.</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>Startet das Polling für Aktivierungen eine angegebene Anzahl von Tagen zuvor. Dieses Flag ist optional.</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>Startet das Polling für Aktivierungen eine angegebene Anzahl von Stunden zuvor. Dieses Flag ist optional.</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>Startet das Polling für Aktivierungen eine angegebene Anzahl von Minuten zuvor. Dieses Flag ist optional.</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>Startet das Polling für Aktivierungen eine angegebene Anzahl von Sekunden zuvor. Dieses Flag ist optional.</dd>
  </dl>

<br />**Beispiel**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

Ruft das Ergebnis von einer bestimmte Aktivierung ab.

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**Befehlsoptionen**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>Die ID für eine bestimmte Aktivierung. Verwenden Sie `ibmcloud fn activation list`, um eine Liste der verfügbaren IDs abzurufen. Dieser Wert ist erforderlich, es sei denn, die Option `--last` oder `-l` wird angegeben.</dd>

  <dt>`--last, -l`</dt>
  <dd>Zeigt das Ergebnis für die letzte Aktivierung an. Dieses Flag ist optional.</dd>

  </dl>

<br />**Beispiel**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## 'api'-Befehle
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

Sie können eine API erstellen.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Der Basispfad für die API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Der Name der API. Der API-Name kann mit dem Basispfad identisch sein.</dd>

   <dt>`API_VERB`</dt>
   <dd>Das Verb für die API, wie z. B. `get` oder `post`.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>Der Name der Aktion.</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>Der Name der API. Dieses Flag wird ignoriert, wenn eine Konfigurationsdatei angegeben wird. Der Standardname ist `BASE_PATH`. Dieses Flag ist optional.</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Swagger-API-Konfiguration enthält. Wenn dieses Flag verwendet wird, wird das Flag des API-Namens ignoriert. Dieses Flag ist erforderlich.</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>Legt für den Antworttyp für die Webaktion entweder `html`, `http`, `json`, `text` oder `svg` fest. Der Standardwert ist `json`. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

Sie können eine API löschen.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Der Basispfad für die API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Der Name der API. Der API-Name kann mit dem Basispfad identisch sein.</dd>

   <dt>`API_PATH`</dt>
   <dd>Der Pfad zu der API. </dd>

   <dt>`API_VERB`</dt>
   <dd>Das Verb für die API, wie z. B. `GET` oder `POST`. </dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Gibt den API-Ausgabetyp als `json` oder `yaml` an. Der Standardwert ist `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Zeigt die vollständigen API-Konfigurationsdetails an.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

Ruft die Metadaten für eine API ab.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Der Basispfad für die API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Der Name der API. Der API-Name kann mit dem Basispfad identisch sein.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Gibt den API-Ausgabetyp als `json` oder `yaml` an. Der Standardwert ist `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Zeigt die vollständigen API-Konfigurationsdetails an.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `ibmcloud fn api list`
{: #cli_api_list}

Sie können alle APIs, die Sie erstellt haben, oder eine bestimmte Anzahl von APIs auflisten. Wenn kein Name oder Basispfad angegeben wird, werden alle APIs aufgelistet.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Der Basispfad für die API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Der Name der API. Der API-Name kann mit dem Basispfad identisch sein.</dd>

   <dt>`API_PATH`</dt>
   <dd>Der Pfad zu der API. </dd>

   <dt>`API_VERB`</dt>
   <dd>Das Verb für die API, wie z. B. `GET` oder `POST`. </dd>

   <dt>`--full, -f`</dt>
   <dd>Zeigt die vollständigen API-Details an. Dieses Flag ist optional. </dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>Listet eine angegebene Anzahl von APIs auf. Standardmäßig werden 30 APIs aufgelistet. Dieses Flag ist optional. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen APIs nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert. Dieses Flag ist optional. </dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>Schließt eine angegebene Anzahl der zuletzt erstellten APIs aus dem Ergebnis aus. Dieses Flag ist optional. </dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## 'deploy'-Befehle
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

Verwenden Sie eine Manifestdatei, um eine Sammlung von Paketen, Aktionen, Auslösern und Regeln bereitzustellen. 

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>Der `wsk`-API-Host. Dieses Flag ist optional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>Die Konfigurationsdatei. Der Standardwert ist `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>Der Pfad zur Bereitstellungsdatei.</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>Der Pfad zur Manifestdatei. Dieses Flag ist erforderlich, wenn sich die Datei 'manifest.yaml' nicht im aktuellen Verzeichnis befindet.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>Der Name oder die ID für einen Namensbereich.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

   <dt>`--preview` </dt>
   <dd>Zeigt vor der Bereitstellung den Bereitstellungsplan an. </dd>

   <dt>`--project PATH`</dt>
   <dd>Der Pfad zum serverunabhängigen Projekt. Der Standardwert ist <code>.</code> (aktuelles Verzeichnis).</dd>

   <dt>`--strict`</dt>
   <dd>Ermöglicht eine benutzerdefinierte Laufzeitversion.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Zeigt die ausführliche Ausgabe an.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Verwenden Sie eine Manifestdatei, um die Bereitstellung einer Sammlung von Paketen, Aktionen, Auslösern und Regeln zurückzunehmen. 

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>Der `wsk`-API-Host. Dieses Flag ist optional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>Die Konfigurationsdatei. Der Standardwert ist `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>Der Pfad zur Bereitstellungsdatei.</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>Der Pfad zur Manifestdatei. Dieses Flag ist erforderlich, wenn sich die Datei 'manifest.yaml' nicht im aktuellen Verzeichnis befindet.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>Der Name oder die ID für einen Namensbereich.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

   <dt>`--preview` </dt>
   <dd>Zeigt das Ergebnis des Befehls an, ohne den Befehl auszuführen. </dd>

   <dt>`--project PATH`</dt>
   <dd>Der Pfad zum serverunabhängigen Projekt. Der Standardwert ist `.` (aktuelles Verzeichnis).</dd>

   <dt>`--strict`</dt>
   <dd>Ermöglicht eine benutzerdefinierte Laufzeitversion.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Zeigt die ausführliche Ausgabe an.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## 'list'-Befehle
{: #cli_list_cmd}


### `ibmcloud fn list`
{: #cli_list}

Sie können eine gruppierte Liste der Pakete, Aktionen, Auslöser und Regeln im Namensbereich anzeigen.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert jede Gruppe von zurückgegebenen Entitäten nach Namen. Andernfalls werden die Gruppen nach Erstellungsdatum sortiert.</dd>
   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## 'namespace'-Befehle
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

Sie können einen IAM-Namensbereich erstellen.

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Der Name für einen Namensbereich. Geben Sie keinen Bindestrich (-) im Namen ein. Dieser Wert ist erforderlich.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Sie können Ihre eigene eindeutige Beschreibung erstellen, um den Namensbereich zu identifizieren. Wenn Ihre Beschreibung mehr als ein Wort enthält, schließen Sie die Beschreibung in Anführungszeichen (") ein. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

Sie können einen IAM-Namensbereich löschen.

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**Beispiel**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Sie können die Entitäten für bzw. die Metadateninformationen von einem Cloud Foundry- oder IAM-Namensbereich abrufen.

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Der Name oder die ID für einen Namensbereich. Dieser Wert ist erforderlich.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen Namensbereiche nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert. Dieses Flag ist optional. </dd>

   <dt>`--properties`</dt>
   <dd>Zeigt die Namensbereichseigenschaften anstelle der darin enthaltenen Entitäten an. Dieses Flag ist optional. </dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **Ausgabe**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

Sie können die verfügbaren Cloud Foundry- und IAM-Namensbereiche auflisten.

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--cf`</dt>
   <dd>Zeigt nur Namensbereiche für Cloud Foundry an. IAM-Namensbereiche werden nicht angezeigt. Dieses Flag ist optional.</dd>

   <dt>`--iam`</dt>
   <dd>Zeigt nur die IAM-Namensbereiche an. Cloud Foundry-Namensbereiche werden nicht angezeigt. Dieses Flag ist optional.</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Listet eine angegebene Anzahl von Namensbereichen auf. Standardmäßig werden 30 Namensbereiche aufgelistet. Dieses Flag ist optional. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen Namensbereiche nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert. Dieses Flag ist optional. </dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Schließt eine angegebene Anzahl der zuletzt erstellten Namensbereiche aus dem Ergebnis aus. Dieses Flag ist optional. </dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

Sie können den Namen oder die Beschreibung eines IAM-Namensbereichs ändern.

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Der Name für einen Namensbereich. Geben Sie keinen Bindestrich (-) im Namen ein. Dieser Wert ist erforderlich.</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>Der neue Name für einen Namensbereich. Geben Sie keinen Bindestrich (-) im Namen ein. Dieser Wert ist optional.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Sie können Ihre eigene eindeutige Beschreibung erstellen, um den Namensbereich zu identifizieren. Wenn Ihre Beschreibung mehr als ein Wort enthält, schließen Sie die Beschreibung in Anführungszeichen (") ein. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## 'package'-Befehle
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

Sie können Parameter an ein Paket binden. Alle Aktionen innerhalb des Pakets übernehmen diese Parameter, sofern nichts anderes angegeben ist.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Befehlsoptionen**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>Der Name des Pakets. Dieser Wert ist erforderlich. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>
  </dl>

<br />**Beispiel**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

Sie können ein Paket erstellen, das so konzipiert ist, dass es eine oder mehrere Aktionen enthält. Wenn Sie eine Aktion im Paket hinzufügen möchten, schließen Sie den Paketnamen in den Aktionsnamen ein, wenn Sie die Aktion erstellen oder aktualisieren.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Befehlsoptionen**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>Der Name des Pakets. Dieser Wert ist erforderlich. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Eine JSON-Datei, die Parameter im Format `KEYS` `VALUE` enthält. Dieses Flag ist optional.</dd>

  <dt>`--shared yes|no`</dt>
  <dd>Wird das Paket ohne einen Wert oder mit dem Wert 'yes' angegeben, wird das Paket mit anderen Benutzern gemeinsam genutzt.</dd>
  </dl>

<br />**Beispiel**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

Sie können Ihren Namensbereich bereinigen, indem Sie Pakete löschen, die Sie nicht mehr verwenden wollen.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

Sie können Metadaten abrufen, die ein bestimmtes Paket beschreiben.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**Befehlsoptionen**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>Der Name eines Pakets. Dieser Wert ist erforderlich.</dd>

   <dt>`--summary`</dt>
   <dd>Ruft eine Zusammenfassung der Paketdetails ab. Parameter mit dem Präfix "*" werden gebunden. Dieses Flag ist optional.</dd>
   </dl>

<br />**Beispiel**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `ibmcloud fn package list`
{: #cli_pkg_list}

Sie können alle Pakete, die Sie erstellt haben, oder eine bestimmte Anzahl von Paketen auflisten.

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>Listet die Pakete in einem bestimmten Namensbereich auf. Dieser Wert ist optional. Wenn keine Angabe gemacht wird, werden alle Pakete aufgelistet.</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>Listet eine angegebene Anzahl von Paketen auf. Standardmäßig werden 30 Pakete aufgelistet.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen Pakete nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert.</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>Schließt eine angegebene Anzahl der zuletzt erstellten Pakete aus dem Ergebnis aus.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Durch Ausführung von `ibmcloud fn package list /whisk.system` kann eine Liste von vorinstallierten Paketen angezeigt werden.
  {: tip}


<br />

### `ibmcloud fn package refresh`
{: #cli_pkg_refresh}

Sie können die Paketbindungen für alle Pakete in einem bestimmten Namensbereich aktualisieren.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>Ein Namensbereich, der mit / beginnt. Dieses Flag ist erforderlich. Führen Sie `ibmcloud fn namespace list` aus, um eine Liste der Namensbereiche zu erhalten, aus der Sie auswählen können.</dd>
   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

Sie können ein Paket aktualisieren, das so konzipiert ist, dass es eine oder mehrere Aktionen enthält. Wenn Sie eine Aktion im Paket hinzufügen möchten, schließen Sie den Paketnamen in den Aktionsnamen ein, wenn Sie die Aktion erstellen oder aktualisieren.

Wenn Sie Parameter für ein Paket, eine Aktion oder einen Auslöser aktualisieren, müssen Sie alle zuvor erstellten Parameter angeben. Andernfalls werden die zuvor erstellten Parameter entfernt. Bei Paketen werden alle Services, die an das Paket gebunden wurden, ebenfalls entfernt. Daher müssen Sie die [Services erneut an Ihr Paket binden](/docs/openwhisk?topic=cloud-functions-services), nachdem Sie andere Parameter aktualisiert haben.
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>Der Name des Pakets. Dieser Wert ist erforderlich. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

   <dt>`--shared yes|no`</dt>
   <dd>Wird das Paket ohne einen Wert oder mit dem Wert `yes` angegeben, wird das Paket mit anderen Benutzern gemeinsam genutzt.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## 'property'-Befehle
{: #cli_prop}

Sie können globale Eigenschaften für Ihre CLI-Umgebung festlegen oder Eigenschaften für die `wsk`-CLI anzeigen, die als Teil der `ibmcloud fn`-CLI ausgeführt wird.

### `ibmcloud fn property get`
{: #cli_prop_get}

Sie können die Metadatendetails für eine Eigenschaft über die `wsk`-CLI anzeigen.

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`--all`</dt>
   <dd>Zeigt alle Eigenschaften für die `wsk`-CLI an. Dieses Flag ist optional.</dd>

   <dt>`---apibuild`</dt>
   <dd>Die `wsk`-API-Buildinformationen. Dieses Flag ist optional.</dd>

   <dt>`--apibuildno`</dt>
   <dd>Die `wsk`-API-Buildnummer. Dieses Flag ist optional.</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>Der `wsk`-API-Host. Dieses Flag ist optional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>Die `wsk`-API-Version. Dieses Flag ist optional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Das `wsk`-Clientzertifikat. Dieses Flag ist optional.</dd>

   <dt>`--cliversion`</dt>
   <dd>Die `wsk`-CLI-Version. Dieses Flag ist optional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>Der `wsk`-Client (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Ein IAM-Namensbereich. Dieses Flag kann für Cloud Foundry-Namensbereiche nicht festgelegt werden. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn property get --auth
  ```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

Sie können eine Eigenschaft festlegen. Mindestens ein Flag ist erforderlich. Wenn eine Eigenschaft festgelegt ist, wird sie auf Ihrer Workstation unter `<home_dir>/.bluemix/plugins/cloud-functions/config.json` gespeichert. Zum Entfernen einer Eigenschaft führen Sie den Befehl [`ibmcloud fn property unset --<property>`](#cli_prop_set) aus.  

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>Der `wsk`-API-Host. Dieses Flag ist optional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>Die `wsk`-API-Version. Dieses Flag ist optional.</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Das `wsk`-Clientzertifikat. Dieses Flag ist optional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>Der `wsk`-Client (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Ein IAM-Namensbereich. Dieses Flag kann für Cloud Foundry-Namensbereiche nicht festgelegt werden. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

Sie können die Festlegung einer Eigenschaft für die `wsk`-CLI wieder aufheben. Mindestens ein Flag ist erforderlich.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>Der `wsk`-API-Host. Dieses Flag ist optional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>Die `wsk`-API-Version. Dieses Flag ist optional.</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>Der `wsk`-Berechtigungsschlüssel (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Das `wsk`-Clientzertifikat. Dieses Flag ist optional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>Der `wsk`-Client (`KEY`). Dieses Flag ist optional.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Ein IAM-Namensbereich. Dieses Flag kann für Cloud Foundry-Namensbereiche nicht festgelegt werden. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## 'rule'-Befehle
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

Sie können eine Regel erstellen, um einem Auslöser eine Aktion zuzuordnen. Bevor Sie eine Regel erstellen können, müssen Sie zuerst einen Auslöser und eine Aktion erstellen.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **Ausgabe**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

Entfernen Sie die Regeln, die Sie nicht mehr benötigen, um Ihren Namensbereich zu bereinigen.

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Der Name einer Regel. Dieser Wert ist erforderlich.</dd>

  <dt>`--disable`</dt>
  <dd>Inaktiviert die Regel, bevor sie gelöscht wird. </dd>
  </dl>


<br />**Beispiel**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

Sie können den Status einer Regel in "Inaktiv" ändern und verhindern, dass sie eine Aktion ausführt, wenn ein Auslöser gestartet wird.

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

Sie können den Status einer Regel von "inaktiv" in "aktiv" ändern. Im Status "aktiv" wird eine Aktion ausgeführt, wenn ein Auslöser gestartet wird.

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

Sie können Metadaten abrufen, die eine bestimmte Regel beschreiben.

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Der Name einer Regel. Dieser Wert ist erforderlich.</dd>

  <dt>`--summary`</dt>
  <dd>Ruft eine Zusammenfassung der Regeldetails ab.</dd>
  </dl>

<br />**Beispiel**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `ibmcloud fn rule list`
{: #cli_rule_list}

Sie können alle Regeln, die Sie erstellt haben, oder eine bestimmte Anzahl von Regeln auflisten.

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Der Name einer Regel. Dieser Wert ist optional. Wenn keine Angabe gemacht wird, werden alle Regeln aufgelistet.</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>Listet eine angegebene Anzahl von Regeln auf. Standardmäßig werden 30 Regeln aufgelistet.</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen Regeln nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert.</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>Schließt eine angegebene Anzahl der zuletzt erstellten Regeln aus dem Ergebnis aus.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

Sie können prüfen, ob eine Regel aktiv oder inaktiv ist. Führen Sie den Befehl `ibmcloud fn rule disable` oder `ibmcloud fn run enable` aus, um den Status zu ändern.

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

Um zu ändern, welche Auslöser und Regeln einander zugeordnet sind, können Sie eine Regel aktualisieren.

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Beispiel**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## 'sdk'-Befehl
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

Sie können ein SDK installieren.

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>Die SDK-Komponente, wie z. B. `docker`, `iOS` und `bashauto`. Dieser Wert ist erforderlich.</dd>

   <dt>`--stdout, --s`</dt>
   <dd>Gibt die Ergebnisse des Bash-Befehls an `STDOUT` aus. Dieses Flag ist optional.</dd>


   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## 'service'-Befehle
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

Sie können Serviceberechtigungsnachweise an eine Aktion oder ein Paket binden.

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>Der Name des Service.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>Der Name des Pakets oder der Aktion, an das/die die Berechtigungsnachweise gebunden werden sollen.</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>Der Name der Serviceinstanz.</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>Der Name der zu bindenden Serviceberechtigungsnachweise (`KEY`).</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

Sie können die Bindung von Serviceberechtigungsnachweisen an eine Aktion oder ein Paket aufheben.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>Der Name des Service.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>Der Name des Pakets oder der Aktion, für das/die die Bindung der Berechtigungsnachweise aufgehoben werden soll. </dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## 'trigger'-Befehle
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

Sie können einen Auslöser erstellen.

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Der Name des Auslösers. Dieser Wert ist erforderlich. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>Legt den Typ des Auslösers als Feed fest. Dieses Flag ist optional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>


   </dl>

<br />**Beispiel**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

Sie können einen Auslöser löschen.

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**Beispiel**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

Sie können einen Auslöser testen, indem Sie ihn aktivieren, anstatt darauf zu warten, dass er automatisch ausgelöst wird.

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>Der Name des Auslösers. Dieser Wert ist erforderlich. </dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameter `VALUES` im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

Sie können Metadaten abrufen, die einen bestimmten Auslöser beschreiben.

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Der Name eines Auslösers. Dieser Wert ist erforderlich.</dd>

  <dt>`--summary`</dt>
  <dd>Ruft eine Zusammenfassung der Auslöserdetails ab.</dd>
  </dl>

<br />**Beispiel**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `ibmcloud fn trigger list`
{: #cli_trigger_list}

Sie können alle Auslöser, die Sie erstellt haben, oder eine bestimmte Anzahl von Auslösern auflisten.

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Der Name eines Auslösers. Dieser Wert ist optional. Wenn keine Angabe gemacht wird, werden alle Auslöser aufgelistet.</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Listet eine angegebene Anzahl von Auslösern auf. Standardmäßig werden 30 Auslöser aufgelistet.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Sortiert die Liste der zurückgegebenen Auslöser nach Namen. Andernfalls wird die Liste nach Erstellungsdatum sortiert.</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Schließt eine angegebene Anzahl der zuletzt erstellten Auslöser aus dem Ergebnis aus.</dd>

   </dl>

<br />**Beispiel**

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

Sie können einen Auslöser aktualisieren.

Wenn Sie Parameter für ein Paket, eine Aktion oder einen Auslöser aktualisieren, müssen Sie alle zuvor erstellten Parameter angeben. Andernfalls werden die zuvor erstellten Parameter entfernt. Bei Paketen werden alle Services, die an das Paket gebunden wurden, ebenfalls entfernt. Daher müssen Sie die [Services erneut an Ihr Paket binden](/docs/openwhisk?topic=cloud-functions-services), nachdem Sie andere Parameter aktualisiert haben.
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Befehlsoptionen**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Der Name des Auslösers. Dieser Wert ist erforderlich. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Annotationen werden im Format `KEY` `VALUE` angegeben. Wenn Sie mehr als eine Annotation einschließen möchten, geben Sie diese Option für jede Annotation an. Dieses Flag ist optional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Eine JSON-Datei, die Annotationen im Format `KEY` `VALUE` enthält. Dieses Flag ist optional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parameterwerte im Format `KEY` `VALUE`. Dieses Flag ist optional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Eine JSON-Datei, die die Parameter `KEYS` und `VALUES` enthält. Dieses Flag ist optional.</dd>
   </dl>

<br />**Beispiel**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






