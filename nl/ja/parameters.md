---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# パラメーターの処理

パッケージおよびアクションでデプロイメントのパラメーターを設定する方法と、呼び出し時にアクションにパラメーターを渡す方法について説明します。 各パラメーターを個々にコマンド・ラインで指定するのではなく、ファイルを使用してパラメーターを保管し、そのファイル名をアクションに渡すこともできます。
{: shortdesc}

サーバーレス・アクションでは、main サーバーレス関数の引数として宣言されたパラメーターをアクションに追加することによって、データが供給されます。 すべてのデータはこの方法で供給され、値を設定する方法はいくつかあります。 最初のオプションは、アクションまたはパッケージが作成 (または更新) されるときにパラメーターを指定する方法です。 このオプションは、すべての実行で同じままであるデータ (これは他のプラットフォームでの環境変数と等価です) や、呼び出し時にオーバーライドされる可能性のあるデフォルト値の場合に便利です。 2 つ目のオプションは、アクションの呼び出し時にパラメーターを指定する方法であり、前に設定されたパラメーターがオーバーライドされます。

## 呼び出し時のアクションへのパラメーターの引き渡し
{: #pass-params-action}

アクションが呼び出されるときにパラメーターを渡すことができます。 以下の例では JavaScript が使用されていますが、他のすべての言語でも同じように機能します。 詳細な例を参照するには、[Javascript アクション](./openwhisk_actions.html#creating-and-invoking-javascript-actions)、[Swift アクション](./openwhisk_actions.html#creating-swift-actions)、[Python アクション](./openwhisk_actions.html#creating-python-actions)、[Java アクション](./openwhisk_actions.html#creating-java-actions)、[PHP アクション](./openwhisk_actions.html#creating-php-actions)、[Docker アクション](./openwhisk_actions.html#creating-docker-actions)、または [Go アクション](./openwhisk_actions.html#creating-go-actions)についてのトピックを確認してください。

1. アクションでパラメーターを使用します。 例えば、以下の内容を含んでいる **hello.js** という名前のファイルを作成します。
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  入力パラメーターが 1 つの JSON オブジェクト・パラメーターとして **main** 関数に渡されます。 この例で、`name` パラメーターと `place` パラメーターが `params` オブジェクトからどのように取り出されるのかに注意してください。

2. **hello** アクションを更新して、使用する準備ができている状態にします。
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

  非サービス資格情報パラメーターを変更する場合、新しいパラメーターを指定して `action update` コマンドを実行すると、現在は存在しているが `action update` コマンドで指定されていないパラメーターはすべて削除されます。 例えば、`action update -p key1 new-value -p key2 new-value` を実行するが、設定されていた他のすべてのパラメーターを省略すると、それらのパラメーターはこのアクションが更新された後は存在しなくなります。 アクションにバインドされていたサービスも削除されるので、他のパラメーターを更新した後、[アクションへのサービスのバインド](./binding_services.html)をもう一度行う必要があります。
  {: tip}

3. パラメーターは、コマンド・ラインを使用するか、必要なパラメーターを含んでいる[ファイルを提供](./parameters.html#using-parameter-files)することによって明示的に指定できます。

  コマンド・ラインを使用して直接パラメーターを渡すには、以下に示すように、`--param` フラグにキー/値のペアを指定します。
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **応答:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  `--result` オプションの使用に注意してください。これは、CLI がアクティベーションの完了を待機してから、結果のみを表示しているブロッキング呼び出しを示しています。 便宜上、自動的に推定される `--blocking` を指定せずにこのオプションを使用することができます。

  また、コマンド・ラインで指定されたパラメーター値が有効な JSON である場合、それらのパラメーター値は構文解析され、1 つの構造化オブジェクトとしてアクションに送信されます。

  例えば、**hello** アクションを次のように更新します。
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  これで、アクションは単一の `person` パラメーターに `name` フィールドと `place` フィールドがあることを予期するようになります。

  次に、以下の例のように、有効な JSON である単一の `person` パラメーターを指定してアクションを呼び出します。
  ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **応答:**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  CLI が自動的に `person` パラメーター値を構文解析して、アクションが今回予期している構造化オブジェクトにするため、結果は同じです。

## アクションでのデフォルト・パラメーターの設定
{: #default-params-action}

複数の名前付きパラメーターを指定してアクションを呼び出すことができます。 例えば、上の例の **hello** アクションは、個人を表す *name* パラメーターと出身地を表す *place* の 2 つのパラメーターを予期しています。

アクションに毎回すべてのパラメーターを渡すのではなく、特定のパラメーターをバインドすることができます。 次の例は、*place* パラメーターをバインドして、アクションがデフォルトで場所「Kansas」を設定するようにします。

1. `--param` オプションを使用してパラメーター値をバインドするか、パラメーターを含むファイルを `--param-file` に渡してアクションを更新します。 ファイルを使用する例については、[パラメーター・ファイルの使用](./parameters.html#using-parameter-files)に関するセクションを参照してください。

  デフォルト・パラメーターを明示的にコマンド・ラインに指定するには、以下に示すように、`param` フラグにキー/値のペアを指定してください。
  ```
  ibmcloud fn action update hello --param place Kansas
  ```
  {: pre}

2. 今回は `name` パラメーターのみを渡してアクションを呼び出します。
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy
  ```
  {: pre}

  出力例:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  アクションを呼び出すときに place パラメーターを指定する必要がなかったことに注意してください。 パラメーターをバインドしても、呼び出し時にそのパラメーターの値を指定すれば上書きできます。

3. `name` と `place` の両方の値を渡してアクションを呼び出し、出力を確認します。

  `--param` フラグを使用してアクションを呼び出します。
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  出力例:
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  作成または更新時にアクションに設定されたパラメーターは、呼び出し時に直接指定されるパラメーターによって常にオーバーライドされます。
  {: tip}

## パッケージでのデフォルト・パラメーターの設定
{: #default-params-package}

パラメーターをパッケージ・レベルで設定して、以下の場合を_除いて_、アクションのデフォルト・パラメーターとして使用できます。

- アクション自体にデフォルト・パラメーターがある。
- アクションの呼び出し時にパラメーターが指定される。複数のパラメーターがある場合は常にこれが優先されます。

以下の例では、**MyApp** パッケージでデフォルト・パラメーター `name` が設定され、それをアクションが使用しています。

1. パラメーターが設定されたパッケージを作成します。

  ```
  ibmcloud fn package update MyApp --param name World
  ```
  {: pre}

2. **MyApp** パッケージ内にアクションを作成します。
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  以下のように、アクションを作成します。
  ```
  ibmcloud fn action update MyApp/hello hello.js
  ```
  {: pre}

3. アクションを呼び出し、使用されるデフォルト・パッケージ・パラメーターを確認します。
  ```
  ibmcloud fn action invoke --result MyApp/hello
  ```
  {: pre}

  出力例:
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## パラメーター・ファイルの使用
{: #using-parameter-files}

パラメーターを JSON 形式でファイルに入れ、`--param-file` フラグでファイル名を指定することによって、それらのパラメーターを渡すことができます。 この方法は、パッケージおよびアクションの作成 (または更新) と、アクション呼び出し時の両方で使用できます。

1. 例として、前の例の **hello** で、以下の内容の `hello.js` を使用することを考えてみましょう。

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. 更新された `hello.js` の内容でアクションを更新します。

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. JSON 形式のパラメーターを含んでいる `parameters.json` という名前のパラメーター・ファイルを作成します。

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. **hello** アクションを呼び出すときに `parameters.json` ファイル名を使用し、出力を確認します。

  ```
  ibmcloud fn action invoke --result hello --param-file parameters.json
  ```

  出力例:
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
