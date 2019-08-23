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


# Mobile SDK
{: #pkg_mobile_sdk}

OpenWhisk fournit un SDK pour applications mobiles destiné aux périphériques iOS et watchOS qui permet à ces applications de lancer des déclencheurs distants et d'appeler des actions à distance. Une version pour Android n'étant pas disponible, les développeurs Android peuvent utiliser directement l'API REST OpenWhisk. Le logiciel SDK pour applications mobiles est écrit en langage Swift 4 et prend en charge iOS 11 et les éditions ultérieures. Vous pouvez générer le logiciel SDK pour applications mobiles en utilisant Xcode 9.
{: shortdesc}


Le SDK pour applications mobiles n'est pas pris en charge pour les espaces de nom basés IAM. Utilisez à la place un espace de nom Cloud Foundry.
{: important}


## Ajout du logiciel SDK à votre application

Vous pouvez installer le logiciel SDK pour applications mobiles à l'aide de CocoaPods, Carthage, ou depuis le répertoire source.

### Installation à l'aide de CocoaPods

Le logiciel SDK OpenWhisk pour applications mobiles est disponible pour distribution publique via CocoaPods. En supposant que CocoaPods est installé, insérez les lignes suivantes dans un fichier nommé `Podfile` sous le répertoire des projets d'applications de démarrage.

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

Depuis la ligne de commande, entrez `pod install`. Cette commande installe le logiciel SDK pour une application iOS avec une extension watchOS. Utilisez le fichier d'espace de travail que CocoaPods a créé pour votre application afin d'ouvrir le projet dans Xcode.

Après l'installation, ouvrez l'espace de travail de votre projet. L'avertissement suivant peut s'afficher lors de la génération :
`Use Legacy Swift Language Version” (SWIFT_VERSION) is required to be configured correctly for targets which use Swift. Use the [Edit > Convert > To Current Swift Syntax…] menu to choose a Swift version or use the Build Settings editor to configure the build setting directly.`
Ce message apparaît si CocoaPods ne met pas à jour la version de Swift dans le projet Pods.  Pour résoudre ce problème, sélectionnez le projet Pods et la cible OpenWhisk.  Accédez aux paramètres de génération et associez le paramètre `Use Legacy Swift Language Version` à la valeur `no`. Vous pouvez aussi ajouter les instructions de postinstallation suivantes à la fin de votre fichier Pod :

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

### Installation à l'aide de Carthage

Créez un fichier dans le répertoire de projet de votre application et appelez-le `Cartfile`. Insérez la ligne suivante dans le fichier :
```
github "openwhisk/openwhisk-client-swift.git" ~> 0.3.0 # Or latest version
```
{: pre}

Depuis la ligne de commande, entrez `carthage update --platform ios`. Carthage télécharge et construit le SDK, crée un répertoire intitulé Carthage dans le répertoire de projets de votre application et place un fichier `OpenWhisk.framework` sous Carthage/build/iOS.

Vous devez ensuite ajouter l'infrastructure OpenWhisk.framework à celles intégrées dans votre projet Xcode.

### Installation depuis le code source

Le code source est disponible sur le site https://github.com/apache/incubator-openwhisk-client-swift.
Ouvrez le projet à l'aide de `OpenWhisk.xcodeproj` et de Xcode.
Le projet contient deux schémas, "OpenWhisk" et "OpenWhiskWatch", ciblant respectivement iOS et watchOS 2.
Générez le projet pour les cibles dont vous avez besoin et ajoutez les infrastructures résultantes à votre application (généralement, dans ~/Library/Developer/Xcode/DerivedData/nom de votre application).

## Installation de l'exemple d'application de démarrage

Vous pouvez utiliser l'interface CLI d'OpenWhisk pour télécharger l'exemple de code servant à intégrer l'infrastructure du SDK OpenWhisk.

Pour installer l'exemple d'application de démarrage, entrez la commande suivante :
```
ibmcloud fn sdk install iOS
```
{: pre}

Cette commande télécharge un fichier compressé contenant l'application de démarrage. Le répertoire de projet contient un fichier Podfile.

Pour installer le logiciel SDK, entrez la commande suivante :
```
pod install
```
{: pre}

## Initiation au logiciel SDK

Pour être opérationnel rapidement, créez un objet `WhiskCredentials` avec vos données d'identification pour l'API OpenWhisk et créez une instance OpenWhisk depuis l'objet.

Par exemple, utilisez l'exemple de code suivant pour créer un objet de données d'identification :
```
let credentialsConfiguration = WhiskCredentials(accessKey: "myKey", accessToken: "myToken")
let whisk = Whisk(credentials: credentialsConfiguration!)
```
{: pre}

Dans l'exemple précédent, vous transmettez les éléments `myKey` et `myToken` obtenus d'OpenWhisk. Vous pouvez extraire la clé et le jeton avec la commande de l'interface de ligne de commande suivante :
```
ibmcloud fn property get --auth
```
{: pre}

**Sortie**
```
whisk auth        kkkkkkkk-kkkk-kkkk-kkkk-kkkkkkkkkkkk:tttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttttt
```
{: pre}

La chaîne qui précède le signe deux-points correspond à votre clé et la chaîne figurant après le signe deux-points correspond à votre jeton.

## Appel d'une action OpenWhisk

Pour appeler une action distante, vous pouvez appeler `invokeAction` avec le nom d'action. Utilisez un dictionnaire pour transmettre des paramètres à l'action, selon les besoins.

**Exemple**
```swift
// Dans cet exemple, nous appelons une action pour imprimer un message dans la console OpenWhisk
var params = Dictionary<String, String>()
params["payload"] = "Hi from mobile"
do {
    try whisk.invokeAction(name: "helloConsole", package: "monpackage", namespace: "monEspaceNom", parameters: params, hasResult: false, callback: {(reply, error) -> Void in
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

## Exécution d'un déclencheur OpenWhisk

Pour exécuter un déclencheur distant, vous pouvez appeler la méthode `fireTrigger` et transmettre des paramètres à l'aide d'un dictionnaire, le cas échéant.

```swift
// Dans cet exemple, nous lançons un déclencheur lorsque notre emplacement a changé en excès de certains paramètres
var locationParams = Dictionary<String, String>()
locationParams["payload"] = "{\"lat\":41.27093, \"lon\":-73.77763}"
do {
    try whisk.fireTrigger(name: "locationChanged", package: "mypackage", namespace: "mynamespace", parameters: locationParams, callback: {(reply, error) -> Void in
        if let error = error {
            print("Error firing trigger \(error.localizedDescription)")
        } else {
            print("Déclencheur exécuté !")
        }
    })
} catch {
    print("Error \(error)")
}
```
{: codeblock}

Dans l'exemple précédent, vous exécutez un déclencheur appelé `locationChanged`.

## Utilisation d'actions qui renvoient un résultat

Si l'action renvoie un résultat, définissez `hasResult` sur true dans l'appel `invokeAction`. Le résultat de l'action est renvoyé dans le dictionnaire de réponse. Exemple :

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

Par défaut, le logiciel SDK renvoie seulement l'ID d'activation et tout résultat généré par l'action appelée. Pour obtenir les métadonnées de l'objet de réponse entier, qui inclut le code de statut de la réponse HTTP, utilisez le paramètre suivant :

```swift
whisk.verboseReplies = true
```
{: codeblock}

## Configuration du logiciel SDK

Vous pouvez configurer le SDK pour qu'il opère dans différentes installations d'OpenWhisk en utilisant le paramètre `baseURL`. Exemple :

```swift
whisk.baseURL = "http://localhost:8080"
```
{: codeblock}

Dans cet exemple, vous utilisez une installation qui s'exécute sur `http://localhost:8080`. Si vous ne spécifiez pas le paramètre `baseURL`, le logiciel SDK pour applications mobiles utilise l'instance qui s'exécute sur https://us-south.functions.cloud.ibm.com.

Vous pouvez transmettre un paramètre NSURLSession personnalisé si vous nécessitez un traitement réseau spécial. Par exemple, votre propre installation OpenWhisk pourrait utiliser des certificats autosignés :

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

### Prise en charge des noms qualifiés

Toutes les actions et tous les déclencheurs possèdent un nom qualifié complet qui est composé d'un espace de nom, d'un package et d'un nom d'action ou de déclencheur. Le logiciel SDK peut accepter ces éléments en tant que paramètres lorsque vous appelez une action ou exécutez un déclencheur. Le logiciel SDK fournit également une fonction qui accepte les noms qualifiés complets similaires à `/mon_espace_nom/mon_package/nom_action_ou_déclencheur`. La chaîne de nom qualifié prend en charge des valeurs par défaut sans nom pour les espaces de nom et les packages dont disposent tous les utilisateurs OpenWhisk, de sorte que les règles d'analyse suivantes s'appliquent :

- `qName = "foo"` results in `namespace = default`, `package = default`, `action/trrigger = "foo"`
- `qName = "mypackage/foo"` results in `namespace = default`, `package = mypackage`, `action/trigger = "foo"`
- `qName = "/mynamespace/foo"` results in `namespace = mynamespace`, `package = default`, `action/trigger = "foo"`
- `qName = "/mynamespace/mypackage/foo"` results in `namespace = mynamespace`, `package = mypackage`, `action/trigger = "foo"`

Toutes les autres combinaisons génèrent une erreur `WhiskError.QualifiedName`. Par conséquent, lorsque vous utilisez des noms qualifiés, vous devez encapsuler l'appel dans une construction "`do/try/catch`".

### Bouton du logiciel SDK

Pour des raisons pratiques, le logiciel SDK inclut un élément `WhiskButton` qui étend l'élément `UIButton` afin qu'il puisse appeler des actions.  Pour utiliser l'élément `WhiskButton`, suivez l'exemple ci-dessous.

```swift
var whiskButton = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButton.setupWhiskAction("helloConsole", package: "mypackage", namespace: "_", credentials: credentialsConfiguration!, hasResult: false, parameters: nil, urlSession: nil)
let myParams = ["name":"value"]
// Appelez cette action lorsque vous détectez un événement de clic (par ex. IBAction) pour appeler l'action
whiskButton.invokeAction(parameters: myParams, callback: { reply, error in
    if let error = error {
        print("Oh no, error: \(error)")
       } else {
        print("Success: \(reply)")
    }
})
// Vous pouvez aussi implanter un bouton "autonome" qui sera à l'écoute d'événements de clic et appellera une action
var whiskButtonSelfContained = WhiskButton(frame: CGRectMake(0,0,20,20))
whiskButtonSelfContained.listenForPressEvents = true
do {
   // Utiliser le nom qualifié d'API qui requiert do/try/catch
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



