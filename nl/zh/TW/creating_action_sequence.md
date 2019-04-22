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


# 建立動作序列
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

您可以建立一個動作，將一連串的動作鏈結在一起。將某個動作的結果當作下一個動作的引數來傳遞。
{: shortdesc}

在 `/whisk.system/utils` 套件中提供了數個公用程式動作，可用來建立第一個序列。

1. 列出 `/whisk.system/utils` 套件中的動作。
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    輸出範例：
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

**附註**：
* 序列中動作之間所傳遞的參數十分明確，但預設參數除外。因此，傳遞給動作序列的參數僅適用於序列中的第一個動作。序列中第一個動作的結果會變成序列中第二個動作的輸入 JSON 物件，以此類推。此物件不包括一開始傳遞給序列的任何參數，除非第一個動作將它們包括在結果中。動作的輸入參數會與動作的預設參數合併，而前者的優先順序較高，並且會置換任何相符的預設參數。如需使用多個具名參數來呼叫動作序列的相關資訊，請參閱[在動作上設定預設參數](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action)。
* 序列不會具有與序列中每個動作之逾時分開的整體逾時。因為序列是作業管線，所以一個動作失敗將會中斷管線。如果有一個動作逾時，則整個序列會因為該失敗而結束。
