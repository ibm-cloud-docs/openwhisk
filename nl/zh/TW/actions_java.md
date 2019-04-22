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

# 建立 Java 動作
{: #creating-java-actions}

下列各節會引導您建立及呼叫單一 Java 動作，以及將參數新增至該動作。

若要編譯、測試及保存 Java 檔案，您必須已在本端安裝 [JDK 8 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://openjdk.java.net/install)。

## 建立及呼叫 Java 動作
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Java 動作是一種 Java 程式，搭配稱為 `main` 的方法。`main` 必須具有下列簽章。

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

建立 Java 動作。

1. 將下列程式碼儲存至名為 `Hello.java` 的檔案中。
    

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

2. 將 `Hello.java` 壓縮成名為 `hello.jar` 的 JAR 檔。**附註**：[google-gson ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/google/gson) 必須存在於您的 Java CLASSPATH。
    

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. 建立動作。
    

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * 您必須使用 `--main` 來指定 main 類別的名稱。有資格的 main 類別是實作 static `main` 方法的類別。如果類別不在預設套件中，請使用 Java 完整類別名稱，例如，`--main com.example.MyMain`。
  * 您可以自訂 Java 動作的方法名稱。這是透過指定動作的完整方法名稱來完成，例如，`--main com.example.MyMain#methodName`。
  * 動作的類型是使用來源副檔名來決定。

4. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    輸出範例。

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## 在 Java 動作中併入相依關係

若要在 Java 動作中使用協力廠商相依關係，動作 JAR 保存檔必須包括所有程式庫。
您可以使用 [Gradle](https://gradle.org) 這類建置工具以從 Maven Central 這類儲存庫提取程式庫，並建置包括程式碼及所有相依關係的最終 JAR 保存檔。

以下範例使用 Gradle 建置可運用 `com.google.zxing` 程式庫的 Java 動作，而此程式庫提供功能來產生 QR 碼影像。

1. 建立 `build.gradle` 檔案，然後指定相依關係。

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

2. 執行 `gradle jar` 指令，以在 `build/libs/` 目錄中產生 JAR 保存檔。
請依照上一節的說明，使用此 JAR 保存檔來部署動作。
如需相關資訊，請閱讀 Gradle 文件：[宣告相依關係](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies)。

以下 Java Web Action 範例採用 `text` 作為輸入並產生 QR 碼影像。在 `java_example/src/main/java/qr` 目錄中，建立 `Generate.java` 檔案。

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

3. 從 `build.gradle` 檔案所在的 `java_example` 目錄中，執行下列指令來建置「Web 動作 JAR」。

  ```bash
  gradle jar
  ```
{: pre}

4. 使用 JAR `build/libs/java_example-1.0.jar` 來部署 Web 動作。

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. 擷取 Web 動作端點的公用 URL，並將它指派給環境變數。

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. 您可以開啟 Web 瀏覽器，方法是使用此 `URL`，並附加具有編碼為 QR 影像之訊息的 `text` 查詢參數。您也可以使用 `curl` 這類 HTTP 用戶端來下載 QR 影像。

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
