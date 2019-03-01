---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-01"

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

# Creating Java actions
{: #creating-java-actions}

The following sections guide you through creating and invoking a single Java action and adding parameters to that action.

In order to compile, test, and archive Java files, you must have [JDK 8 ![External link icon](../icons/launch-glyph.svg "External link icon")](http://openjdk.java.net/install) installed locally.

## Creating and invoking a Java action
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

A Java action is a Java program with a method called `main`. `main` must have the following signature.

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Create a Java action.

1. Save the following code in a file named `Hello.java`.

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

2. Compile `Hello.java` into a JAR file named `hello.jar`. **Note**: [google-gson ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/google/gson) must exist in your Java CLASSPATH.

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Create an action.

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * You must specify the name of the main class by using `--main`. An eligible main class is one that implements a static `main` method. If the class is not in the default package, use the Java fully qualified class name, for example, `--main com.example.MyMain`.
  * You can customize the method name of your Java action. This is done by specifying the fully-qualified method name of your action, for example, `--main com.example.MyMain#methodName`.
  * The type of action is determined by using the source file extension.

4. Invoke the action.

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Example output.

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Including dependencies in Java actions

To use a 3rd party dependency in your Java action, the action jar archive must include all libraries.
You can use a build a tool such as [Gradle](https://gradle.org) to fetch the libraries from a repository like Maven Central and build a final jar archive that includes your code and all dependencies.

Here is an example using Gradle to build a Java action that leverages the library `com.google.zxing` that provides the functionality to genrate a QR code image.

1. Create a file `build.gradle` and specify the dependencies.

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

2. Run the command `gradle jar`, which generates a jar archive in the directory `build/libs/`.
Use this jar archive to deploy the action as described in the previous section.
For more information, read the Gradle documentation [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies).

Here is an example of a Java Web Action that takes `text` as input and generates a QR code image. Create a file `Generate.java` in the directory `java_example/src/main/java/qr`.

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


3. Build the Web Action jar by running the following command from the directory `java_example` where the file `build.gradle` is located.

```bash
gradle jar
```
{: pre}

4. Deploy the web action by using the jar `build/libs/java_example-1.0.jar`.

```bash
ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
```
{: pre}

5. Retrieve the public URL of the web action endpoint and assign it to an environment variable.

```bash
ibmcloud fn action get QRGenerate --url
URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
```
{: pre}

6. You can open a web browser by using this `URL` and appending the query parameter `text` with the message to be encoded into the QR image. You can also use an HTTP client like `curl` to download a QR image.

```bash
curl -o QRImage.png $URL\?text=https://cloud.ibm.com
```
{: pre}
