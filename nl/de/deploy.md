---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: deploying actions, manifest, manifest file

subcollection: cloud-functions

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Entitäten mit einer Manifestdatei bereitstellen
{: #deploy}

Mit {{site.data.keyword.openwhisk_short}} können Sie alle Ihre Namensbereichsentitäten mithilfe einer in YAML geschriebenen Manifestdatei beschreiben und bereitstellen. Sie können diese Datei verwenden, um alle Ihre Functions-[Pakete](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#openwhisk_packages), [Aktionen](/docs/openwhisk?topic=cloud-functions-openwhisk_actions#openwhisk_actions), [Auslöser und Regeln](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_triggers) mit einem einzigen Befehl bereitzustellen. 

Die Manifestdatei beschreibt die Gruppe von Entitäten, die Sie als Gruppe bereitstellen und deren Bereitstellung als Gruppe Sie aufheben möchten. Der Inhalt der Manifestdatei muss der [YAML-Spezifikation für OpenWhisk-Bereitstellungen](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification) entsprechen. Einmal definiert, können Sie Ihre Manifestdatei verwenden, um eine Gruppe von Functions-Entitäten in demselben oder in unterschiedlichen Functions-Namensbereichen bereitzustellen bzw. erneut bereitzustellen. Sie können die Functions-Plug-in-Befehle `ibmcloud fn deploy` und `ibmcloud fn undeploy` verwenden, um die Functions-Entitäten, die in Ihrer Manifestdatei definiert sind, zu implementieren bzw. ihre Implementierung aufzuheben. 

## Hello World-API-Beispiel erstellen
{: #deploy_helloworld_example}

In diesem Beispiel wird einfacher Node.js-Code (`helloworld.js`) verwendet, eine Webaktion (`hello_world`) in einem Paket (`hello_world_package`) erstellt und eine REST-API für diese Aktion definiert.
{: shortdesc}

1. Erstellen Sie eine Datei `helloworld.js` mit dem folgenden Code. 

```javascript
function main() {
   return {body: 'Hello world'};
}
```
{: codeblock}

Die Bereitstellungsmanifestdatei definiert die folgenden Variablen. 
* Den Paketnamen. 
* Den Aktionsnamen. 
* Die Aktionsannotation, die angibt, dass es sich um eine Webaktion handelt. 
* Der Dateiname des Aktionscodes. 
* Die API mit dem Basispfad `/hello`. 
* Der Endpunktpfad `/world`. 

2. Erstellen Sie die Datei `hello_world_manifest.yml`. 

```yaml
packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
```
{: codeblock}

3. Verwenden Sie den Befehl `deploy`, um das Paket, die Aktion und die API bereitzustellen. 

```sh
$ ibmcloud fn deploy --manifest hello_world_manifest.yml
```
{: pre}

Sie können die Aktionen, Pakete und APIs auflisten, um zu bestätigen, dass die drei erwarteten Entitäten erfolgreich erstellt wurden.
{: shortdesc}

1. Listen Sie die Aktionen mit dem folgenden Befehl auf. 

```sh
$ ibmcloud fn action list
```
{: pre}

2. Listen Sie die Pakete mit dem folgenden Befehl auf. 

```sh
$ ibmcloud fn package list
```
{: pre}

3. Listen Sie die APIs mit dem folgenden Befehl auf. 
```sh
$ ibmcloud fn api list
```
{: pre}

4. Rufen Sie die API auf.

```sh
$ curl URL-FROM-API-LIST-OUTPUT
Hello World
```
{: codeblock}

Sie können die Implementierung derselben Entitäten mit dem Befehl `undeploy` aufheben. 

```sh
$ ibmcloud fn undeploy --manifest hello_world_manifest.yml
Success: Undeployment completed successfully.
```
{: codeblock}

## Zusätzliche Beispiele für eine OpenWhisk-Bereitstellung
{: more_deploy_examples}

Die Functions-Bereitstellung basiert auf dem OpenWhisk-Bereitstellungsmodell, das [mehrere Beispiele für Bereitstellungsmanifeste](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples) umfasst, die in Functions verwendet werden können. Sie können statt `wskdeploy` den Befehl `ibmcloud fn deploy` verwenden. 

## Spezifikation des Implementierungsmanifests
{: manifest_specification}

Functions-Bereitstellungsmanifeste müssen der Spezifikation für OpenWhisk-Bereitstellungsmanifeste entsprechen. Details finden Sie in der [Dokumentation zur Spezifikation von OpenWhisk-Bereitstellungsmanifesten](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification). 


