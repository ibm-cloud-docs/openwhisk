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


# Programas de utilidad
{: #pkg_utils}

Se proporcionan varias acciones de utilidad en el paquete `/whisk.system/utils` que puede utilizar. En este ejemplo se crea una secuencia con varias acciones desde el paquete `utils`.
{: shortdesc}

1. Liste las acciones del paquete `/whisk.system/utils`.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Salida de ejemplo:
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

2. Utilizando las acciones `split` y `sort`, cree una secuencia de acciones de forma que el resultado de `split` se pase como argumento a `sort`. Esta secuencia de acciones convierte algunas líneas de texto en una matriz, y ordena las líneas.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Invoque la acción.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    En la salida, las líneas divididas se clasifican alfabéticamente.
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




