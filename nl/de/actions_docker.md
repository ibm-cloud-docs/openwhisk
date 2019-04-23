---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: docker, actions, serverless

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

# Docker-Aktionen erstellen
{: #creating-docker-actions}


Bei {{site.data.keyword.openwhisk_short}}-Docker-Aktionen können Sie Ihre Aktionen in einer beliebigen Sprache schreiben.
{: shortdesc}

Ihr Code wird in eine ausführbare Binärdatei kompiliert und in ein Docker-Image eingebettet. Das Binärprogramm interagiert mit dem System durch den Empfang von Eingaben über `stdin` und Ausgabe von Antworten über `stdout`.   Weitere Informationen zur Erstellung von Docker-Aktionen finden Sie im Abschnitt mit den [Laufzeiten](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker). 

Mithilfe von Aktionen können Sie Bilder nur aus öffentlichen Registrys abrufen, z. B. Bilder, die auf Docker Hub öffentlich verfügbar sind. Private Registrys werden nicht unterstützt.
{: tip}


## Docker-Aktion erstellen und aufrufen

**Vorbereitende Schritte:**

Sie müssen über ein Docker Hub-Konto verfügen. Sie können eine kostenlose Docker-ID mit entsprechendem Konto bei [Docker Hub ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://hub.docker.com) einrichten. 

**So richten Sie eine Docker-Aktion ein:**

1. Laden Sie das Docker-Gerüst (Skeleton) herunter und installieren Sie es. Das Gerüst ist eine Docker-Containervorlage, in die Sie Ihren Code in Form von angepassten Binärdateien einfügen können.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Richten Sie ihre angepasste Binärdatei in dem Blackbox-Gerüst ein. Das Gerüst schließt ein C-Programm ein, das Sie verwenden können. Ein Teil der Datei `example.c` wird im Rahmen des Buildprozesses für das Docker-Image kompiliert, sodass Sie keine C-Kompilierung auf Ihrer Maschine benötigen.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Beispielausgabe:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Optional: Fügen Sie anhand entsprechender Änderungen an der `Dockerfile` zusätzlichen Code und Abhängigkeiten zum Docker-Image hinzu, um die ausführbare Datei zu erstellen. Beachten Sie die folgenden Voraussetzungen und Einschränkungen:
  * Die Binärdatei muss sich im Container unter `/action/exec` befinden.
  * Die Binärdatei empfängt ein einzelnes Argument über die Befehlszeile. Bei diesem Argument handelt es sich um eine Zeichenfolgeserialisierung des JSON-Objekts, das die Argumente für die Aktion darstellt.
  * Das Programm kann die Protokolle in `stdout` oder `stderr` ausgeben.
  * Die letzte Zeile der Ausgabe muss der Konvention entsprechend ein in eine Zeichenfolge konvertiertes JSON-Objekt sein, das das Aktionsergebnis darstellt.

4. Erstellen Sie das Docker-Image und laden Sie es mithilfe eines bereitgestellten Scripts hoch.
    1. Melden Sie sich bei Docker an.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Navigieren Sie zum Verzeichnis `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Führen Sie das Script aus.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Verwenden Sie Ihren Docker-Container zum Erstellen einer Aktion.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Rufen Sie die Aktion auf.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Laden Sie zum Aktualisieren der Docker-Aktion das neueste Image auf Docker Hub hoch. Dies ermöglicht dem System das Extrahieren Ihres neuen Docker-Images bei der nächsten Ausführung des Codes für Ihre Aktion.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. Wenn ein aktiver Container vorhanden ist, der eine ältere Version Ihres Docker-Images nutzt, verwenden alle neuen Aufrufe weiterhin dieses Image. Aktualisieren Sie die Aktion, sodass neue Aufrufe künftig das neue Image verwenden.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Optional: Sie können das Argument `--native` als Kurzform für `--docker openwhisk/dockerskeleton` verwenden. Dieses Argument vereinfacht die Erstellung und Implementierung einer ausführbaren Datei, die innerhalb des standardmäßigen SDKs für Docker-Aktionen ausgeführt wird. 
    1. In den Schritten oben wird eine ausführbare Binärdatei innerhalb des Containers im Verzeichnis `/action/exec` erstellt. Kopieren Sie die Datei `/action/exec` in Ihr lokales Dateisystem und komprimieren Sie sie in die Datei `exec.zip`. 
    2. Erstellen Sie eine Docker-Aktion, die die ausführbare Datei als Initialisierungsdaten empfängt. Hierbei ersetzt das Argument `--native` das Argument `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
