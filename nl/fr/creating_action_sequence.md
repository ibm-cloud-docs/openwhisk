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


# Création de séquences d'actions
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

Vous pouvez créer une action qui assemble des actions dans une séquence. Le résultat d'une action est transmis sous forme d'argument à l'action suivante.
{: shortdesc}

Plusieurs actions d'utilitaire sont fournies dans le package `/whisk.system/utils` que vous pouvez utiliser pour créer votre première séquence.

1. Affichez les actions du package `/whisk.system/utils`.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    Exemple de sortie :
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

**Remarque **:
* Les paramètres transmis entre les actions de la séquence sont explicites, sauf les paramètres par défaut. Par conséquent, les paramètres qui sont transmis à la séquence d'actions ne sont disponibles que pour la première action de la séquence. Le résultat de la première action dans la séquence devient l'objet JSON d'entrée de la deuxième action de la séquence ,et ainsi de suite. Cet objet ne comporte pas les paramètres qui sont transmis initialement à la séquence sauf si la première action les inclut dans son résultat. Les paramètres d'entrée d'une action sont fusionnés avec ses paramètres par défaut, les premiers étant prioritaires et remplaçant tout paramètre par défaut correspondant. Pour plus d'informations sur l'appel des séquences d'actions avec plusieurs paramètres nommés, voir [Définir les paramètres par défaut sur une action](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action).
* Une séquence n'a pas de délai d'attente global distinct des délais de chaque action au sein de la séquence. Etant donné qu'une séquence est un pipeline d'opérations, une action qui échoue rompt le pipeline. Si une action dépasse le délai d'attente, la séquence entière est sortie avec cet échec.
