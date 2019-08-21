---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: mobile, sdk, cocoapods, carthage, functions

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


# 行動 SDK
{: #pkg_mobile_sdk}

OpenWhisk 提供適用於 iOS 及 watchOS 裝置的行動 SDK，讓行動應用程式發動遠端觸發程式以及呼叫遠端動作。沒有適用於 Android 的版本，因此 Android 開發人員可以直接使用 OpenWhisk REST API。行動 SDK 是以 Swift 4 撰寫而成，並且支援 iOS 11 以及更新版本。可以使用 Xcode 9 來建置行動 SDK。
{: shortdesc}


以 IAM 為基礎的名稱空間不支援行動 SDK。請改為使用以 Cloud Foundry 為基礎的名稱空間。
{: important}


## 將 SDK 新增至應用程式

您可以使用 CocoaPods 或 Carthage 或者從來源目錄中安裝行動 SDK。

### 使用 CocoaPods 進行安裝

適用於行動的 OpenWhisk SDK 可用於透過 CocoaPods 進行的公用配送。假設 CocoaPods 已安裝，請將下列行放到入門範本應用程式專案目錄中名為 `Podfile` 的檔案中。

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

從指令行鍵入 `pod install`。這個指令會安裝適用於具有 watchOS 副檔名的 iOS 應用程式的 SDK。使用 CocoaPods 為您的應用程式所建立的工作區檔案，在 Xcode 中開啟專案。

安裝之後，請開啟專案工作區。您可能會在建置時收到下列警告：
`Use Legacy Swift Language Version (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
導致此警告的原因是 CocoaPods 未更新 Pods 專案中的 Swift 版本。若要修正，請選取 Pods 專案及 OpenWhisk 目標。移至「建置設定」，並將 `Use Legacy Swift Language Version` 設定變更為 `no`。或者，您也可以在 Podfile 尾端新增下列後置安裝指示：

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

### 使用 Carthage 進行安裝

在應用程式的專案目錄中建立檔案並將其命名為 `Cartfile`。請在檔案中放入下一行：
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

從指令行鍵入 `carthage update --platform ios`。Carthage 會下載並建置 SDK，並在應用程式的專案目錄中建立稱為 Carthage 的目錄，然後將 `OpenWhisk.framework` 檔案放入 Carthage/build/iOS 中。

然後，您必須將 OpenWhisk.framework 新增至 Xcode 專案中的內嵌架構

### 從原始碼進行安裝

原始碼在 https://github.com/apache/incubator-openwhisk-client-swift 中提供。使用 Xcode，以透過使用 `OpenWhisk.xcodeproj` 來開啟專案。專案包含兩個方法："OpenWhisk"（目標設為 iOS）及 "OpenWhiskWatch"（目標設為 watchOS 2）。建置所需目標的專案，以及將產生的架構新增至應用程式（通常是在 ~/Library/Developer/Xcode/DerivedData/your app name 中）。

## 安裝入門範本應用程式範例

您可以使用 OpenWhisk CLI 來下載內嵌 OpenWhisk SDK 架構的範例程式碼。

若要安裝入門範本應用程式範例，請輸入下列指令：
```
ibmcloud fn sdk install iOS
```
{: pre}

這個指令會下載包含入門範本應用程式的壓縮檔。專案目錄包含 Podfile。

若要安裝 SDK，請輸入下列指令：
```
pod install
```
{: pre}

## 開始使用 SDK

若要快速入門和熟悉運用，請使用 OpenWhisk API 認證建立 `WhiskCredentials` 物件，然後透過該物件建立 OpenWhisk 實例。

例如，使用下列範例程式碼來建立認證物件：
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

在前一個範例中，您傳入從 OpenWhisk 取得的 `myKey` 及 `myToken`。您可以使用下列 CLI 指令來擷取金鑰及記號：
```
ibmcloud fn property get --auth
```
{: pre}

**輸出**
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

冒號前面的字串是金鑰，冒號後面的字串是記號。

## 呼叫 OpenWhisk 動作

若要呼叫遠端動作，您可以使用動作名稱來呼叫 `invokeAction`。請視需要使用字典將參數傳遞給動作。

**例如**
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

## 發動 OpenWhisk 觸發程式

若要發動遠端觸發程式，您可以使用字典來呼叫 `fireTrigger` 方法，並視需要傳入參數。

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

在前一個範例中，您是發動稱為 `locationChanged` 的觸發程式。

## 使用會傳回結果的動作

如果動作會傳回結果，請在 `invokeAction` 呼叫中將 `hasResult` 設定為 true。回覆字典中會傳回動作的結果，例如：

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

SDK 預設只會傳回啟動 ID 以及所呼叫動作所產生的任何結果。若要取得整個回應物件的 meta 資料（包括 HTTP 回應狀態碼），請使用下列設定：

```swift
whisk.verboseReplies = true
```
{: codeblock}

## 配置 SDK

可以使用 `baseURL` 參數將 SDK 配置為使用 OpenWhisk 的不同安裝。例如：

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

在此範例中，使用的是在 `http://localhost:8080` 上執行的安裝。如果未指定 `baseURL`，則行動 SDK 將使用在 https://us-south.functions.cloud.ibm.com 上執行的實例。

如果您需要特殊網路處理，則可以傳入自訂 NSURLSession。例如，您可能自己有使用自簽憑證的 OpenWhisk 安裝：

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

### 完整名稱支援

所有動作及觸發程式的完整名稱都是由名稱空間、套件及動作或觸發程式名稱所構成。呼叫動作或發動觸發程式時，SDK 可以接受這些元素作為參數。SDK 也提供接受類似 `/mynamespace/mypackage/nameOfActionOrTrigger` 之完整名稱的函數。完整名稱字串支援所有 OpenWhisk 使用者都有的名稱空間及套件的未命名預設值，因此適用下列剖析規則：

- `qName = "foo"` 導致的結果是 `namespace = default`，`package = default`，`action/trrigger = "foo"`
- `qName = "mypackage/foo"` 導致的結果是 `namespace = default`，`package = mypackage`，`action/trigger = "foo"`
- `qName = "/mynamespace/foo"` 導致的結果是 `namespace = mynamespace`，`package = default`，`action/trigger = "foo"`
- `qName = "/mynamespace/mypackage/foo"` 導致的結果是 `namespace = mynamespace`，`package = mypackage`，`action/trigger = "foo"`

其他所有組合都會發出 `WhiskError.QualifiedName` 錯誤。因此，當您使用完整名稱時，必須使用 "`do/try/catch`" 建構括住呼叫。

### SDK 按鈕

為方便起見，SDK 包含 `WhiskButton`，以擴充 `UIButton` 容許它呼叫動作。若要使用 `WhiskButton`，請遵循此範例：

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



