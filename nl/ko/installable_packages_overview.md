---

copyright:
  years: 2018
lastupdated: "2018-07-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}

# 설치 가능 패키지 사용
{: #installable-packages-overview}

설치 가능 패키지는 {{site.data.keyword.openwhisk}} 내에서 패키지를 관리하고 이와 상호작용하기 위한 새 디자인을 나타냅니다. {: shortdesc}

## 개요
{: #overview}

**설치 가능 패키지의 정의**

패키지는 관련된 액션과 피드를 번들링한 세트입니다. 각 패키지는 서비스 및 이벤트 제공자와의 특정 상호작용을 위해 디자인되었습니다. 설치 가능 패키지는 사용자가 자체 요구사항에 따라 선택, 설치 및 편집을 위해 사용할 수 있는 패키지입니다. 

**설치 가능 패키지와 사전 설치된 패키지 간의 차이점**

[사전 설치된 패키지](openwhisk_packages.html#browse-packages)는 `/whisk.system` 네임스페이스의 {{site.data.keyword.openwhisk_short}} 내에서 자동으로 등록됩니다. 사전 설치된 패키지에서 신임 정보나 기타 매개변수를 저장하려면 [패키지 바인딩](openwhisk_packages.html#openwhisk_package_bind)을 작성해야 합니다. 

설치 가능 패키지는 {{site.data.keyword.openwhisk_short}} 시스템 내에 상주하지 않습니다. 그 대신에 설치 가능 패키지는 개별 Github 저장소에서 외부적으로 수용됩니다. [wskDeploy](https://github.com/apache/incubator-openwhisk-wskdeploy#whisk-deploy-wskdeploy) 도구를 사용하여 자체 네임스페이스에 직접 이러한 패키지를 설치하고 패키지에 사용자 정의 이름을 부여할 수 있습니다. 패키지가 자신의 네임스페이스에 설치되므로 사용자는 필요에 따라 패키지에서 액션과 피드를 수정할 수 있습니다. 

## wskDeploy를 사용하여 패키지 설치
{: #installing}

시작하기 전에 다음을 수행하십시오. 
  1. [{{site.data.keyword.Bluemix_notm}} CLI용 {{site.data.keyword.openwhisk_short}} 플러그인을 설치하십시오](bluemix_cli.html#cloudfunctions_cli).
  2. [`wskdeploy` 명령 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)을 설치하고 다운로드된 2진을 PATH에 추가하십시오. 

패키지를 설치하려면 다음을 수행하십시오.

1. 패키지 저장소를 복제하십시오. 패키지 저장소는 이 문서 세트의 각 패키지에 대한 개별 페이지에서 찾을 수 있습니다.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. `manifest.yaml` 또는 `manifest.yml` 파일이 포함된 패키지 디렉토리로 이동하십시오. 네임스페이스에 설치될 패키지 및 액션과 `wskdeploy` 명령에 포함되어야 할 메타데이터를 포함하여 `manifest.yaml` 파일은 패키지의 전체 구조를 지정합니다. `manifest.yaml` 파일에 대해 자세히 알아보려면 [wskdeploy 문서 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example)를 참조하십시오.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. 패키지를 배치하십시오. 패키지의 올바른 작동을 보장하기 위해 일부 패키지에서는 특정 환경 변수가 필요합니다.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

### {{site.data.keyword.cos_full_notm}} 패키지 사용 예제
{: #example}

패키지 설치 방법에 대한 예제를 보려면 [{{site.data.keyword.cos_short}} 패키지](cloud_object_storage_actions.html)를 체크아웃하십시오. {{site.data.keyword.cos_full}}는 사용자가 이미지, 동영상, 음악 및 텍스트 등과 같은 모든 유형의 파일을 저장할 수 있도록 허용하는 서비스입니다. 파일과의 상호작용을 위해 키/값 쌍의 클라우드 기반 데이터 저장소는 버킷에 저장됩니다. 따라서 [{{site.data.keyword.cos_short}} 패키지](cloud_object_storage_actions.html)를 사용하려면 우선 {{site.data.keyword.cos_short}} 서비스 인스턴스를 작성한 후에 버킷을 작성해야 합니다. 버킷은 이 패키지를 설치하는 데 필요한 환경 변수로서 사용됩니다. 

서비스 인스턴스 및 버킷의 작성 이후에 패키지 설치를 위해서는 다음 명령이 필요합니다. 

1. 패키지 저장소를 복제하십시오.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. `manifest.yaml`이 포함된 패키지 디렉토리로 이동하십시오. 이 예제에서는 {{site.data.keyword.cos_short}} 패키지의 Node.js 런타임 버전이 사용됩니다.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. 환경 변수로서 버킷을 사용하여 패키지를 배치하십시오. `PACKAGE_NAME` 환경 변수의 종속 항목은 사용자가 이 패키지에 사용자 정의 이름을 부여할 수 있도록 허용합니다.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> wskdeploy
    ```
    {: pre}
