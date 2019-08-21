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


# 实用程序
{: #pkg_utils}

在 `/whisk.system/utils` 包中提供了多个实用程序操作可供您使用。此示例使用来自 `utils` 包中的多个操作来创建序列。
{: shortdesc}

1. 列出 `/whisk.system/utils` 包中的操作。
    

    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    **示例输出**
    
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






