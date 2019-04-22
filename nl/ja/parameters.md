---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# パラメーターの処理

サーバーレス・アクションでは、パラメーターをアクションに追加することによってデータが供給されます。パラメーターは、main サーバーレス関数の引数として宣言されます。
{: shortdesc}

パラメーターの値を指定する方法として、次の 2 つがあります。
* **呼び出しの際にアクションにパラメーターを渡す**: アクションを呼び出すときに、CLI フラグまたはファイルを介してパラメーターを指定します。呼び出し時に指定されるパラメーターは、以前に設定されたデフォルトのパラメーターをオーバーライドします。
* **アクションまたはパッケージにパラメーターをバインドする**: アクションまたはパッケージが作成または更新されるときにデフォルト・パラメーターを設定します。このオプションは、すべての実行で同じままであるデータ (これは他のプラットフォームでの環境変数と等価です) や、呼び出し時にオーバーライドされる可能性のあるデフォルト値の場合に便利です。

## 呼び出し時のアクションへのパラメーターの引き渡し
{: #pass-params-action}

アクションが呼び出されるときにパラメーターを渡すことができます。

1. 以下のコードを `hello.js` という名前のファイルに保存します。

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. `hello` アクションを作成します。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. このアクションを以前使用したことがある場合は、以前に設定されたパラメーターをアクションから消去するために、更新してください。
```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. `name` および `place` パラメーターを渡してアクションを呼び出します。
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **注**: 代わりに、JSON 形式のパラメーターのファイルを渡すこともできます。
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    出力例:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

5. 構造化オブジェクトによってパラメーターをアクションに渡すこともできます。例えば、`hello` アクションを次のように更新します。
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    これで、アクションは単一の `person` パラメーターに `name` フィールドと `place` フィールドがあることを予期するようになります。

6. 有効な JSON オブジェクトである単一の `person` パラメーターを指定してアクションを呼び出します。
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    出力例:
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

## アクションへのパラメーターのバインド
{: #default-params-action}

複数の名前付きパラメーターを指定してアクションを呼び出すことができます。 例えば、基礎となる `hello` アクションは、個人を表す `name` と出身地を表す `place` の 2 つのパラメーターを予期しています。

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

アクションに毎回すべてのパラメーターを渡すのではなく、デフォルトのパラメーターをアクションにバインドすることもできます。以下のステップでは、`place` パラメーターを基礎となる `hello` アクションにバインドして、アクションのデフォルトの場所が「Kansas」になるようにします。

1. 以下のコードを `hello.js` という名前のファイルに保存します。

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. `hello` アクションを作成します。
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. `--param` フラグとキー/値のペアを使用して、パラメーター値をバインドするようにアクションを更新します。

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **注**: 代わりに、JSON 形式のパラメーターのファイルを渡すこともできます。
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    非サービス資格情報パラメーターを変更する場合、新しいパラメーターを指定して `action update` コマンドを実行すると、現在は存在しているが `action update` コマンドで指定されていないパラメーターはすべて削除されます。 例えば、`action update -p key1 new-value -p key2 new-value` を実行するが、設定されていた他のすべてのパラメーターを省略すると、それらのパラメーターはこのアクションが更新された後は存在しなくなります。 アクションにバインドされていたサービスも削除されるので、他のパラメーターを更新した後、[アクションへのサービスのバインド](/docs/openwhisk?topic=cloud-functions-binding_services)をもう一度行う必要があります。
    {: tip}

4. `name` パラメーターのみを渡してアクションを呼び出します。
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

    アクションを呼び出したときに `place` パラメーターを指定しなかったので、バインドされているデフォルトのパラメーター値 `Kansas` が使用されます。

5. バインドされたパラメーターは、呼び出し時にそのパラメーターの値を指定すれば上書きできます。`name` および `place` の両方を渡してアクションを呼び出します。
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

## パッケージへのパラメーターのバインド
{: #default-params-package}

デフォルトのパラメーターは、パッケージ・レベルで設定することもできます。以下の場合を除いて、バインドされたパラメーターは、パッケージ内のアクションのデフォルト・パラメーターとして機能します。

- アクション自体にデフォルト・パラメーターがある
- アクションに呼び出し時に指定されたパラメーターがある

以下の例では、`MyApp` パッケージでデフォルト・パラメーター `name` が設定され、それをアクションが使用しています。

1. パッケージを作成して、それにデフォルトのパラメーター `name` をバインドします。
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. 以下のコードを `helloworld.js` という名前のファイルに保存します。

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. `MyApp` パッケージ内にアクションを作成します。
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    非サービス資格情報パラメーターを変更する場合、新しいパラメーターを指定して `action update` コマンドを実行すると、現在は存在しているが `action update` コマンドで指定されていないパラメーターはすべて削除されます。 例えば、`action update -p key1 new-value -p key2 new-value` を実行するが、設定されていた他のすべてのパラメーターを省略すると、それらのパラメーターはこのアクションが更新された後は存在しなくなります。 アクションにバインドされていたサービスも削除されるので、他のパラメーターを更新した後、[アクションへのサービスのバインド](/docs/openwhisk?topic=cloud-functions-binding_services)をもう一度行う必要があります。
    {: tip}

3. パラメーターを何も渡さずにアクションを呼び出します。
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

    パッケージにバインドされたデフォルトのパラメーターが使用されます。
