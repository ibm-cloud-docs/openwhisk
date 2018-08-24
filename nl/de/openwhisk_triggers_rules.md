---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Mit Auslösern und Regeln auf Ereignisse antworten
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk}}-Auslöser und -Regeln statten die Plattform mit ereignisgesteuerten Funktionen aus. Ereignisse aus externen und internen Ereignisquellen werden durch einen Auslöser kanalisiert. Durch Regeln können Ihre Aktionen auf diese Ereignisse reagieren.
{: shortdesc}

## Allgemeine Konzepte
{: #definitions}

### Auslöser
{: #openwhisk_triggers_create}

Ein Auslöser ist ein benannter Kanal für eine Klasse von Ereignissen.
{: shortdesc}

Bei einem Auslöser handelt es sich um eine Deklaration, die auf einen bestimmten Typ von Ereignis reagieren soll, entweder durch einen Benutzer oder eine Ereignisquelle. Es gibt zum Beispiel die folgenden Auslöser.
- Auslöser bei Ereignissen der Standortaktualisierung
- Auslöser bei Dokumentuploads auf eine Website
- Auslöser beim Eingang von E-Mails

Auslöser können mithilfe eines Wörterverzeichnisses mit Schlüssel/Wert-Paaren aktiviert (also angewendet oder ausgelöst) werden. Manchmal wird dieses Wörterverzeichnis als das Ereignis bezeichnet. Auslöser können explizit durch einen Benutzer oder für einen Benutzer durch eine externe Ereignisquelle aktiviert werden. Wie bei Aktionen ergibt sich bei jedem Aktivieren eines Auslösers, der einer Regel zugeordnet ist, eine Aktivierungs-ID. Ein Auslöser, der keiner Regel zugeordnet ist, hat bei seiner Aktivierung keine sichtbare Wirkung zur Folge.

Ein Feed ist eine bequeme Methode zum Konfigurieren einer externen Ereignisquelle, damit diese Auslöserereignisse aktiviert, die von {{site.data.keyword.openwhisk_short}} verarbeitet werden können. Es gibt zum Beispiel die folgenden Feeds.
- Feed für {{site.data.keyword.cloudant}}-Datenänderungen, der immer dann ein Auslöserereignis aktiviert, wenn ein Dokument zu einer Datenbank hinzugefügt oder ein in der Datenbank vorhandenes Dokument geändert wird
- Git-Feed, der ein Auslöserereignis für jede Festschreibung (Commit) in einem Git-Repository aktiviert

### Regeln
{: #openwhisk_rules_use}

Eine Regel ordnet einen Auslöser einer Aktion zu.
{: shortdesc}

Jedes Mal, wenn der Auslöser zur Anwendung kommt, verwendet die Regel das Auslöserereignis als Eingabe und ruft die zugehörige Aktion auf. Mit dem entsprechenden Satz von Regeln kann ein einzelnes Auslöserereignis mehrere Aktionen aufrufen oder eine Aktion als Reaktion auf Ereignisse aus mehreren Auslösern aufgerufen werden.

Stellen Sie sich zum Beispiel ein System mit den folgenden Aktionen vor.
- `classifyImage`: Eine Aktion, die die Objekte in einem Bild ermittelt und klassifiziert.
- `thumbnailImage`: Eine Aktion, die eine Piktogrammversion eines Bilds erstellt.

Nehmen Sie außerdem an, dass zwei Ereignisquellen die folgenden Auslöser aktivieren.
- `newTweet`: Ein Auslöser, der aktiviert wird, wenn ein neuer Tweet gepostet wird.
- `imageUpload`: Ein Auslöser, der aktiviert wird, wenn ein Bild auf eine Website hochgeladen wird.

Sie können Regeln so einrichten, dass ein einzelnes Auslöserereignis mehrere Aktionen aufruft und dass mehrere Auslöser dieselbe Aktion aufrufen.
- Regel `newTweet -> classifyImage`
- Regel `imageUpload -> classifyImage`
- Regel `imageUpload -> thumbnailImage`

Die drei Regeln sorgen für das folgende Verhalten:
- Bilder in beiden Tweets werden klassifiziert.
- Hochgeladene Bilder werden klassifiziert
- Eine Piktogrammversion wird generiert.

## Auslöser zum Kanalisieren von Ereignissen erstellen
{: #openwhisk_triggers_fire}

Die folgenden Schritten veranschaulichen, wie Sie einen Beispielauslöser zum Senden von Aktualisierungen des Benutzerstandorts erstellen und wie Sie den Auslöser manuell auslösen.

1. Erstellen Sie den Auslöser. Auslöser müssen direkt in einem Namensbereich erstellt werden. Ihre Erstellung in einem Paket ist nicht möglich.
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: created trigger locationUpdate
    ```
    {: screen}

2. Überprüfen Sie ob der Auslöser erstellt wurde.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Beispielausgabe:
    ```
    triggers
  /someNamespace/locationUpdate                            private
    ```
    {: screen}
    Der Auslöser dient als benannter Kanal, in dem Ereignisse ausgelöst werden können.

3. Aktivieren Sie ein Auslöserereignis.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Da diesem Auslöser keine Regel zugeordnet ist, werden die übergebenen Parameter bei keiner Aktion als Eingabe verwendet. Beispielausgabe:
    ```
    ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

Im nächsten Abschnitt können Sie den Auslöser einer Aktion zuordnen, indem Sie eine Regel erstellen.

## Regeln zum Zuordnen von Auslösern zu Aktionen verwenden
{: #openwhisk_rules_assoc}

Regeln werden dazu verwendet, einen Auslöser einer Aktion zuzuordnen. Jedes Mal, wenn ein Auslöserereignis aktiviert wird, wird die Aktion mit den Parametern des Auslöserereignisses aufgerufen.

Nachdem Sie den [Auslöser `locationUpdate`](#openwhisk_triggers_fire) erstellt haben, können Sie wie in den folgenden Schritten gezeigt eine Beispielregel erstellen, die die Aktion `hello` immer dann aufruft, wenn eine Standortaktualisierung gepostet wird.

1. Erstellen Sie eine Datei 'hello.js' mit dem folgenden Aktionscode:
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: pre}

2. Erstellen Sie die Aktion `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Erstellen Sie die Regel `myRule`, um den Auslöser `locationUpdate` der Aktion `hello` zuzuordnen. Regeln müssen direkt in einem Namensbereich erstellt werden. Ihre Erstellung in einem Paket ist nicht möglich.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. Aktivieren Sie den Auslöser `locationUpdate`. Jedes Mal, wenn ein Auslöserereignis eintritt, wird die Aktion `hello` mit den Ereignisparametern aufgerufen.
```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Beispielausgabe:
    ```
    ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Stellen Sie fest, ob die Aktion `hello` aufgerufen wurde, indem Sie das Protokoll der letzten Aktivierung prüfen.
    ```
    ibmcloud fn activation list --limit 1 hello
    ```
    {: pre}

    Beispielausgabe:
    ```
    activations
  9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. Rufen Sie weitere Informationen zu der Aktivierungs-ID aus der Ausgabe des vorherigen Befehls ab.
    ```
    ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    Beispielausgabe:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    Wie Sie sehen, hat die Aktion `hello` die Ereignisnutzdaten (payload) empfangen und die erwartete Zeichenfolge zurückgegeben.

7. Zum Inaktivieren der Regel können Sie den folgenden Befehl ausführen.
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

Anhand von Regeln können Sie Auslöser auch mit Sequenzen verbinden. Sie können zum Beispiel eine Aktionssequenz namens `recordLocationAndHello` erstellen,
die durch die Regel `anotherRule` aktiviert wird:
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
