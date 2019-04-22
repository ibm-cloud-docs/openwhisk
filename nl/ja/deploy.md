---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: deploying actions, manifest, manifest file

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# マニフェスト・ファイルを使用したエンティティーのデプロイ
{: #deploy}

{{site.data.keyword.openwhisk_short}} を利用すると、YAML で作成されたマニフェスト・ファイルを使用して、名前空間のあらゆるエンティティーを記述してデプロイすることができます。このファイルを使用することによって、Functions のすべての[パッケージ](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#openwhisk_packages)、[アクション](/docs/openwhisk?topic=cloud-functions-openwhisk_actions#openwhisk_actions)、[トリガー、およびルール](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_triggers)を単一のコマンドでデプロイできます。

マニフェスト・ファイルには、グループとしてデプロイおよびアンデプロイするエンティティーのセットを記述します。マニフェスト・ファイルの内容は、[OpenWhisk デプロイメント YAML 仕様](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification)に準拠する必要があります。マニフェスト・ファイルを定義したら、このファイルを使用して Functions エンティティーのグループを、同じ Functions 名前空間または異なる Functions 名前空間にデプロイしたり再デプロイしたりできます。マニフェスト・ファイルに定義された Functions エンティティーをデプロイおよびアンデプロイするには、Functions プラグイン・コマンドの `ibmcloud fn deploy` と `ibmcloud fn undeploy` を使用することができます。

## Hello World API サンプルの作成
{: #deploy_helloworld_example}

このサンプルでは、ある単純な Node.js コード (`helloworld.js`) を使用し、Web アクション (`hello_world`) をパッケージ (`hello_world_package`) 内に作成し、そのアクションのための REST API を定義します。
{: shortdesc}

1. 次のコードを使用して `helloworld.js` ファイルを作成します。

```javascript
function main() {
   return {body: 'Hello world'};
}
```
{: codeblock}

デプロイメント・マニフェスト・ファイルで、以下の変数を定義します。
* パッケージ名。
* アクション名。
* Web アクションとなることを示すアクション・アノテーション。
* アクション・コード・ファイル名。
* 基本パスが `/hello` である API。
* `/world` のエンドポイント・パス。

2. `hello_world_manifest.yml` ファイルを作成します。

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

3. `deploy` コマンドを使用して、パッケージ、アクション、および API をデプロイします。

```sh
$ ibmcloud fn deploy --manifest hello_world_manifest.yml
```
{: pre}

アクション、パッケージ、および API をリストすることによって、必要な 3 つのエンティティーが正常に作成されたことを確認します。
{: shortdesc}

1. 次のコマンドを使用して、アクションをリストします。

```sh
$ ibmcloud fn action list
```
{: pre}

2. 次のコマンドを使用して、パッケージをリストします。

```sh
$ ibmcloud fn package list
```
{: pre}

3. 次のコマンドを使用して、API をリストします。
```sh
$ ibmcloud fn api list
```
{: pre}

4. API を呼び出します。

```sh
$ curl URL-FROM-API-LIST-OUTPUT
Hello World
```
{: codeblock}

同じエンティティーをアンデプロイするには、`undeploy` コマンドを使用します。

```sh
$ ibmcloud fn undeploy --manifest hello_world_manifest.yml
Success: Undeployment completed successfully.
```
{: codeblock}

## その他の OpenWhisk デプロイメント・サンプル
{: more_deploy_examples}

Functions のデプロイメントは、OpenWhisk デプロイメント・プロジェクトに基づいています。ここには、Functions 内で使用できる[複数のデプロイメント・マニフェストのサンプル](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples)が用意されています。`wskdeploy` の代わりに、`ibmcloud fn deploy` コマンドを使用することができます。

## デプロイメント・マニフェスト仕様
{: manifest_specification}

Functions のデプロイメント・マニフェストは、OpenWhisk デプロイメント・マニフェスト仕様に準拠する必要があります。詳しくは、[OpenWhisk デプロイメント・マニフェスト仕様の資料](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification)を参照してください。


