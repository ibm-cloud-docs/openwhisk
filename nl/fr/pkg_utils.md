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


# Utilitaires
{: #pkg_utils}

Plusieurs actions d'utilitaire sont fournies dans le package `/whisk.system/utils` que vous pouvez utiliser. Dans cet exemple, une séquence contenant plusieurs actions est créée à partir du package `utils`.
{: shortdesc}

1. Affichez les actions du package `/whisk.system/utils`.

    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    **Exemple de sortie**
    
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

2. A l'aide des actions `split` et `sort`, créez une séquence d'actions de sorte que le résultat de `split` soit transmis en tant qu'argument à `sort`. Cette séquence d'actions convertit des lignes de texte en tableau et trie les lignes.

  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Appelez l'action.

    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Dans la sortie, les lignes fractionnées sont triées par ordre alphabétique.
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






