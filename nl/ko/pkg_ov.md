---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# 패키지 통합
{: #pkg_ov}

패키지는 관련된 액션과 피드를 번들링한 세트입니다. 각 패키지는 서비스 및 이벤트 제공자와의 특정 상호작용을 위해 디자인되었습니다. 일부 패키지는 사용자가 사용할 수 있도록 이미 {{site.data.keyword.openwhisk}}에 설치되어 있으나 사용자는 다른 패키지를 설치할 수도 있습니다.
{: shortdesc}

## 개요
{: #pkg_overview}

[사전 설치된 패키지](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse)는 `/whisk.system` 네임스페이스의 {{site.data.keyword.openwhisk_short}} 내에서 자동으로 등록됩니다. 설치 단계를 완료하지 않고 사전 설치 패키지를 사용할 수 있습니다.

설치 가능 패키지는 사용자가 자체 요구사항에 따라 설치, 편집 및 사용을 위해 사용할 수 있는 패키지입니다. 설치 가능 패키지는 {{site.data.keyword.openwhisk_short}} 시스템 내에 상주하지 않습니다. 그 대신에 설치 가능 패키지는 개별 GitHub 저장소에서 외부적으로 수용됩니다.

이 패키지 또는 자체 패키지를 네임스페이스에 직접 설치하고 패키지에 사용자 정의 이름을 부여할 수 있습니다. 패키지가 자신의 네임스페이스에 설치되므로 사용자는 필요에 따라 패키지에서 액션과 피드를 수정할 수 있습니다.



## 사전 설치된 패키지 찾아보기
{: #pkg_browse}

여러 패키지가 사용자를 위해 {{site.data.keyword.openwhisk_short}}에 이미 등록되어 있습니다. 네임스페이스에서 패키지의 목록을 가져오고, 패키지의 엔티티를 나열하며 패키지의 개별 엔티티에 대한 설명을 가져올 수 있습니다.
{: shortdesc}

1. `/whisk.system` 네임스페이스에서 패키지의 목록을 가져오십시오.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  패키지 목록 출력:
  ```
  packages
  /whisk.system/cloudant                                                 shared
  /whisk.system/alarms                                                   shared
  /whisk.system/watson                                                   shared
  /whisk.system/websocket                                                shared
  /whisk.system/weather                                                  shared
  /whisk.system/system                                                   shared
  /whisk.system/utils                                                    shared
  /whisk.system/slack                                                    shared
  /whisk.system/samples                                                  shared
  /whisk.system/github                                                   shared
  /whisk.system/pushnotifications                                        shared
  ```
  {: screen}

2. 패키지에서 엔티티의 목록을 가져오십시오.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **예**
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  **출력 예**
  ```
package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  이 출력은 {{site.data.keyword.cloudant_short_notm}} 패키지에 액션 및 피드가 포함되어 있음을 보여줍니다. 예를 들면, 두 개의 액션(`read` 및 `write`)과 하나의 트리거 피드(`changes`)입니다. `changes` 피드는 문서가 지정된 {{site.data.keyword.cloudant_short_notm}} 데이터베이스에 추가될 때 트리거가 실행되도록 합니다.

  또한 {{site.data.keyword.cloudant_short_notm}} 패키지는 `username`, `password`, `host` 및 `port` 매개변수도 정의합니다. 이러한 매개변수는 액션 및 피드가 유의미하도록 지정해야 합니다. 매개변수는 액션이 특정 {{site.data.keyword.cloudant_short_notm}} 계정에서 작동하도록 합니다. 예를 들어, 다음과 같습니다.

3. 필요한 매개변수를 보려면 액션 또는 피드에 대한 설명을 가져오십시오.

  **예**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **출력 예**
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  이 출력은 검색할 데이터베이스 및 문서 ID를 포함하여 {{site.data.keyword.cloudant_short_notm}} `read` 액션에서 세 개의 매개변수가 필요함을 표시합니다.



## 사전 설치된 패키지에 매개변수 바인딩
{: #pkg_bind}

패키지의 엔티티를 직접 사용할 수 있지만, 사용자 자신이 동일한 매개변수를 매번 액션에 전달한다는 사실을 발견할 수 있습니다. 패키지를 바인딩하고 패키지의 액션에 의해 상속된 기본 매개변수를 지정하여 프로세스를 단순화할 수 있습니다.
{: shortdesc}

예를 들어, `/whisk.system/cloudant` 패키지에서 기본 `username`, `password` 및 `dbname` 값을 패키지 바인딩에서 설정하면 이러한 값이 자동으로 패키지 내의 모든 액션에 전달됩니다.

다음은 `/whisk.system/samples` 패키지에 바인딩하는 예입니다.

1. `/whisk.system/samples` 패키지에 바인딩하고 기본 `place` 매개변수값을 설정하십시오.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **출력 예**
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. 패키지 바인딩의 설명을 가져오십시오.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  **출력 예**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  `/whisk.system/samples` 패키지의 모든 액션을 `valhallaSamples` 패키지 바인딩에서 사용할 수 있다는 점에 유념하십시오.

3. 패키지 바인딩에서 액션을 호출하십시오.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **출력 예**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  결과에서 `valhallaSamples` 패키지 바인딩을 작성할 때 설정된 `place` 매개변수를 액션이 상속한다는 점에 유념하십시오.

4. 액션을 호출하고 기본 매개변수값을 겹쳐쓰십시오.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **출력 예**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  액션 호출에서 지정된 `place` 매개변수값이 `valhallaSamples` 패키지 바인딩에 설정된 기본값을 겹쳐쓴다는 점에 유념하십시오.



## 자체 패키지 추가
{: #pkg_add}

로컬 코드의 패키지 또는 GitHub 저장소의 복제본을 작성할 수 있습니다.
{: shortdesc}

**시작하기 전에**
- [{{site.data.keyword.cloud_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](/docs/openwhisk?topic=cloud-functions-cli_install).
- 앱에 적합한 `manifest.yaml` 또는 `manifest.yml` 파일을 작성하거나 루트 디렉토리에 저장하십시오. `ibmcloud fn deploy` 명령에 포함되어야 할 메타데이터를 포함하여 `manifest.yaml` 파일은 패키지의 전체 구조를 지정합니다. `manifest.yaml` 파일에 대해 자세히 알아보려면 [<ph class="ignoreSpelling">wskdeploy</ph> 문서](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}를 참조하십시오.

패키지를 추가하려면 다음을 수행하십시오.

1. 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. `manifest.yaml` 파일이 포함된 디렉토리로 이동하십시오.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. 패키지를 배치하십시오.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    패키지의 올바른 작동을 보장하기 위해 일부 패키지에서는 특정 환경 변수가 필요합니다. 해당 경우, 환경 변수를 `deploy` 명령에 포함하십시오. 예를 들어, 패키지의 이름을 선택하고 PACKAGE_NAME 변수로 이름을 지정할 수 있습니다.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### {{site.data.keyword.cos_full_notm}} 패키지 예제
{: #pkg_ex}

패키지 설치 방법에 대한 예제를 보려면 [{{site.data.keyword.cos_full_notm}} 패키지](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)를 체크아웃하십시오. {{site.data.keyword.cos_full}}는 사용자가 이미지, 동영상, 음악 및 텍스트 등과 같은 모든 유형의 파일을 저장할 수 있도록 허용하는 서비스입니다. 파일과의 상호작용을 위해 키-값 쌍의 클라우드 기반 데이터 저장소는 버킷에 저장됩니다. 따라서 [{{site.data.keyword.cos_full_notm}} 패키지](/docs/openwhisk?topic=cloud-functions-pkg_obstorage)를 사용하려면 우선 {{site.data.keyword.cos_full_notm}} 서비스 인스턴스를 작성한 후에 버킷을 작성해야 합니다. 버킷은 이 패키지를 설치하는 데 필요한 환경 변수로서 사용됩니다.

서비스 인스턴스 및 버킷을 작성한 후 다음 명령을 사용하여 패키지를 설치할 수 있습니다.

1. 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `manifest.yaml`이 포함된 패키지 디렉토리로 이동하십시오. 이 예제에서는 {{site.data.keyword.cos_full_notm}} 패키지의 Node.js 런타임 버전이 사용됩니다.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 환경 변수로서 버킷을 사용하여 패키지를 배치하십시오.  `PACKAGE_NAME` 환경 변수를 사용하여 패키지에 사용자 정의 이름을 지정할 수 있습니다.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



