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


# Mobil-SDK
{: #pkg_mobile_sdk}

OpenWhisk stellt ein Mobile-SDK für iOS- und watchOS-Geräte bereit, mit dem mobile Apps dazu eingerichtet werden können, ferne Auslöser zu aktivieren und ferne Aktionen aufzurufen. Da keine Version für Android verfügbar ist, können Android-Entwickler die OpenWhisk-REST-API direkt verwenden. Das Mobil-SDK wurde in Swift 4 geschrieben und unterstützt iOS 11 und höhere Releases. Sie können das Mobil-SDK mithilfe von Xcode 9 erstellen.
{: shortdesc}


Das Mobil-SDK wird für IAM-basierte Namensbereiche nicht unterstützt. Verwenden Sie stattdessen Cloud Foundry-basierte Namensbereiche.
{: important}


## SDK der App hinzufügen

Sie können das Mobil-SDK mithilfe von CocoaPods, Carthage oder aus dem Quellenverzeichnis installieren.

### Mit CocoaPods installieren

Das OpenWhisk-SDK für den mobilen Einsatz ist über CocoaPods für die öffentliche Verteilung verfügbar. Wenn CocoaPods installiert ist, fügen Sie die folgenden Zeilen in eine Datei mit dem Namen `Podfile` im Projektverzeichnis der Starter-App ein. 

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

Geben Sie über die Befehlszeile den Befehl `pod install` ein. Dieser Befehl installiert das SDK für eine iOS-App mit einer watchOS-Erweiterung. Verwenden Sie die Arbeitsbereichsdatei, die CocoaPods für Ihre App erstellt, um das Projekt in Xcode zu öffnen.

Öffnen Sie nach der Installation Ihren Projektarbeitsbereich. Bei der Erstellung kann folgende Warnmeldung angezeigt werden:
`Use Legacy Swift Language Version” (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
Dies geschieht, wenn CocoaPods die Swift-Version im Pods-Projekt nicht aktualisiert. Zur Behebung wählen Sie das Pods-Projekt und das OpenWhisk-Ziel aus.  Rufen Sie den Editor zum Erstellen der Einstellungen auf und ändern Sie die Einstellung `Bisherige Swift-Sprachversion verwenden` in `no`. Alternativ können Sie am Ende Ihrer Podfile die folgenden Nachinstallationsschritte hinzufügen:

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

### Mit Carthage installieren

Erstellen Sie eine Datei mit dem Namen `Cartfile` im Projektverzeichnis Ihrer App. Fügen Sie die folgende Zeile in die Datei ein:
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

Geben Sie über die Befehlszeile den Befehl `carthage update --platform ios` ein. Carthage lädt das SDK herunter und führt einen Build durch, erstellt ein Verzeichnis mit dem Namen 'Carthage' im Projektverzeichnis Ihrer App und fügt eine `OpenWhisk.framework`-Datei in Carthage/build/iOS ein.

Anschließend müssen Sie OpenWhisk.framework den eingebetteten Frameworks in Ihrem Xcode-Projekt hinzufügen.

### Aus Quellcode installieren

Quellcode ist unter https://github.com/apache/incubator-openwhisk-client-swift verfügbar.
Öffnen Sie mit Xcode ein Projekt unter Verwendung von `OpenWhisk.xcodeproj`.
Das Projekt enthält die beiden Schemas 'OpenWhisk' (für iOS) und 'OpenWhiskWatch' (für watchOS 2).
Erstellen Sie das Projekt (Build) für die Ziele, die Sie benötigen, und fügen Sie die resultierenden Frameworks Ihrer App (in der Regel in ~/Library/Developer/Xcode/DerivedData/Ihr App-Name) hinzu.

## Starter-App-Beispiel installieren

Sie können die OpenWhisk-CLI zum Herunterladen von Beispielcode verwenden, der das OpenWhisk-SDK-Framework einbettet.

Geben Sie den folgenden Befehl ein, um das Starter-App-Beispiel zu installieren:
```
ibmcloud fn sdk install iOS
```
{: pre}

Mit diesem Befehl wird eine komprimierte Datei mit der Starter-App heruntergeladen. Im Projektverzeichnis befindet sich eine Datei 'Podfile'. 

Geben Sie den folgenden Befehl ein, um das SDK zu installieren:
```
pod install
```
{: pre}

## Erste Schritte mit dem SDK

Um die Arbeit rasch aufnehmen zu können, erstellen Sie ein Objekt `WhiskCredentials` mit Ihren OpenWhisk-API-Berechtigungsnachweisen und erstellen Sie eine OpenWhisk-Instanz aus diesem Objekt. 

Sie können zum Beispiel folgenden Beispielcode zum Erstellen des Berechtigungsnachweisobjekts verwenden:
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

Im obigen Beispiel werden die aus OpenWhisk abgerufenen Parameter `myKey` und `myToken` übergeben. Sie können den Schlüssel (Key) und das Token mit dem folgenden CLI-Befehl abrufen:
```
ibmcloud fn property get --auth
```
{: pre}

**Ausgabe**
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

Die Zeichenfolge vor dem Doppelpunkt ist der Schlüssel, die Zeichenfolge nach dem Doppelpunkt das Token. 

## OpenWhisk-Aktion aufrufen

Zum Aufrufen einer fernen Aktion können Sie `invokeAction` mit dem Aktionsnamen aufrufen. Verwenden Sie ein Wörterverzeichnis (Dictionary), um Parameter nach Bedarf an die Aktion zu übergeben.

**Beispiel**
```swift
// In diesem Beispiel wird eine Aktion zur Ausgabe einer Nachricht an die OpenWhisk-Konsole aufgerufen.
var params = Dictionary<String, String>()
params["payload"] = "Hi from mobile"
do {
    try whisk.invokeAction(name: "helloConsole", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: false, callback: {(reply, error) -> Void in
        if let error = error {
            //Operationen ausführen
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

## OpenWhisk-Auslöser aktivieren

Zur Aktivierung eines fernen Auslösers können Sie die Methode `fireTrigger` aufrufen und Parameter nach Bedarf mithilfe eines Wörterverzeichnisses an die Methode übergeben.

```swift
// In diesem Beispiel wird ein Auslöser aktiviert, wenn die Position um einen bestimmten Betrag geändert wurde.
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

Im obigen Beispiel wird ein Auslöser mit dem Namen `locationChanged` aktiviert.

## Aktionen mit Rückgabe eines Ergebnisses verwenden

Wenn die Aktion ein Ergebnis zurückgibt, setzen Sie im Aufruf `invokeAction` den Parameter `hasResult` auf 'true'. Das Ergebnis der Aktion wird im Reply-Wörterverzeichnis zurückgegeben. Beispiel:

```swift
do {
    try whisk.invokeAction(name: "actionWithResult", package: "mypackage", namespace: "mynamespace", parameters: params, hasResult: true, callback: {(reply, error) -> Void in
        if let error = error {
            //Operationen ausführen
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

Standardmäßig gibt das SDK nur die Aktivierungs-ID und das durch die aufgerufene Aktion erzeugte Ergebnis zurück. Wenn Sie Metadaten des gesamten Antwortobjekts abrufen wollen, zu denen der HTTP-Antwortstatuscode gehört, verwenden Sie die folgende Einstellung:

```swift
whisk.verboseReplies = true
```
{: codeblock}

## SDK konfigurieren

Sie können das SDK mithilfe des Parameters `baseURL` so konfigurieren, dass es mit verschiedenen OpenWhisk-Installationen arbeitet. Beispiel:

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

In diesem Beispiel wird eine Installation verwendet, die unter `http://localhost:8080` ausgeführt wird. Wenn Sie `baseURL` nicht angeben, verwendet das Mobil-SDK die Instanz, die unter 'https://us-south.functions.cloud.ibm.com' ausgeführt wird. 

Sie können einen angepassten Parameter NSURLSession übergeben, wenn Sie eine besondere Netzverwaltung benötigen. Sie könnten zum Beispiel eine eigene OpenWhisk-Installation haben, die mit selbst signierten Zertifikaten arbeitet:

```swift
// Network-Delegate erstellen, der allen vertraut
class NetworkUtilsDelegate: NSObject, NSURLSessionDelegate {
    func URLSession(session: NSURLSession, didReceiveChallenge challenge: NSURLAuthenticationChallenge, completionHandler: (NSURLSessionAuthChallengeDisposition, NSURLCredential?) -> Void) {
        completionHandler(NSURLSessionAuthChallengeDisposition.UseCredential, NSURLCredential(forTrust: challenge.protectionSpace.serverTrust!))
    }
}
// NSURLSession erstellen, die den vertrauenden Delegate verwendet
let session = NSURLSession(configuration: NSURLSessionConfiguration.defaultSessionConfiguration(), delegate: NetworkUtilsDelegate(), delegateQueue:NSOperationQueue.mainQueue())
// Festlegen, dass das SDK diese urlSession anstelle der gemeinsam genutzten Standardsitzung verwendet
whisk.urlSession = session
```
{: codeblock}

### Unterstützung für qualifizierte Namen

Alle Aktionen und Auslöser haben einen vollständig qualifizierten Namen, der aus einem Namensbereich, einem Paketnamen und einem Aktions- bzw. Auslösernamen besteht. Das SDK kann diese Elemente als Parameter beim Aufruf einer Aktion bzw. Aktivieren eines Auslösers akzeptieren. Das SDK stellt zudem eine Funktion bereit, die einen vollständig qualifizierten Namen akzeptiert, der wie folgt aussieht: `/mynamespace/mypackage/nameOfActionOrTrigger`. Die Zeichenfolge für den qualifizierten Namen unterstützt unbenannte Standardwerte für Namensbereiche und Pakete, die alle OpenWhisk-Benutzer haben, sodass die folgenden Syntaxanalyseregeln gelten:

- `qName = "foo"` ergibt `namespace = default`, `package = default`, `action/trrigger = "foo"`
- `qName = "mypackage/foo"` ergibt `namespace = default`, `package = mypackage`, `action/trigger = "foo"`
- `qName = "/mynamespace/foo"` ergibt `namespace = mynamespace`, `package = default`, `action/trigger = "foo"`
- `qName = "/mynamespace/mypackage/foo"` ergibt `namespace = mynamespace`, `package = mypackage`, `action/trigger = "foo"`

Bei allen anderen Kombinationen wird ein Fehler `WhiskError.QualifiedName` ausgegeben. Daher müssen Sie bei Verwendung von qualifizierten Namen den Aufruf in ein Konstrukt "`do/try/catch`" einschließen.

### SDK-Schaltfläche

Aus Gründen der Verwendungsfreundlichkeit enthält das SDK eine Schaltfläche `WhiskButton`, die die Schaltfläche `UIButton` erweitert, sodass sie Aktionen aufrufen kann.  Gehen Sie zur Verwendung von `WhiskButton` wie im folgenden Beispiel vor:

```swift
var whiskButton = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButton.setupWhiskAction("helloConsole", package: "mypackage", namespace: "_", credentials: credentialsConfiguration!, hasResult: false, parameters: nil, urlSession: nil)
let myParams = ["name":"value"]
// Dies aufrufen, wenn ein Tastenereignis erkannt wird, z. B. in IBAction, um Aktion aufzurufen.
whiskButton.invokeAction(parameters: myParams, callback: { reply, error in
    if let error = error {
        print("Oh no, error: \(error)")
       } else {
        print("Success: \(reply)")
    }
})
// Alternativ können Sie eine eigenständige Schaltfläche einrichten, die selbst für Tastenereignisse empfangsbereit ist und eine Aktion aufruft.
var whiskButtonSelfContained = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButtonSelfContained.listenForPressEvents = true
do {
   // Verwendung der API für qualifizierte Namen erfordert do/try/catch.
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



