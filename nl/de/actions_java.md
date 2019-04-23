---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: actions, serverless, java

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

# Java-Aktionen erstellen
{: #creating-java-actions}

In den folgenden Abschnitten werden die Schritte zum Erstellen und Aufrufen einer einzelnen Java-Aktion sowie zum Hinzufügen von Parametern zu dieser beschrieben.

Damit Sie Java-Dateien kompilieren, testen und archivieren können, muss lokal ein [JDK 8 ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](http://openjdk.java.net/install) installiert sein.

## Java-Aktion erstellen und aufrufen
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Eine Java-Aktion ist ein Java-Programm mit einer Methode namens `main`. Die Methode `main` muss die folgende Signatur aufweisen. 

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Erstellen Sie eine Java-Aktion. 

1. Speichern Sie den folgenden Code in einer Datei namens `Hello.java`.

    ```java
    import com.google.gson.JsonObject;
    public class Hello {
        public static JsonObject main(JsonObject args) {
            String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
    }
    ```
    {: codeblock}

2. Komprimieren Sie `Hello.java` in eine JAR-Datei namens `hello.jar`. **Hinweis**: [google-gson ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/google/gson) muss in Ihrem CLASSPATH für Java vorhanden sein.

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Erstellen Sie eine Aktion.

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * Sie müssen den Namen der Hauptklasse mithilfe von `--main` angeben. Eine infrage kommende Hauptklasse ist eine Klasse, die eine statische Methode `main` implementiert. Wenn sich die Klasse nicht im Standardpaket befindet, verwenden Sie den vollständig qualifizierten Java-Klassennamen (Beispiel: `--main com.example.MyMain`).
  * Sie können den Methodennamen Ihrer Java-Aktion anpassen. Diese Anpassung erfolgt, indem Sie den vollständig qualifizierten Namen Ihrer Aktion angeben. Beispiel: `--main com.example.MyMain#methodName`.
  * Der Typ der Aktion wird durch die Verwendung der Erweiterung der Quellendatei bestimmt. 

4. Rufen Sie die Aktion auf.

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Beispielausgabe. 

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Abhängigkeiten in Java-Aktionen einschließen

Um eine Abhängigkeit eines anderen Anbieters in Ihrer Java-Aktion zu verwenden, muss das JAR-Aktionsarchiv alle Bibliotheken enthalten.
Sie können ein Tool wie [Gradle](https://gradle.org) verwenden, um die Bibliotheken aus einem Repository wie Maven Central abzurufen und ein finales JAR-Archiv zu erstellen, das Ihren Code und alle Abhängigkeiten enthält. 

Im Folgenden finden Sie ein Beispiel für die Verwendung von Gradle zum Erstellen einer Java-Aktion, die die Bibliothek `com.google.zxing` nutzt, in der die Funktionalität zum Generieren eines QR-Code-Images enthalten ist. 

1. Erstellen Sie eine Datei `build.gradle` und geben Sie die Abhängigkeiten an. 

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
{: codeblock}

2. Führen Sie den Befehl `gradle jar` aus, der ein JAR-Archiv im Verzeichnis `build/libs/` generiert.
Verwenden Sie dieses JAR-Archiv, um die Aktion gemäß der Beschreibung im vorherigen Abschnitt bereitzustellen. Weitere Informationen finden Sie in der Gradle-Dokumentation [Abhängigkeiten deklarieren](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies). 

Im Folgenden finden Sie ein Beispiel für eine Java-Webaktion, die `text` als Eingabe verwendet und ein QR-Code-Image generiert. Erstellen Sie eine Datei `Generate.java` im Verzeichnis `java_example/src/main/java/qr`. 

  ```java
  package qr;

  import java.io.*;
  import java.util.Base64;

  import com.google.gson.JsonObject;

  import com.google.zxing.*;
  import com.google.zxing.client.j2se.MatrixToImageWriter;
  import com.google.zxing.common.BitMatrix;

  public class Generate {
    public static JsonObject main(JsonObject args) throws Exception {
      String property = "text";
      String text = "Hello. Try with a 'text' value next time.";
      if (args.has(property)) {
        text = args.get(property).toString();
      }

      ByteArrayOutputStream baos = new ByteArrayOutputStream();
      OutputStream b64os = Base64.getEncoder().wrap(baos);

      BitMatrix matrix = new MultiFormatWriter().encode(text, BarcodeFormat.QR_CODE, 300, 300);
      MatrixToImageWriter.writeToStream(matrix, "png", b64os);
      b64os.close();

      String output = baos.toString("utf-8");

      JsonObject response = new JsonObject();
      JsonObject headers = new JsonObject();
      headers.addProperty("content-type", "image/png; charset=UTF-8");
      response.add("headers", headers);
      response.addProperty("body", output);
      return response;
    }
  }
  ```
{: codeblock}

3. Erstellen Sie die Webaktions-JAR, indem Sie den folgenden Befehl im Verzeichnis `java_example` ausführen, in dem sich die Datei `build.gradle` befindet. 

  ```bash
  gradle jar
  ```
{: pre}

4. Stellen Sie die Webaktion mithilfe der JAR-Datei `build/libs/java_example-1.0.jar` bereit. 

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. Rufen Sie die öffentliche URL des Webaktionsendpunkts ab und weisen Sie sie einer Umgebungsvariablen zu. 

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. Sie können einen Web-Browser öffnen, indem Sie diese `URL` verwenden und den Abfrageparameter `text` an die Nachricht anhängen, die im QR-Image verschlüsselt werden soll. Sie können auch einen HTTP-Client wie `curl` verwenden, um ein QR-Image herunterzuladen. 

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
