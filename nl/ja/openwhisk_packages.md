---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# パッケージ内のアクションの編成
{: #openwhisk_packages}

{{site.data.keyword.openwhisk}} では、パッケージを使用して関連するアクションをまとめ、そのパッケージを他のユーザーと共有することができます。
{: shortdesc}

パッケージは、*アクション* および*フィード* を含むことができます。
- アクションは、{{site.data.keyword.openwhisk_short}} で実行されるコードです。 例えば、{{site.data.keyword.cloudant}} パッケージには、{{site.data.keyword.cloudant_short_notm}} データベースのレコードの読み取りや書き込みのためのアクションが含まれています。
- フィードは、トリガー・イベントを起動するための外部イベント・ソースを構成するために使用されます。 例えば、Alarm パッケージには、指定された頻度でトリガーを起動できるフィードが含まれています。

パッケージも含めて各 {{site.data.keyword.openwhisk_short}} エンティティーは*名前空間* に属し、エンティティーの完全修飾名は `/namespaceName[/packageName]/entityName` です。 詳しくは、[命名のガイドライン](./openwhisk_reference.html#openwhisk_entities)を参照してください。

以降のセクションでは、パッケージの参照方法と、パッケージ内のトリガーおよびフィードの使用方法について説明します。 また、独自のパッケージをカタログに提供することに関心がある場合は、パッケージの作成と共有に関するセクションをお読みください。

## パッケージの参照
{: #browse-packages}

いくつかのパッケージが {{site.data.keyword.openwhisk_short}} に登録されています。 名前空間内のパッケージのリストを取得したり、パッケージ内のエンティティーをリストしたり、パッケージ内の個々のエンティティーの説明を取得したりできます。

1. `/whisk.system` 名前空間内のパッケージのリストを取得します。
  ```
  ibmcloud wsk package list /whisk.system
  ```
  {: pre}

  package list の出力:
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

2. `/whisk.system/cloudant` パッケージ内のエンティティーのリストを取得します。
  ```
  ibmcloud wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}

  出力例:
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  この出力は、{{site.data.keyword.cloudant_short_notm}} パッケージに、`read` と `write` という 2 つのアクションと、`changes` という 1 つのトリガー・フィードがあることを示しています。 `changes` フィードによって、指定された {{site.data.keyword.cloudant_short_notm}} データベースに文書が追加されるとトリガーが起動されます。

  {{site.data.keyword.cloudant_short_notm}} パッケージでは、パラメーター `username`、`password`、`host`、および `port` も定義されています。 アクションおよびフィードを有意味なものにするために、これらのパラメーターを指定する必要があります。 例えば、これらのパラメーターによって、アクションは特定の {{site.data.keyword.cloudant_short_notm}} アカウントで作動することができます。

3. `/whisk.system/cloudant/read` アクションの説明を取得します。
  ```
  ibmcloud wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  出力例:
  ```
  action /whisk.system/cloudant/read: Read document from database
     (params: dbname includeDoc id)
  ```
  {: screen}

  この出力は、{{site.data.keyword.cloudant_short_notm}} `read` アクションには、データベースおよび取得する文書 ID などの 3 つのパラメーターが必要であることを示しています。

## パッケージ内のアクションの呼び出し
{: #openwhisk_package_invoke}

他のアクションと同様に、パッケージ内のアクションを呼び出すことができます。 次のいくつかのステップでは、`/whisk.system/samples` パッケージ内の `greeting` アクションを異なるパラメーターを指定して呼び出す方法を示します。

1. `/whisk.system/samples/greeting` アクションの説明を取得します。
  ```
  ibmcloud wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  出力例:
  ```
  action /whisk.system/samples/greeting: Print a friendly greeting
     (params: name place)
  ```
  {: screen}

  `greeting` アクションには `name` と `place` という 2 つのパラメーターがあることに注目してください。

2. パラメーターを指定せずにアクションを呼び出します。
  ```
  ibmcloud wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  出力例:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  パラメーターが指定されなかったため、出力は汎用メッセージです。

3. パラメーターを指定してアクションを呼び出します。
  ```
  ibmcloud wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  出力例:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  この出力ではアクションに渡された `name` パラメーターおよび `place` パラメーターが使用されていることに注目してください。

## パッケージ・バインディングの作成と使用
{: #openwhisk_package_bind}

パッケージ内のエンティティーを直接使用することができますが、いつも同じパラメーターをアクションに渡している場合があります。 こういった処理を単純化するため、パッケージにバインドしてデフォルト・パラメーターを指定し、パッケージ内のアクションがそれを継承するようにできます。

例えば、`/whisk.system/cloudant` パッケージでは、`username`、`password`、および `dbname` のデフォルト値をパッケージ・バインディング内に設定できます。そうすると、それらの値がこのパッケージ内のどのアクションにも自動的に渡されるようになります。

`/whisk.system/samples` パッケージにバインドする単純な例を以下に示します。

1. `/whisk.system/samples` パッケージにバインドし、`place` パラメーターのデフォルト値を設定します。
  ```
  ibmcloud wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  出力例:
  ```
  ok: created binding valhallaSamples
  ```
  {: screen}

2. パッケージ・バインディングの説明を取得します。
  ```
  ibmcloud wsk package get --summary valhallaSamples
  ```
  {: pre}

  出力例:
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Returns a friendly greeting
   action /myNamespace/valhallaSamples/wordCount: Count words in a string
   action /myNamespace/valhallaSamples/helloWorld: Demonstrates logging facilities
   action /myNamespace/valhallaSamples/curl: Curl a host url
  ```
  {: screen}

  `/whisk.system/samples` パッケージ内のすべてのアクションが `valhallaSamples` パッケージ・バインディング内で使用可能であることに注目してください。

3. パッケージ・バインディング内のアクションを呼び出します。
  ```
  ibmcloud wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  出力例:
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  この結果では `valhallaSamples` パッケージ・バインディングを作成したときに設定した `place` パラメーターをアクションが継承していることに注目してください。

4. アクションを呼び出し、パラメーターのデフォルト値を上書きします。
  ```
  ibmcloud wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  出力例:
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  このアクション呼び出しで指定された `place` パラメーター値が、`valhallaSamples` パッケージ・バインディングに設定したデフォルト値を上書きしていることに注目してください。

## トリガー・フィードの作成と使用
{: #openwhisk_package_trigger}

フィードは、{{site.data.keyword.openwhisk_short}} トリガーへこれらのイベントを起動するための外部イベント・ソースを構成する簡便な方法を提供します。 次の例は、Alarms パッケージ内のフィードを使用して毎秒 1 つのトリガーを起動する方法と、ルールを使用して毎秒 1 つのアクションを呼び出す方法を示します。

1. `/whisk.system/alarms` パッケージ内のフィードの説明を取得します。
  ```
  ibmcloud wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  出力例:
  ```
  package /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud wsk action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  出力例:
  ```
  action /whisk.system/alarms/alarm: Fire trigger when alarm occurs
     (params: cron trigger_payload)
  ```
  {: screen}

  `/whisk.system/alarms/alarm` フィードには次の 2 つのパラメーターがあります。
  - `cron`: トリガーをいつ起動するのかについての crontab 指定。
  - `trigger_payload`: 各トリガー・イベントに設定するペイロード・パラメーター値。

2. 8 秒ごとに起動されるトリガーを作成します。
  ```
  ibmcloud wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  出力例:
  ```
  ok: created trigger feed everyEightSeconds
  ```
  {: screen}

3. 以下のアクション・コードを含んでいる **hello.js** という名前のファイルを作成します。
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. アクションが存在することを確認します。
  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

5. **everyEightSeconds** トリガーが起動されるたびに `hello` アクションを呼び出すルールを作成します。
  ```
  ibmcloud wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}

  出力例:
  ```
  ok: created rule myRule
  ```
  {: screen}

6. アクティベーション・ログをポーリングして、アクションが呼び出されていることを確認します。
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

  トリガー、ルール、およびアクションに関して、8 秒ごとにアクティベーションが観察されることを確認できます。 アクションは、すべての呼び出しでパラメーター `{"name":"Mork", "place":"Ork"}` を受け取ります。

## パッケージの作成
{: #openwhisk_packages_create}

パッケージは、関連するアクションおよびフィードの集合を編成するために使用されます。
パッケージを使用すると、パッケージ内のすべてのエンティティーでパラメーターを共有することもできます。

単純なアクションを 1 つ含んでいるカスタム・パッケージを作成する例を以下に示します。

1. **「custom」**という名前のパッケージを作成します。
  ```
  ibmcloud wsk package create custom
  ```
  {: pre}

  出力例:
  ```
  ok: created package custom
  ```
  {: screen}

2. パッケージのサマリーを取得します。
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  出力例:
  ```
  package /myNamespace/custom
  ```
  {: screen}

  パッケージは空であることに注目してください。

3. 以下のアクション・コードを含んでいる `identity.js` という名前のファイルを作成します。 このアクションは、すべての入力パラメーターを返します。
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. `custom` パッケージ内に、**identity** という名前のアクションを作成します。
  ```
  ibmcloud wsk action create custom/identity identity.js
  ```
  {: pre}

  出力例:
  ```
  ok: created action custom/identity
  ```
  {: screen}

  パッケージ内にアクションを作成するには、アクション名にパッケージ名の接頭部を付ける必要があります。 パッケージのネスティングは許可されません。 パッケージにはアクションのみを含めることができ、別のパッケージを含めることはできません

5. もう一度パッケージのサマリーを取得します。
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  出力例:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  今回は名前空間に **custom/identity** アクションがあることを確認できます。

6. パッケージ内のアクションを呼び出します。
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  出力例:
  ```
  {}
  ```
  {: screen}

パッケージ内のすべてのアクションで継承されるパッケージ・レベルのパラメーターを設定することによって、パッケージ内のすべてのエンティティーのデフォルト・パラメーターを設定できます。 この継承がどのように機能するのかを以下の例で示します。

1. 2 つのパラメーター **city** と `country` で `custom` パッケージを更新します。
  ```
  ibmcloud wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  出力例:
  ```
  ok: updated package custom
  ```
  {: screen}

2. **custom** パッケージおよび **identidy** アクションのパラメーターを表示し、パッケージ内の **identity** アクションがパッケージからパラメーターをどのように継承しているのかを確認します。
  ```
  ibmcloud wsk package get custom parameters
  ```
  {: pre}

  出力例:
  ```
  ok: got package custom, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

  ```
  ibmcloud wsk action get custom/identity parameters
  ```
  {: pre}

  出力例:
  ```
  ok: got action custom/identity, displaying field parameters

  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```
  {: screen}

3. **identity** アクションをパラメーターを指定せずに呼び出して、アクションが本当にパラメーターを継承することを確認します。
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  出力例:
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. 一部のパラメーターを指定して **identity** アクションを呼び出します。 呼び出しパラメーターはパッケージ・パラメーターとマージされます。その際、呼び出しパラメーターがパッケージ・パラメーターをオーバーライドします。
  ```
  ibmcloud wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  出力例:
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## パッケージの共有
{: #openwhisk_packages_share}

パッケージを構成するアクションおよびフィードのデバッグとテストが完了したら、そのパッケージをすべての {{site.data.keyword.openwhisk_short}} ユーザーで共有できます。 パッケージを共有すると、ユーザーはパッケージをバインドしたり、パッケージ内のアクションを呼び出したり、{{site.data.keyword.openwhisk_short}} ルールおよびシーケンス・アクションを作成したりできます。

1. すべてのユーザーでパッケージを共有します。
  ```
  ibmcloud wsk package update custom --shared yes
  ```
  {: pre}

  出力例:
  ```
  ok: updated package custom
  ```
  {: screen}

2. パッケージの `publish` プロパティーを表示して、このプロパティーが true になっていることを確認します。
  ```
  ibmcloud wsk package get custom publish
  ```
  {: pre}

  出力例:
  ```
  ok: got package custom, displaying field publish

  true
  ```
  {: screen}

これで、パッケージへのバインドや、パッケージ内のアクションを直接呼び出すことも含めて、**custom** パッケージの使用を他のユーザーが行えるようになりました。 他のユーザーは、パッケージのバインドやパッケージ内のアクションの呼び出しを行うために、パッケージの完全修飾名を知る必要があります。 共有パッケージ内のアクションおよびフィードは、_パブリック_ です。 パッケージがプライベートの場合は、そのコンテンツもすべてプライベートになります。

1. パッケージの説明を取得して、パッケージおよびアクションの完全修飾名を表示します。
  ```
  ibmcloud wsk package get --summary custom
  ```
  {: pre}

  出力例:
  ```
  package /myNamespace/custom
   action /myNamespace/custom/identity
  ```
  {: screen}

  上の例では、**myNamespace** 名前空間で作業していて、この名前空間が完全修飾名に含まれています。
