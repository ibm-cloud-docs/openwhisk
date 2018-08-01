---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Allgemeine Konzepte
{: #openwhisk_triggers}

{{site.data.keyword.openwhisk_short}}-Auslöser und -Regeln statten die Plattform mit ereignisgesteuerten Funktionen aus. Ereignisse aus externen und internen Ereignisquellen werden durch einen Auslöser kanalisiert. Durch Regeln können Ihre Aktionen auf diese Ereignisse reagieren.
{: shortdesc}

## Was ist ein Auslöser?
{: #openwhisk_triggers_create}

Ein Auslöser ist ein benannter Kanal für eine Klasse von Ereignissen. Es gibt zum Beispiel die folgenden Auslöser:
- Auslöser von Ereignissen zur Positionsaktualisierung.
- Auslöser von Dokumentuploads auf eine Website.
- Auslöser von eingehenden E-Mails.

Auslöser können mithilfe eines Wörterverzeichnisses mit Schlüssel/Wert-Paaren *aktiviert* (ausgelöst) werden. Manchmal wird dieses Wörterverzeichnis als das *Ereignis* bezeichnet. Wie bei Aktionen hat das Aktivieren eines Auslösers eine **Aktivierungs-ID** zur Folge.

Auslöser können explizit durch einen Benutzer oder für einen Benutzer durch eine externe Ereignisquelle aktiviert werden.
Ein *Feed* ist eine bequeme Methode zum Konfigurieren einer externen Ereignisquelle zum Aktivieren von Auslöserereignissen, die von {{site.data.keyword.openwhisk_short}} verarbeitet werden können. Sehen Sie sich die folgenden Beispielfeeds an:
- Der Feed für {{site.data.keyword.cloudant}}-Datenänderungen, der jedes Mal ein Auslöserereignis aktiviert, wenn ein Dokument in einer Datenbank hinzugefügt oder geändert wird.
- Ein Git-Feed, der ein Auslöserereignis für jede Festschreibung (Commit) in einem Git-Repository aktiviert.

## Wie wirken sich Regeln auf Auslöser aus?
{: #openwhisk_rules_use}

Eine Regel ordnet genau einen Auslöser genau einer Aktion zu, wobei jede Aktivierung des Auslösers zur Folge hat, dass die entsprechende Aktion mit dem Auslöserereignis als Eingabe aufgerufen wird.

Mit dem entsprechenden Satz von Regeln kann ein einzelnes Auslöserereignis mehrere Aktionen aufrufen oder eine Aktion als Reaktion auf Ereignisse aus mehreren Auslösern aufgerufen werden.

Betrachten Sie zum Beispiel ein System mit den folgenden Aktionen:
- `classifyImage`: Eine Aktion, die die Objekte in einem Bild ermittelt und klassifiziert.
- `thumbnailImage`: Eine Aktion, die eine Piktogrammversion eines Bilds erstellt.

Nehmen Sie außerdem an, dass zwei Ereignisquellen die folgenden Auslöser aktivieren:
- `newTweet`: Ein Auslöser, der aktiviert wird, wenn ein neuer Tweet gepostet wird.
- `imageUpload`: Ein Auslöser, der aktiviert wird, wenn ein Bild auf eine Website hochgeladen wird.

Sie können Regeln so einrichten, dass ein einzelnes Auslöserereignis mehrere Aktionen aufruft und dass mehrere Auslöser dieselbe Aktion aufrufen:
- Regel `newTweet -> classifyImage`.
- Regel `imageUpload -> classifyImage`.
- Regel `imageUpload -> thumbnailImage`.

Die drei Regeln richten das folgende Verhalten ein:
- Bilder in beiden Tweets werden klassifiziert.
- Hochgeladene Bilder werden klassifiziert
- Eine Piktogrammversion wird generiert.

## Auslöser erstellen und aktivieren
{: #openwhisk_triggers_fire}

Auslöser können aktiviert werden, wenn bestimmte Ereignisse stattfinden, oder sie können manuell aktiviert werden.

Erstellen Sie zum Beispiel einen Auslöser, um Aktualisierungen an Benutzerstandorten zu senden, und aktivieren Sie den Auslöser manuell.
1. Geben Sie den folgenden Befehl ein, um den Auslöser zu erstellen:
  ```
  ibmcloud fn trigger create locationUpdate
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: created trigger locationUpdate
  ```
  {: screen}

2. Prüfen Sie, ob der Auslöser erstellt wurde, indem Sie die Gruppe von Auslösern auflisten.
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

  Jetzt ist ein benannter "Kanal" erstellt, an den Ereignisse aktiviert werden können.

3. Als Nächstes aktivieren Sie ein Auslöserereignis, indem Sie den Auslösernamen und die Parameter des Auslösers angeben:
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: triggered locationUpdate with id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

Ein Auslöser, der ohne begleitende Regel aktiviert wird, die mit ihm abzugleichen ist, hat keine sichtbaren Auswirkungen.
Auslöser können nicht innerhalb eines Pakets erstellt werden; sie müssen direkt in einem **Namensbereich** erstellt werden.

## Regeln zum Zuordnen von Auslösern zu Aktionen verwenden
{: #openwhisk_rules_assoc}

Regeln werden dazu verwendet, einen Auslöser einer Aktion zuzuordnen. Jedes Mal, wenn ein Auslöserereignis aktiviert wird, wird die Aktion mit den Ereignisparametern aufgerufen.

Erstellen Sie zum Beispiel eine Regel, die die Aktion `hello` aufruft, wenn eine Standortaktualisierung gesendet wird.
1. Erstellen Sie eine Datei 'hello.js' mit dem folgenden Aktionscode:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Stellen Sie sicher, dass der Auslöser und die Aktion vorhanden sind:
  ```
  ibmcloud fn trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. Der nächste Schritt ist die Erstellung der Regel. Die Regel wird nach der Erstellung aktiviert. Das heißt, sie ist unverzüglich verfügbar, um auf die Aktivierung Ihres Auslösers zu antworten. Die drei Parameter sind der _Name der Regel_, der _Name des Auslösers_ und der _Name der Aktion_.
  ```
  ibmcloud fn rule create myRule locationUpdate hello
  ```
  {: pre}

  Sie können eine Regel jederzeit inaktivieren:
  ```
  ibmcloud fn rule disable myRule
  ```
  {: pre}

4. Aktivieren Sie den Auslöser **locationUpdate**. Jedes Mal, wenn Sie ein Ereignis auslösen, wird die Aktion **hello** mit den Ereignisparametern aufgerufen.
  ```
  ibmcloud fn trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Beispielausgabe:
  ```
  ok: triggered locationUpdate with id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. Stellen Sie fest, ob die Aktion **hello** aufgerufen wurde, indem Sie die letzte Aktivierung prüfen.
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

  Fragen Sie anschließend die Aktivierungs-ID ab, die in der vorherigen Befehlsausgabe aufgeführt ist:
  ```
  ibmcloud fn activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  Beispielausgabe:
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  Wie Sie sehen, hat die Aktion **hello** die Ereignisnutzdaten (payload) empfangen und die erwartete Zeichenfolge zurückgegeben.

Sie können mehrere Regeln erstellen, die denselben Auslöser verschiedenen Aktionen zuordnen.
Auslöser und Regeln können nicht zu einem Paket gehören. Die Regel kann jedoch einer Aktion zugeordnet werden, die zu einem Paket gehört. Beispiel: 
  ```
  ibmcloud fn rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

Sie können Regeln auch mit Sequenzen verwenden. Beispielsweise können Sie eine Aktionssequenz `recordLocationAndHello` erstellen,
die durch die Regel `anotherRule` aktiviert wird.
  ```
  ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
