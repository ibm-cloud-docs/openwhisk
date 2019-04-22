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


# 액션 시퀀스 작성
{: #create_action_sequence}
{: #openwhisk_create_action_sequence}

일련의 액션을 연결하는 액션을 작성할 수 있습니다. 한 액션의 결과는 다음 액션에 대한 인수로 전달됩니다.
{: shortdesc}

여러 유틸리티 액션이 첫 번째 시퀀스 작성에 사용할 수 있는 `/whisk.system/utils` 패키지에서 제공됩니다.

1. `/whisk.system/utils` 패키지의 액션을 나열하십시오.
    ```
    ibmcloud fn package get --summary /whisk.system/utils
    ```
    {: pre}

    출력 예:
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

**참고**:
* 기본 매개변수를 제외하면 시퀀스의 액션 간에 전달된 매개변수는 명시적입니다. 따라서 액션 시퀀스에 전달된 매개변수는 시퀀스의 첫 번째 액션에만 사용 가능합니다. 시퀀스에서 첫 번째 액션의 결과는 시퀀스에서 두 번째 액션에 대한 입력 JSON 오브젝트가 됩니다(계속 동일하게 반복됨). 첫 번째 액션이 결과에 이를 포함하지 않는 한, 이 오브젝트에는 시퀀스에 원래 전달된 매개변수가 포함되지 않습니다. 액션에 대한 입력 매개변수는 액션의 기본 매개변수와 병합되며, 전자가 우선순위를 갖고 일치하는 기본 매개변수를 대체합니다. 복수의 이름 지정된 매개변수로 액션 시퀀스 호출에 대한 자세한 정보는 [액션에 기본 매개변수 설정](/docs/openwhisk?topic=cloud-functions-working-with-parameters#default-params-action)을 참조하십시오.
* 시퀀스에는 시퀀스 내의 각 액션의 제한시간과 별도인 전체 제한시간이 없습니다. 시퀀스가 오퍼레이션의 파이프라인이므로 한 액션의 장애는 파이프라인을 손상시킵니다. 하나의 액션의 제한시간이 초과되면 전체 시퀀스가 해당 장애로 종료됩니다.
