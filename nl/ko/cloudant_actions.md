---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant 패키지
{: #cloudant_actions}

`/whisk.system/cloudant` 패키지를 사용하면 [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) 데이터베이스 관련 작업을 수행할 수 있으며, 여기에는 다음 액션과 피드가 포함되어 있습니다. 

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` |패키지 |dbname, host, username, password | Cloudant 데이터베이스 관련 작업을 수행합니다. |
|`/whisk.system/cloudant/read` |액션 |dbname, id | 데이터베이스에서 문서를 읽습니다. |
|`/whisk.system/cloudant/write` |액션 |dbname, overwrite, doc | 데이터베이스에 문서를 작성합니다. |
|`/whisk.system/cloudant/changes` |피드 |dbname, filter, query_params, maxTriggers | 데이터베이스 변경 시 트리거 이벤트를 실행합니다. |
{: shortdesc}

다음 절에서는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스를 설정할 수 있고 이 데이터베이스를 읽고 작성하는 방법을 알아볼 수 있습니다.
`/whisk.system/cloudant` 패키지에서 피드를 사용하는 방법에 대한 자세한 정보는 [{{site.data.keyword.cloudant_short_notm}} 이벤트 소스](./openwhisk_cloudant.html)를 참조하십시오. 

## {{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.cloudant_short_notm}} 데이터베이스 설정
{: #cloudantdb_cloud}

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우, {{site.data.keyword.openwhisk_short}}가 {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스에 대한 패키지 바인딩을 자동으로 작성합니다. {{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}} 및 {{site.data.keyword.cloudant_short_notm}}를 사용하지 않는 경우 다음 섹션으로 건너뛰십시오.

1. [{{site.data.keyword.Bluemix_notm}} 대시보드](http://console.bluemix.net)에서 {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스를 작성하십시오.

  각 새 서비스 인스턴스에 대한 신임 정보 키를 작성하십시오.

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치면 정의된 신임 정보 키를 사용하여 각 {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  출력 예:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  이제 패키지 바인딩에 {{site.data.keyword.cloudant_short_notm}} 서비스 인스턴스와 연관된 신임 정보가 포함됩니다.

3. 이전에 작성된 패키지 바인딩이 {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}} 서비스 인스턴스 호스트 및 신임 정보로 구성되었는지 확인하십시오.

  ```
  ibmcloud fn package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  출력 예:
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

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
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.cloudant_short_notm}} 데이터베이스 설정
{: #cloudantdb_nocloud}

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.Bluemix_notm}} 외부에서 {{site.data.keyword.cloudant_short_notm}} 데이터베이스를 설정하려는 경우 {{site.data.keyword.cloudant_short_notm}} 계정에 대한 패키지 바인딩을 수동으로 작성해야 합니다. {{site.data.keyword.cloudant_short_notm}} 계정 호스트 이름, 사용자 이름 및 비밀번호가 필요합니다.

1. {{site.data.keyword.cloudant_short_notm}} 계정에 대해 구성된 패키지 바인딩을 작성하십시오.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. 패키지 바인딩이 존재하는지 확인하십시오.
  ```
  wsk package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## {{site.data.keyword.cloudant_short_notm}} 데이터베이스에서 읽기
{: #cloudant_read}

액션을 사용하여 **testdb**라고 하는 {{site.data.keyword.cloudant_short_notm}} 데이터베이스의 문서를 페치할 수 있습니다. 이 데이터베이스가 {{site.data.keyword.cloudant_short_notm}} 계정에 있는지 확인하십시오.

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
