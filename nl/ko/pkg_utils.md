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


# 수도/전기/가스
{: #pkg_utils}

여러 유틸리티 액션이 사용할 수 있는 `/whisk.system/utils` 패키지에서 제공됩니다. 이 예에서는 `utils` 패키지에서 여러 액션을 사용하여 시퀀스를 작성합니다.
{: shortdesc}

1. `/whisk.system/utils` 패키지의 액션을 나열하십시오.

    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    **출력 예**
    
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

2. `split` 및 `sort` 액션을 사용하여 `split`의 결과가 `sort`에 대한 인수로 전달되도록 액션 시퀀스를 작성하십시오. 이 액션 시퀀스는 텍스트의 일부 행을 배열로 변환하고 해당 행을 정렬합니다.

  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    출력에서 행 나누기는 알파벳순으로 정렬됩니다.
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






