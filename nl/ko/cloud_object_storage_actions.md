---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Object Storage 패키지
{: #cloud_object_storage_actions}

{{site.data.keyword.cos_full_notm}} 패키지는 {{site.data.keyword.cos_full}} 인스턴스와의 상호작용을 위한 액션 세트를 제공합니다. 이러한 액션을 사용하면 {{site.data.keyword.cos_short}} 인스턴스에 존재하는 버킷에서 읽기, 쓰기 및 삭제가 허용됩니다.
{: shortdesc}

{{site.data.keyword.cos_short}} 패키지에는 다음 액션이 포함되어 있습니다. 

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/cloud-object-storage` |패키지 | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key |{{site.data.keyword.cos_full_notm}} 인스턴스 관련 작업을 수행합니다. |
| `/cloud-object-storage/object-write` |액션 | bucket, key, body |버킷에 오브젝트를 씁니다. |
| `/cloud-object-storage/object-read` |액션 | bucket, key |버킷에서 오브젝트를 읽습니다. |
| `/cloud-object-storage/object-delete` |액션 | bucket, key |버킷에서 오브젝트를 삭제합니다. |
| `/cloud-object-storage/bucket-cors-put` |액션 | bucket, corsConfig |버킷에 CORS 구성을 지정합니다. |
| `/cloud-object-storage/bucket-cors-get` |액션 | bucket |버킷의 CORS 구성을 읽습니다. |
| `/cloud-object-storage/bucket-cors-delete` |액션 | bucket |버킷의 CORS 구성을 삭제합니다. |
| `/cloud-object-storage/client-get-signed-url` |액션 | bucket, key, operation |버킷에서 오브젝트의 쓰기, 읽기 및 삭제를 제한하기 위해 서명된 URL을 가져옵니다. |

## {{site.data.keyword.cos_full_notm}} 서비스 인스턴스 작성
{: #cloud_object_storage_service_instance}

패키지를 설치하려면 우선 {{site.data.keyword.cos_short}}의 인스턴스를 요청하고 최소한 하나의 버킷을 작성해야 합니다. 

1. [{{site.data.keyword.cos_full_notm}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](/docs/services/cloud-object-storage/basics/order-storage.html#creating-a-new-service-instance). 

2. {{site.data.keyword.cos_short}} 서비스 인스턴스에 대한 [HMAC 서비스 신임 정보 세트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials). **인라인 구성 매개변수 추가(선택사항)** 필드에서 `{"HMAC":true}`를 추가하십시오. 

3. [최소한 하나의 버킷 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 작성하십시오](/docs/services/cloud-object-storage/getting-started.html#create-buckets).

## {{site.data.keyword.cos_short}} 패키지 설치
{: #cloud_object_storage_installation}

{{site.data.keyword.cos_short}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI 또는 UI를 사용하여 네임스페이스에 {{site.data.keyword.cos_short}} 패키지를 설치하십시오.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #cloud_object_storage_cli}

시작하기 전에 다음을 수행하십시오. 
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](bluemix_cli.html#cloudfunctions_cli).
  2. `wskdeploy` 명령을 설치하십시오. [Apache OpenWhisk 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-wskdeploy#building-the-project)를 참조하십시오. 

{{site.data.keyword.cos_short}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.cos_short}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `runtimes/nodejs` 또는 `runtimes/python` 디렉토리로 이동하십시오. {{site.data.keyword.cos_short}} 패키지의 액션은 선택한 런타임에 배치됩니다.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 패키지를 배치하십시오. 나중에 다른 런타임에서 이 패키지의 액션을 실행하기로 결정한 경우에는 이전 단계와 이 단계를 반복하여 패키지를 재배치할 수 있습니다.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

4. `cloud-object-storage` 패키지가 패키지 목록에 추가되었는지 확인하십시오.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    출력:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage-pkg private
    ```
    {: screen}

5. 작성한 {{site.data.keyword.cos_short}} 인스턴스의 신임 정보를 패키지에 바인드하십시오.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage-pkg
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage-pkg'.
    ```
    {: screen}

6. 패키지가 {{site.data.keyword.cos_short}} 서비스 인스턴스 신임 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage-pkg, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI에서 설치
{: #cloud_object_storage_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/create)로 이동하십시오. 

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록에서 {{site.data.keyword.cos_short}} 패키지가 설치될 네임스페이스를 선택하십시오. 네임스페이스는 결합된 조직 및 영역 이름으로 구성됩니다. 

3. **패키지 설치**를 클릭하십시오. 

4. **IBM Cloud Object Storage** 패키지 그룹을 클릭한 후에 **IBM Cloud Object Storage** 패키지를 클릭하십시오. 

5. 사용 가능한 런타임 섹션의 드롭 다운 목록에서 NodeJS 또는 Python을 선택한 후에 **설치**를 클릭하십시오. 

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **cloud-object-storage**인 새 패키지를 검색할 수 있습니다. 

7. **cloud-object-storage** 패키지의 액션을 사용하려면 서비스 신임 정보를 액션에 바인드해야 합니다. 
  * 서비스 신임 정보를 패키지의 모든 액션에 바인드하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오. 
  * 서비스 신임 정보를 개별 액션에 바인드하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다. 
    1. 사용할 **cloud-object-storage** 패키지의 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다. 
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오. 
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 신임 정보 JSON 오브젝트를 붙여넣으십시오. 

## {{site.data.keyword.cos_short}} 버킷에 쓰기
{: #cloud_object_storage_write}

`object-write` 액션을 사용하여 {{site.data.keyword.cos_short}} 버킷에 오브젝트를 쓸 수 있습니다.
{: shortdesc}

**참고**: 다음 단계에서는 `testbucket`이라는 이름이 예로서 사용됩니다. {{site.data.keyword.cos_full_notm}}의 버킷이 글로벌하게 고유해야 하므로 `testbucket`은 고유한 버킷 이름으로 대체되어야 합니다. 

### CLI에서 버킷에 쓰기
{: #write_bucket_cli}

`object-write` 액션을 사용하여 버킷에 오브젝트를 씁니다. 
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

출력 예:
```
{
  "body": {
      "ETag": "\"32cef9b573122b1cf8fd9aec5fdb898c\""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### UI에서 버킷에 쓰기
{: #write_bucket_ui}

1. [{{site.data.keyword.openwhisk_short}} 콘솔의 액션 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/actions)로 이동하십시오. 

2. `cloud-object-storage` 패키지 아래에서 **object-write** 액션을 클릭하십시오. 

3. 코드 분할창에서 **입력 변경**을 클릭하십시오. 

4. 오브젝트 키로서 버킷, 키 및 본문이 포함된 JSON 오브젝트를 입력하십시오.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. **저장**을 클릭하십시오. 

6. **호출**을 클릭하십시오. 

7. 출력이 다음과 유사하게 나타나는지 확인하십시오.
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}

## {{site.data.keyword.cos_short}} 버킷에서 읽기
{: #cloud_object_storage_read}

`object-read` 액션을 사용하여 {{site.data.keyword.cos_short}} 버킷의 오브젝트에서 읽을 수 있습니다.
{: shortdesc}

**참고**: 다음 단계에서는 `testbucket`이라는 이름이 예로서 사용됩니다. {{site.data.keyword.cos_full_notm}}의 버킷이 글로벌하게 고유해야 하므로 `testbucket`은 고유한 버킷 이름으로 대체되어야 합니다. 

### CLI에서 버킷에서 읽기
{: #read_bucket_cli}

`object-read` 액션을 사용하여 버킷의 오브젝트에서 읽습니다. 
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

출력 예:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

### UI에서 버킷에서 읽기
{: #read_bucket_ui}

1. [{{site.data.keyword.openwhisk_short}} 콘솔의 액션 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/actions)로 이동하십시오. 

2. `cloud-object-storage` 패키지 아래에서 **object-read** 액션을 클릭하십시오. 

3. 코드 분할창에서 **입력 변경**을 클릭하십시오. 

4. 오브젝트 키로서 버킷 및 키가 포함된 JSON 오브젝트를 입력하십시오.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. **저장**을 클릭하십시오. 

6. **호출**을 클릭하십시오. 

7. 출력이 다음과 유사하게 나타나는지 확인하십시오.    
    ```
    object-write 3855 ms 6/7/2018, 14:56:09
    Activation ID: bb6eba3cf69wereaeba3cf691a1aad8
    Results:
    {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\""
      }
    }
    Logs:
    []
    ```
    {: screen}
