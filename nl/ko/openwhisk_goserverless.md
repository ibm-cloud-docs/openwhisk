---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 서버리스 프레임워크
{: #openwhisk_goserverless}

[서버리스 프레임워크](https://serverless.com/)는 서버리스 애플리케이션을 빌드하기 위한 오픈 소스 프레임워크입니다. 개발자는 단순 Manifest 파일을 사용하여 서버리스 기능을 정의하고 이를 이벤트 소스에 연결하며 해당 애플리케이션에서 필요한 클라우드 서비스를 선언할 수 있습니다. 이 프레임워크는 이러한 서버리스 애플리케이션을 클라우드 제공자에게 배치하는 작업을 처리합니다. 또한 개발자는 이를 사용하여 프로덕션의 서비스를 모니터하고 업데이트를 롤아웃하며 디버깅 문제를 지원할 수 있습니다. 프레임워크의 기능을 확장하는 써드파티 플러그인의 강력한 에코시스템도 있습니다. OpenWhisk는 서버리스 프레임워크를 구동하는 기술입니다.
{:shortdesc}

OpenWhisk에는 [서버리스 프레임워크의 자체 제공자 플러그인](https://github.com/serverless/serverless-openwhisk)이 있습니다. 서버리스 프레임워크를 사용하는 개발자는 ({{site.data.keyword.Bluemix_notm}} 또는 기타 클라우드나 프라이빗에 호스팅된) 임의의 OpenWhisk 플랫폼 인스턴스에 해당 애플리케이션을 배치하도록 선택할 수 있습니다. 또한 훨씬 쉬운 플랫폼 간의 애플리케이션 이동을 의미하는 다중 제공자 지원도 제공되며, 이에 따라 개발자는 다중 클라우드 서버리스 애플리케이션을 개발할 수 있습니다.

## 시작하기
{: #openwhisk_goserverless_starting}

공식 서버리스 프레임워크 [Getting Started Guide for OpenWhisk](https://serverless.com/framework/docs/providers/openwhisk/guide/intro/).
* 설치, 개발 워크플로우, 우수 사례, 작동 중인 OpenWhisk 애플리케이션을 빌드하고 배치하기 위한 단계별 안내 등을 커버하는 안내서입니다.

OpenWhisk 제공자 플러그인의 서버리스 프레임워크를 사용하는 방법을 설명하는 동영상인 [The Serverless Framework and OpenWhisk](https://youtu.be/GJY10W98Itc)를 보십시오.

## 문서
{: #openwhisk_goserverless_docs}

서버리스 프레임워크에서 OpenWhisk를 사용하는 방법에 대한 최신 문서는 [여기서 찾을 수](https://serverless.com/framework/docs/providers/openwhisk/) 있습니다.

## 샘플
{: #openwhisk_goserverless_samples}
[Serverless Framework examples GitHub repository](https://github.com/serverless/examples)는 이제 HTTP API, cron 기반 스케줄러 및 체인 기능 등을 빌드하는 방법을 보여주는 OpenWhisk를 기능으로 제공합니다.
