---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: planning, functions, actions, serverless

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


# 서버리스 앱 계획
{: #plan}

함수 작성을 시작하기 전에 수행해야 할 의사결정에 대해 알아보십시오.
{: shortdesc}

## 런타임 지원 검토
{: #plan_runtime}

{{site.data.keyword.openwhisk_short}}는 표준 [런타임](/docs/openwhisk?topic=cloud-functions-runtimes#runtimes)을 제공하여 코드를 실행합니다. 그러나 {{site.data.keyword.openwhisk_short}}는 서버리스이므로 사용할 수 있는 런타임에서 제한되지 않습니다. 코드를 패키징하려면 고유한 [사용자 정의 Docker 이미지](/docs/openwhisk?topic=cloud-functions-actions#actions-docker)를 작성하여 사용자 정의 런타임을 작성할 수 있습니다.
{: shortdesc}



## 코드 설계
{: #plan_architect}

{{site.data.keyword.openwhisk}}에서 실행하려면 기존 코드를 일부 변경해야 할 수 있습니다. 코드를 아직 작성하지 않은 경우 작성 시 이를 염두에 두십시오.
{: shortdesc}

1. 코드를 컴포넌트화하십시오.

    서버리스로 작성할 앱이 이미 있는 경우 어떻게 앱을 더 작은 단위로 분리해야 하는지를 고려하십시오. 각 함수에는 실행할 코드에 대한 트리거 세트가 포함되어 있습니다. 예를 들어, GitHub에서 문제가 작성된 경우 이 JavaScript 코드를 실행하십시오. 앱에 여러 이벤트 또는 액션이 포함된 경우 다른 기능으로 분리할 것을 고려하십시오.

2. 프레임워크 대신 {{site.data.keyword.cloud_notm}} 서비스를 사용하십시오.

    런타임 시 함수에 기능을 사용할 수 있도록 프레임워크를 사용하는 대신 {{site.data.keyword.cloud}} 서비스를 사용할 수 있습니다. 프레임워크를 사용하면 사용자가 완료하는 데 도움이 되는 다수의 일반 태스크가 {{site.data.keyword.cloud}}에서 서비스로 사용 가능합니다.
    {: shortdesc}

    예를 들어, 인증을 위해 프레임워크를 사용하는 대신 {{site.data.keyword.appid_full}}를 사용해 보십시오. 외부 파일 스토리지가 필요한 경우 {{site.data.keyword.cos_full}}를 사용해 보십시오.

    통합할 기능이 {{site.data.keyword.cloud}}에서 서비스로 사용 불가능한 경우 프레임 없이도 기능을 항상 피드 및 API와 통합할 수 있습니다.

3. [코드가 시스템 한계를 충족하는지 확인하십시오.](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)

    코드는 실행할 시스템 한계 내에 있어야 합니다. 예를 들어, 패키징된 코드가 48MB보다 크면 계속해서 더 작은 단위로 분할하거나 Docker 이미지로 패키징해야 할 수 있습니다.

    많은 서드파티 모듈, 기본 라이브러리 또는 외부 도구가 포함된 애플리케이션은 이 한계에 도달할 수 있습니다. 48MB를 초과하는.zip 또는 .jar 패키지 액션을 작성하는 경우, 사용자는 종속 항목의 런타임 이미지를 확장해야 합니다. 그런 다음, 48MB 미만의 소형 아카이브 또는 단일 소스 파일을 사용하십시오. 예를 들어, 필수 공유 라이브러리가 포함된 사용자 정의 Docker 런타임을 빌드하면 종속 항목이 아카이브 파일에 존재할 필요가 없습니다. 개인용 소스 파일은 계속해서 아카이브에 번들링되고 런타임에 삽입될 수 있습니다.

4. 코드에 삽입되어야 하는 매개변수를 결정하십시오.

    서버가 없는 액션에서는 데이터가 액션에 매개변수를 추가하여 제공됩니다. 매개변수는 서버가 없는 기본 함수에 대한 인수로 선언됩니다. 일반적으로 이 매개변수는 서비스에 대한 인증 정보이지만, 다른 트리거가 포함된 코드를 재사용할 수 있도록 하는 값을 포함할 수 있습니다.

5. [함수에 사용할 코드에 대한 구조적인 요구사항을 확인하십시오.](/docs/openwhisk?topic=cloud-functions-prep)

    앱을 이미 보유하고 있거나 사용할 스크립트를 개발할 계획인 경우 모두, {{site.data.keyword.openwhisk}}에서 이용할 수 있도록 일부 코드를 변경해야 할 수 있습니다. 코드는 입력 매개변수 및 출력 결과와 같은 일부 구조적인 요구사항을 자체적으로 충족해야 합니다. 또한 모든 종속성이 포함된 단일 파일로 패키징되어야 할 수 있습니다.







## 이벤트 소스 결정
{: #plan_source}

실행할 코드를 트리거할 이벤트의 관점에서 함수에 대해 생각해 보십시오. 사용자는 GitHub 저장소에 대한 모든 커미트마다 코드를 실행하려고 할 수 있습니다. 또는 Cloudant 데이터베이스가 업데이트될 때마다 코드를 실행하려고 할 수 있습니다.
{: shortdesc}

함수를 트리거할 이벤트를 선택한 후 이미 사용 가능한 패키지를 검토하십시오. 패키지를 사용하여 함수 개발을 간소화할 수 있습니다. 그렇지 않으면, 이벤트 소스에 적합한 자체 패키지를 작성할 수 있습니다.

코드로 다시 돌아가서 이벤트 선택사항에 따라 일부 사항을 수정해야 할 수 있습니다.


## 환경 내 배치 구성
{: #plan_environments}

환경(예: 개발, 스테이징 및 프로덕션 환경)에서 함수를 배치할 방식을 결정하십시오.
{: shortdesc}

{{site.data.keyword.openwhisk_short}}가 Cloud Foundry 기반 서비스이므로, {{site.data.keyword.cloud_notm}}에서 사용자에게 제공하는 Cloud Foundry 조직 및 영역에서 함수의 배치를 관리할 수 있습니다. 이 환경에서 함수를 구성하기 위해 함수당 하나의 조직을 작성하도록 선택할 수 있습니다. 그런 다음 필요한 각 환경마다 공간을 작성하십시오. 대신, 각 환경마다 하나의 조직을 보유하고 함수당 영역을 작성할 수도 있습니다. 조직 및 공간을 배열하는 방식에 관계 없이 함수 엔티티를 효과적으로 관리하도록 사용할 수 있는 구조를 선택하십시오.

[네임스페이스](/docs/openwhisk?topic=cloud-functions-namespaces)를 사용하여 리소스를 격리할 수도 있습니다. 기본적으로 각 {{site.data.keyword.cloud_notm}} 영역에는 Open Whisk 네임스페이스가 포함되어 있습니다. 네임스페이스의 액션 또는 트리거와 같은 엔티티를 그룹화한 후 IAM(Identity and Access) 정책을 작성하여 해당 그룹에 대한 사용자 권한을 관리할 수 있습니다.




