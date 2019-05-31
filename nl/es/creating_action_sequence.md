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


# Creación de secuencias de acciones
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

Puede crear una acción que encadene una secuencia de acciones. El resultado de una acción se pase como argumento a la acción siguiente.
{: shortdesc}

Se proporcionan varias acciones de utilidad en el paquete `/whisk.system/utils` que puede utilizar para crear su primera secuencia.

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

**Nota**:
* Los parámetros que se pasan entre acciones de la secuencia son explícitos, salvo los predeterminados. Por tanto, los parámetros que se pasan a la secuencia de acción solo están disponibles para la primera acción de la secuencia. El resultado de la primera acción de la secuencia se convierte en el objeto JSON de entrada de la segunda acción de la secuencia, y así sucesivamente. Este objeto no incluye ninguno de los parámetros que originalmente se han pasado a la secuencia, a menos que la primera acción los incluya en el resultado. Los parámetros de entrada a una acción se fusionan con los parámetros predeterminados de la acción, dando prioridad a los primeros y sustituyendo los parámetros predeterminados coincidentes. Para obtener más información sobre la invocación de secuencias de acciones con varios parámetros con nombre, consulte [Establecimiento de parámetros predeterminados en una acción](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action).
* Una secuencia no tiene un tiempo de espera excedido global distinto de los tiempos de espera de cada acción dentro de la secuencia. Debido a que una secuencia se trata como un conducto de operaciones, un error en una acción interrumpe el conducto. Si una acción excede el tiempo de espera, toda la secuencia finaliza con ese error.
