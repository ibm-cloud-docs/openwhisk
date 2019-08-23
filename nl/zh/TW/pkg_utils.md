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


# 公用程式
{: #pkg_utils}

在 `/whisk.system/utils` 套件中提供了多個公用程式動作可供您使用。此範例使用來自 `utils` 套件中的多個動作來建立序列。
{: shortdesc}

1. 列出 `/whisk.system/utils` 套件中的動作。

    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    **輸出範例**
    
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

2. 使用 `split` 和 `sort` 動作，建立動作序列，以將 `split` 的結果當成引數傳給 `sort`。此動作序列會將數行文字轉換為一個陣列，並排序這些行。

  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. 呼叫動作。

    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    在輸出中，會依字母順序來排序分割行。
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






