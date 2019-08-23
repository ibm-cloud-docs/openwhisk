---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# 룰을 통한 트리거와 액션의 연관
{: #rules}

트리거가 실행될 때마다 룰은 입력으로서 트리거 이벤트를 사용하고 연관된 액션을 호출합니다. 적절한 룰 세트를 사용하면 단일 트리거가 다중 액션을 호출하거나 액션이 다중 트리거의 이벤트에 대한 응답으로 호출될 수 있습니다.
{: shortdesc}


## UI에서 룰 작성
{: #rules_ui}

사용자 인터페이스에서 액션과 트리거를 연관시키는 룰이 사용자에 맞게 작성됩니다.
{: shortdesc}

액션 또는 트리거에 대한 세부사항을 작성하거나 액세스할 때 기존 또는 새 액션이나 트리거에 연결하는 옵션이 제공됩니다. 연결할 때 룰이 사용자에 맞게 작성되고 형식 `ACTION_NAME-TRIGGER_NAME`으로 이름이 지정됩니다.

CLI에서 `ibmcloud fn rule list`를 실행하여 룰이 사용자에 맞게 작성되었는지 확인할 수 있습니다.


## CLI에서 룰 작성
{: #rules_create}

룰은 트리거를 액션과 연관시키는 데 사용됩니다. 트리거 이벤트가 실행될 때마다 트리거 이벤트의 매개변수를 사용하여 액션이 호출됩니다.

시작하기 전에 [조치](/docs/openwhisk?topic=cloud-functions-actions) 및 [트리거](/docs/openwhisk?topic=cloud-functions-triggers)를 작성하십시오.


트리거를 액션과 연관시키도록 룰을 작성하십시오. 룰은 네임스페이스 내에서 직접 작성되어야 하며 패키지 내에서는 작성될 수 없습니다.
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


룰을 사용하지 않으려면 다음 명령을 실행할 수 있습니다.
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## 액션 시퀀스에 대한 룰 작성
{: #rules_seq}

룰을 사용하여 트리거를 액션 시퀀스와 연관시킬 수 있습니다.

시작하기 전에 [액션 시퀀스](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) 및 [트리거](/docs/openwhisk?topic=cloud-functions-triggers)를 작성하십시오.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## 다중 트리거 및 액션 연관
{: #rules_assoc}

각 조합마다 룰을 작성하여 트리거와 액션의 다른 조합을 사용할 수 있습니다. 트리거 및 액션이 일대일 비율일 필요는 없습니다.

예를 들어, 다음 액션을 고려하십시오.

|액션 |설명 |
| --- | --- |
|`classifyImage` |이미지에서 오브젝트를 발견하고 이를 분류하는 액션입니다. |
|`thumbnailImage` |이미지의 작은 그림 버전을 작성하는 액션입니다. |

또한 두 개의 이벤트 소스가 다음 트리거를 실행한다고 가정하십시오.

|트리거 |설명 |
| --- | --- |
|`newTweet` |새 트윗이 게시될 때 실행되는 트리거입니다. |
|`imageUpload` |이미지가 웹 사이트에 업로드될 때 실행되는 트리거입니다. |

단일 트리거 이벤트가 다중 액션을 호출하고 다중 트리거가 동일한 액션을 호출하도록 하는 룰을 설정할 수 있습니다.
- `newTweet -> classifyImage` 룰
- `imageUpload -> classifyImage` 룰
- `imageUpload -> thumbnailImage` 룰

3개의 룰은 다음 동작을 설정합니다.
- 두 트윗의 이미지가 모두 분류됩니다.
- 업로드된 이미지가 분류됩니다.
- 작은 그림 버전이 생성됩니다.

