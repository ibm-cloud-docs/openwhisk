---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: runtimes, support

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# 런타임

## 액션 런타임
액션을 다양한 프로그래밍 언어로 코딩하고 실행할 수 있습니다(예: Javascript, Python 등). . 사용 가능한 런타임 환경은 다음 섹션에서 볼 수 있습니다.

다음 링크에서는 각 지역의 IBM Cloud Functions에 대해 사용 가능한 런타임을 보여주는 JSON 응답을 리턴합니다. 

응답의 `runtimes` 섹션에는 사용 가능한 런타임 세트가 포함되어 있습니다. 

  - [us-south](https://us-south.functions.cloud.ibm.com/)
  - [us-east](https://us-east.functions.cloud.ibm.com/)
  - [eu-gb](https://eu-gb.functions.cloud.ibm.com/)
  - [eu-de](https://eu-de.functions.cloud.ibm.com/)

`image` 섹션에는 [DockerHub](https://hub.docker.com/) 및 사용되는 태그에 대한 런타임 이미지 이름이 포함되어 있습니다.  

다음 예는 `ibmfunctions/action-nodejs-v10` 및 `openwhisk/nodejs6action` 이미지를 가리킵니다.
태그는 `1.9.0` 같은 버전 번호이거나 `b99d71e` 같은 짧은 형식의 GIT 커미트 해시일 수 있습니다.

예제 이미지 필드.
  ```
  image:   "ibmfunctions/action-nodejs-v10:1.9.0"
  ```
  ```
  image:   "openwhisk/nodejs6action:b99d71e"
  ```

- 액션 런타임은 정기적으로 업데이트됩니다. 이러한 업데이트에는 런타임 내의 패키지에 대한 보안 수정사항 및 부 버전 업데이트가 포함됩니다. 부 버전 업데이트 시에는 이전 버전과의 호환성이 중단될 수 있습니다. 런타임 업데이트는 사용자 액션에 영향을 줄 수 있습니다. 동일한 유형의 새 런타임에 대한 자동 마이그레이션이 없습니다. 
- 더 이상 사용되지 않는 런타임에 대한 액션은 런타임이 지원되는 런타임으로 업데이트될 때까지 성공적으로 완료할 수 없습니다. 실패한 액션에 대한 문제점 해결 시 런타임이 더 이상 사용되지 않는지 여부를 식별하려면 쿼리 응답에서 `deprecated=true`를 확인하십시오. 런타임을 업데이트하려면 [액션 런타임 변경](/docs/openwhisk?topic=cloud-functions-openwhisk_managing#changing-action-runtime)의 내용을 참조하십시오. 

## JavaScript 런타임 환경
{: #openwhisk_ref_javascript_environments}

JavaScript 액션은 Node.js 버전 8 또는 10에서 실행할 수 있습니다. 

Node.js 버전 6은 기본 버전이지만 2018년 12월 6일부로 더 이상 사용되지 않습니다. JavaScript 액션을 계속해서 사용하려면 Node.js 버전 8 또는 10으로 업데이트하십시오.
{: deprecated}

### IBM SDK의 Node.js 버전 10 환경
{: #openwhisk_ref_javascript_environments_10}
Node.js 버전 10 환경은 액션을 작성하거나 업데이트할 때 `--kind` 플래그가 `nodejs:10`의 값으로 명시적으로 지정된 경우에 사용됩니다. 

#### `nodejs:8`에서 `nodejs:10`으로 마이그레이션 
- `ibm_db` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. `ibm_db` 패키지는 Node.js 10을 지원하지 않습니다. [issue ibmdb/node-ibm_db/issues/482](https://github.com/ibmdb/node-ibm_db/issues/482#issuecomment-436895541)에서 이 문제의 진행상태를 추적할 수 있습니다.
- `cloudant` npm 패키지는 `nodejs:10`에서 사용할 수 없으며 더 이상 사용되지 않습니다. nodejs 모듈을 가져올 때 공식 npm 패키지 [@cloudant/cloudant](https://www.npmjs.com/package/@cloudant/cloudant) v3.0.0을 사용해야 합니다(예를 들어 `require('@cloudant/cloudant')` [v3.x도 Promises만 리턴함](https://github.com/cloudant/nodejs-cloudant/blob/master/api-migration.md#2x--3x)).
- `cradle` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. 
- `log4js` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. 이 문제는 [log4js-node/issues/805](https://github.com/log4js-node/log4js-node/issues/805)에서 추적할 수 있습니다.
- `watson-developer-cloud` npm 패키지는 `nodejs:10`에서 사용할 수 없습니다. 새 버전의 진행상태는 [watson-developer-cloud/node-sdk/issues/780](https://github.com/watson-developer-cloud/node-sdk/issues/780)에서 추적할 수 있습니다.

nodejs 버전 10 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs10/CHANGELOG.md)에서 찾을 수 있습니다.

### IBM SDK의 Node.js 버전 8 환경
{: #openwhisk_ref_javascript_environments_8}
Node.js 버전 8 환경은 액션을 작성하거나 업데이트할 때 `--kind` 플래그가 `nodejs:8`의 값으로 명시적으로 지정된 경우에 사용됩니다. 

Node.js 버전 8은 유지보수 모드에 있으며 2019년 12월까지 사용 가능합니다. [Node.js 릴리스 스케줄](https://github.com/nodejs/Release)의 내용을 참조하십시오.
{: deprecated}
 
Node.js 버전 8 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-nodejs/blob/master/nodejs8/CHANGELOG.md)에서 찾을 수 있습니다.

### Node.js 버전 6 환경(더 이상 사용되지 않음)
{: #openwhisk_ref_javascript_environments_6}
Node.js 버전 6은 기본 버전이지만 더 이상 사용되지 않습니다. JavaScript 액션을 계속해서 사용하려면 Node.js 버전 8 또는 10으로 업데이트하십시오.
{: deprecated}

nodejs 버전 6 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-nodejs/blob/master/core/nodejs6Action/CHANGELOG.md)에서 찾을 수 있습니다.

## Python 런타임 환경
{: #openwhisk_ref_python_environments}

OpenWhisk는 두 개의 서로 다른 런타임 버전을 사용하여 Python 액션의 실행을 지원합니다.

### Python 3.7 액션(Debian Stretch 기반)
{: #openwhisk_ref_python_environments_3.7}

Python 3.7 액션은 Python 3.7.x에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 `wsk` CLI 매개변수 `--kind python:3.7`을 지정하십시오. 

런타임에는 Python 3.7 표준 라이브러리에 외에 Python 액션에서 사용할 수 있는 IBM Cloud 서비스에 대한 SDK 패키지가 포함되어 있습니다.

Python 3.7 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.7/CHANGELOG.md)에서 찾을 수 있습니다.

### Python 3.6 액션(Debian Jessie 기반)
{: #openwhisk_ref_python_environments_3.6}

Python 3 액션은 Python 3.6.x에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 `wsk` CLI 매개변수 `--kind python:3.6`을 지정하십시오. 

런타임에는 Python 3.6 표준 라이브러리에 외에 Python 액션에서 사용할 수 있는 IBM Cloud 서비스에 대한 SDK 패키지가 포함되어 있습니다.

Python 3.6 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/ibm-functions/runtime-python/blob/master/python3.6/CHANGELOG.md)에서 찾을 수 있습니다.

### Python 2 액션

Python 2 액션은 액션을 작성하거나 업데이트할 때 `--kind` 플래그를 지정하지 않는 한 Python 2.7.15에서 실행됩니다. 이 런타임을 명시적으로 선택하려면 `--kind python:2`를 사용하십시오.

virtualenv를 사용하여 python 액션을 작성할 때는 Docker 이미지 `openwhisk/python2action`을 사용하십시오.
Python 2.7 표준 라이브러리 외에 다음 패키지를 Python 2 액션에서 사용할 수 있습니다.

Python 2 런타임 환경에 대한 자세한 정보는 [CHANGELOG.md](https://github.com/apache/incubator-openwhisk-runtime-python/blob/master/core/python2Action/CHANGELOG.md)에서 찾을 수 있습니다.

## Swift 액션
{: #swift-actions}

Swift 3.1.1 및 4.1 런타임은 2019년 2월 28일부로 더 이상 사용되지 않습니다.
새 액션을 시작하거나 `swift:4.2` 유형 및 새 컴파일 프로세스를 사용하여 Swift 4.2 런타임에 기존 액션을 마이그레이션하십시오.
{: tip}

### Swift 3
Swift 3 액션은 Swift 3.1.1 `--kind swift:3.1.1`을 사용하여 실행됩니다. 이전 Swift 버전이 지원되지 않으므로 항상 `swift:3.1.1` 유형을 지정하십시오.

`swift:3.1.1` 유형을 사용하려면 모든 Swift 액션을 마이그레이션해야 합니다. 액션을 작성하거나 업데이트할 때 항상 특정 유형을 제공하는 것이 가장 좋습니다.
{: tip}

Swift 3.1.1 액션은 단일 Swift 소스 파일을 사용할 때 다음 패키지를 사용할 수 있습니다.
- KituraNet 버전 1.7.6, https://github.com/IBM-Swift/Kitura-net
- SwiftyJSON 버전 15.0.1, https://github.com/IBM-Swift/SwiftyJSON
- Watson Developer Cloud SDK 버전 0.16.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 4
Swift 4 액션은 `--kind swift:4.1` 또는 `--kind swift:4.2`를 각각 사용하는 Swift 4.1 또는 4.2를 사용하여 실행될 수 있습니다.
기본 `--kind swift:default`는 Swift 4.2입니다.

Swift 4.x 액션 런타임에는 패키지가 포함되어 있지 않습니다. Package.swift를 사용하여 종속 항목을 포함하도록 [패키징된 swift 액션](/docs/openwhisk?topic=cloud-functions-creating-swift-actions#packaging-an-action-as-a-swift-executable)에 대한 지시사항을 따르십시오. 

Swift 4.1 액션은 단일 Swift 소스 파일을 사용할 때 다음 패키지를 사용할 수 있습니다.
- Watson Developer Cloud SDK 버전 0.38.1, https://github.com/watson-developer-cloud/swift-sdk

Swift 4.2 액션은 단일 Swift 소스 파일을 사용할 때 다음 패키지를 사용할 수 있습니다.
- Watson Developer Cloud SDK 버전 1.2.0, https://github.com/watson-developer-cloud/swift-sdk

### Swift 3.1.1을 Swift 4.1로 마이그레이션

#### 단일 소스 액션 파일을 사용한 SwiftyJSON
컴파일되지 않은 `swift:3.1.1` 액션이 있고 **SwiftyJSON** 패키지를 사용하는 경우, 액션을 사전 컴파일하고 `swift:4.2` 유형의 액션에 사용하려는 SwiftyJSON의 버전을 지정하십시오. JSON 데이터 관리를 위한 개선사항이 있는 Swift 4.1로 시작할 것을 고려하십시오.

## PHP 액션
{: #openwhisk_ref_php}

PHP 7.1 및 7.2은 2019년 1월 11일부로 더 이상 사용되지 않습니다. PHP 액션을 계속하려면 PHP 7.3으로 업데이트하십시오.
{: deprecated}

PHP 액션은 PHP 7.3.0에서 실행됩니다. 이 런타임을 사용하려면 액션을 작성하거나 업데이트할 때 `wsk` CLI 매개변수 `--kind php:7.3`을 지정하십시오. `.php` 확장자가 있는 파일로 액션을 작성할 때는 이 작동이 기본값입니다.

PHP 7.1 및 7.2 런타임은 더 이상 사용되지 않습니다. 지연 시간 개선과 보다 빠른 엔드 투 엔드 실행을 위해서는 모든 액션을 PHP 7.3으로 마이그레이션하십시오. 

표준 확장자에 추가하여 다음의 PHP 확장자를 사용할 수 있습니다.

- bcmath
- curl
- gd
- intl
- mbstring
- mysqli
- pdo_mysql
- pdo_pgsql
- pdo_sqlite
- soap
- zip

## Docker 액션
{: #openwhisk_ref_docker}

Docker 액션은 Docker 컨테이너에서 사용자 제공 2진을 실행합니다. 2진이 [python:3.6-alpine](https://hub.docker.com/r/library/python) 기반의 Docker 이미지에서 실행되므로, 2진은 이 배포와 호환 가능해야 합니다.

Docker 스켈레톤은 OpenWhisk 호환 가능 Docker 이미지를 빌드하는 편리한 방법입니다. `ibmcloud fn sdk install docker` CLI 플러그인 명령을 사용하여 스켈레톤을 설치할 수 있습니다.

기본 2진 프로그램은 컨테이너 내부의 `/action/exec`에 있어야 합니다. 실행 파일은 `JSON` 오브젝트로서 역직렬화될 수 있는 단일 명령행 인수 문자열을 통해 입력 인수를 수신합니다. 이는 직렬화된 `JSON`의 단일행 문자열로서 `stdout`을 사용하여 결과를 리턴해야 합니다.

`dockerSkeleton`에 포함된 `Dockerfile`을 수정하여 컴파일 단계나 종속 항목을 포함할 수 있습니다.
