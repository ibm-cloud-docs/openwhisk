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


# ユーティリティー
{: #pkg_utils}

`/whisk.system/utils` パッケージには、使用できるユーティリティー・アクションがいくつか用意されています。この例では、`utils` パッケージのいくつかのアクションを使用してシーケンスを作成します。
{: shortdesc}

1. `/whisk.system/utils` パッケージに含まれているアクションをリストします。
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    出力例:
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

2. `split` アクションと `sort` アクションを使用して、`split` の結果が `sort`への引数として渡されるようにアクション・シーケンスを作成します。 このアクション・シーケンスは、数行のテキストを 1 つの配列に変換し、行をソートします。
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. アクションを呼び出します。
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    出力では、分割行がアルファベット順にソートされています。
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




