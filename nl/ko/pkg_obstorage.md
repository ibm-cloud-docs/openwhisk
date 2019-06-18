---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: object storage, bucket, package

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
{:gif: data-image-type='gif'}

# Object Storage
{: #pkg_obstorage}

{{site.data.keyword.cos_full}} 인스턴스와 통합하여 {{site.data.keyword.openwhisk}} 앱의 기능을 확장할 수 있습니다. 

**시작하기 전에:** {{site.data.keyword.cos_full_notm}}에 대해 알아보려면 [Object Storage 정보](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api)를 참조하십시오. {{site.data.keyword.cos_full_notm}} 인스턴스 설정 방법에 대한 자세한 정보는 [인스턴스 {{site.data.keyword.cos_full_notm}} 프로비저닝](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision)의 내용을 참조하십시오.

## 패키지
{: #obstorage_packages}
|패키지 |가용성 |설명 |
| --- | --- | --- |
| [{{site.data.keyword.cos_full_notm}} 패키지](#pkg_obstorage)| 설치 가능 | {{site.data.keyword.cos_full_notm}} 인스턴스에서 읽고, 작성하고, 삭제합니다. |
| [(실험) {{site.data.keyword.cos_full_notm}} 이벤트 소스](#pkg_obstorage_ev) | 사전 설치됨(미국 남부 전용) | {{site.data.keyword.cos_full_notm}} 인스턴스에 대한 변경사항을 청취합니다. |

## IBM Cloud Object Storage 서비스 인스턴스 작성
{: #pkg_obstorage_service}

패키지를 사용하려면 우선 {{site.data.keyword.cos_full_notm}}의 인스턴스를 요청하고 최소한 하나의 버킷을 작성해야 합니다.

1. [{{site.data.keyword.cos_full_notm}} 서비스 인스턴스 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. {{site.data.keyword.cos_full_notm}} 서비스 인스턴스에 대한 [ HMAC 서비스 인증 정보 세트 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")를 작성하십시오.](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) **인라인 구성 매개변수 추가(선택사항)** 필드에서 `{"HMAC":true}`를 추가하십시오.

3. [최소한 하나의 버킷 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")을 작성하십시오](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## {{site.data.keyword.cos_full_notm}} 패키지를 사용하여 버킷에 대한 읽기 및 쓰기
{: #pkg_obstorage_install}

{{site.data.keyword.cos_full_notm}} 서비스 인스턴스가 보유되면 {{site.data.keyword.openwhisk}} CLI 또는 UI를 사용하여 네임스페이스에 {{site.data.keyword.cos_full_notm}} 패키지를 설치할 수 있습니다.
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI에서 설치
{: #pkg_obstorage_cli}

시작하기 전에 다음을 수행하십시오.

[{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).

{{site.data.keyword.cos_full_notm}} 패키지를 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.cos_full_notm}} 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `runtimes/nodejs` 또는 `runtimes/python` 디렉토리로 이동하십시오. {{site.data.keyword.cos_full_notm}} 패키지의 액션은 선택한 런타임에 배치됩니다.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 패키지를 배치하십시오. 이전 단계를 반복하여 다른 런타임에서 패키지를 재배치할 수 있습니다.
    ```
    ibmcloud fn deploy -m manifest.yaml
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
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. 작성한 {{site.data.keyword.cos_full_notm}} 인스턴스의 인증 정보를 패키지에 바인딩하십시오.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    출력 예:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. 패키지가 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스 인증 정보로 구성되었는지 확인하십시오.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    출력 예:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
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
{: #pkg_obstorage_ui}

1. {{site.data.keyword.openwhisk_short}} 콘솔에서 [작성 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/openwhisk/create)로 이동하십시오.

2. **Cloud Foundry 조직** 및 **Cloud Foundry 영역** 목록을 사용하여 {{site.data.keyword.cos_full_notm}} 패키지를 설치할 네임스페이스를 선택하십시오. 

3. **패키지 설치**를 클릭하십시오.

4. **IBM Cloud Object Storage** 패키지 그룹을 클릭한 후에 **IBM Cloud Object Storage** 패키지를 클릭하십시오.

5. **사용 가능한 런타임** 섹션에서 `Node.JS` 또는 `Python` 중 하나를 드롭 다운 목록에서 선택하십시오. 그런 다음 **Install**을 클릭하십시오.

6. 일단 패키지가 설치되면 사용자는 액션 페이지로 경로 재지정되며, 이름이 **cloud-object-storage**인 새 패키지를 검색할 수 있습니다.

7. **cloud-object-storage** 패키지의 액션을 사용하려면 서비스 인증 정보를 액션에 바인딩해야 합니다.
  * 서비스 인증 정보를 패키지의 모든 액션에 바인딩하려면 위에 나열된 CLI 지시사항의 5 - 6단계를 수행하십시오.
  * 서비스 인증 정보를 개별 액션에 바인딩하려면 UI에서 다음 단계를 완료하십시오. **참고**: 사용할 각 액션마다 다음 단계를 완료해야 합니다.
    1. 사용할 **cloud-object-storage** 패키지에서 액션을 클릭하십시오. 해당 액션에 대한 세부사항 페이지가 열립니다.
    2. 왼쪽 탐색 창에서 **매개변수** 섹션을 클릭하십시오.
    3. 새 **매개변수**를 입력하십시오. 키에 대해 `__bx_creds`를 입력하십시오. 값에 대해 이전에 작성한 서비스 인스턴스의 서비스 인증 정보 JSON 오브젝트를 붙여넣으십시오.


### 사용 가능한 액션
{: #pkg_obstorage_actions}

{{site.data.keyword.cos_full_notm}} 패키지에는 다음 액션이 포함되어 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/cloud-object-storage` |패키지 | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key |{{site.data.keyword.cos_full_notm}} 인스턴스 관련 작업을 수행합니다. |
| `/cloud-object-storage/object-write` |액션 | bucket, key, body, endpoint, ibmAuthEndpoint |버킷에 오브젝트를 씁니다. |
| `/cloud-object-storage/object-read` |액션 | bucket, key, endpoint, ibmAuthEndpoint |버킷에서 오브젝트를 읽습니다. |
| `/cloud-object-storage/object-delete` |액션 | bucket, key, endpoint, ibmAuthEndpoint |버킷에서 오브젝트를 삭제합니다. |
| `/cloud-object-storage/bucket-cors-put` |액션 | bucket, corsConfig, endpoint, ibmAuthEndpoint |버킷에 CORS 구성을 지정합니다. |
| `/cloud-object-storage/bucket-cors-get` |액션 | bucket, endpoint, ibmAuthEndpoint |버킷의 CORS 구성을 읽습니다. |
| `/cloud-object-storage/bucket-cors-delete` |액션 | bucket, endpoint, ibmAuthEndpoint |버킷의 CORS 구성을 삭제합니다. |
| `/cloud-object-storage/client-get-signed-url` |액션 | bucket, key, operation, expires, endpoint, ibmAuthEndpoint |버킷에서 오브젝트의 쓰기, 읽기 및 삭제를 제한하기 위해 서명된 URL을 가져옵니다. |


### 패키지 매개변수
{: #pkg_obstorage_pkgparams}

다음 매개변수는 패키지에 바인딩되어야 합니다. 이렇게 하면 모든 액션에 대해 자동으로 사용 가능하게 됩니다. 또한 액션 중 하나를 호출할 때 이러한 매개변수를 지정할 수도 있습니다.

**apikey**: `apikey` 매개변수는 {{site.data.keyword.cos_full_notm}} 인스턴스용 IAM API 키입니다.

**resource_instance_id**: `resource_instance_id` 매개변수는 {{site.data.keyword.cos_full_notm}} 인스턴스 ID입니다.

**cos_hmac_keys**: `cos_hmac_keys` 매개변수는 {{site.data.keyword.cos_full_notm}} 인스턴스 HMAC 인증 정보이며, 여기에는 `access_key_id` 및 `secret_access_key` 값이 포함됩니다.  이러한 인증 정보는 `client-get-signed-url` 액션에 의해 독점적으로 사용됩니다.  {{site.data.keyword.cos_full_notm}} 인스턴스에 대한 HMAC 인증 정보를 생성하는 방법에 대한 지시사항은 [HMAC 인증 정보 사용](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials)을 참조하십시오.

#### 액션 매개변수
{: #pkg_obstorage_actparams}

다음 매개변수는 개별 액션을 호출할 때 지정됩니다.  이러한 매개변수 전부가 모든 액션에 의해 지원되는 것은 아닙니다. 위의 표를 참조하여 어떤 매개변수가 어떤 액션에 의해 지원되는지 확인하십시오.

**bucket**: `bucket` 매개변수는 {{site.data.keyword.cos_full_notm}} 버킷의 이름입니다.

**endpoint**: `endpoint` 매개변수는 {{site.data.keyword.cos_full_notm}} 인스턴스에 연결하는 데 사용된 {{site.data.keyword.cos_full_notm}} 엔드포인트입니다. [{{site.data.keyword.cos_full_notm}} 문서](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)에서 사용자 엔드포인트를 찾을 수 있습니다.

**expires**: `expires` 매개변수는 사전 서명 URL 오퍼레이션을 만료하기까지의 시간(초)입니다.  기본 `expires` 값은 15분입니다.

**ibmAuthEndpoint**: `ibmAuthEndpoint` 매개변수는 `apikey`에서 토큰을 생성하기 위해 {site.data.keyword.cos_short}}에서 사용하는 IBM Cloud 권한 엔드포인트입니다. 기본 권한 엔드포인트는 모든 IBM Cloud 영역에 대해 작동해야 합니다.

**key**: `key` 매개변수는 버킷 오브젝트 키입니다.

**operation**: `operation` 매개변수는 호출을 위한 사전 서명 URL의 오퍼레이션입니다.

**corsConfig**: `corsConfig` 매개변수는 버킷의 CORS 구성입니다.


### {{site.data.keyword.cos_full_notm}} 버킷에 쓰기
{: #pkg_obstorage_write}

`object-write` 액션을 사용하여 {{site.data.keyword.cos_full_notm}} 버킷에 오브젝트를 쓸 수 있습니다.
{: shortdesc}

**참고**: 다음 단계에서는 `testbucket`이라는 이름이 예로서 사용됩니다. {{site.data.keyword.cos_full_notm}}의 버킷이 글로벌하게 고유해야 하므로 `testbucket`은 고유한 버킷 이름으로 대체되어야 합니다.

#### CLI에서 버킷에 쓰기
{: #pkg_obstorage_write_cli}

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
{: #pkg_obstorage_write_ui}

1. [{{site.data.keyword.openwhisk_short}} 콘솔의 액션 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")로 이동하십시오](https://cloud.ibm.com/openwhisk/actions).

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

### {{site.data.keyword.cos_full_notm}} 버킷에서 읽기
{: #pkg_obstorage_read}

`object-read` 액션을 사용하여 {{site.data.keyword.cos_full_notm}} 버킷의 오브젝트에서 읽을 수 있습니다.
{: shortdesc}

**참고**: 다음 단계에서는 `testbucket`이라는 이름이 예로서 사용됩니다. {{site.data.keyword.cos_full_notm}}의 버킷이 글로벌하게 고유해야 하므로 `testbucket`은 고유한 버킷 이름으로 대체되어야 합니다.

#### CLI에서 버킷에서 읽기
{: #pkg_obstorage_read_cli}

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

#### UI에서 버킷에서 읽기
{: #pkg_obstorage_read_ui}

1. [{{site.data.keyword.openwhisk_short}} 콘솔의 액션 페이지 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")로 이동하십시오](https://cloud.ibm.com/openwhisk/actions).

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


## (실험) Object Storage 이벤트 소스를 사용하여 버킷에 대한 변경사항 청취
{: #pkg_obstorage_ev}

`/whisk.system/cos-experimental` 패키지는 안정적이지 않을 수 있고, 이전 버전과 호환되지 않는 방법으로 자주 변경할 수 있으며, 짧은 통지로 중단될 수도 있습니다. 이 패키지는 프로덕션 환경에서 사용하도록 권장되지 않습니다. 이 실험용 패키지는 현재 미국 남부 지역에서만 사용할 수 있습니다.
{: important}

{{site.data.keyword.openwhisk}}를 사용하여 [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) 버킷에 대한 변경사항을 청취하고 액션을 사용하여 버킷에서 하나 이상의 오브젝트를 처리할 수 있습니다.

<br>

**샘플 유스 케이스:** `/whisk.system/cos-experimental` 패키지를 사용하여 {{site.data.keyword.cos_full_notm}} 버킷에 저장된 GPS 상세 주소 데이터에 대한 변경사항을 청취할 수 있습니다. 그런 다음 변경사항이 발생하면 사용자가 해당 GPS 애플리케이션에 대한 최신 상세 주소 데이터에 액세스할 수 있도록 GPS 맵의 자동 재생성을 트리거할 수 있습니다.

### (실험) Object Storage 이벤트 소스 매개변수
{: #pkg_obstorage_ev_ch}

`/whisk.system/cos-experimental` 패키지를 사용하면 {{site.data.keyword.cos_full_notm}} 인스턴스로부터 이벤트를 구성하고 다음 피드를 포함시킬 수 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` |패키지 | apikey, auth_endpoint, bucket, endpoint, interval | `changes` 피드 액션이 포함된 패키지입니다. |
| `/whisk.system/cos-experimental/changes` |피드 | apikey, auth_endpoint, bucket, endpoint, interval | {{site.data.keyword.cos_full_notm}} 버킷에 대한 변경사항에 대해 트리거 이벤트를 실행합니다. |
{: shortdesc}

`changes` 피드를 사용하면 {{site.data.keyword.cos_full_notm}} 이벤트 소스 서비스를 구성하여 {{site.data.keyword.cos_full_notm}} 인스턴스에서 버킷에 대한 모든 변경사항에 대해 트리거를 실행할 수 있습니다.

이 예에서 사용되는 매개변수는 다음과 같습니다.

**apikey**: _(패키지에 바인딩되지 않는 한 필수임)_. `apikey` 매개변수는 {{site.data.keyword.cos_full_notm}} 인스턴스에 대한 IAM API 키입니다.  일반적으로 이 값은 패키지에 바인딩됩니다. 그러나 `apikey` 값이 `changes` 피드 액션을 사용할 때 지정되는 경우, 바운드 인증 정보의 API 키 대신 지정된 값이 인증 정보에 사용됩니다.

**auth_endpoint**: _(선택사항)_. `auth_endpoint` 매개변수는 `apikey`로부터 토큰을 생성하기 위해 {{site.data.keyword.cos_full_notm}}에서 사용되는 권한 엔드포인트입니다.  기본 엔드포인트는 {{site.data.keyword.Bluemix}} 엔드포인트입니다.

**bucket**: _(필수)_. `bucket` 매개변수는 {{site.data.keyword.cos_full_notm}} 버킷의 이름입니다.

**endpoint**: _(필수)_. `endpoint` 매개변수는 사용자의 {{site.data.keyword.cos_full_notm}} 인스턴스에 연결하는 데 사용되는 {{site.data.keyword.cos_full_notm}} 엔드포인트입니다. [{{site.data.keyword.cos_full_notm}} 문서](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints)에서 사용자 엔드포인트를 찾을 수 있습니다.

**interval**: _(선택사항)_. `interval` 매개변수는 버킷 폴링 간격(전체 분 단위)입니다. `interval` 값은 최소한 1분이어야 하며 기본적으로 1분으로 설정됩니다.

### 변경 피드에 응답하도록 트리거 작성
{: #pkg_obstorage_ev_trig}

트리거를 작성할 경우 `cos-experimental` 패키지에 직접 인증 정보를 바인딩함으로써 {{site.data.keyword.cos_full_notm}} 인증 정보를 `changes` 피드 액션에 전달하지 않도록 할 수 있습니다.
 {: shortdesc}

 1. 먼저 인증 정보를 포함시키기 위해 수정할 수 있는 패키지 바인딩을 작성하십시오. 다음은 네임스페이스에서 패키지 바인딩 `myCosPkg`를 작성합니다.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. {{site.data.keyword.cos_full_notm}} 인증 정보를 패키지에 바인딩하십시오.
 {{site.data.keyword.cos_full_notm}} 인증 정보를 패키지에 바인딩할 경우 `apikey` 값이 패키지에 바인딩되므로 `changes` 피드 액션이 호출될 때 `apikey` 값을 지정할 필요가 없습니다.
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. 작성한 패키지 바인딩에서 `changes` 피드와 함께 이름이 `myCosTrigger`인 트리거를 작성하십시오. 버킷 이름 및 {{site.data.keyword.cos_full_notm}} 엔드포인트 매개변수 값을 사용하십시오.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

출력 예:
    ```
    ok: created trigger feed myCosTrigger
    ```
  {: pre}
 4. 트리거, 변경 피드 및 룰이 모두 구성되어 있으며 올바르게 작동하는지 확인하는 역할만 수행하는 단순 액션을 작성하십시오. 예를 들어, 다음 `showCosChange.js` JavaScript 코드가 포함된 `showCosChange`라는 액션을 작성하십시오. 
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
Sample code showing
 5. `showCosChange` 액션을 `myCosTrigger` 트리거에 연결하려면 룰을 작성하십시오.
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 6. 개별 창에서 활성화에 대한 폴링을 시작하여 발생한 상황을 명확하게 보여주십시오. 트리거가 실행되고 액션이 실행되는 경우 이 명령을 사용하면 발생하는 각 조작마다 활성화 레코드가 나열됩니다. 
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 7. {{site.data.keyword.cos_full_notm}} 대시보드에서 기존 버킷 오브젝트를 수정하거나 하나를 작성하십시오. 버킷에 오브젝트를 추가하는 방법을 알아보려면 [버킷에 일부 오브젝트 추가](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects)의 내용을 참조하십시오.

 8. 각 버킷 오브젝트 변경사항의 경우, `myCosTrigger` 트리거 및 `showCosChange` 액션에 대한 새 활성화를 관찰하십시오. 구성된 버킷 폴링 간격 내에서 `ibmcloud fn activation poll` 명령을 실행하면 이 활성화가 창에 표시됩니다. 

새 활성화를 관찰할 수 없는 경우, `apikey`, `endpoint` 및 `bucket` 매개변수 값이 올바른지 확인하십시오.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Object Storage 트리거 이벤트의 데이터 구조
{: #pkg_obstorage_ev_data}

생성된 이벤트의 컨텐츠에는 다음 매개변수가 있습니다.

  - `file`: 파일 또는 오브젝트 메타데이터. 이 구조는 [특정 버킷에서 오브젝트 나열](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets)에 설명되어 있습니다.
  - `status`: 발견된 변경사항.  이 값은 `added`, `modified` 또는 `deleted` 중 하나입니다.
  - `bucket`: {{site.data.keyword.cos_full_notm}} 버킷의 이름.
  - `endpoint`: {{site.data.keyword.cos_full_notm}} 인스턴스에 연결하는 데 사용되는 {{site.data.keyword.cos_full_notm}} 엔드포인트입니다.
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

### 변경된 오브젝트를 처리하기 위한 액션 작성
{: #pkg_obstorage_ev_act}

해당 오브젝트를 검색 및 처리하는 단일 액션을 작성할 수 있습니다. 또는 오브젝트 및 이 오브젝트를 처리하기 위한 다른 액션을 검색하기 위해 하나의 액션을 사용하는 시퀀스를 작성할 수 있습니다.

### 오브젝트를 검색 및 처리하기 위한 액션 작성
{: #pkg_obstorage_ev_act_ret}

이 샘플 액션 코드는 버킷 변경 알림 문서를 검색 및 처리합니다. 수동 액션 호출 동안 `ibmcloud fn service bind` 명령으로 액션에 바인딩되어야 하는 {{site.data.keyword.cos_full_notm}}에서 이러한 값을 얻어야 하는 트리거에 의해 이 액션이 호출되는 경우, `apikey` 및 `serviceInstanceId` 매개변수를 직접 액션에 전달할 수 있습니다.

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
exports.main = main;
```
{: codeblock}

이 액션은 `ibm-cos-sdk` npm 패키지를 사용하므로 액션을 [Node.js module](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) 또는 [single bundle](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg) 중 하나로 패키징해야 합니다.

이 액션을 `myCosAction.zip`이라는 .zip 파일로 패키징하고 나면 {{site.data.keyword.cos_full_notm}}에서 오브젝트를 검색 및 처리하기 위한 액션을 작성하십시오. 

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

### 오브젝트를 검색 및 처리하기 위한 액션 시퀀스 작성
{: #pkg_obstorage_ev_act_seq}

액션에서 오브젝트 검색 코드를 포함하는 대신 [수동으로 설치](#pkg_obstorage_install)되어야 하는 `cloud-object-storage` 패키지로부터 `object-read` 액션을 사용할 수 있습니다. 액션 코드는 `object-read`에서 리턴된 결과를 처리하기만 하면 됩니다.

버킷 오브젝트만 처리하는 액션의 `myCosAction.js` 예제 코드는 다음과 같습니다.
```javascript
  function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. {{site.data.keyword.cos_full_notm}}에서 오브젝트만 처리하기 위한 액션을 작성하십시오.
  ```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
  ```
  {: pre}
2. {{site.data.keyword.cos_full_notm}} 인증 정보를 수동으로 설치된 `cloud-object-storage` 패키지에 바인딩하십시오. 
  ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}
3. `object-read` 액션은 액션 시퀀스를 작성하기 위한 `myCosProcessObjectAction`으로 구성될 수 있습니다.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

`object-read` 액션 외에도 설치 가능한 `cloud-object-storage` 패키지에 포함된 다른 액션을 사용할 수 있습니다. 

### 엑션에 인증 정보 바인딩
{: #pkg_obstorage_ev_bind}

{{site.data.keyword.cos_full_notm}} 인증 정보를 다음 명령으로 액션에 바인딩함으로써 호출 동안 민감한 인증 정보의 전달을 피할 수 있습니다.
```
 ibmcloud fn service bind cloud-object-storage myCosAction
```
{: pre}

### 변경 트리거와 액션을 연관시키기 위한 규칙 작성
{: #pkg_obstorage_ev_rule}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

[rule](/docs/openwhisk?topic=cloud-functions-rules)에서 액션 또는 액션 시퀀스를 사용하여 {{site.data.keyword.cos_full_notm}} 변경 이벤트와 연관된 오브젝트를 페치 및 처리할 수 있습니다.

새 {{site.data.keyword.cos_full_notm}} 트리거 이벤트에서 `MyCosAction` 액션을 활성화하는 규칙을 작성하십시오.
```
ibmcloud fn rule create myRule myCosTrigger myCosAction
```
{: pre}

