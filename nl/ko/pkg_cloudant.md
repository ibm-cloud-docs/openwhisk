---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

사전 설치된 `/whisk.system/cloudant` 패키지를 사용하면 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 데이터베이스 관련 작업을 수행할 수 있습니다. 이 패키지를 사용하는 데 서비스 바인딩이 필요하지 않습니다.
{: shortdesc}


## 사용 가능한 엔티티
{: #cloudant_available}
다음 표는 `whisk.system/cloudant` 패키지에서 사용 가능한 엔티티의 선택사항을 보여줍니다. `whisk.system/cloudant` 패키지를 사용하여 문서를 읽거나, 쓰거나, 업데이트하거나, 삭제할 수 있습니다. 또한 `changes` 피드를 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 대한 변경사항을 청취할 수 있습니다.
{: shortdesc}

`/whisk.system/cloudant` 패키지에서 사용 가능한 엔티티의 전체 목록을 확인하려면 `ibmcloud fn package get /whisk.system/cloudant`를 실행하십시오.
{: note}

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |패키지 |`dbname`, `host`, `username`, `password` | Cloudant 데이터베이스 관련 작업을 수행합니다. |
|`/whisk.system/cloudant/read` |액션 |`dbname`, `id` | 데이터베이스에서 문서를 읽습니다. |
|`/whisk.system/cloudant/write` |액션 |`dbname`, `overwrite`, `doc` | 데이터베이스에 문서를 작성합니다. |
|`/whisk.system/cloudant/update-document` |액션 |`dbname`, `doc` |데이터베이스에서 문서를 업데이트합니다. |
|`/whisk.system/cloudant/changes` |피드 |`dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | 데이터베이스 변경 시 트리거 이벤트를 실행합니다. |

`includeDoc` 매개변수는 더 이상 `/whisk.system/cloudant/changes` 피드에서 사용하도록 지원되지 않습니다. 이 매개변수를 사용하는 트리거를 작성한 경우 `includeDoc` 매개변수 없이 해당 트리거를 재작성해야 합니다.
{: deprecated}

## `/whisk.system/cloudant` 패키지를 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 바인딩하십시오.
{{site.data.keyword.cloud_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우, {{site.data.keyword.openwhisk}} CLI 플러그인을 사용하여 서비스를 액션 또는 패키지에 바인딩할 수 있습니다.
{: #cloudant_db}

**시작하기 전에**
{{site.data.keyword.cloudant_short_notm}}의 인스턴스가 있어야 합니다. 인스턴스를 작성하려면 [{{site.data.keyword.cloudant_short_notm}} 시작하기](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)를 참조하십시오.

1. {{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 `/whisk.system/cloudant` 패키지 바인딩을 작성하십시오. 이 예에서는 패키지 이름이 `myCloudant`입니다.

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 패키지 바인딩이 존재하는지 확인하십시오.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **출력 예**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. 액션 또는 패키지에 바인딩할 서비스 인스턴스의 이름을 가져오십시오.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **출력 예**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. 이전 단계에서 가져온 서비스 인스턴스에 대해 정의된 인증 정보의 이름을 가져오십시오.

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **출력 예**

    ```
Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. 1단계에서 작성한 패키지에 서비스를 바인딩하십시오.

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. 인증 정보가 바인딩되었는지 확인하십시오.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **출력 예**

    ```
ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "serviceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

이 예에서는 {{site.data.keyword.cloudant_short_notm}} 서비스에 대한 인증 정보가 `__bx_creds`라는 매개변수에 속합니다.

## {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서 관련 작업
{: #cloudant_read}

액션을 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서를 읽고, 작성하고, 업데이트하고, 삭제할 수 있습니다.
{: shortdesc}

### 문서 읽기
`/whisk.system/cloudant/read` 액션을 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서를 읽을 수 있습니다.

**시작하기 전에**
{{site.data.keyword.cloudant_short_notm}} 데이터베이스에 문서가 없는 경우 [{{site.data.keyword.cloudant_short_notm}} 대시보드를 사용하여 작성할 수 있습니다. 대시보드의 URL은 `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`입니다.

`read` 액션을 사용하여 문서를 페치하십시오. `/_/myCloudant`를 패키지 이름으로 대체하고, `<database_name>`을 데이터베이스 이름으로 대체하고, `<document_id>`를 파일 ID로 대체하십시오. 액션을 호출하여 문서 페치를 테스트하십시오.

**명령 구문**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**`test` 데이터베이스의 읽기 조치 예제**
파일 읽기를 테스트하는 조치를 호출하십시오. 이 예제는 비어 있는 파일이며 `id`가 `9f86f4955e7a38ab0169462e6ac0f476`인 파일을 읽어들입니다.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**출력 예**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 문서 쓰기
{: #cloudant_write}

액션을 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서를 작성 또는 업데이트할 수 있습니다.
{: shortdesc}

**시작하기 전에**
  {{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 `/whisk.system/cloudant` [패키지 바인딩](#cloudant_db)을 작성하십시오.

1. 작성한 패키지 바인딩에서 `write` 액션을 사용하여 문서를 저장하십시오. `/_/myCloudant`를 패키지 이름으로 대체하고, `<database_name>`을 데이터베이스 이름으로 대체하고, `<document_id>`를 문서 ID로 대체하고, `<test_name>`을 이름으로 대체하십시오.

  **명령 구문**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **`test` 데이터베이스에 대한 쓰기 조치 예제**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **출력 예**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. 문서가 {{site.data.keyword.cloudant_short_notm}} 대시보드에 있는지 확인하십시오. `test` 데이터베이스에 대한 대시보드 URL의 형식은 다음과 같습니다. `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`

  **{{site.data.keyword.cloudant_short_notm}} 대시보드의 문서 예**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### 문서 업데이트
`/update-document` 액션을 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서를 업데이트할 수 있습니다.
{: short desc}

**시작하기 전에**
{{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 `/whisk.system/cloudant` [패키지 바인딩](#cloudant_db)을 작성하십시오.

다음 예제는 [{{site.data.keyword.cloudant_short_notm}} 데이터베이스에 문서 작성](#cloudant_write) 절에서 작성한 문서를 업데이트합니다.
{: note}

`<test>`를 데이터베이스 이름으로 대체하고 `--param doc` 플래그를 데이터베이스에서 업데이트할 문서의 `id` 및 컨텐츠로 대체하여 데이터베이스의 문서를 업데이트할 수 있습니다.


1. 다음 명령을 실행하여 `test` 데이터베이스에서 문서를 업데이트있습니다. 이 예제는 `color` 문서에 `shade` 값을 추가합니다. 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **출력**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. 업데이트를 보려면 문서를 다시 페치하십시오.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **문서 예**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## 필터 함수를 사용하여 트리거 작성
{: #cloudant_trigger}

`changes` 피드를 사용하여 {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 변경 시마다 트리거를 실행하도록 서비스를 구성할 수 있습니다.

**시작하기 전에**
  {{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 `/whisk.system/cloudant` [패키지 바인딩](#cloudant_db)을 작성하십시오.

이 예제에서 사용되는 매개변수는 다음과 같습니다.

|매개변수 |설명 |
| --- | --- |
|`dbname` |(필수) {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 이름입니다. |
|`iamApiKey` |Cloudant 데이터베이스의 IAM API 키입니다. 지정된 경우 사용자 이름 및 비밀번호 대신 인증 정보로 사용됩니다. |
|`iamUrl` |`iamApiKey`가 지정될 때 사용되는 IAM 토큰 서비스 URL입니다. 기본값은 `https://iam.cloud.ibm.com/identity/token`입니다. | 
|`maxTriggers` |이 한계에 도달하면 트리거 실행을 중지합니다. 기본값은 무제한입니다. |
|`filter` |(선택사항) 디자인 문서에 정의된 필터 함수입니다. |
|`query_params` | (선택사항) 필터 기능에 필요한 추가적인 조회 매개변수입니다. |
|`includeDoc` | (더 이상 사용되지 않음) `includeDoc` 매개변수는 더 이상 `/whisk.system/cloudant/changes` 피드에서 사용하도록 지원되지 않습니다. |

</br>

1. 이전에 작성한 패키지 바인딩에서 `changes` 피드로 트리거 `cloudantTrigger`를 작성하십시오. 상태가 `new`인 경우 문서가 추가되거나 수정될 때 트리거를 실행하려면 `filter` 및 `query_params` 함수를 포함하십시오.

  `/_/myCloudant`를 패키지의 이름으로 대체하십시오. 이 예에서는 `test` 데이터베이스를 사용합니다.
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **출력 예**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. 다음 JavaScript 코드를 `cloudantChange.js`로 저장하십시오.

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. 변경 피드를 관찰하기 위해 사용할 수 있는 `cloudantChange`라는 조치를 작성하십시오. `<file_path>`를 컴퓨터에 있는 `cloudantChange.js` 파일의 경로로 대체하십시오.

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. `cloudantRule`로 이름 지정된 규칙을 작성하여 `cloudantChange` 조치를 이전에 작성한 `cloudantTrigger`에 연결하십시오.

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. 다른 터미널 창에서 활성화가 발생할 때 확인할 수 있도록 폴링을 시작하십시오.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. {{site.data.keyword.cloudant_short_notm}} 대시보드에서 기존 문서를 수정하거나 하나를 작성하십시오.

7. 각 문서 변경사항에 대한 `cloudantTrigger` 트리거의 활성화를 관찰하십시오.

**`cloudantTrigger`의 활성화 예**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### 트리거 활성화의 데이터 구조
{: #cloudant_struct}

생성된 이벤트의 컨텐츠에는 다음 매개변수가 있습니다.

|매개변수 |설명 |
| --- | --- |
|`id` |문서 ID입니다. |
|`seq` |{{site.data.keyword.cloudant_short_notm}}에서 생성된 시퀀스 ID입니다. |
|`changes` |오브젝트의 배열이며, 각각에는 문서의 개정 ID를 포함하는 `rev` 필드가 있습니다. |

**트리거 활성화의 JSON 표시**

```json
{
    "dbname": "test",
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
{: #cloudant_filter}

트리거를 실행하는 불필요한 변경 이벤트를 피하기 위한 필터 함수를 정의할 수 있습니다.

**시작하기 전에**
{{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 `/whisk.system/cloudant` [패키지 바인딩](#cloudant_db)을 작성하십시오.

필터 함수를 작성하기 위해 액션을 사용할 수 있습니다.

1. `design_doc.json` 파일에 다음 JSON 필터를 저장하십시오.

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

2. 다음 필터 함수로 데이터베이스에서 디자인 문서를 작성하십시오. `<database_name>`을 데이터베이스의 이름으로 대체하고 `<file_path>`를 `design_doc.json`의 파일 경로로 대체하십시오. `write` 액션을 호출하여 디자인 문서 작성을 테스트하십시오.

**명령 구문**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**`test` 데이터베이스에 `design_doc.json` 파일을 작성하는 명령 예제**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**출력 예**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


{{site.data.keyword.cloudant_short_notm}} 디자인 문서에 대한 자세한 정보는 [디자인 문서](/docs/services/Cloudant?topic=cloudant-design-documents)를 참조하십시오.

## 조치 시퀀스를 사용하여 개별 문서 처리
{: #cloudant_seq}

룰의 액션 시퀀스를 사용하여 {{site.data.keyword.cloudant_short_notm}} 변경 이벤트와 연관된 문서를 페치하고 처리할 수 있습니다.

**시작하기 전에**
{{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 `/whisk.system/cloudant` [패키지 바인딩](#cloudant_db)을 작성하십시오. 

이 예에서는 [{{site.data.keyword.cloudant_short_notm}} 데이터베이스에 문서 쓰기](#cloudant_write) 섹션에서 작성한 문서를 업데이트합니다.
{: note}

### 개별 문서를 처리하기 위한 액션 작성

개별 문서에 대한 변경사항을 처리하는 액션을 작성하려면 다음 명령을 실행하십시오.
{: shortdesc}

1. 다음 코드를 `docChange.js`로 저장하십시오.

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. `docChange`로 이름 지정된 조치를 작성하여 이전에 작성한 `blue`로 이름 지정된 문서를 처리하십시오. `<file_path>`를 `docChange.js`의 파일 경로로 대체하십시오.

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **출력**
  ```
  ok: created action docChange
  ```
  {: screen}

### `read` 조치가 포함된 시퀀스 작성 

`read` 액션은 액션 시퀀스를 작성하기 위한 `docChange` 액션으로 구성될 수 있습니다.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **출력**
  ```
  ok: created action docSequence
  ```
  {: screen}

### `changes` 피드 관련 트리거 작성

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### 시퀀스와 트리거를 연관시키기 위한 규칙 작성

`docSequence` 액션은 새 {{site.data.keyword.cloudant_short_notm}} 트리거 이벤트에서 액션을 활성화하는 룰에서 사용될 수 있습니다.

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **출력**
  ```
  ok: created rule docRule
  ```

  **활성화 예**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### 시퀀스 테스트

1. 이전에 작성한 `blue` 파일을 변경하여 `docSequence`를 테스트하십시오. 이 예제에서는 `shade` 값이 `indigo`로 변경되었습니다.

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **활성화 예**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. `read` 조치를 호출하여 `shade` 값을 포함하도록 파일이 업데이트되었는지 확인하십시오. `<database>` 이름을 데이터베이스의 이름으로 대체하십시오.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **출력**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### 다음 단계
{{site.data.keyword.cloudant_short_notm}} 데이터베이스의 문서에 대한 변경사항을 청취하고 있으므로 [`/whisk.system/slack` 패키지](/docs/openwhisk?topic=cloud-functions-pkg_slack)를 사용하여 변경사항에 대한 Slack 알림을 트리거할 수 있습니다.


