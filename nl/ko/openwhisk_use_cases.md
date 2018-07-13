---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 공통 유스 케이스
{: #openwhisk_common_use_cases}

{{site.data.keyword.openwhisk_short}}에서 제공하는 실행 모델은 다양한 유스 케이스를 지원합니다. 다음 절에는 일반적인 예제가 포함되어 있습니다. 서버리스 아키텍처, 예제 유스 케이스, 장단점 논의 및 구현 우수 사례에 대한 세부 논의사항을 알아보려면 탁월한 [Martin Fowler의 블로그에 있는 Mike Roberts 기사](https://martinfowler.com/articles/serverless.html)를 읽으십시오.
{: shortdesc}

## 마이크로서비스
{: #openwhisk_common_use_cases_microservices}

이점이 있음에도 불구하고 마이크로서비스 기반 솔루션은 메인스트림 클라우드 기술을 사용하여 빌드하기에 여전히 어려움이 있으며, 종종 별도의 빌드 및 운영 파이프라인과 복잡한 도구 체인의 제어가 필요합니다. 작고 기민한 팀은 인프라와 운영 복잡도(예: 결함 허용, 로드 밸런싱, Auto-Scaling 및 로깅)로 너무나 많은 시간을 소비합니다. 이러한 팀은 이미 익숙하고 애용하고 있으며 특정 문제점 해결에 최적인 프로그래밍 언어로 효율적인 부가 가치 코드를 개발하는 방법을 원합니다.

{{site.data.keyword.openwhisk_short}}의 모듈식 기본 확장 가능 특성 때문에 이는 액션에서 로직의 세부적인 부분을 구현하는 데 이상적입니다. {{site.data.keyword.openwhisk_short}} 액션은 상호 독립적이고 {{site.data.keyword.openwhisk_short}}에서 지원하는 서로 다른 다양한 언어를 사용하여 구현될 수 있으며 다양한 백엔드 시스템에 액세스할 수 있습니다. 각 액션은 별도로 배치되고 관리될 수 있으며, 다른 액션과는 별도로 스케일링됩니다. 액션 간의 상호연결은 룰, 시퀀스 및 이름 지정 규칙의 양식으로 {{site.data.keyword.openwhisk_short}}에 의해 제공됩니다. 이러한 유형의 환경은 마이크로서비스 기반 애플리케이션에 바람직하다고 예견됩니다.

{{site.data.keyword.openwhisk_short}}를 선호하는 다른 중요한 이유는 재해 복구 구성의 시스템 비용입니다. 컨테이너 또는 Cloud Foundry 런타임을 사용하는 10개의 마이크로서비스가 있다고 가정하고, {{site.data.keyword.openwhisk_short}}를 사용하여 마이크로서비스를 PaaS 또는 CaaS와 비교하십시오. 이 비교는 단일 가용성 구역에서 10개의 연속적으로 실행 중인 청구 가능한 프로세스에 해당되며, 2개의 AZ 간에서 실행될 때는 20개이고 각각 2개 구역의 2개 지역 간에서 실행될 때는 40개입니다. {{site.data.keyword.openwhisk_short}}에서 동일한 목표를 달성하기 위해, 증분 비용을 전혀 지불하지 않고도 원하는 수 만큼의 AZ 또는 지역 간에서 이를 실행할 수 있습니다.

[Logistics 마법사](https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/)는 {{site.data.keyword.openwhisk_short}} 및 CloudFoundry를 활용하여 12-factor 스타일 애플리케이션을 빌드하는 엔터프라이즈급 샘플 애플리케이션입니다. 이는 ERP 시스템을 실행하는 환경의 시뮬레이션을 목표로 하는 탁월한 공급 체인 관리 솔루션입니다. 이는 애플리케이션으로 이 ERP 시스템을 보강하여 공급 체인 관리자의 가시성과 기민성을 개선합니다.

## 웹 앱
{: #openwhisk_common_use_cases_webapps}

{{site.data.keyword.openwhisk_short}}의 이벤트 구동 특성이 부여된 경우, 이는 사용자 대면 애플리케이션에 대해 여러 가지의 이점을 제공하는 반면에 사용자 브라우저에서 수신되는 HTTP 요청은 이벤트로서의 역할을 수행합니다. {{site.data.keyword.openwhisk_short}} 애플리케이션은 컴퓨팅 용량을 사용하며 사용자 요청을 서비스하는 경우에만 비용이 청구됩니다. 유휴 대기 또는 대기 모드는 존재하지 않습니다. 전통적인 컨테이너 또는 Cloud Foundry 애플리케이션과 비교할 때 이 기능으로 {{site.data.keyword.openwhisk_short}}의 비용은 상당히 저렴해집니다. 둘 다 대부분의 시간을 유휴 상태로 보낼 수 있으며, 인바운드 사용자 요청을 대기하고 해당되는 모든 "휴면" 시간에 대해 비용이 청구됩니다.

전체 웹 애플리케이션이 빌드될 수 있으며 {{site.data.keyword.openwhisk_short}}에서 실행될 수 있습니다. 서버리스 API를 HTML, JavaScript 및 CSS 등의 사이트 리소스에 대해 호스팅하는 정적 파일과 결합한다는 것은 완전한 서버리스 웹 애플리케이션을 빌드할 수 있음을 의미합니다. 호스팅되는 {{site.data.keyword.openwhisk_short}} 환경 운용의 단순함은 아무 것도 운용할 필요가 없음을 의미합니다. {{site.data.keyword.openwhisk_short}}가 {{site.data.keyword.Bluemix_notm}}에서 호스팅되므로, Node.js Express 또는 기타 전통적인 서버 런타임의 스탠드업 및 운용과 비교할 때 상당한 이점이 있습니다.

{{site.data.keyword.openwhisk_short}}를 사용하여 웹 앱을 빌드하는 방법에 대한 다음 예제를 참조하십시오.
- [웹 액션: {{site.data.keyword.openwhisk_short}}의 서버리스 웹 앱](https://medium.com/openwhisk/web-actions-serverless-web-apps-with-openwhisk-f21db459f9ba).
- [{{site.data.keyword.Bluemix_notm}} 및 Node.js에서 사용자 대면 {{site.data.keyword.openwhisk_short}} 애플리케이션 빌드](https://www.ibm.com/developerworks/cloud/library/cl-openwhisk-node-bluemix-user-facing-app/index.html)
- [{{site.data.keyword.openwhisk_short}}의 서버리스 HTTP 핸들러](https://medium.com/openwhisk/serverless-http-handlers-with-openwhisk-90a986cc7cdd)

## IoT
{: #openwhisk_common_use_cases_iot}

IoT(Internet of Things) 시나리오는 종종 기본적으로 센서 구동형입니다. 예를 들어, 특정 온도를 초과하는 센서에 반응해야 할 때 {{site.data.keyword.openwhisk_short}}의 액션이 트리거될 수 있습니다. IoT 상호작용은 자연 재해, 거대한 기상 폭풍 또는 교통 체증과 같은 주요 자연적 이벤트의 로드 증가 가능성으로 인해 일반적으로 Stateless입니다. 정상적인 워크로드는 작을 수 있지만 예측 가능한 응답 시간으로 빠르게 스케일링해야 하는 탄력적 시스템에 대해 수요가 생성됩니다. 따라서 시스템에 사전 경고 없이 많은 동시 이벤트를 처리하는 기능이 바람직합니다. 전통적인 서버 아키텍처를 사용하여 이러한 요구사항을 충족하는 시스템을 빌드하는 것은 쉽지 않습니다. 그 이유는 전통적인 서버 아키텍처는 파워가 부족하고 트래픽의 최대 로드를 처리할 수 없거나 프로비저닝이 과도하고 고비용인 경향이 있기 때문입니다.

전통적인 서버 아키텍처를 사용하는 IoT 애플리케이션을 구현할 수는 있습니다. 그러나 대부분의 경우에 서로 다른 서비스와 데이터 브릿지의 조합에서는 고성능의 유연한 파이프라인이 요구됩니다. IoT 디바이스에서 클라우드 스토리지와 분석 플랫폼에 이르기까지 그 범위가 걸쳐 있습니다. 종종 사전 구성된 브릿지에는 특정 솔루션 아키텍처를 구현하고 미세 조정해야 하는 데 필요한 프로그래밍 가능성이 부족합니다. 다양한 파이프라인이 제공되며 일반적으로(특히 IoT) 데이터 결합에서 표준화가 부족한 경우에는 파이프라인이 사용자 정의 데이터 변환을 필요로 하는 환경을 찾는 것이 일반적입니다. 이러한 사용자 정의 데이터 변환은 형식 변환, 필터링 또는 기능 보강에 적용됩니다. {{site.data.keyword.openwhisk_short}}는 '서버리스' 방식으로 해당 변환을 구현하기 위한 탁월한 도구이며, 여기서 사용자 정의 로직은 완벽히 관리되는 탄력적 클라우드 플랫폼에서 호스팅됩니다.

{{site.data.keyword.openwhisk_short}}, NodeRed, Cognitive 및 기타 서비스를 사용하는 다음의 샘플 IoT 애플리케이션을 보십시오. [{{site.data.keyword.openwhisk_short}}에서 구동 중인 IoT 데이터의 서버리스 변환](https://medium.com/openwhisk/serverless-transformation-of-iot-data-in-motion-with-openwhisk-272e36117d6c#.akt3ocjdt).

![IoT 솔루션 아키텍처 예제](images/IoT_solution_architecture_example.png)

## API 백엔드
{: #openwhisk_api_backend}

서버리스 컴퓨팅 플랫폼은 서버 없이 API를 빌드하는 빠른 방법을 개발자에게 제공합니다. {{site.data.keyword.openwhisk_short}}는 액션에 대한 REST API의 자동 생성을 지원합니다. {{site.data.keyword.openwhisk_short}}의 [실험적 기능](./openwhisk_apigateway.html)은 {{site.data.keyword.openwhisk_short}} API 게이트웨이를 통한 액션의 권한 부여 API 키 없이 POST 이외의 HTTP 메소드로 액션을 호출할 수 있습니다. 이 기능은 API를 외부 이용자에게 노출시키는 데는 물론 마이크로서비스 애플리케이션을 빌드하는 데도 유용합니다.

또한 {{site.data.keyword.openwhisk_short}} 액션은 선택한 API 관리 도구(예: [IBM API Connect](https://www-03.ibm.com/software/products/en/api-connect) 또는 기타)에 연결될 수 있습니다. 기타 유스 케이스와 유사하게, 확장성과 기타 서비스 품질(QoS)에 대한 모든 고려사항이 적용됩니다.

[Emoting](https://github.com/l2fprod/openwhisk-emoting)은 REST API를 통해 {{site.data.keyword.openwhisk_short}} 액션을 사용하는 샘플 앱입니다.

[API 백엔드로서 서버리스 사용](https://martinfowler.com/articles/serverless.html#ACoupleOfExamples)의 논의가 포함된 다음 예제를 참조하십시오.

## 모바일 백엔드
{: #openwhisk_common_use_cases_mobile}

많은 모바일 애플리케이션에서는 서버 측 로직이 필요합니다. 그러나 모바일 개발자는 일반적으로 서버 측 로직의 관리에 대한 경험이 없으며, 오히려 디바이스에서 실행 중인 앱에 집중합니다. 이 개발 목표는 서버 측 백엔드로서 {{site.data.keyword.openwhisk_short}}를 사용하여 손쉽게 얻을 수 있으며, 이는 훌륭한 솔루션입니다. 또한 서버 측 Swift에 대한 기본 제공 지원을 사용하여 개발자는 자신의 기존 iOS 프로그래밍 스킬을 재사용할 수 있습니다. 모바일 애플리케이션에 종종 예상할 수 없는 로드 패턴이 있으므로, {{site.data.keyword.Bluemix}} 등의 {{site.data.keyword.openwhisk_short}} 솔루션을 사용하고자 할 수 있습니다. 사전에 리소스를 프로비저닝해야 할 필요 없이 워크로드의 수요를 실질적으로 충족시키기 위해 이 솔루션을 스케일링할 수 있습니다.

[Skylink](https://github.com/IBM-Bluemix/skylink)는 iPad를 사용하여 무인 항공기를 {{site.data.keyword.openwhisk_short}}, IBM {{site.data.keyword.cloudant_short_notm}}, IBM Watson 및 Alchemy Vision을 활용하는 거의 실시간의 이미지 분석 기능이 있는 IBM Cloud에 연결하는 샘플 애플리케이션입니다.

[BluePic](https://github.com/IBM-Swift/BluePic)은 사진을 찍고 이를 다른 BluePic 사용자와 공유하는 데 사용될 수 있는 사진 및 이미지 공유 애플리케이션입니다. 이 애플리케이션은 모바일 iOS 10 애플리케이션에서 Swift로 작성되었으며 이미지 데이터에 대해 {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.cos_full_notm}}를 사용하는 Kitura 기반 서버 애플리케이션을 활용하는 방법을 보여줍니다. 또한 AlchemyAPI는 이미지를 분석하고 이미지의 컨텐츠를 기반으로 텍스트 태그를 추출하며 최종적으로 푸시 알림을 사용자에게 전송하기 위해 {{site.data.keyword.openwhisk_short}} 시퀀스에서 사용됩니다.

## 데이터 처리
{: #data-processing}

이제 방대한 양의 데이터를 사용할 수 있으므로, 애플리케이션 개발에서는 새 데이터를 처리하고 잠재적으로 이에 반응하는 기능이 필요합니다. 이 요구사항에는 구조화되지 않은 문서, 이미지 또는 동영상은 물론 구조화된 데이터베이스 레코드의 처리가 모두 포함됩니다. {{site.data.keyword.openwhisk_short}}는 데이터의 변경에 반응하고 데이터의 수신 피드에서 자동으로 액션을 실행하도록 시스템 제공되거나 사용자 정의 피드에 의해 구성될 수 있습니다. 액션은 변경사항 처리, 데이터 형식 변환, 메시지 송수신, 기타 액션 호출 및 다양한 데이터 저장소 업데이트 등을 수행하도록 프로그래밍될 수 있습니다. 지원되는 데이터 저장소에는 SQL 기반 관계형 데이터베이스, 인메모리 데이터 그리드, NoSQL 데이터베이스, 파일, 메시징 브로커 및 기타 다양한 시스템이 포함됩니다. {{site.data.keyword.openwhisk_short}} 규칙 및 시퀀스는 프로그래밍 없이 처리 파이프라인에서 변경사항을 작성하는 유연성을 제공하며, 단순 구성 업데이트를 통해 수행됩니다. 데이터 저장소 옵션 및 낮은 관리 오버헤드로 인해 {{site.data.keyword.openwhisk_short}} 기반 시스템은 매우 기민하며 요구사항 변경에 손쉽게 적응할 수 있습니다.

[OpenChecks](https://github.com/krook/openchecks) 프로젝트는 광학 문자 인식을 사용하여 은행 계정에 수표 예치를 처리하는 데 {{site.data.keyword.openwhisk_short}}가 사용될 수 있는 방법을 보여주는 개념 증명(proof of concept)입니다. 이는 공용 {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.openwhisk_short}} 서비스에 빌드되며 {{site.data.keyword.cloudant}} 및 {{site.data.keyword.cos_full_notm}}에 의존합니다. 온프레미스의 경우, 이는 CouchDB 및 OpenStack Swift를 사용할 수 있습니다. 기타 스토리지 서비스에는 FileNet 또는 Cleversafe가 포함될 수 있습니다. Tesseract는 OCR 라이브러리를 제공합니다.

## 코그너티브
{: #openwhisk_common_use_cases_cognitive}

코그너티브 기술을 {{site.data.keyword.openwhisk_short}}와 효과적으로 결합하여 강력한 애플리케이션을 작성할 수 있습니다. 예를 들어, IBM Alchemy API 및 Watson Visual Recognition을 {{site.data.keyword.openwhisk_short}}와 함께 사용하여 따로 지켜보지 않고도 동영상에서 유용한 정보를 자동으로 추출할 수 있습니다. 이 기술은 앞에서 논의된 [데이터 처리](#data-processing) 유스 케이스의 “코그너티브” 확장기능입니다. {{site.data.keyword.openwhisk_short}}의 다른 유용한 용도는 코그너티브 서비스와 결합된 Bot 기능의 구현입니다.

샘플 애플리케이션인 [Dark Vision](https://github.com/IBM-Bluemix/openwhisk-darkvisionapp)이 제공되며 바로 이를 수행합니다. 이 애플리케이션에서 사용자는 {{site.data.keyword.cloudant_short_notm}} DB에 동영상이나 이미지를 저장하는 Dark Vision 웹 애플리케이션을 사용하여 이를 업로드합니다. 일단 동영상이 업로드된 경우, {{site.data.keyword.openwhisk_short}}는 {{site.data.keyword.cloudant_short_notm}} 변경사항(트리거)을 청취하여 새 동영상을 발견합니다. {{site.data.keyword.openwhisk_short}}는 동영상 추출기 액션을 트리거합니다. 이의 실행 중에 추출기는 프레임(이미지)를 생성하고 이를 {{site.data.keyword.cloudant_short_notm}}에 저장합니다. 그리고 프레임은 Watson Visual Recognition으로 처리되며 결과는 동일한 {{site.data.keyword.cloudant_short_notm}} DB에 저장됩니다. Dark Vision 웹 애플리케이션이나 iOS 애플리케이션을 사용하면 해당 결과를 볼 수 있습니다. {{site.data.keyword.cloudant_short_notm}}에 추가하여 {{site.data.keyword.cos_full_notm}}를 사용할 수 있습니다. 여기서 비디오와 이미지 메타데이터는 {{site.data.keyword.cloudant_short_notm}}에 저장되며, 미디어 파일은 {{site.data.keyword.cos_short}}에 저장됩니다.

{{site.data.keyword.openwhisk_short}}, IBM Mobile Analytics 및 Watson을 사용하여 어조를 분석하고 Slack 채널에 게시하는 [예제 iOS Swift 애플리케이션](https://github.com/gconan/BluemixMobileServicesDemoApp)이 사용 가능합니다.

## Kafka 또는 {{site.data.keyword.messagehub}}에서 이벤트 처리
{: #openwhisk_event_processing}

{{site.data.keyword.openwhisk_short}}를 Kafka, {{site.data.keyword.messagehub_full}}(Kafka 기반) 및 기타 메시징 시스템과 조합하여 사용하면 더할 나위 없이 좋습니다. 해당 시스템의 이벤트 구동 특성은 메시지 처리를 위해 이벤트 구동 런타임을 필요로 합니다. 런타임은 비즈니스 로직을 해당 메시지에 적용할 수 있으며, 이는 해당 피드, 트리거 및 액션으로 {{site.data.keyword.openwhisk_short}}에서 제공하는 내용과 정확히 일치합니다. Kafka 및 {{site.data.keyword.messagehub}}는 종종 방대하고 예측 불가능한 워크로드 볼륨에 사용되며, 해당 메시지의 이용자에게 즉각적인 확장성을 갖도록 요구합니다. 다시 말하면, 이 상황은 {{site.data.keyword.openwhisk_short}}의 스윗 스팟입니다. {{site.data.keyword.openwhisk_short}}에는 메시지 이용 및 [openwhisk-package-kafka](https://github.com/openwhisk/openwhisk-package-kafka) 패키지에서 제공되는 메시지 공개를 위한 기본 제공 기능이 있습니다.

[이벤트 처리 시나리오를 구현하는 예제 애플리케이션](https://github.com/IBM/openwhisk-data-processing-message-hub)은 {{site.data.keyword.openwhisk_short}}, {{site.data.keyword.messagehub}} 및 Kafka와 함께 제공됩니다.
