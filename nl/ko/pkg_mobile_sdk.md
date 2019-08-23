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


# 모바일 SDK
{: #pkg_mobile_sdk}

OpenWhisk는 모바일 앱이 원격 트리거를 실행하고 원격 액션을 호출할 수 있도록 하는 iOS 및 watchOS 디바이스용 모바일 SDK를 제공합니다. Android용 버전은 사용할 수 없습니다. 따라서 Android 개발자는 OpenWhisk REST API를 직접 사용하십시오. 모바일 SDK는 Swift 4로 작성되며 iOS 11 이상의 릴리스를 지원합니다. Xcode 9을 사용하여 모바일 SDK를 빌드할 수 있습니다.
{: shortdesc}


모바일 SDK는 IAM 기반 네임스페이스에서 지원되지 않습니다. 대신 Cloud Foundry 기반 네임스페이스를 사용하십시오.
{: important}


## 앱에 SDK 추가

CocoaPods, Carthage를 사용하거나 소스 디렉토리에서 모바일 SDK를 설치할 수 있습니다.

### CocoaPods로 설치

모바일용 OpenWhisk SDK는 CocoaPods를 통해 공용 배포로 사용 가능합니다. CocoaPods가 설치되었다고 가정하고, 스타터 앱 프로젝트 디렉토리 내의 `Podfile`이라고 하는 파일에 다음 행을 추가하십시오.

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

명령행에서 `pod install`을 입력하십시오. 이 명령은 watchOS 확장의 iOS 앱용 SDK를 설치합니다. CocoaPods가 앱용으로 작성하는 작업공간 파일을 사용하여 Xcode의 프로젝트를 여십시오.

설치 후에 프로젝트 작업공간을 여십시오. 빌드 시에 다음 경고가 나타날 수 있습니다.
`Use Legacy Swift Language Version” (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
이는 CocoaPods가 Pods 프로젝트에서 Swift 버전을 업데이트하지 않은 경우 발생합니다. 이를 해결하려면 Pods 프로젝트 및 OpenWhisk 대상을 선택하십시오.  빌드 설정으로 이동하여 `Use Legacy Swift Language Version` 설정을 `no`로 변경하십시오. 또는 다음의 설치 후 지시사항을 Podfile의 끝에 추가할 수 있습니다.

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

### Carthage로 설치

앱의 프로젝트 디렉토리에서 파일을 작성하고 이름을 `Cartfile`로 지정하십시오. 파일에 다음 행을 추가하십시오.
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

명령행에서 `carthage update --platform ios`를 입력하십시오. Carthage는 SDK를 다운로드 및 빌드하고 앱의 프로젝트 디렉토리에서 Carthage라고 하는 디렉토리를 작성하며 `OpenWhisk.framework` 파일을 Carthage/build/iOS 내에 둡니다.

그리고 Xcode 프로젝트의 임베디드 프레임워크에 OpenWhisk.framework를 추가해야 합니다.

### 소스 코드에서 설치

소스 코드는 https://github.com/apache/incubator-openwhisk-client-swift에서 사용 가능합니다.
Xcode의 `OpenWhisk.xcodeproj`를 사용하여 프로젝트를 여십시오.
프로젝트에는 "OpenWhisk"(iOS를 대상으로 함) 및 "OpenWhiskWatch"(watchOS 2를 대상으로 함)의 두 개의 스킴이 포함됩니다.
필요한 대상의 프로젝트를 빌드하고 결과 프레임워크를 앱(일반적으로 "~/Library/Developer/Xcode/DerivedData/your app name"에 있음)에 추가하십시오.

## 스타터 앱 예제 설치

OpenWhisk CLI를 사용하여 OpenWhisk SDK 프레임워크를 임베드하는 예제 코드를 다운로드할 수 있습니다.

스타터 앱 예제를 설치하려면 다음 명령을 입력하십시오.
```
ibmcloud fn sdk install iOS
```
{: pre}

이 명령은 스타터 앱이 포함된 압축 파일을 다운로드합니다. 프로젝트 디렉토리에 Podfile이 있습니다.

SDK를 설치하려면 다음 명령을 입력하십시오.
```
pod install
```
{: pre}

## SDK 시작하기

빠르게 시작하고 실행하려면, OpenWhisk API 인증 정보로 `WhiskCredentials` 오브젝트를 작성하고 오브젝트에서 OpenWhisk 인스턴스를 작성하십시오.

예를 들어, 다음 예제 코드를 사용하여 인증 정보 오브젝트를 작성하십시오.
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

이전 예제에서는 OpenWhisk에서 가져온 `myKey` 및 `myToken`을 전달합니다. 다음 CLI 명령으로 키 및 토큰을 검색할 수 있습니다.
```
ibmcloud fn property get --auth
```
{: pre}

**출력**
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

콜론 앞의 문자열은 키이고 콜론 뒤의 문자열은 토큰입니다.

## OpenWhisk 액션 호출

원격 액션을 호출하기 위해 액션 이름으로 `invokeAction`을 호출할 수 있습니다. 사전을 사용하여 필요에 따라 매개변수를 액션으로 전달할 수 있습니다.

**예제**
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

## OpenWhisk 트리거 실행

원격 트리거를 실행하기 위해 `fireTrigger` 메소드를 호출하고 사전을 사용하여 필요에 따라 매개변수를 전달할 수 있습니다.

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

앞의 예제에서 `locationChanged`라는 트리거를 실행합니다.

## 결과를 리턴하는 액션 사용

액션이 결과를 리턴하면 `invokeAction` 호출에서 `hasResult`를 true로 설정하십시오. 액션의 결과는 응답 사전에서 리턴됩니다. 예를 들어, 다음과 같습니다.

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

기본적으로 SDK는 호출된 액션에서 생성되는 결과와 활성화 ID만 리턴합니다. HTTP 응답 상태 코드가 포함된 전체 응답 오브젝트의 메타데이터를 가져오려면 다음 설정을 사용하십시오.

```swift
whisk.verboseReplies = true
```
{: codeblock}

## SDK 구성

`baseURL` 매개변수를 사용하여 OpenWhisk의 서로 다른 설치에서 작동하도록 SDK를 구성할 수 있습니다. 예를 들어, 다음과 같습니다.

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

이 예제에서는 `http://localhost:8080`에서 실행 중인 설치를 사용합니다. `baseURL`을 지정하지 않은 경우, 모바일 SDK는 https://us-south.functions.cloud.ibm.com에서 실행 중인 인스턴스를 사용합니다.

특수 네트워크 처리가 필요한 경우에는 사용자 정의 NSURLSession을 전달할 수 있습니다. 예를 들어, 자체 서명된 인증서를 사용하는 자체 OpenWhisk 설치가 있을 수 있습니다.

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

### 규정된 이름 지원

모든 액션 및 트리거는 네임스페이스, 패키지 및 액션 또는 트리거 이름으로 구성된 완전한 이름을 가지고 있습니다. SDK는 사용자가 트리거를 실행하거나 액션을 호출할 때 매개변수로서 이러한 요소를 허용할 수 있습니다. 또한 SDK는 `/mynamespace/mypackage/nameOfActionOrTrigger`와 유사한 완전한 이름을 허용하는 함수를 제공합니다. 규정된 이름 문자열은 모든 OpenWhisk 사용자가 보유한 네임스페이스와 패키지에 대해 이름이 지정되지 않은 기본값을 지원합니다. 따라서 다음의 구문 분석 규칙이 적용됩니다.

- `qName = "foo"`는 결과적으로 `namespace = default`, `package = default`, `action/trrigger = "foo"`가 됨
- `qName = "mypackage/foo"`는 결과적으로 `namespace = default`, `package = mypackage`, `action/trigger = "foo"`가 됨
- `qName = "/mynamespace/foo"`는 결과적으로 `namespace = mynamespace`, `package = default`, `action/trigger = "foo"`가 됨
- `qName = "/mynamespace/mypackage/foo"`는 결과적으로 `namespace = mynamespace`, `package = mypackage`, `action/trigger = "foo"`가 됨

모든 기타 조합은 `WhiskError.QualifiedName` 오류를 유발합니다. 따라서 규정된 이름을 사용할 경우에는 "`do/try/catch`" 구성에서 호출을 랩핑해야 합니다.

### SDK 단추

편의상 SDK에는 액션을 호출할 수 있도록 하는 `UIButton`을 확장하는 `WhiskButton`이 포함되어 있습니다.  `WhiskButton`을 사용하려면 다음 예제를 따르십시오.

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



