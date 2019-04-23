---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-07"

keywords: action sequence, serverless,

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


# Aktionssequenzen erstellen
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

Sie können eine Aktion erstellen, die eine Sequenz von Aktionen miteinander verkettet. Dabei wird das Ergebnis einer Aktion als Argument an die nächste Aktion übergeben.
{: shortdesc}

Im Paket `/whisk.system/utils` werden verschiedene Dienstprogrammaktionen bereitgestellt, die Sie zum Erstellen Ihrer ersten Sequenz verwenden können.

1. Listen Sie die Aktionen im Paket `/whisk.system/utils` auf.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Beispielausgabe:
    ```
    package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
    ```
    {: screen}

2. Erstellen Sie mithilfe der Aktionen `split` und `sort` eine Aktionssequenz, sodass das Ergebnis von `split` als Argument an `sort` übergeben wird. Diese Aktionssequenz konvertiert Zeilen von Text in ein Array und sortiert die Zeilen.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Rufen Sie die Aktion auf.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    In der Ausgabe werden die getrennten Zeilen alphabetisch sortiert.
    ```
    {
        "length": 3,
      "lines": [
            "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
    }
    ```
    {: screen}

**Hinweis**:
* Parameter, die zwischen den Aktionen in der Sequenz übergeben werden, sind (mit Ausnahme der Standardparameter) explizit. Daher sind die Parameter, die an die Aktionssequenz übergeben werden, nur für die erste Aktion in der Sequenz verfügbar. Das Ergebnis der ersten Aktion in der Sequenz wird zum JSON-Eingabeobjekt für die zweite Aktion in der Sequenz usw. Das Objekt enthält keine Parameter, die ursprünglich an die Sequenz übergeben wurden, es sei denn, die erste Aktion schließt sie explizit in ihr Ergebnis ein. Die Eingabeparameter für eine Aktion werden mit den Standardparametern der Aktion zusammengeführt. Erstere haben Vorrang und überschreiben alle übereinstimmenden Standardparameter. Weitere Informationen zum Aufrufen von Aktionssequenzen mit mehreren benannten Parametern finden Sie unter [Standardparameter für eine Aktion festlegen](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action).
* Eine Sequenz besitzt kein Gesamtzeitlimit, das jenseits der Zeitlimits für die einzelnen Aktionen innerhalb der Aktionssequenz gilt. Da es sich bei einer Sequenz um eine Hintereinanderschaltung oder Aneinanderkettung von Operationen ähnlich einer Pipeline handelt, bewirkt ein Fehler in einer Aktion die Unterbrechung dieser Pipeline. Wenn eine Aktion das Zeitlimit überschreitet, wird die gesamte Sequenz mit diesem Fehler beendet.
