---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

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

# SDK mobile
{: #pkg_mobile_sdk}

OpenWhisk fornisce un SDK mobile per dispositivi iOS e watchOS che consente alle applicazioni mobili di attivare trigger remoti e richiamare azioni remote. Non è disponibile una versione per Android, pertanto gli sviluppatori Android possono utilizzare direttamente l'API REST di OpenWhisk. L'SDK mobile è scritto in Swift 4 e supporta iOS 11 e release successive. Puoi creare l'SDK mobile utilizzando Xcode 9.
{: shortdesc}



## Aggiungi l'SDK alla tua applicazione

Puoi installare l'SDK mobile utilizzando CocoaPods, Carthage oppure dalla directory di origine.

### Installa con CocoaPods

L'SDK OpenWhisk per i dispositivi mobili è disponibile per la distribuzione pubblica attraverso CocoaPods. Supponendo che CocoaPods sia installato, inserisci le seguenti righe in un file denominato 'Podfile' all'interno della directory del progetto dell'applicazione starter.

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

Nella riga di comando, digita `pod install`. Questo comando installa l'SDK per un'applicazione iOS con un'estensione watchOS. Utilizza il file spazio di lavoro creato da CocoaPods per la tua applicazione, per l'apertura del progetto in Xcode.

Al termine dell'installazione, apri lo spazio di lavoro del tuo progetto. Durante la fase di creazione, potresti visualizzare la seguente avvertenza:
`Use Legacy Swift Language Version” (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
Questo si verifica se Cocoapods non aggiorna la versione Swift nel progetto Pods.  Per correggere il problema, seleziona il progetto Pods e la destinazione OpenWhisk.  Vai a Build Settings e modifica l'impostazione `Use Legacy Swift Language Version` su `no`. In alternativa, puoi aggiungere le seguenti istruzioni di post installazione alla fine del tuo Podfile:

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

### Installa con Carthage

Crea un file nella directory del progetto della tua applicazione e denominalo 'Cartfile'. Inserisci la seguente riga nel file:
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

Nella riga di comando, digita `carthage update --platform ios`. Carthage scarica e crea l'SDK, crea una directory denominata Carthage della directory del progetto della tua applicazione e inserisce un file `OpenWhisk.framework` all'interno di Carthage/build/iOS.

Devi quindi aggiungere OpenWhisk.framework ai framework integrati nel tuo progetto Xcode

### Installa dal codice sorgente

Il codice sorgente è disponibile all'indirizzo https://github.com/apache/incubator-openwhisk-client-swift.git.
Apri il progetto utilizzando `OpenWhisk.xcodeproj` con Xcode.
Il progetto contiene due schemi: "OpenWhisk" (destinato a iOS) e "OpenWhiskWatch" (destinato a watchOS 2).
Crea il progetto per le destinazioni richieste e aggiungi i framework risultanti alla tua applicazione (solitamente in ~/Library/Developer/Xcode/DerivedData/nome tua applicazione).

## Installa l'esempio di applicazione starter

Puoi utilizzare la CLI OpenWhisk per scaricare il codice di esempio che incorpora il framework SDK OpenWhisk.

Per installare l'esempio di applicazione starter, immetti il seguente comando:
```
ibmcloud fn sdk install iOS
```
{: pre}

Questo comando scarica un file compresso che contiene l'applicazione starter. Nella directory del progetto è presente un podfile.

Per installare l'SDK, immetti il seguente comando:
```
pod install
```
{: pre}

## Introduzione all'SDK

Per iniziare rapidamente, crea un oggetto WhiskCredentials con le tue credenziali API OpenWhisk e crea un'istanza di OpenWhisk dall'oggetto.

Ad esempio, utilizza il seguente codice di esempio per creare un oggetto credenziali:
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

Nell'esempio precedente, passi `myKey` e `myToken` che ottieni da OpenWhisk. Puoi richiamare la chiave e il token con il seguente comando CLI:
```
ibmcloud fn property get --auth
```
{: pre}

Output:
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

Le stringhe prima dei due punti rappresentano la tua chiave e la stringa dopo i due punti rappresenta il token.

## Richiama un'azione OpenWhisk

Per richiamare un'azione remota, puoi chiamare `invokeAction` con il nome dell'azione. Utilizza un dizionario per passare i parametri all'azione secondo necessità.

Ad esempio:
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

## Attiva un trigger OpenWhisk

Per attivare un trigger remoto, puoi chiamare il metodo `fireTrigger` e passare i parametri secondo necessità utilizzando un dizionario.

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

Nell'esempio precedente, attivi un trigger denominato `locationChanged`.

## Utilizza azioni che restituiscono un risultato

Se l'azione restituisce un risultato, imposta hasResult su true nella chiamata invokeAction. Il risultato dell'azione viene restituito nel dizionario della risposta, ad esempio:

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

Per impostazione predefinita, l'SDK restituisce solo l'ID di attivazione e qualsiasi risultato prodotto dall'azione richiamata. Per ottenere i metadati dell'intero oggetto risposta, che include il codice di stato della risposta HTTP, utilizza la seguente impostazione:

```swift
whisk.verboseReplies = true
```
{: codeblock}

## Configurazione dell'SDK

Puoi configurare l'SDK per lavorare con diverse installazioni di OpenWhisk utilizzando il parametro baseURL. Ad esempio:

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

In questo esempio, utilizzi un'installazione in esecuzione in http://localhost:8080. Se non specifichi baseUrl, l'SDK mobile utilizza l'istanza in esecuzione in https://us-south.functions.cloud.ibm.com.

Puoi passare una NSURLSession personalizzata nel caso tu richieda una gestione della rete speciale. Ad esempio, potresti avere la tua installazione OpenWhisk che utilizza certificati autofirmati:

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

### Supporto per i nomi completi

Tutte le azioni e i trigger hanno un nome completo composto da uno spazio dei nomi, un pacchetto e un nome di azione o di trigger. L'SDK può accettare questi elementi come parametri quando richiami un'azione o attivi un trigger. L'SDK fornisce anche una funzione che accetta un nome completo che si presenta come `/mynamespace/mypackage/nameOfActionOrTrigger`. La stringa del nome completo supporta valori predefiniti senza nome per gli spazi dei nomi e i pacchetti che hanno tutti gli utenti di OpenWhisk, quindi si applicano le seguenti regole di analisi:

- qName = "foo" dà come risultato spazio dei nomi = default, pacchetto = default, azione/trigger = "foo"
- qName = "mypackage/foo" dà come risultato spazio dei nomi = default, pacchetto = mypackage, azione/trigger = "foo"
- qName = "/mynamespace/foo" dà come risultato spazio dei nomi = mynamespace, pacchetto = default, azione/trigger = "foo"
- qName = "/mynamespace/mypackage/foo dà come risultato spazio dei nomi = mynamespace, pacchetto = mypackage, azione/trigger = "foo"

Tutte le altre combinazioni generano un errore WhiskError.QualifiedName. Pertanto, se utilizzi dei nomi completi, devi racchiudere la chiamata in un costrutto "`do/try/catch`".

### Pulsante SDK

Per praticità, l'SDK include un `WhiskButton`, che estende l'`UIButton` per consentirgli di richiamare le azioni.  Per utilizzare il `WhiskButton`, attieniti a questo esempio:

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

