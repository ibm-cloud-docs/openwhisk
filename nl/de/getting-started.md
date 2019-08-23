---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions, ibm cloud functions, cloud functions, getting started, creating actions

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


# Erste Schritte mit IBM {{site.data.keyword.openwhisk_short}}
{: #getting-started}

Mit {{site.data.keyword.openwhisk}} können Sie Ihre bevorzugte Programmiersprache zum Schreiben von einfachem Code verwenden, der App-Logik in skalierbarer Weise ausführt. Sie können Code bei Bedarf mit HTTP-basierten API-Anforderungen von Anwendungen oder in Antwort auf {{site.data.keyword.cloud_notm}}-Services und Drittanbieterereignisse ausführen. Die FaaS-Programmierplattform (Function as a Service) basiert auf dem Open-Source-Projekt Apache OpenWhisk.
{: shortdesc}

## Mit Aktionen arbeiten
{: #gs_actions}

Mit {{site.data.keyword.openwhisk}} können Sie statusunabhängige Code-Snippets erstellen, die eine bestimmte Task, auch als Aktion bezeichnet, ausführen sollen. Weitere Informationen zu Aktionen und anderen Funktionen finden Sie unter [Terminologie](/docs/openwhisk?topic=cloud-functions-about).
{:shortdesc}

Klicken Sie auf eine Option, um zu beginnen. 

<img usemap="#home_map" border="0" class="image" id="image_ztx_crb_f1b" src="images/imagemap.png" width="440" alt="Auf ein Symbol klicken, um {{site.data.keyword.openwhisk_short}} schnell zu starten." style="width:440px;" />
<map name="home_map" id="home_map">
<area href="#gs_hello_world" alt="Aktion erstellen" title="Aktion erstellen" shape="rect" coords="-7, -8, 108, 211" />
<area href="/docs/openwhisk?topic=cloud-functions-cli_install" alt="Die {{site.data.keyword.openwhisk_short}}-CLI-Plug-in einrichten" title="Die {{site.data.keyword.openwhisk_short}}-CLI-Plug-in einrichten" shape="rect" coords="155, -1, 289, 210" />
<area href="/docs/openwhisk?topic=cloud-functions-about" alt="Plattformarchitektur anzeigen" title="Plattformarchitektur anzeigen" shape="rect" coords="326, -10, 448, 218" />
</map>

## Aktion in der Benutzerschnittstelle erstellen
{: #gs_hello_world}

Versuchen Sie, für den Einstieg in {{site.data.keyword.openwhisk_short}} die Quickstart-Vorlage 'Hello World' zu erstellen. 

1. Erstellen Sie ein [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/registration){: external}-Konto oder melden Sie sich an einem vorhandenen Konto an.

2. Navigieren Sie zum [{{site.data.keyword.openwhisk_short}}-Dashboard](https://cloud.ibm.com/openwhisk){: external}. 

2. Klicken Sie auf **Erstellung starten** > **Quickstart-Vorlagen** und wählen Sie die Vorlage **Hello World** aus.

3. Erstellen Sie ein Paket für Ihre Aktionen, indem Sie einen eindeutigen Namen im Feld **Paketname** eingeben.

4. Wählen Sie eine Laufzeit im Dropdown-Menü im Abschnitt **Aktion 'helloworld'** aus. Sie können den Code für die Beispielaktion in allen verfügbaren Laufzeiten voranzeigen, bevor Sie die Vorlage bereitstellen.

5. Klicken Sie auf **Bereitstellen**. Sie haben eine Aktion erstellt. Gratulation!

6. Führen Sie die Aktion aus, indem Sie auf **Aufrufen** klicken. Wenn Sie eine Aktion aufrufen, wird die App-Logik, die die Aktion definiert, manuell ausgeführt. In der Anzeige **Aktivierungen** sehen den Text "Hello stranger!", der von der Aktion generiert wurde.

7. Optional: Klicken Sie auf **Eingabe ändern**, um die Aktion zu ändern.

  a. Fügen Sie den folgenden Code in das Feld **Eingabe ändern** ein und ändern Sie den Namenswert.
  ```
  { "name": "xxxx" }
  ```
  {: codeblock}
  b. Klicken Sie anschließend auf **Aufrufen**, um die Aktion mit Ihren Aktualisierungen auszuführen. Das Ergebnis wird im Fenster **Aktivierungen** angezeigt. Sie können diesen Prozess beliebig oft wiederholen.

Gute Arbeit! Sie haben Ihre erste Aktion erstellt. Um diese Aktion wieder zu entfernen, klicken Sie auf das Überlaufmenü und wählen Sie **Aktion löschen** aus.

## Aktion in der Befehlszeilenschnittstelle erstellen
{: #gs_hello_world_cli}

Mit dem [JavaScript-Beispielcode 'Hello World'](/docs/openwhisk?topic=cloud-functions-prep#prep-js) können Sie sich schnell einarbeiten. In diesem Beispiel wird eine einfache Aktion `hello` erstellt, die Sie manuell aufrufen können, um die zugehörige App-Logik auszuführen.

## Weitere Schritte
{: #gs_next_steps}

Sie haben Ihre erste Vorlage bereitgestellt. Wie geht es jetzt weiter? 

* Machen Sie sich vertraut mit der [Terminologie](/docs/openwhisk?topic=cloud-functions-about#about_technology).
* Machen Sie Ihre ersten Schritte mit [Ihren eigenen Aktionen](/docs/openwhisk?topic=cloud-functions-actions).
* Erfahren Sie mehr über das Organisieren von Aktionen in [Paketen](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Erweiterte Option - Erstellen Sie eine [serverunabhängige REST-API](/docs/openwhisk?topic=cloud-functions-apigateway). 



