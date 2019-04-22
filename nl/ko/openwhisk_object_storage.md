---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (실험) Object Storage 이벤트 소스
{: #cloud_object_storage}

`/whisk.system/cos-experimental` 패키지는 안정적이지 않을 수 있고, 이전 버전과 호환되지 않는 방법으로 자주 변경할 수 있으며, 짧은 통지로 중단될 수도 있습니다. 이 패키지는 프로덕션 환경에서 사용하도록 권장되지 않습니다. 이 실험용 패키지는 현재 미국 남부 지역에서만 사용할 수 있습니다.
{: important}

이 예에서는 다음 방법에 대해 학습합니다. 
* {{site.data.keyword.cos_full}} 인스턴스에 대한 [변경사항을 청취](#listening_to_cos_bucket_changes)합니다.
* 이러한 변경사항에 응답하기 위해 [트리거를 작성](#creating_a_trigger_cos)합니다.
* 변경사항을 검색하고 처리하기 위한 [액션을 작성](#creating_action_to_process_object)합니다.
* 액션을 변경 트리거에 연관시키기 위해 [규칙을 작성](#associating_action_with_change_trigger)합니다. 
<br>

**샘플 유스 케이스:** `/whisk.system/cos-experimental` 패키지를 사용하여 {{site.data.keyword.cos_full_notm}} 버킷에 저장된 GPS 상세 주소 데이터에 대한 변경사항을 청취할 수 있습니다. 그런 다음 변경사항이 발생하면 사용자가 해당 GPS 애플리케이션에 대한 최신 상세 주소 데이터에 액세스할 수 있도록 GPS 맵의 자동 재생성을 트리거할 수 있습니다. 

## IBM Cloud Object Storage 정보
{: #cloud_object_storage_info}

**시작하기 전에:** {{site.data.keyword.cos_full_notm}}에 대해 알아보려면 [Object Storage 정보](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage)를 참조하십시오. {{site.data.keyword.cos_full_notm}} 인스턴스 설정 방법에 대한 자세한 정보는 [인스턴스 {{site.data.keyword.cos_short}} 프로비저닝](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage)의 내용을 참조하십시오.

## IBM Cloud Object Storage 버킷에 대한 변경사항 청취
{: #listening_to_cos_bucket_changes}

{{site.data.keyword.openwhisk}}를 사용하여 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) 버킷에 대한 변경사항을 청취하고 액션을 사용하여 버킷에서 하나 이상의 오브젝트를 처리할 수 있습니다.  

`/whisk.system/cos-experimental` 패키지를 사용하면 {{site.data.keyword.cos_full_notm}} 인스턴스로부터 이벤트를 구성하고 다음 피드를 포함시킬 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` |패키지 | apikey, auth_endpoint, bucket, endpoint, interval | `changes` 피드 액션이 포함된 패키지입니다. |
| `/whisk.system/cos-experimental/changes` |피드 | apikey, auth_endpoint, bucket, endpoint, interval | {{site.data.keyword.cos_full_notm}} 버킷에 대한 변경사항에 대해 트리거 이벤트를 실행합니다. |
{: shortdesc}

`changes` 피드를 사용하면 {{site.data.keyword.cos_full_notm}} 이벤트 소스 서비스를 구성하여 {{site.data.keyword.cos_full_notm}} 인스턴스에서 버킷에 대한 모든 변경사항에 대해 트리거를 실행할 수 있습니다. 

이 예에서 사용되는 매개변수는 다음과 같습니다. 

**apikey**: _(패키지에 바인딩되지 않는 한 필수임)_. `apikey` 매개변수는 {{site.data.keyword.cos_full_notm}} 인스턴스에 대한 IAM API 키입니다. 일반적으로 이 값은 패키지에 바인딩됩니다. 그러나 `apikey` 값이 `changes` 피드 액션을 사용할 때 지정되는 경우, 바운드 인증 정보의 API 키 대신 지정된 값이 인증 정보에 사용됩니다.

**auth_endpoint**: _(선택사항)_. `auth_endpoint` 매개변수는 `apikey`로부터 토큰을 생성하기 위해 {{site.data.keyword.cos_full_notm}}에서 사용되는 권한 엔드포인트입니다. 기본 엔드포인트는 {{site.data.keyword.Bluemix}} 엔드포인트입니다. 

**bucket**: _(필수)_. `bucket` 매개변수는 {{site.data.keyword.cos_full_notm}} 버킷의 이름입니다. 

**endpoint**: _(필수)_. `endpoint` 매개변수는 사용자의 {{site.data.keyword.cos_full_notm}} 인스턴스에 연결하는 데 사용되는 {{site.data.keyword.cos_full_notm}} 엔드포인트입니다. [{{site.data.keyword.cos_full_notm}} 문서](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints)에서 사용자 엔드포인트를 찾을 수 있습니다.

**interval**: _(선택사항)_. `interval` 매개변수는 버킷 폴링 간격(전체 분 단위)입니다. `interval` 값은 최소한 1분이어야 하며 기본적으로 1분으로 설정됩니다. 

## 변경 피드에 응답하도록 트리거 작성
{: #creating_a_trigger_cos}

트리거를 작성할 경우 `cos-experimental` 패키지에 직접 인증 정보를 바인딩함으로써 {{site.data.keyword.cos_full_notm}} 인증 정보를 `changes` 피드 액션에 전달하지 않도록 할 수 있습니다.
 {: shortdesc}
 
 1. 먼저 인증 정보를 포함시키기 위해 수정할 수 있는 패키지 바인딩을 작성하십시오. 다음은 네임스페이스에서 패키지 바인딩 `myCosPkg`를 작성합니다. 
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. {{site.data.keyword.cos_short}} 인증 정보를 패키지에 바인딩하십시오.
 {{site.data.keyword.cos_short}} 인증 정보를 패키지에 바인딩할 경우 `apikey` 값이 패키지에 바인딩되므로 `changes` 피드 액션이 호출될 때 `apikey` 값을 지정할 필요가 없습니다.  
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 작성한 패키지 바인딩에서 `changes` 피드와 함께 이름이 `myCosTrigger`인 트리거를 작성하십시오. 버킷 이름 및 {{site.data.keyword.cos_short}} 엔드포인트 매개변수 값을 사용하십시오. 
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
출력 예:
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. 활성화에 대한 폴링을 시작하여 발생한 상황을 명확하게 보여주십시오.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. 변경 피드를 관찰하기 위한 액션을 작성하십시오. 예를 들어, 다음 JavaScript 코드가 포함된 `showCosChange`라는 액션을 작성할 수 있습니다.
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
 6. `showCosChange` 액션을 `myCosTrigger` 트리거에 연결하려면 규칙을 작성하십시오. 
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. {{site.data.keyword.cos_short}} 대시보드에서 기존 버킷 오브젝트를 수정하거나 하나를 작성하십시오. 버킷에 오브젝트를 추가하는 방법을 알아보려면 [버킷에 일부 오브젝트 추가](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects)의 내용을 참조하십시오.
 
 8. 각 버킷 오브젝트 변경사항의 경우, `myCosTrigger` 트리거 및 `showCosChange` 액션에 대한 새 활성화를 관찰하십시오. 이러한 활성화는 구성된 버킷 폴링 간격 내에 표시됩니다. 

새 활성화를 관찰할 수 없는 경우, `apikey`, `endpoint` 및 `bucket` 매개변수 값이 올바른지 확인하십시오. 
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### 트리거 이벤트의 데이터 구조
{: #data_structure_trigger_event}

생성된 이벤트의 컨텐츠에는 다음 매개변수가 있습니다.

  - `file`: 파일 또는 오브젝트 메타데이터.
  - `status`: 발견된 변경사항. 이 값은 `added`, `modified` 또는 `deleted` 중 하나입니다.
  - `bucket`: {{site.data.keyword.cos_short}} 버킷의 이름.
  - `endpoint`: {{site.data.keyword.cos_short}} 인스턴스에 연결하는 데 사용되는 {{site.data.keyword.cos_short}} 엔드포인트입니다.
  - `key`: 변경된 버킷 오브젝트의 ID. 이 값은 `file.Key`와 동일하지만 트리거 이벤트 JSON의 맨 위에서 사용 가능합니다.

버킷 변경 트리거 이벤트의 예제 JSON 표시:
```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## 변경된 오브젝트를 처리하기 위한 액션 작성
{: #creating_action_to_process_object}

해당 오브젝트를 검색 및 처리하는 단일 액션을 작성할 수 있습니다. 또는 오브젝트 및 이 오브젝트를 처리하기 위한 다른 액션을 검색하기 위해 하나의 액션을 사용하는 시퀀스를 작성할 수 있습니다. 

### 오브젝트를 검색 및 처리하기 위한 액션 작성
{: #creating_action_to_retrieve_object}

이 샘플 액션 코드는 버킷 변경 알림 문서를 검색 및 처리합니다. 수동 액션 호출 동안 `ibmcloud fn service bind` 명령으로 액션에 바인딩되어야 하는 {{site.data.keyword.cos_short}}에서 이러한 값을 얻어야 하는 트리거에 의해 이 액션이 호출되는 경우, `apikey` 및 `serviceInstanceId` 매개변수를 직접 액션에 전달할 수 있습니다. 

예제 코드:

```javascript
const COS = require('ibm-cos-sdk')

function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

  const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

  return new Promise(function(resolve, reject) {
    client.getObject({ Bucket: bucket, Key: file }, (err, results) => {
      if (err != null) {
        console.log(err)
        reject({ err: err })
      } else {
        console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
    })
  });
}
```
{: codeblock}

이 액션은 `ibm-cos-sdk` npm 패키지를 사용하므로 액션을 [Node.js module](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action) 또는 [single bundle](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action) 중 하나로 패키징해야 합니다.

이 액션을 .zip 파일로 패키징하고 나면 {{site.data.keyword.cos_short}}에서 오브젝트를 검색 및 처리하기 위한 액션을 작성하십시오. 

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

{{site.data.keyword.cos_short}} 인증 정보를 이 액션에 [바인딩](#cos_binding_credentials_to_action)하십시오. 그런 다음 트리거가 실행될 때 이 액션을 호출하도록 [규칙을 작성](#associating_action_with_change_trigger)하십시오. 

### 오브젝트를 검색 및 처리하기 위한 액션 시퀀스 작성

액션에서 오브젝트 검색 코드를 포함하는 대신 [수동으로 설치](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation)되어야 하는 {{site.data.keyword.cos_short}} 패키지로부터 `object-read` 액션을 사용할 수 있습니다. 액션 코드는 `object-read`에서 리턴된 결과를 처리하기만 하면 됩니다.

버켓 오브젝트만 처리하는 액션의 예제 코드는 다음과 같습니다.
```javascript
  function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. {{site.data.keyword.cos_short}}에서 오브젝트만 처리하기 위한 액션을 작성하십시오.
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. {{site.data.keyword.cos_short}} 인증 정보를 `cos-experimental` 패키지 바인딩에 바인딩하십시오. 
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. `object-read` 액션은 액션 시퀀스를 작성하기 위한 `myCosProcessObjectAction`으로 구성될 수 있습니다.
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

`object-read` 액션 외에도 설치 가능한 {{site.data.keyword.cos_short}} 패키지에 포함된 다른 액션을 사용할 수 있습니다. 

{{site.data.keyword.cos_short}} 인증 정보를 이 액션에 [바인딩](#cos_binding_credentials_to_action)하십시오. 그런 다음 트리거가 실행될 때 이 액션을 호출하도록 [규칙을 작성](#associating_action_with_change_trigger)하십시오. 

 ## 엑션에 인증 정보 바인딩
 {: #cos_binding_credentials_to_action}
 
 {{site.data.keyword.cos_short}} 인증 정보를 다음 명령으로 액션에 바인딩함으로써 호출 동안 민감한 인증 정보의 전달을 피할 수 있습니다. 
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## 변경 트리거와 액션을 연관시키기 위한 규칙 작성
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

[rule](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use)에서 액션 또는 액션 시퀀스를 사용하여 {{site.data.keyword.cos_short}} 변경 이벤트와 연관된 오브젝트를 페치 및 처리할 수 있습니다. 

새 {{site.data.keyword.cos_short}} 트리거 이벤트에서 `MyCosAction` 액션을 활성화하는 규칙을 작성하십시오. 
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}



