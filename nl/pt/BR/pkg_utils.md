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


# Utilitários de Armazenamento
{: #pkg_utils}

Várias ações do utilitário são fornecidas no pacote `/whisk.system/utils` que você pode usar. Esse exemplo cria uma sequência com várias ações por meio do pacote `utils`.
{: shortdesc}

1. Liste as ações no pacote `/whisk.system/utils`.
    ```
    ibmcloud fn package get -- summary /whisk.system/utils
    ```
    {: pre}

    Exemplo de Saída:
    ```
    package /whisk.system/utils: construindo blocos que formatam e montam dados
   action /whisk.system/utils/head: extrair prefixo de uma matriz
   action /whisk.system/utils/split: dividir uma sequência em uma matriz
   action /whisk.system/utils/sort: classifica uma matriz
   action /whisk.system/utils/echo: retorna a entrada
   action /whisk.system/utils/date: data e hora atual
   action /whisk.system/utils/cat: concatena a entrada em uma sequência
    ```
    {: screen}

2. Usando as ações `split` e `sort`, crie uma sequência de ações para que o resultado de `split` seja passado como um argumento para `sort`. Essa sequência de ações converte algumas linhas de texto a uma matriz e classifica as linhas.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Na saída, as linhas de divisão são classificadas em ordem alfabética.
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




