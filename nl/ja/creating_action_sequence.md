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


# アクション・シーケンスの作成
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

アクションのシーケンスを一緒にチェーニングしたアクションを作成できます。 1 つのアクションの結果が次のアクションへの引数として渡されます。
{: shortdesc}

`/whisk.system/utils` パッケージには、最初のシーケンスを作成するために使用できるユーティリティー・アクションがいくつか用意されています。

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

**注**:
* シーケンス中のアクション間で渡されるパラメーターは、デフォルト・パラメーターを除いて、明示的です。 したがって、アクション・シーケンスに渡されるパラメーターを使用できるのはシーケンス中の先頭アクションのみです。 シーケンス中の最初のアクションの結果が、シーケンス中の 2 番目のアクションへの入力 JSON オブジェクトになり、以降同様に渡されます。 このオブジェクトには、最初にシーケンスに渡されたパラメーターはどれも含まれません (ただし、先頭アクションがその結果にそれらのパラメーターを組み込んだ場合は除きます)。 あるアクションへの入力パラメーターは、そのアクションのデフォルト・パラメーターとマージされます。その際、入力パラメーターが優先され、一致するデフォルト・パラメーターはオーバーライドされます。 複数の名前付きパラメーターを指定したアクション・シーケンス呼び出しについて詳しくは、[アクションでのデフォルト・パラメーターの設定](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action)を参照してください。
* シーケンスには、シーケンス内の各アクションのタイムアウトとは別の、全体的なタイムアウトはありません。 シーケンスは操作のパイプラインなので、1 つのアクションで失敗すると、パイプラインが中断します。 1 つのアクションでタイムアウトが発生すると、その失敗でシーケンス全体が終了します。
