---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, functions

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


# Dienstprogramme
{: #pkg_utils}

Im Paket `/whisk.system/utils` werden verschiedene Dienstprogrammaktionen bereitgestellt, die Sie verwenden können. In diesem Beispiel wird eine Sequenz mit mehreren Aktionen aus dem Paket `utils` erstellt.
{: shortdesc}

1. Listen Sie die Aktionen im Paket `/whisk.system/utils` auf.

    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    **Beispielausgabe**
    
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






