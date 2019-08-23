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


# 移动 SDK
{: #pkg_mobile_sdk}

OpenWhisk 提供了用于 iOS 和 watchOS 设备的移动 SDK，以支持移动应用程序触发远程触发器以及调用远程操作。Android 的版本不可用，因此 Android 开发者可直接使用 OpenWhisk REST API。移动 SDK 是用 Swift 4 编写的，支持 iOS 11 及更高发行版。可以使用 Xcode 9 来构建移动 SDK。
{: shortdesc}


基于 IAM 的名称空间不支持移动 SDK。请改为使用基于 Cloud Foundry 的名称空间。
{: important}


## 向应用程序添加 SDK

可以使用 CocoaPods、Carthage 或从源代码目录安装移动 SDK。

### 使用 CocoaPods 进行安装

OpenWhisk 移动 SDK 可通过 CocoaPods 进行公共分发。假定 CocoaPods 已安装，请将以下行放到入门模板应用程序项目目录中名为 `Podfile` 的文件中。

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

在命令行中，输入 `pod install`。此命令会安装用于具有 watchOS 扩展的 iOS 应用程序的 SDK。使用 CocoaPods 为应用程序创建的工作空间文件在 Xcode 中打开项目。

安装后，打开项目工作空间。构建时，您可能会收到以下警告：`必须为使用 Swift 的目标正确配置“使用旧 Swift 语言版本”(SWIFT_VERSION)。请使用 [编辑 > 转换 > 至当前 Swift 语法...] 菜单来选择 Swift 版本，或使用“构建设置”编辑器直接配置构建设置。`
导致此警告的原因是 CocoaPods 未更新 Pods 项目中的 Swift 版本。要解决此问题，请选择 Pods 项目和 OpenWhisk 目标。转至“构建设置”，并将`使用旧 Swift 语言版本`设置更改为`否`。或者，可以在 Podfile 末尾添加以下安装后指令：

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

### 使用 Carthage 进行安装

在应用程序的项目目录中创建文件并将其命名为 `Cartfile`。在文件中添加以下行：

```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

在命令行中，输入 `carthage update --platform ios`。Carthage 会下载并构建 SDK，在应用程序的项目目录中创建名为 Carthage 的目录，然后将 `OpenWhisk.framework` 文件放入 Carthage/build/iOS 中。

然后，您必须将 OpenWhisk.framework 添加到 Xcode 项目中的嵌入式框架

### 通过源代码安装

源代码在 https://github.com/apache/incubator-openwhisk-client-swift 中提供。使用 Xcode 中的 `OpenWhisk.xcodeproj` 打开项目。
该项目包含两个方案：“OpenWhisk”（用于 iOS）和“OpenWhiskWatch”（用于 watchOS 2）。
针对需要的目标构建项目，然后将生成的框架添加到应用程序（通常位于 ~/Library/Developer/Xcode/DerivedData/your app name 中）。

## 安装入门模板应用程序示例

可以使用 OpenWhisk CLI 来下载用于嵌入 OpenWhisk SDK 框架的示例代码。

要安装入门模板应用程序示例，请输入以下命令：

```
ibmcloud fn sdk install iOS
```
{: pre}

此命令会下载包含入门模板应用程序的压缩文件。项目目录包含 Podfile。

要安装 SDK，请输入以下命令：

```
pod install
```
{: pre}

## SDK 入门

要快速入门和熟悉运用，请使用 OpenWhisk API 凭证创建 `WhiskCredentials` 对象，然后通过该对象创建 OpenWhisk 实例。

例如，使用以下示例代码来创建凭证对象：
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

在上面的示例中，传入的是从 OpenWhisk 中获取的 `myKey` 和 `myToken`。可以使用以下 CLI 命令来检索密钥和令牌：
```
ibmcloud fn property get --auth
```
{: pre}

**输出**
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

冒号前面的字符串是密钥，冒号后面的字符串是令牌。

## 调用 OpenWhisk 操作

要调用远程操作，可以使用操作名称来调用 `invokeAction`。使用字典来根据需要将参数传递到该操作。

**例如**
```swift
// 在此示例中，我们将调用操作以将消息显示到 OpenWhisk 控制台
var params = Dictionary<String, String>()
params["payload"] = "Hi from mobile"
do {
    try whisk.invokeAction(name: "helloConsole", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: false, callback: {(reply, error) -> Void in
        if let error = error {
            //执行某些操作
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

## 触发 OpenWhisk 触发器

要触发远程触发器，可以调用 `fireTrigger` 方法，然后使用字典根据需要传入参数。

```swift
// 在此示例中，当位置发生特定量的变化时，将触发触发器
var locationParams = Dictionary<String, String>()
locationParams["payload"] = "{\"lat\":41.27093, \"lon\":-73.77763}"
do {try whisk.fireTrigger(name: "locationChanged", package: "mypackage", namespace: "mynamespace", parameters: locationParams, callback: {(reply, error) -> Void in
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

在上面的示例中，触发的是名为 `locationChanged` 的触发器。

## 使用可返回结果的操作

如果操作会返回结果，请在 `invokeAction` 调用中将 `hasResult` 设置为 true。这将在 reply 字典中返回操作的结果，例如：

```swift
do {try whisk.invokeAction(name: "actionWithResult", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: true, callback: {(reply, error) -> Void in
        if let error = error {
            //执行某些操作
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

缺省情况下，SDK 只会返回激活标识，以及所调用操作生成的任何结果。要获取整个响应对象的元数据（包括 HTTP 响应状态码），请使用以下设置：

```swift
whisk.verboseReplies = true
```
{: codeblock}

## 配置 SDK

可以通过 `baseURL` 参数将 SDK 配置为使用 OpenWhisk 的不同安装。例如：

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

在此示例中，使用的是在 `http://localhost:8080` 上运行的安装。如果未指定 `baseURL`，那么移动 SDK 将使用在 https://us-south.functions.cloud.ibm.com 上运行的实例。

可以传入定制 NSURLSession，以便在需要特殊网络处理的情况下使用。例如，您自己可能有使用自签名证书的 OpenWhisk 安装：

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

### 支持限定名

所有操作和触发器都具有由名称空间、包以及操作名称或触发器名称组成的标准名称。调用操作或触发触发器时，SDK 可以接受这些元素作为参数。SDK 还提供了接受类似 `/mynamespace/mypackage/nameOfActionOrTrigger` 的标准名称的函数。限定名字符串针对所有 OpenWhisk 用户具有的名称空间和包支持未指定缺省值，因此以下解析规则适用：

- `qName = "foo"` 生成的结果是 `namespace = default`，`package = default`，`action/trrigger = "foo"`
- `qName = "mypackage/foo"` 生成的结果是 `namespace = default`，`package = mypackage`，`action/trigger = "foo"`
- `qName = "/mynamespace/foo"` 生成的结果是 `namespace = mynamespace`，`package = default`，`action/trigger = "foo"`
- `qName = "/mynamespace/mypackage/foo"` 生成的结果是 `namespace = mynamespace`，`package = mypackage`，`action/trigger = "foo"`

其他所有组合都会发出 `WhiskError.QualifiedName` 错误。因此，使用限定名时，必须将调用包装在“`do/try/catch`”构造中。

### SDK 按钮

为了方便起见，SDK 包含 `WhiskButton`，用于扩展 `UIButton` 以允许它调用操作。要使用 `WhiskButton`，请遵循以下示例：

```swift
var whiskButton = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButton.setupWhiskAction("helloConsole", package: "mypackage", namespace: "_", credentials: credentialsConfiguration!, hasResult: false, parameters: nil, urlSession: nil)
let myParams = ["name":"value"]
// 检测到新闻事件（例如，在 IBAction 中）时调用此项以调用操作
whiskButton.invokeAction(parameters: myParams, callback: { reply, error in
    if let error = error {
        print("Oh no, error: \(error)")
       } else {
           print("Success: \(reply)")
    }
})
// 或者，您也可以设置“自包含”按钮，用于侦听自身上的新闻事件并调用操作
var whiskButtonSelfContained = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButtonSelfContained.listenForPressEvents = true
do {// use qualified name API which requires do/try/catch
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



