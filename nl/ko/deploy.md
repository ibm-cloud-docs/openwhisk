---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: deploying actions, manifest, manifest file

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

# Manifest 파일로 엔티티 배치
{: #deploy}

{{site.data.keyword.openwhisk_short}}를 사용하면 YAML에 기록된 Manifest 파일을 사용하여 모든 네임스페이스 엔티티를 설명하고 배치할 수 있습니다. 이 파일을 사용하여 단일 명령으로 모든 함수 [패키지](/docs/openwhisk?topic=cloud-functions-pkg_ov), [액션](/docs/openwhisk?topic=cloud-functions-actions), [트리거](/docs/openwhisk?topic=cloud-functions-triggers) 및 룰](/docs/openwhisk?topic=cloud-functions-rules)을 배치할 수 있습니다. 

Manifest 파일은 그룹으로 배치 및 배치 해제할 수 있는 엔티티 세트를 설명합니다. Manifest 파일 컨텐츠는 [OpenWhisk 배치 YAML 스펙](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification)을 따라야 합니다. 정의되고 나면 Manifest 파일을 사용하여 함수 엔티티 그룹을 같거나 다른 함수 네임스페이스에 배치하거나 배치 해제할 수 있습니다. 함수 플러그인 명령 `ibmcloud fn deploy` 및 `ibmcloud fn undeploy`를 사용하여 Manifest 파일에 정의된 함수 엔티티를 배치 및 배치 해제할 수 있습니다.

## Hello World API 예제 작성
{: #deploy_helloworld_example}

이 예에서는 일부 간단한 Node.js 코드(`helloworld.js`)를 사용하여 웹 액션(`hello_world`)을 패키지(`hello_world_package`)에 작성하고 이 액션에 대해 REST API를 정의합니다.
{: shortdesc}

1. 다음 코드로 `helloworld.js` 파일을 작성하십시오.

    ```javascript
function main() {
       return {body: 'Hello world'};
}
    ```
    {: codeblock}

    배치 Manifest 파일은 다음 변수를 정의합니다.
    * 패키지 이름.
    * 액션 이름.
    * 웹 액션임을 나타내는 액션 어노테이션.
    * 액션 코드 파일 이름.
    * 기본 경로가 `/hello`인 API.
    * `/world`의 엔드포인트 경로.

2. `hello_world_manifest.yml` 파일을 작성하십시오.

    ```yaml
packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
    ```
    {: codeblock}

3. `deploy` 명령을 사용하여 패키지, 액션 및 API를 배치하십시오.

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. 3개의 예상 엔티티가 성공적으로 작성되었는지 확인하기 위해 액션, 패키지 및 API를 나열할 수 있습니다.

    1. 다음 명령을 사용하여 액션을 나열하십시오.

      ```sh
    ibmcloud fn action list
      ```
      {: pre}

    2. 다음 명령을 사용하여 패키지를 나열하십시오.

      ```sh
    ibmcloud fn package list
      ```
      {: pre}

    3. 다음 명령을 사용하여 API를 나열하십시오.

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. API를 호출하십시오.

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

선택사항: `undeploy` 명령을 사용하여 동일한 엔티티를 배치 해제할 수 있습니다. 

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## 추가 OpenWhisk 배치 예제
{: more_deploy_examples}

함수 배치는 함수 내에서 사용될 수 있는 [다중 배치 Manifest 예](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples)에 있는 OpenWhisk 배치 프로젝트를 기반으로 합니다.  `wskdeploy` 대신 `ibmcloud fn deploy` 명령을 사용할 수 있습니다.

## Manifest 스펙 배치
{: manifest_specification}

함수 배치 Manifest는 OpenWhisk 배치 Manifest 스펙을 따라야 합니다. 자세한 내용은 [OpenWhisk 배치 Manifest 스펙 문서](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification)의 내용을 참조하십시오.
