---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# Serverunabhängige Apps testen
{: #test}

Testen Sie jede Entität, die Sie über die CLI erstellen, um zu prüfen, ob Ihre serverunabhängige App funktioniert, oder um eventuelle Fehler zu beheben.
{: shortdesc}


## Aktionen testen
{: #test-js}

Sie können Aktionen testen, indem Sie den Befehl `invoke` ausführen. Sie können die Aktion mit oder ohne Parameter testen.
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

**Hello World-Beispiel**
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

**Ausgabe**
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### In JSON-Dateien gespeicherte Parameter testen
{: #test_json_file}

Sie können eine Datei mit JSON-formatierten Parametern übergeben.
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

**Beispielausgabe**
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### Im JSON-Format eingegebene Parameter testen
{: #test_json}

Sie können JSON-formatierten Parameter mit Ihrem Aufruf übergeben.
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

**Beispielausgabe**
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
```
{: screen}


### Blockierende Aktionen testen
{: #test-block}

Der Aufruf einer Aktion kann blockierend oder nicht blockierend sein. Die Aufrufe sind standardmäßig nicht blockierend. Wenn Sie das Aktionsergebnis nicht sofort benötigen, verwenden Sie einen nicht blockierenden Aufruf.
{: shortdesc}

Blockierende Aufrufe verwenden einen Anforderung/Antwort-Typ und warten, bis das Aktivierungsergebnis verfügbar ist. Der Wartezeitraum beträgt weniger als 60 Sekunden oder das [Zeitlimit](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) der Aktion, je nachdem, welcher Wert kürzer ist.

Führen Sie die Aktion durch Ausführen eines blockierenden Aufrufs aus.

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


**Beispielausgabe**
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
      "status": "success",
      "success": true
  }
```
{: screen}

Der Befehl gibt die folgenden Informationen aus. 
* Das Aufrufergebnis, wenn es innerhalb des erwarteten Wartezeitraums verfügbar ist
* Ohne die Option `--result` wird die Aktivierungs-ID im Ergebnis angezeigt. Die Aktivierungs-ID (`44794bd6aab74415b4e42a308d880e5b`), mit der die Protokolle oder das Ergebnis der Aktivierung abgerufen werden kann.


## Auslöser testen
{: #test_triggers}

Auslöser können mithilfe eines Wörterverzeichnisses mit Schlüssel/Wert-Paaren aktiviert (also angewendet oder ausgelöst) werden. Manchmal wird dieses Wörterverzeichnis als das Ereignis bezeichnet. Auslöser können explizit durch einen Benutzer oder für einen Benutzer durch eine externe Ereignisquelle aktiviert werden. Wie bei Aktionen ergibt sich bei jedem Aktivieren eines Auslösers, der einer Regel zugeordnet ist, eine Aktivierungs-ID.
{: shortdesc}

1. Aktivieren Sie den Auslöser.

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    Ein Auslöser, der keiner Regel zugeordnet ist, hat bei seiner Aktivierung keine sichtbare Wirkung zur Folge. Da diesem Auslöser keine Regel zugeordnet ist, werden die übergebenen Parameter bei keiner Aktion als Eingabe verwendet. 

    **Beispielausgabe**

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. Stellen Sie fest, ob die Aktion aufgerufen wurde, indem Sie das Protokoll der letzten Aktivierung prüfen.
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    **Beispielausgabe**
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. Rufen Sie weitere Informationen zu der Aktivierungs-ID aus der Ausgabe des vorherigen Befehls ab.
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    **Beispielausgabe**
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## Dauer von Aktivierungen testen
{: #test_time}

Prüfen Sie, wie lange die Ausführung einer Aktivierung bis zu ihrem Abschluss gedauert hat. Rufen Sie dazu das Aktivierungsprotokoll ab. Wenn die Dauer zu lang ist oder Sie das Standardzeitlimit anpassen müssen, damit die Funktion länger ausgeführt werden kann, können Sie Ihre Aktion mit einer Zeitlimitwert aktualisieren.
{: shortdesc}

1. Rufen Sie die Aktivierungs-ID ab.

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    Beispielausgabe:
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. Rufen Sie das Protokoll für die Aktivierungs-ID ab.

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    Die Angabe bei `duration` zeigt die Zeit in Millisekunden an. Für die Ausführung der Aktivierung wurden etwas mehr als 2 Sekunden benötigt. 

    ```
    ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
        ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
        ...
    }
    ```
    {: screen}

3. Aktualisieren Sie die Aktion mit einem Zeitlimit in Millisekunden.

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    Beispiel:
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## Speicherbelegung testen
{: #test_memory}

Wenn Ihre App in einem Docker-Image gepackt ist, können Sie die Speichernutzung Ihrer App mit Docker-Befehlen überprüfen.
{: shortdesc}

1. Erstellen Sie lokal einen Container, in dem Ihr Docker-Image ausgeführt wird.

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. Rufen Sie eine Liste der Container ab, um eine Container-ID zu erhalten. 

    ```
    docker ps
    ```
    {: pre}

3. Prüfen Sie die Statistik des ausgeführten Containers.

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. Überprüfen Sie den Speicherbelegungswert für den Container. Wenn der Wert die Systemgrenzwerte überschreitet, passen Sie Ihr Script an. 

5. Nachdem Sie die Informationen geprüft haben, können Sie den aktiven Container stoppen.

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. Entfernen Sie den Container.

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}








