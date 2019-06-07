---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-07"

keywords: mobile, sdk, cocoapods, carthage

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


# Mobile SDK
{: #pkg_mobile_sdk}

OpenWhisk provides a mobile SDK for iOS and watchOS devices that enables mobile apps to fire remote triggers and invoke remote actions. A version for Android is not available, so Android developers can use the OpenWhisk REST API directly. The mobile SDK is written in Swift 4 and supports iOS 11 and later releases. You can build the mobile SDK using Xcode 9.
{: shortdesc}



## Add the SDK to your app

You can install the mobile SDK by using CocoaPods, Carthage, or from the source directory.

### Install with CocoaPods

The OpenWhisk SDK for mobile is available for public distribution through CocoaPods. Assuming CocoaPods is installed, put the following lines into a file called 'Podfile' inside the starter app project directory.

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

From the command line, type `pod install`. This command installs the SDK for an iOS app with a watchOS extension. Use the workspace file CocoaPods creates for your app to open the project in Xcode.

After installation, open your project workspace. You might get the following warning when building:
`Use Legacy Swift Language Version” (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
This is caused if Cocoapods does not update the Swift version in the Pods project.  To fix, select the Pods project and the OpenWhisk target.  Go to Build Settings and change the setting `Use Legacy Swift Language Version` to `no`. Alternatively, you can add the following post installation instructions at the end of you Podfile:

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

### Install with Carthage

Create a file in your app's project directory and name it 'Cartfile'. Put the following line in the file:
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

From the command line, type `carthage update --platform ios`. Carthage downloads and builds the SDK, creates a directory that is called Carthage in your app's project directory, and puts an `OpenWhisk.framework` file inside Carthage/build/iOS.

You must then add OpenWhisk.framework to the embedded frameworks in your Xcode project

### Install from source code

Source code is available at https://github.com/apache/incubator-openwhisk-client-swift.
Open the project by using the `OpenWhisk.xcodeproj` with Xcode.
The project contains two schemes: "OpenWhisk" (targeted for iOS) and "OpenWhiskWatch" (targeted for watchOS 2).
Build the project for the targets that you need and add the resulting frameworks to your app (usually in ~/Library/Developer/Xcode/DerivedData/your app name).

## Install the starter app example

You can use the OpenWhisk CLI to download example code that embeds the OpenWhisk SDK framework.

To install the starter app example, enter the following command:
```
ibmcloud fn sdk install iOS
```
{: pre}

This command downloads a compressed file that contains the starter app. Inside the project directory is a Podfile.

To install the SDK, enter the following command:
```
pod install
```
{: pre}

## Getting started with the SDK

To get up and running quickly, create a `WhiskCredentials` object with your OpenWhisk API credentials and create an OpenWhisk instance from the object.

For example, use the following example code to create a credentials object:
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

In previous example, you pass in the `myKey` and `myToken` that you get from OpenWhisk. You can retrieve the key and token with the following CLI command:
```
ibmcloud fn property get --auth
```
{: pre}

Output:
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

The strings before the colon is your key, and the string after the colon is your token.

## Invoke an OpenWhisk action

To invoke a remote action, you can call `invokeAction` with the action name. Use a dictionary to pass parameters to the action as needed.

For example:
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

## Fire an OpenWhisk trigger

To fire a remote trigger, you can call the `fireTrigger` method, and pass in parameters as needed by using a dictionary.

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

In the previous example, you are firing a trigger that is called `locationChanged`.

## Use actions that return a result

If the action returns a result, set `hasResult` to true in the `invokeAction` call. The result of the action is returned in the reply dictionary, for example:

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

By default, the SDK returns only the activation ID and any result that is produced by the invoked action. To get metadata of the entire response object, which includes the HTTP response status code, use the following setting:

```swift
whisk.verboseReplies = true
```
{: codeblock}

## Configuring the SDK

You can configure the SDK to work with different installations of OpenWhisk by using the `baseURL` parameter. For instance:

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

In this example, you use an installation that is running at http://localhost:8080. If you do not specify the `baseURL`, the mobile SDK uses the instance that is running at https://us-south.functions.cloud.ibm.com.

You can pass in a custom NSURLSession in case you require special network handling. For example, you might have your own OpenWhisk installation that uses self-signed certificates:

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

### Support for qualified names

All actions and triggers have a fully qualified name that is made up of a namespace, a package, and an action or trigger name. The SDK can accept these elements as parameters when you are invoking an action or Firing a trigger. The SDK also provides a function that accepts a fully qualified name that looks like `/mynamespace/mypackage/nameOfActionOrTrigger`. The qualified name string supports unnamed default values for namespaces and packages that all OpenWhisk users have, so the following parsing rules apply:

- `qName = "foo"` results in `namespace = default`, `package = default`, `action/trrigger = "foo"`
- `qName = "mypackage/foo"` results in `namespace = default`, `package = mypackage`, `action/trigger = "foo"`
- `qName = "/mynamespace/foo"` results in `namespace = mynamespace`, `package = default`, `action/trigger = "foo"`
- `qName = "/mynamespace/mypackage/foo"` results in `namespace = mynamespace`, `package = mypackage`, `action/trigger = "foo"`

All other combinations issue a `WhiskError.QualifiedName` error. Therefore, when you are using qualified names, you must wrap the call in a "`do/try/catch`" construct.

### SDK button

For convenience, the SDK includes a `WhiskButton`, which extends the `UIButton` to allow it to invoke actions.  To use the `WhiskButton`, follow this example:

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

