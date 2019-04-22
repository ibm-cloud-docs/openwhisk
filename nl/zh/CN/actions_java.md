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

# 创建 Java 操作
{: #creating-java-actions}

以下各部分将指导您创建并调用单个 Java 操作，然后向该操作添加参数。

为了编译、测试和归档 Java 文件，您必须在本地安装 [JDK 8 ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](http://openjdk.java.net/install)。

## 创建并调用 Java 操作
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Java 操作是包含名为 `main` 的方法的 Java 程序。`main` 必须具有以下特征符。

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

创建 Java 操作。

1. 将以下代码保存在名为 `Hello.java` 的文件中。
    

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

2. 将 `Hello.java` 压缩成名为 `hello.jar` 的 JAR 文件。**注**：[google-gson ![外部链接图标](../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/gson) 必须存在于 Java CLASSPATH 中。
    

    ```
javac Hello.java
```
    {: pre}

    ```
jar cvf hello.jar Hello.class
```
    {: pre}

3. 创建操作。
    

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * 必须使用 `--main` 来指定主类的名称。符合要求的主类是实现了静态 `main` 方法的主类。如果该类不在缺省包中，请使用 Java 标准类名，例如 `--main com.example.MyMain`。
  * 可以定制 Java 操作的方法名称。这将通过指定操作的标准方法名称来实现，例如 `--main com.example.MyMain#methodName`。
  * 操作类型使用源文件扩展名来确定。

4. 调用操作。
    

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    示例输出。

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 在 Java 操作中包含依赖项

要在 Java 操作中使用第三方依赖项，操作 JAR 归档必须包含所有库。
可以使用构建工具（如 [Gradle](https://gradle.org)）来访存存储库（如 Maven Central）中的库，并构建包含代码和所有依赖项的最终 JAR 归档。

下面是使用 Gradle 来构建利用 `com.google.zxing` 库的 Java 操作的示例，该库提供了生成 QR 代码映像的功能。

1. 创建 `build.gradle` 文件并指定依赖项。

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

2. 运行 `gradle jar` 命令，这将在 `build/libs/` 目录中生成 JAR 归档。
使用此 JAR 归档可部署操作，如先前部分中所述。有关更多信息，请阅读 Gradle 文档 [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies)。

下面是 Java Web 操作的示例，该操作采用 `text` 作为输入，并生成 QR 代码映像。在 `java_example/src/main/java/qr` 目录中创建 `Generate.java` 文件。

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

3. 通过从 `build.gradle` 文件所在的 `java_example` 目录运行以下命令来构建 Web 操作 JAR。

  ```bash
  gradle jar
  ```
{: pre}

4. 使用 JAR `build/libs/java_example-1.0.jar` 部署 Web 操作。

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. 检索 Web 操作端点的公共 URL，并将其分配给环境变量。

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. 可以使用此 `URL` 来打开 Web 浏览器，并向查询参数 `text` 附加要编码为 QR 映像的消息。此外，还可以使用 HTTP 客户机（如 `curl`）来下载 QR 映像。

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
