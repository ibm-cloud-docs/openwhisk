---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Cloudant 패키지 사용
{: #openwhisk_catalog_cloudant}
`/whisk.system/cloudant` 패키지를 사용하면 Cloudant 데이터베이스 관련 작업을 수행할 수 있습니다. 여기에는 다음의 조치 및 피드가 포함됩니다. 

| 엔티티 | 유형 | 매개변수 | 설명 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | 패키지 | dbname, host, username, password | Cloudant 데이터베이스 관련 작업 |
| `/whisk.system/cloudant/read` | 조치 | dbname, id | 데이터베이스에서 문서 읽기 |
| `/whisk.system/cloudant/write` | 조치 | dbname, overwrite, doc | 데이터베이스에 문서 쓰기 |
| `/whisk.system/cloudant/changes` | 피드 | dbname, filter, query_params, maxTriggers | 데이터베이스에 대한 변경 시에 트리거 이벤트 실행 |
{: shortdesc}

다음 주제에서는 Cloudant 데이터베이스 설정, 연관 패키지 구성 및 `/whisk.system/cloudant` 패키지에서 조치 및 피드의 사용법에 대해 안내합니다. 

## {{site.data.keyword.Bluemix_notm}}에서 Cloudant 데이터베이스에서 설정
{: #openwhisk_catalog_cloudant_in}

{{site.data.keyword.Bluemix_notm}}에서 OpenWhisk를 사용 중인 경우, OpenWhisk는 Cloudant 서비스 인스턴스에 대한 패키지 바인딩을 자동으로 작성합니다. 
{{site.data.keyword.Bluemix_notm}}에서 OpenWhisk 및 Cloudant를 사용 중이지 않으면 다음 단계로 건너뛰십시오. 

1. {{site.data.keyword.Bluemix_notm}} [대시보드](http://console.ng.Bluemix.net)에서 Cloudant 서비스 인스턴스를 작성하십시오.

  각각의 새 서비스 인스턴스에 대한 신임 정보 키를 반드시 작성하십시오. 

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 정의된 신임 정보 키를 사용하여 각각의 Cloudant 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다. 

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```

  이제 패키지 바인딩에 Cloudant 서비스 인스턴스와 연관된 신임 정보가 포함되어 있습니다. 

3. 이전에 작성된 패키지 바인딩이 Cloudant {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스 호스트 및 신임 정보로 구성되었는지 확인하십시오. 

  ```
  wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters
  ```
  ```json
  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```

## {{site.data.keyword.Bluemix_notm}} 외부의 Cloudant 데이터베이스에서 설정
{: #openwhisk_catalog_cloudant_outside}

{{site.data.keyword.Bluemix_notm}}에서 OpenWhisk를 사용하지 않거나 {{site.data.keyword.Bluemix_notm}} 외부에서 Cloudant 데이터베이스를 설정하고자 하는 경우에는 Cloudant 계정에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Cloudant 계정 호스트 이름, 사용자 이름 및 비밀번호가 필요합니다. 

1. Cloudant 계정에 대해 구성된 패키지 바인딩을 작성하십시오. 

  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}
  

2. 패키지 바인딩이 존재하는지 확인하십시오. 

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myNamespace/myCloudant private binding
  ```


## Cloudant 데이터베이스에 대한 변경 청취
{: #openwhisk_catalog_cloudant_listen}

### 데이터베이스 변경 이벤트 필터링

트리거를 실행하는 불필요한 변경 이벤트를 피하기 위한 필터 함수를 정의할 수 있습니다. 

새 필터 함수를 작성하기 위해 조치를 사용할 수 있습니다. 

다음의 필터 함수로 json 문서 파일 `design_doc.json` 작성
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

필터 함수로 데이터베이스에서 디자인 문서 작성
```
wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
새 디자인 문서에 대한 정보가 화면에 출력됩니다.
```json
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```

### 필터 함수를 사용하여 트리거 작성

`changes` 피드를 사용하여 Cloudant 데이터베이스에 대한 변경 시마다 트리거를 실행하도록 서비스를 구성할 수 있습니다. 매개변수는 다음과 같습니다. 

- `dbname`: Cloudant 데이터베이스의 이름입니다.
- `maxTriggers`: 이 한계에 도달하면 트리거 실행을 중지합니다. 기본값은 무제한입니다. 
- `filter`: 디자인 문서에서 정의된 필터 함수입니다. 
- `query_params`: 필터 함수에 대한 추가 조회 매개변수입니다. 


1. 이전에 작성한 패키지 바인딩의 `changes` 피드로 트리거를 작성하십시오. 상태가 `new`인 경우 문서가 추가되거나 수정될 때 트리거를 실행하려면 `filter` 및 `query_params` 함수를 포함하십시오.
반드시 `/_/myCloudant`를 패키지 이름으로 대체하십시오. 

  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}
  ```
  ok: created trigger feed myCloudantTrigger
  ```

2. 활성화에 대해 폴링하십시오. 

  ```
  wsk activation poll
  ```
  {: pre}

3. Cloudant 대시보드에서 기존 문서를 수정하거나 새 문서를 작성하십시오. 

4. 필터 함수 및 조회 매개변수를 기반으로 문서 상태가 `new`인 경우에만 각 문서 변경에 대해 `myCloudantTrigger` 트리거에 대한 새 활성화를 관찰하십시오. 
  
  **참고**: 새 활성화를 관찰할 수 없는 경우에는 Cloudant 데이터베이스에 대한 읽기 및 쓰기 관련 후속 절을 참조하십시오. 다음의 읽기 및 쓰기 단계를 테스트하면 Cloudant 신임 정보가 올바른지 여부를 확인하는 데 도움이 됩니다. 
  
  이제 규칙을 작성하고 이를 조치에 연관시켜서 문서 업데이트에 반응할 수 있습니다. 
  
  생성된 이벤트의 컨텐츠에는 다음 매개변수가 있습니다. 
  
  - `id`: 문서 ID입니다. 
  - `seq`: Cloudant에 의해 생성된 시퀀스 ID입니다. 
  - `changes`: 오브젝트의 배열이며, 각각에는 문서의 개정 ID를 포함하는 `rev` 필드가 있습니다. 
  
  트리거 이벤트의 JSON 표시는 다음과 같습니다. 
  
  ```json
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```
  
## Cloudant 데이터베이스에 쓰기
{: #openwhisk_catalog_cloudant_write}

조치를 사용하여 `testdb`라고 하는 Cloudant 데이터베이스에 문서를 저장할 수 있습니다. 이 데이터베이스가 Cloudant 계정에 존재하는지 확인하십시오.

1. 이전에 작성한 패키지 바인딩의 `write` 조치를 사용하여 문서를 저장하십시오. 반드시 `/_/myCloudant`를 패키지 이름으로 대체하십시오. 

  ```
  wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287
  ```
  ```json
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```

2. Cloudant 대시보드에서 문서를 찾아보고 해당 문서가 존재하는지 확인하십시오. 

  `testdb` 데이터베이스에 대한 대시보드 URL은 다음과 유사합니다. `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


## Cloudant 데이터베이스에서 읽기
{: #openwhisk_catalog_cloudant_read}

조치를 사용하여 `testdb`라고 하는 Cloudant 데이터베이스의 문서를 페치할 수 있습니다. 이 데이터베이스가 Cloudant 계정에 존재하는지 확인하십시오.

- 이전에 작성한 패키지 바인딩의 `read` 조치를 사용하여 문서를 페치하십시오. 반드시 `/_/myCloudant`를 패키지 이름으로 대체하십시오. 

  ```
  wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```json
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```

## 조치 시퀀스 및 변경 트리거를 사용하여 Cloudant 데이터베이스의 문서 처리
{: #openwhisk_catalog_cloudant_read_change notoc}

규칙의 조치 시퀀스를 사용하여 Cloudant 변경 이벤트와 연관된 문서를 페치하고 처리할 수 있습니다. 

문서를 처리하는 조치의 샘플 코드: 
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```

Cloudant에서 문서를 처리하기 위한 조치를 작성하십시오. 
```
wsk action create myAction myAction.js
```
{: pre}

데이터베이스에서 문서를 읽기 위해 Cloudant 패키지에서 `read` 조치를 사용할 수 있습니다.
`read` 조치는 조치 시퀀스를 작성하기 위한 `myAction`으로 작성될 수 있습니다. 
```
wsk action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

`sequenceAction` 조치는 새 Cloudant 트리거 이벤트에서 조치를 활성화하는 규칙에서 사용될 수 있습니다. 
```
wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**참고**: `includeDoc` 매개변수 지원에 사용되던 Cloudant `changes` 트리거는 더 이상 지원되지 않습니다. `includeDoc`로 이전에 작성된 트리거를 재작성할 수 있습니다. 트리거를 재작성하려면 다음 단계를 수행하십시오.  
  ```
  wsk trigger delete myCloudantTrigger
  ```
  {: pre}
  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
  ```
  {: pre}

  예제를 사용하여 변경된 문서를 읽고 기존 조치를 호출하는 조치 시퀀스를 작성할 수 있습니다. 반드시 더 이상 유효하지 않은 규칙을 사용 중지하고 조치 시퀀스 패턴을 사용하여 새로 규칙을 작성하십시오. 

