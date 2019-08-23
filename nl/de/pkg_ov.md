---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

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


# Pakete einbinden
{: #pkg_ov}

Pakete sind gebündelte Gruppen zusammengehöriger Aktionen und Feeds. Jedes Paket ist für eine ganz bestimmte Interaktion mit Services und Ereignisprovidern konzipiert. Einige Pakete sind bereits mit {{site.data.keyword.openwhisk}} für Sie installiert, aber Sie können auch andere installieren.
{: shortdesc}

## Übersicht
{: #pkg_overview}

[Vorinstallierte Pakete](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse) werden in {{site.data.keyword.openwhisk_short}} automatisch im Namensbereich `/whisk.system` registriert. Sie können sie verwenden, ohne sie installieren zu müssen.

Installierbare Pakete sind Pakete, die Ihnen zum Installieren und Bearbeiten und zur wunschgemäßen Verwendung zur Verfügung stehen. Installierbare Pakete befinden sich nicht innerhalb des {{site.data.keyword.openwhisk_short}}-Systems. Stattdessen sind installierbare Pakete extern in einzelnen GitHub-Repositorys enthalten. 

Diese sowie eigene Pakete können Sie direkt in Ihrem Namensbereich installieren und ihnen einen angepassten Namen geben. Da das Paket in Ihrem eigenen Namensbereich installiert ist, können Sie die darin enthaltenen Aktionen und Feeds nach Bedarf ändern.



## Vorinstallierte Pakete durchsuchen
{: #pkg_browse}

In {{site.data.keyword.openwhisk_short}} sind bereits verschiedene Pakete für Sie registriert. Sie können eine Liste der Pakete in einem Namensbereich abrufen, die Entitäten in einem Paket auflisten und eine Beschreibung der einzelnen Entitäten in einem Paket abrufen.
{: shortdesc}

1. Rufen Sie eine Liste der Pakete im Namensbereich `/whisk.system` ab.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Paketlistenausgabe:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. Rufen Sie eine Liste der Entitäten in einem Paket ab.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Beispiel**
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **Beispielausgabe**
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Diese Ausgabe zeigt, dass das {{site.data.keyword.cloudant_short_notm}}-Paket Aktionen und einen Feed enthält. Zum Beispiel die beiden Aktionen `read` und `write` sowie einen Auslöserfeed mit dem Namen `changes`. Der Feed `changes` bewirkt, dass Auslöser aktiviert werden, wenn Dokumente zu der angegebenen {{site.data.keyword.cloudant_short_notm}}-Datenbank hinzugefügt werden.

  Das {{site.data.keyword.cloudant_short_notm}}-Paket definiert außerdem die Parameter `username`, `password`, `host` und `port`. Diese Parameter müssen für die Aktionen und Feeds angegeben werden, damit diese eine Bedeutung haben. Durch die Parameter können Aktionen zum Beispiel auf einem bestimmten {{site.data.keyword.cloudant_short_notm}}-Konto operieren.

3. Rufen Sie eine Beschreibung für eine Aktion oder einen Feed ab, um die erforderlichen Parameter anzuzeigen.

  **Beispiel**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **Beispielausgabe**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  Diese Ausgabe zeigt, dass die {{site.data.keyword.cloudant_short_notm}}-Aktion `read` drei Parameter erfordert, zu denen die Datenbank und die abzurufende Dokument-ID gehören.



## Parameter an vorinstallierte Pakete binden
{: #pkg_bind}

Sie können die Entitäten in einem Paket immer direkt verwenden, was jedoch bedeuten kann, dass Sie jedes Mal dieselben Parameter an die Aktion übergeben. Sie können den Prozess vereinfachen, indem Sie eine Bindung an ein Paket erstellen und Standardparameter angeben, die von den Aktionen in diesem Paket übernommen werden.
{: shortdesc}

Beispiel: Im Paket `/whisk.system/cloudant` können Sie Standwerte für die Parameter `username`, `password` und `dbname` in einer Paketbindung festlegen, sodass diese Werte automatisch an alle Aktionen in dem Paket übergeben werden.

In dem folgenden Beispiel erstellen Sie eine Bindung an das Paket `/whisk.system/samples`.

1. Erstellen Sie eine Bindung an das Paket `/whisk.system/samples` und legen Sie einen Standardwert für den Parameter `place` fest.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **Beispielausgabe**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. Rufen Sie eine Beschreibung der Paketbindung ab.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  **Beispielausgabe**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  Beachten Sie, dass alle Aktionen im Paket `/whisk.system/samples` in der Paketbindung `valhallaSamples` verfügbar sind.

3. Rufen Sie eine Aktion in der Paketbindung auf.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **Beispielausgabe**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Beachten Sie im Ergebnis, dass die Aktion den Parameter `place` übernimmt, den Sie beim Erstellen der Paketbindung `valhallaSamples` festgelegt haben.

4. Rufen Sie eine Aktion auf und überschreiben Sie den Standardparameterwert.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **Beispielausgabe**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Beachten Sie, dass der Wert des Parameters `place`, der im Aktionsaufruf angegeben wird, den Standardwert überschreibt, der in der Paketbindung `valhallaSamples` festgelegt wurde.



## Eigene Pakete hinzufügen
{: #pkg_add}

Sie können ein Paket mit lokalem Code oder einem Klon eines beliebigen GitHub-Repositorys erstellen.
{: shortdesc}

**Vorbereitende Schritte**
- [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.cloud_notm}}-CLI](/docs/openwhisk?topic=cloud-functions-cli_install). 
- Erstellen Sie eine Datei `manifest.yaml` oder `manifest.yml` für Ihre App und speichern Sie sie im Stammverzeichnis. Die Datei `manifest.yaml` gibt die Gesamtstruktur des Pakets an, einschließlich aller Metadaten, die mit dem Befehl `ibmcloud fn deploy` eingebunden werden müssen. Weitere Informationen zu `manifest.yaml`-Dateien finden Sie in der [Dokumentation zu <ph class="ignoreSpelling">wskdeploy</ph>](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}. 

So fügen Sie ein Paket hinzu:

1. Klonen Sie das Repository für das Paket
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Navigieren Sie zu dem Verzeichnis, das die Datei `manifest.yaml` enthält.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Stellen Sie das Paket bereit.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Manche Pakete erfordern bestimmte Umgebungsvariablen, damit das Paket ordnungsgemäß funktionieren kann. Ist dies der Fall, schließen Sie die Umgebungsvariablen mit dem Befehl `deploy` ein. Sie können z. B. einen Namen für das Paket auswählen und ihn mit der Variablen PACKAGE_NAME angeben.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Beispiel für {{site.data.keyword.cos_full_notm}}-Paket
{: #pkg_ex}

Wenn Sie ein Beispiel für die Installation eines Pakets sehen möchten, prüfen Sie das [{{site.data.keyword.cos_full_notm}}-Paket](/docs/openwhisk?topic=cloud-functions-pkg_obstorage). {{site.data.keyword.cos_full}} ist ein Service, der Benutzern das Speichern jeglicher Arten von Dateien wie Bildern, Videos, Musik und Text ermöglicht. Für die Interaktion mit den Dateien wird in einem Bucket ein Cloud-basierter Datenspeicher von Schlüssel/Wert-Paaren gespeichert. Damit Sie das [{{site.data.keyword.cos_full_notm}}-Paket](/docs/openwhisk?topic=cloud-functions-pkg_obstorage) verwenden können, müssen Sie zuerst eine {{site.data.keyword.cos_full_notm}}-Serviceinstanz und anschließend ein Bucket erstellen. Das Bucket wird als Umgebungsvariable verwendet, die zum Installieren dieses Pakets erforderlich ist.

Nachdem Sie die Serviceinstanz und das Bucket erstellt haben, können Sie das Paket mithilfe der folgenden Befehle installieren: 

1. Klonen Sie das Repository für das Paket
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigieren Sie zu dem Paketverzeichnis, das die Datei `manifest.yaml` enthält. In diesem Beispiel wird die Node.js-Laufzeitversion des {{site.data.keyword.cos_full_notm}}-Pakets verwendet.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Implementieren Sie das Paket. Verwenden Sie dazu Ihr Bucket als Umgebungsvariable.  Sie können dem Paket einen angepassten Namen geben, indem Sie die Umgebungsvariable `PACKAGE_NAME` verwenden.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



