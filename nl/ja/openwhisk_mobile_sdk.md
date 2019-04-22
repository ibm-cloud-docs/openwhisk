---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: mobile, sdk, cocoapods, carthage

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# モバイル SDK
{: #openwhisk_mobile_sdk}

OpenWhisk は、iOS デバイスおよび watchOS デバイス向けのモバイル SDK を提供しています。これを使用すると、モバイル・アプリは、リモート・トリガーの起動およびリモート・アクションの呼び出しを行うことができます。 Android 用のバージョンはありませんが、Android 開発者は、直接、OpenWhisk REST API を使用できます。 モバイル SDK は、Swift 4 で作成されており、iOS 11 以降のリリースをサポートしています。 Xcode 9 を使用してモバイル SDK をビルドできます。
{: shortdesc}



## アプリへの SDK の追加

モバイル SDK は、CocoaPods または Carthage を使用してインストールするか、ソース・ディレクトリーからインストールすることができます。

### CocoaPods を使用したインストール

モバイル用 OpenWhisk SDK は、CocoaPods を通して公開配布で入手できます。 CocoaPods がインストールされていることを前提として、スターター・アプリのプロジェクト・ディレクトリー内部の「Podfile」という名前のファイルに以下の行を入れます。

```ruby
install! 'cocoapods', :deterministic_uuids => false
use_frameworks!

target 'MyApp' do
     pod 'OpenWhisk', :git => 'https://github.com/apache/incubator-openwhisk-client-swift.git', :tag => '0.3.0'
end

target 'MyApp WatchKit Extension' do
     pod 'OpenWhisk', :git => 'https://github.com/apache/incubator-openwhisk-client-swift.git', :tag => '0.3.0'
end
```
{: codeblock}

コマンド・ラインから `pod install` と入力します。 このコマンドにより、watchOS 拡張機能が付いた iOS アプリ用の SDK がインストールされます。 CocoaPods によってアプリ用に作成されるワークスペース・ファイルを使用して、プロジェクトを Xcode で開きます。

インストール後に、プロジェクト・ワークスペースを開きます。 ビルド時に次のような警告を受け取ることがあります。
`Use Legacy Swift Language Version” (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
これは、Cocoapods が Pods プロジェクトで Swift バージョンを更新しない場合に発生します。  修正するには、Pods プロジェクトおよび OpenWhisk ターゲットを選択します。  「Build Settings」に移動し、設定 `Use Legacy Swift Language Version`を `no` に変更します。あるいは、Podfile の末尾に以下のインストール後の指示を追加します。

```ruby
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['SWIFT_VERSION'] = '4.0'
    end
  end
end
```
{: codeblock}

### Carthage を使用したインストール

アプリのプロジェクト・ディレクトリー内にファイルを作成して「Cartfile」という名前を付けます。 そのファイルに以下の行を入れます。
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

コマンド・ラインから `carthage update --platform ios` と入力します。 Carthage は、SDK をダウンロードおよびビルドし、Carthage という名前のディレクトリーをアプリのプロジェクト・ディレクトリー内に作成し、Carthage/build/iOS 内に `OpenWhisk.framework` ファイルを置きます。

次に、Xcode プロジェクトの組み込みフレームワークに OpenWhisk.framework を追加する必要があります。

### ソース・コードからのインストール

ソース・コードは https://github.com/apache/incubator-openwhisk-client-swift.git で入手可能です。
Xcode で `OpenWhisk.xcodeproj` を使用することによって、プロジェクトを開きます。
プロジェクトには、「OpenWhisk」 (iOS がターゲット) と「OpenWhiskWatch」 (watchOS 2 がターゲット) の 2 つのスキームが含まれます。
必要なターゲット用にプロジェクトをビルドし、結果のフレームワークをご使用のアプリに追加します (通常は ~/Library/Developer/Xcode/DerivedData/ご使用のアプリ名)。

## スターター・アプリ・サンプルのインストール

OpenWhisk CLI を使用して、OpenWhisk SDK フレームワークを組み込むサンプル・コードをダウンロードできます。  

スターター・アプリ・サンプルをインストールするには、次のコマンドを入力します。
```
ibmcloud fn sdk install iOS
```
{: pre}

このコマンドにより、スターター・アプリが入った圧縮ファイルがダウンロードされます。 プロジェクト・ディレクトリー内に podfile があります。

SDK をインストールするには、次のコマンドを入力します。
```
pod install
```
{: pre}

## SDK 入門

迅速に稼働中にするためには、OpenWhisk API 資格情報を使用して WhiskCredentials オブジェクトを作成し、そのオブジェクトから OpenWhisk インスタンスを作成します。

例えば、次のサンプル・コードを使用して資格情報オブジェクトを作成します。
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

上の例では、OpenWhisk から取得した `myKey` と `myToken` を受け渡します。 次の CLI コマンドでキーとトークンを取得できます。
```
ibmcloud fn property get --auth
```
{: pre}

出力:
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

コロンの前のストリングがキーで、コロンの後のストリングがトークンです。

## OpenWhisk アクションの呼び出し

リモート・アクションを呼び出すために、アクション名を指定して `invokeAction` を呼び出すことができます。 必要に応じて、ディクショナリーを使用してパラメーターをアクションに渡します。

例えば次のようにします。
```swift
// In this example, we are invoking an action to print a message to the OpenWhisk Console
var params = Dictionary<String, String>()
params["payload"] = "Hi from mobile"
do {
    try whisk.invokeAction(name: "helloConsole", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: false, callback: {(reply, error) -> Void in
        if let error = error {
            //do something
            print("Error invoking Action \(error.localizedDescription)")
        } else {
            print("Action invoked!")
        }
    })
} catch {
    print("Error \(error)")
}
```
{: codeblock}

## OpenWhisk トリガーの起動

リモート・トリガーを起動するために、`fireTrigger` メソッドを呼び出し、必要に応じてディクショナリーを使用してパラメーターを渡すことができます。

```swift
// In this example we are firing a trigger when our location has changed by a certain amount
var locationParams = Dictionary<String, String>()
locationParams["payload"] = "{\"lat\":41.27093, \"lon\":-73.77763}"
do {
    try whisk.fireTrigger(name: "locationChanged", package: "mypackage", namespace: "mynamespace", parameters: locationParams, callback: {(reply, error) -> Void in
        if let error = error {
            print("Error firing trigger \(error.localizedDescription)")
        } else {
            print("Trigger fired!")
        }
    })
} catch {
    print("Error \(error)")
}
```
{: codeblock}

前の例では、`locationChanged` という名前のトリガーを起動しています。

## 結果を返すアクションの使用

アクションが結果を返す場合、invokeAction 呼び出しで hasResult を true に設定します。 アクションの結果は、次の例に示すように、reply ディクショナリーに返されます。

```swift
do {
    try whisk.invokeAction(name: "actionWithResult", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: true, callback: {(reply, error) -> Void in
        if let error = error {
            //do something
            print("Error invoking Action \(error.localizedDescription)")
        } else {
            var result = reply["result"]
            print("Got result \(result)")
        }
    })
} catch {
    print("Error \(error)")
}
```
{: codeblock}

デフォルトでは、SDK はアクティベーション ID と、呼び出されたアクションによって生成された結果のみを返します。 HTTP 応答状況コードを含む完全な応答オブジェクトのメタデータを取得するには、次の設定を使用します。

```swift
whisk.verboseReplies = true
```
{: codeblock}

## SDK の構成

baseURL パラメーターを使用することによって、異なる OpenWhisk インストール済み環境で作業するように SDK を構成できます。 以下に例を示します。

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

この例では、http://localhost:8080 で実行されているインストール済み環境が使用されます。 baseUrl を指定しない場合、モバイル SDK は https://us-south.functions.cloud.ibm.com で実行されているインスタンスを使用します。

特殊なネットワーク処理が必要な場合、カスタム NSURLSession を渡すことができます。 例えば、自己署名証明書を使用する独自の OpenWhisk インストール済み環境がある場合などです。

```swift
// create a network delegate that trusts everything
class NetworkUtilsDelegate: NSObject, NSURLSessionDelegate {
    func URLSession(session: NSURLSession, didReceiveChallenge challenge: NSURLAuthenticationChallenge, completionHandler: (NSURLSessionAuthChallengeDisposition, NSURLCredential?) -> Void) {
        completionHandler(NSURLSessionAuthChallengeDisposition.UseCredential, NSURLCredential(forTrust: challenge.protectionSpace.serverTrust!))
    }
}
// create an NSURLSession that uses the trusting delegate
let session = NSURLSession(configuration: NSURLSessionConfiguration.defaultSessionConfiguration(), delegate: NetworkUtilsDelegate(), delegateQueue:NSOperationQueue.mainQueue())
// set the SDK to use this urlSession instead of the default shared one
whisk.urlSession = session
```
{: codeblock}

### 修飾名のサポート

すべてのアクションおよびトリガーには完全修飾名があり、完全修飾名は、名前空間、パッケージ、およびアクション名またはトリガー名からなります。 アクションが呼び出されるか、トリガーが起動されるときに、SDK はこれらの要素をパラメーターとして受け入れることができます。 SDK では、`/mynamespace/mypackage/nameOfActionOrTrigger` のような完全修飾名を受け入れる関数も提供されています。 修飾名のストリングは、すべての OpenWhisk ユーザーが持つ名前空間およびパッケージの未指定のデフォルト値をサポートします。したがって、以下の構文解析規則が適用されます。

- qName = "foo" の場合は、名前空間 = デフォルト、パッケージ = デフォルト、アクション/トリガー = "foo" となります。
- qName = "mypackage/foo" の場合は、名前空間 = デフォルト、パッケージ = mypackage、アクション/トリガー = "foo" となります。
- qName = "/mynamespace/foo" の場合は、名前空間 = mynamespace、パッケージ = デフォルト、アクション/トリガー = "foo" となります。
- qName = "/mynamespace/mypackage/foo" の場合は、名前空間 = mynamespace、パッケージ = mypackage、アクション/トリガー = "foo" となります。

他のすべての組み合わせでは WhiskError.QualifiedName エラーが発行されます。 そのため、修飾名を使用する際は、`do/try/catch` 構造で呼び出しをラップする必要があります。

### SDK ボタン

利便性のため、SDK には `WhiskButton` が含まれています。これはアクションの呼び出しを可能にするために `UIButton` を拡張したものです。  `WhiskButton` を使用するには、次の例に従います。

```swift
var whiskButton = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButton.setupWhiskAction("helloConsole", package: "mypackage", namespace: "_", credentials: credentialsConfiguration!, hasResult: false, parameters: nil, urlSession: nil)
let myParams = ["name":"value"]
// Call this when you detect a press event, e.g. in an IBAction, to invoke the action
whiskButton.invokeAction(parameters: myParams, callback: { reply, error in
    if let error = error {
        print("Oh no, error: \(error)")
    } else {
        print("Success: \(reply)")
    }
})
// or alternatively you can set up a "self contained" button that listens for press events on itself and invokes an action
var whiskButtonSelfContained = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButtonSelfContained.listenForPressEvents = true
do {
   // use qualified name API which requires do/try/catch
   try whiskButtonSelfContained.setupWhiskAction("mypackage/helloConsole", credentials: credentialsConfiguration!, hasResult: false, parameters: nil, urlSession: nil)
   whiskButtonSelfContained.actionButtonCallback = { reply, error in
       if let error = error {
           print("Oh no, error: \(error)")
       } else {
           print("Success: \(reply)")
       }
   }
} catch {
   print("Error setting up button \(error)")
}
```
{: codeblock}
