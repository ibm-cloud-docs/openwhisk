---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: actions, serverless

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
{:gif: data-image-type='gif'}


# Programmi di utilità
{: #pkg_utils}

Nel pacchetto `/whisk.system/utils` vengono fornite varie azioni di programmi di utilità utilizzabili. Questo esempio crea una sequenza con varie azioni provenienti dal pacchetto `utils`.
{: shortdesc}

1. Elenca le azioni nel pacchetto `/whisk.system/utils`.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Output di esempio:
    ```
    package /whisk.system/utils: Mattoni che formattano e assemblano i dati
   action /whisk.system/utils/head: Estrae il prefisso di un array
   action /whisk.system/utils/split: Suddivide una stringa in un array
   action /whisk.system/utils/sort: Ordina un array
   action /whisk.system/utils/echo: Restituisce l'input
   action /whisk.system/utils/date: Data e ora corrente
   action /whisk.system/utils/cat: Concatena l'input in una stringa
    ```
    {: screen}

2. Utilizzando le azioni `split` and `sort`, crea una sequenza di azioni in modo che il risultato di `split` sia passato come un argomento a `sort`. Questa sequenza di azioni converte alcune righe di testo in array e ordina le righe.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Richiama l'azione.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Nell'output, le righe di suddivisione sono ordinate alfabeticamente.
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




