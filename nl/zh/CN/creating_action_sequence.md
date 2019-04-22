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


# 创建操作序列
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

可以创建用于将一序列操作链接在一起的操作。一个操作的结果将作为自变量传递给下一个操作。
{: shortdesc}

在 `/whisk.system/utils` 包中提供了多个实用程序操作，可用于创建第一个序列。

1. 列出 `/whisk.system/utils` 包中的操作。
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    示例输出：
    ```
      package /whisk.system/utils：构建用于设置数据格式和组合数据的块
   action /whisk.system/utils/head：抽取数组的前缀
   action /whisk.system/utils/split：将字符串拆分成数组
   action /whisk.system/utils/sort：对数组排序
   action /whisk.system/utils/echo：返回输入
   action /whisk.system/utils/date：当前日期和时间
   action /whisk.system/utils/cat：将输入连接成一个字符串
  ```
    {: screen}

2. 使用 `split` 和 `sort` 操作来创建操作序列，以便 `split` 的结果将作为自变量传递给 `sort`。此操作序列会将一些文本行转换为数组，然后对这些行排序。
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. 调用操作。
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    在输出中，拆分的行按字母顺序排序。
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

**注**：
* 在序列中的操作之间传递的参数为显式参数，但缺省参数除外。因此，传递到操作序列的参数只可用于序列中的第一个操作。序列中第一个操作的结果成为序列中第二个操作的输入 JSON 对象，依此类推。此对象不包含初始传递到序列的任何参数，除非第一个操作在其结果中包含这些参数。操作的输入参数会与操作的缺省参数合并，并且操作的输入参数优先于并覆盖任何匹配的缺省参数。有关使用多个指定参数来调用操作序列的更多信息，请参阅[在操作上设置缺省参数](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action)。
* 序列的总体超时会受该序列中各个操作的超时的影响。因为序列是操作的管道，所以一个操作发生故障会使管道中断。如果一个操作超时，那么将退出整个序列，并返回该故障。
