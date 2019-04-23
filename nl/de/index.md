---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: getting started, creating actions, invoking actions, 

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Einführung
{: #index}

Mit {{site.data.keyword.openwhisk}} können Sie Ihre bevorzugte Programmiersprache zum Schreiben von einfachem Code verwenden, der App-Logik in skalierbarer Weise ausführt. Sie können Code bei Bedarf mit HTTP-basierten API-Anforderungen von Anwendungen oder automatisch in Antwort auf {{site.data.keyword.Bluemix_notm}}-Services und Drittanbieterereignisse ausführen. Die FaaS-Programmierplattform (Function as a Service) basiert auf dem Open-Source-Projekt Apache OpenWhisk.
{: shortdesc}

Da {{site.data.keyword.openwhisk_short}} serverunabhängig ist, sind Sie im Hinblick auf die Sprachen, die Sie verwenden können, nicht eingeschränkt und müssen nicht explizit Zeit für das Bereitstellen einer Back-End-Infrastruktur aufbringen. Sie können sich auf das Schreiben von App-Logik konzentrieren, anstatt sich Gedanken über die automatische Skalierung, Hochverfügbarkeit, Aktualisierungen oder Wartung zu machen. IBM stellt ohne Vorbereitungs- oder Anpassungsaufwand die Hardware, das Netzwerk, die Softwareverwaltung, den Lastausgleich, Plug-ins usw. bereit. Sie müssen lediglich den Code beisteuern. 

## Mit Aktionen arbeiten
{: #creating_actions}
{: #openwhisk_actions}

Mit {{site.data.keyword.openwhisk}} können Sie statusunabhängige Code-Snippets erstellen, die eine bestimmte Task, auch als Aktion bezeichnet, ausführen sollen.
{:shortdesc}

**Was ist eine Aktion?**
{: #what_is_an_action}

Eine Aktion ist ein kleiner Teil des Codes, der aufgerufen werden kann oder in Antwort auf ein Ereignis ausgeführt wird. So oder so resultiert jede Ausführung in einem Datensatz, der durch eine eindeutige Aktivierungs-ID angegeben wird. Die Eingabe und das Ergebnis einer Aktion können als Schlüssel/Wert-Paar angesehen werden. Der Schlüssel ist eine Zeichenfolge und der Wert ein gültiger JSON-Wert. Eine Aktion kann in der Sprache Ihrer Wahl geschrieben sein und wird dem Service entweder als Quellcode oder als Docker-Image bereitgestellt. Der Aktionscode wird ausgeführt, wenn er direkt von der Cloud Functions-API, der CLI oder dem iOS-SDK aufgerufen wird. Eine Aktion kann automatisch auf Ereignisse von IBM Cloud oder Services anderer Anbieter antworten. 

**Wann verwende ich eine Aktion?**
{: #why_use_an_action}

Durch die Verwendung von Aktionen begrenzen Sie die Zeitspanne, die Ihr Code ausgeführt wird, wodurch sich die Aufwandskosten verringern. 

Aktionen können zum Beispiel verwendet werden, um Gesichter in einem Bild zu erkennen, auf Änderungen in einer Datenbank zu reagieren, eine Gruppe von API-Aufrufen zu aggregieren oder auch um einen Tweet zu posten. 

**Kann ich mehrere Aktionen gleichzeitig ausführen?**
{: #more_than_one_action}

Ja. Sie können Aktionen verwenden, um andere Aktionen aufzurufen, oder Sie können Aktionen in Sequenzen zusammenführen. Damit dies funktioniert, müsste die Ausgabe einer Aktion die Eingabe der nächsten Aktion sein, die wiederum eine Ausgabe erzeugt, mit der eine weitere Aktion ausgelöst werden kann usw. Sie können die Gruppe von erstellten Aktionen sogar bündeln, um ein Paket zu formen. Mit einem Paket können Sie gängige Aktionen oder Sequenzen wiederverwenden, indem Sie das Paket aufrufen, statt die Aktion oder die Sequenz erneut zu konfigurieren. 

Klicken Sie als ersten Schritt auf eine Option:

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Auf ein Symbol klicken, um {{site.data.keyword.openwhisk_short}} schnell zu starten." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="/docs/openwhisk?topic=cloud-functions-index#openwhisk_start_hello_world" alt="Aktion erstellen" title="Aktion erstellen" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli" alt="Die {{site.data.keyword.openwhisk_short}}-CLI-Plug-in einrichten" title="Die {{site.data.keyword.openwhisk_short}}-CLI-Plug-in einrichten" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-openwhisk_about" alt="Plattformarchitektur anzeigen" title="Plattformarchitektur anzeigen" shape="rect" coords="326, -10, 448, 218" />
</map>

## Aktion in der Benutzerschnittstelle erstellen
{: #openwhisk_start_hello_world}

Für den Einstieg in {{site.data.keyword.openwhisk_short}} können Sie die HelloWorld-Quickstart-Vorlage verwenden.

1.  Öffnen Sie das Functions-Dashboard im [{{site.data.keyword.Bluemix_notm}}-**Katalog** ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk). 

2. Klicken Sie auf **Erstellung starten** > **Quickstart-Vorlagen** und wählen Sie die Vorlage **Hello World** aus. 

3. Erstellen Sie ein Paket für Ihre Aktionen, indem Sie einen eindeutigen Namen im Feld **Paketname** eingeben. 

4. Wählen Sie eine Laufzeit im Dropdown-Menü im Abschnitt **Aktion 'helloworld'** aus. Sie können den Code für die Beispielaktion in allen verfügbaren Laufzeiten voranzeigen bevor Sie die Vorlage bereitstellen. 

5. Klicken Sie auf **Bereitstellen**. Sie haben eine Aktion erstellt. Gratulation! 

6. Führen Sie die Aktion aus, indem Sie auf **Aufrufen** klicken. Wenn Sie eine Aktion aufrufen, wird die App-Logik, die die Aktion definiert, manuell ausgeführt. In der Anzeige **Aktivierungen** sehen den Text "Hello stranger!", der von der Aktion generiert wurde.

7. Optional: Klicken Sie auf **Eingabe ändern**, um die Aktion zu ändern. Klicken Sie anschließend auf **Aufrufen**, um die Aktion mit Ihren Aktualisierungen auszuführen. Das Ergebnis wird im Fenster **Aktivierungen** angezeigt. Sie können diesen Prozess beliebig oft wiederholen. 

Gute Arbeit! Sie haben Ihre erste Aktion erstellt. Um diese Aktion wieder zu entfernen, klicken Sie auf das Überlaufmenü und wählen Sie **Aktion löschen** aus.

## Aktion in der Befehlszeilenschnittstelle erstellen
{: #openwhisk_start_hello_world_cli}

Mit dem HelloWorld-JavaScript-Beispielcode können Sie Ihre Arbeit schon nach kürzester Zeit aufnehmen. In diesem Beispiel wird eine einfache Aktion `hello` erstellt, die Sie manuell aufrufen können, um die zugehörige App-Logik auszuführen.

1. [Richten Sie das {{site.data.keyword.openwhisk_short}}-CLI-Plug-in](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) ein.

2. Speichern Sie den folgenden Code in einer Datei mit dem Namen **hello.js**.

    ```javascript
    /**
     * Hello world as an OpenWhisk action.
     */
function main(params) {
        var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
    }
    ```
    {: codeblock}

3. Erstellen Sie die Aktion `hello`.

    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

4. Rufen Sie die Aktion ohne Übergabe von Parametern auf.

    ```
    ibmcloud fn action invoke hello --blocking --result
    ```
    {: pre}  

    Ausgabe:
    ```
    {
        "payload": "Hello, World!"
    }
    ```
    {: screen}

5. Rufen Sie die Aktion nochmals auf und übergeben Sie den Namensparameter, um die App-Logik zu testen.

    ```
    ibmcloud fn action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Ausgabe:
    ```
    {
        "payload": "Hello, Fred!"
    }
    ```
    {: screen}

Gute Arbeit! Sie haben Ihre erste Aktion erstellt. Um diese Aktion wieder zu entfernen, führen Sie den Befehl `ibmcloud fn action delete hello` aus.

## Weitere Schritte
{: #next-steps}

Sie haben Ihre erste Vorlage bereitgestellt. Wie geht es jetzt weiter? Sie haben folgende Möglichkeiten: 

* Machen Sie sich vertraut mit der [Terminologie](/docs/openwhisk?topic=cloud-functions-openwhisk_about#technology). 
* Machen Sie Ihre ersten Schritte mit [Ihren eigenen Aktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_actions). 
* Erfahren Sie mehr über das Organisieren von Aktionen in [Paketen](/docs/openwhisk?topic=cloud-functions-openwhisk_packages). 
* Option für Fortgeschrittene: Erstellen Sie eine [serverunabhängige REST-API](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway). 
