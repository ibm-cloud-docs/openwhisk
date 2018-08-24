---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant 이벤트 소스
{: #openwhisk_cloudant}

{{site.data.keyword.cloudant}} 데이터베이스 변경사항을 청취하는 방법을 알아보고 데이터베이스 변경 이벤트를 필터링하고 액션 시퀀스를 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서를 처리하십시오. `/whisk.system/cloudant` 패키지를 사용하면 {{site.data.keyword.cloudant_short_notm}} 데이터베이스 관련 작업을 수행할 수 있으며, 여기에는 다음 액션과 피드가 포함되어 있습니다. 

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |패키지 |dbname, host, username, password | Cloudant 데이터베이스 관련 작업을 수행합니다. |
|`/whisk.system/cloudant/read` |액션 |dbname, id | 데이터베이스에서 문서를 읽습니다. |
|`/whisk.system/cloudant/write` |액션 |dbname, overwrite, doc | 데이터베이스에 문서를 작성합니다. |
|`/whisk.system/cloudant/changes` |피드 |dbname, filter, query_params, maxTriggers | 데이터베이스 변경 시 트리거 이벤트를 실행합니다. |
{: shortdesc}

다음 절에서는 연관된 패키지의 구성 및 `/whisk.system/cloudant` 패키지에서 액션과 피드를 사용하는 방법에 대해 안내합니다. {{site.data.keyword.cloudant_short_notm}} 데이터베이스 설정 및 이 데이터베이스 읽기 또는 쓰기에 대한 자세한 정보는 [{{site.data.keyword.cloudant_short_notm}} 액션](./cloudant_actions.html)을 참조하십시오.

## 필터 함수를 사용하여 트리거 작성

`changes` 피드를 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 변경 시마다 트리거를 실행하도록 서비스를 구성할 수 있습니다. 

이 예제에서 사용된 매개변수는 다음과 같습니다.

**dbname**: {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 이름입니다_(필수)_.

**maxTriggers**: 이 한계에 도달하면 트리거 실행을 중지합니다_(선택사항)_. 기본값은 무제한입니다.

**filter**: 디자인 문서에 정의된 함수를 필터링합니다_(선택사항)_

**query_params**: 필터 함수에 대한 추가 조회 매개변수입니다_(선택사항)_.

1. 이전에 작성한 패키지 바인딩에서 `changes` 피드로 트리거 **myCloudantTrigger**를 작성하십시오. 상태가 `new`인 경우 문서가 추가되거나 수정될 때 트리거를 실행하려면 `filter` 및 `query_params` 함수를 포함하십시오.

  반드시 `/_/myCloudant`를 패키지 이름으로 대체하십시오.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  출력 예:
  ```
  ok: created trigger feed myCloudantTrigger
  ```
  {: screen}

2. 활성화에 대한 폴링을 시작하여 발생한 상황을 명확하게 보여주십시오.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. 변경 피드의 결과를 관찰하는 데 사용할 수 있는 액션을 작성하십시오. 예를 들어, 다음 JavaScript 코드가 포함된 **showCloudantChange** 액션을 작성할 수 있습니다.
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. 이전에 작성된 트리거에 **showCloudantChange** 액션을 연결하기 위한 룰을 작성하십시오.
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. 액션 및 **myCloudantTrigger** 트리거에 이 액션을 연관시키기 위한 룰을 작성하십시오.

6. {{site.data.keyword.cloudant_short_notm}} 대시보드에서 기존 문서를 수정하거나 새로 작성하십시오. 문서에는 **new**로 설정된 _status_ 필드가 있어야 합니다.

7. 필터 함수 및 조회 매개변수를 기반으로 문서 상태가 **new**인 경우에만 각 문서 변경에 대해 **myCloudantTrigger** 트리거에 대한 새 활성화를 관찰하십시오.

새 활성화를 관찰할 수 없는 경우 {{site.data.keyword.cloudant_short_notm}} 데이터베이스를 읽고 쓰는 방법을 보여주는 [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) 주제를 참조하십시오. {{site.data.keyword.cloudant_short_notm}} 신임 정보가 올바른지 확인하는 데 도움이 되도록 읽기 및 쓰기 단계를 테스트하십시오.
{: tip}

## 트리거 이벤트의 데이터 구조

생성된 이벤트의 컨텐츠에는 다음 매개변수가 있습니다.

  - `id`: 문서 ID입니다.
  - `seq`: {{site.data.keyword.cloudant_short_notm}}에서 생성된 시퀀스 ID입니다.
  - `changes`: 오브젝트의 배열이며, 각각에는 문서의 개정 ID를 포함하는 `rev` 필드가 있습니다.

트리거 이벤트의 JSON 표시는 다음과 같습니다.
```json
{
    "dbname": "testdb",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

## 데이터베이스 변경 이벤트 필터링

트리거를 실행하는 불필요한 변경 이벤트를 피하기 위한 필터 함수를 선택적으로 정의할 수 있습니다.

새 필터 함수를 작성하기 위해 액션을 사용할 수 있습니다.

다음 필터 함수로 json 문서 파일 `design_doc.json`을 작성하십시오.
```json
{
  "doc": {
    "_id": "_design/mailbox",
    "filters": {
      "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
    }
  }
}
```
{: codeblock}

다음 필터 함수로 데이터베이스에서 디자인 문서를 작성하십시오.
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

새 디자인 문서에 대한 정보가 화면에 출력됩니다.
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

## 액션 시퀀스 및 변경 트리거를 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 문서 처리
{: #openwhisk_catalog_cloudant_read_change notoc}

룰의 액션 시퀀스를 사용하여 {{site.data.keyword.cloudant_short_notm}} 변경 이벤트와 연관된 문서를 페치하고 처리할 수 있습니다.

문서를 처리하는 액션의 샘플 코드:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

{{site.data.keyword.cloudant_short_notm}}에서 문서를 처리하기 위한 액션을 작성하십시오.
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

데이터베이스에서 문서를 읽기 위해 {{site.data.keyword.cloudant_short_notm}} 패키지에서 `read` 액션을 사용할 수 있습니다.
`read` 액션은 액션 시퀀스를 작성하기 위한 `myAction`으로 구성될 수 있습니다.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

`sequenceAction` 액션은 새 {{site.data.keyword.cloudant_short_notm}} 트리거 이벤트에서 액션을 활성화하는 룰에서 사용될 수 있습니다.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**참고**: `includeDoc` 매개변수 지원에 사용되던 {{site.data.keyword.cloudant_short_notm}} `changes` 트리거는 더 이상 지원되지 않습니다.

`includeDoc`로 이전에 작성된 트리거를 재작성할 수 있습니다. 트리거를 재작성하려면 다음 단계를 수행하십시오.
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

예제를 사용하여 변경된 문서를 읽고 기존 액션을 호출하는 액션 시퀀스를 작성할 수 있습니다. 더 이상 유효하지 않은 룰을 사용 안함으로 설정하고 액션 시퀀스 패턴을 사용하여 새 룰을 작성하십시오.
