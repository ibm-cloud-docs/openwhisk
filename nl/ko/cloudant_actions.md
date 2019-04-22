---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Cloudant 패키지
{: #cloudant_actions}

`/whisk.system/cloudant` 패키지를 사용하면 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started) 데이터베이스 관련 작업을 수행할 수 있으며, 여기에는 다음 액션과 피드가 포함되어 있습니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |패키지 |dbname, host, username, password | Cloudant 데이터베이스 관련 작업을 수행합니다. |
|`/whisk.system/cloudant/read` |액션 |dbname, id | 데이터베이스에서 문서를 읽습니다. |
|`/whisk.system/cloudant/write` |액션 |dbname, overwrite, doc | 데이터베이스에 문서를 작성합니다. |
|`/whisk.system/cloudant/changes` |피드 | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | 데이터베이스 변경 시 트리거 이벤트를 실행합니다. |
{: shortdesc}

다음 절에서는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스를 설정하는 방법과 이를 읽고 쓰는 방법을 설명합니다. `/whisk.system/cloudant` 패키지에서 피드를 사용하는 방법에 대한 자세한 정보는 [{{site.data.keyword.cloudant_short_notm}} 이벤트 소스](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant)를 참조하십시오.

## {{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.cloudant_short_notm}} 데이터베이스 설정
{: #cloudantdb_cloud}

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우, [{{site.data.keyword.openwhisk}} CLI 플러그인](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli)을 사용하여 서비스를 액션 또는 패키지에 바인딩할 수 있습니다. 

먼저 {{site.data.keyword.cloudant_short_notm}} 계정에 대한 패키지 바인딩을 수동으로 작성해야 합니다. 

1. {{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. 패키지 바인딩이 존재하는지 확인하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}
  
3. 액션 또는 패키지에 바인딩할 서비스 인스턴스의 이름을 가져오십시오.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    출력 예:
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

    출력 예:
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

    출력 예:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters": [
        {
                "key": "bluemixServiceName",
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

    이 예에서는 Cloudant 서비스에 대한 인증 정보가 `__bx_creds`라는 매개변수에 속합니다.
  

## {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 읽기
{: #cloudant_read}

액션을 사용하여 **testdb**라는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 문서를 페치할 수 있습니다. 이 데이터베이스가 {{site.data.keyword.cloudant_short_notm}} 계정에 있는지 확인하십시오.

- 이전에 작성한 패키지 바인딩에서 **read** 액션을 사용하여 문서를 페치하십시오. 반드시 `/_/myCloudant`를 패키지 이름으로 대체하십시오.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  출력 예:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

## {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 쓰기
{: #cloudant_write}

액션을 사용하여 **testdb**라고 하는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 문서를 저장할 수 있습니다. 이 데이터베이스가 {{site.data.keyword.cloudant_short_notm}} 계정에 있는지 확인하십시오.

1. 앞에서 작성한 패키지 바인딩에서 **write** 액션을 사용하여 문서를 저장하십시오. 반드시 `/_/myCloudant`를 패키지 이름으로 대체하십시오.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  출력 예:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. {{site.data.keyword.cloudant_short_notm}} 대시보드에서 문서를 찾아보고 해당 문서가 있는지 확인하십시오.

  **testdb** 데이터베이스에 대한 대시보드 URL은 다음과 유사합니다. `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.
