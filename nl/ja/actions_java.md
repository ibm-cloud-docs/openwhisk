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

# Java アクションの作成
{: #creating-java-actions}

以下のセクションでは、単一 Java アクションの作成と呼び出し、および、そのアクションへのパラメーターの追加について説明します。

Java ファイルをコンパイル、テスト、およびアーカイブするには、[JDK 8 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://openjdk.java.net/install) がローカルにインストールされている必要があります。

## Java アクションの作成と呼び出し
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Java のアクションというのは、`main` という名前のメソッドを持つ Java プログラムのことです。 `main` は、以下のシグニチャーを持っていなければなりません。

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Java アクションを作成します。

1. 以下のコードを `Hello.java` という名前のファイルに保存します。

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

2. `Hello.java` を `hello.jar` という名前の JAR ファイルに圧縮します。**注**: Java の CLASSPATH に [google-gson ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/google/gson) が存在している必要があります。

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. アクションを作成します。

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * `--main` を使用して、メイン・クラスの名前を指定する必要があります。 適格なメイン・クラスは、静的 `main` メソッドを実装するクラスです。 クラスがデフォルト・パッケージ内にない場合は、完全修飾 Java クラス名を使用してください (例: `--main com.example.MyMain`)。
  * Java のアクションのメソッド名はカスタマイズできます。 これを行うには、アクションの完全修飾メソッド名 (例えば、`--main com.example.MyMain#methodName`) を指定します。
  * アクションのタイプは、ソース・ファイルの拡張子を使用して判別されます。

4. アクションを呼び出します。

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    出力例:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Java アクションへの依存項目の組み込み

Java アクションでサード・パーティーの依存項目を使用するには、そのアクションの JAR アーカイブにすべてのライブラリーを組み込むことが必要です。
[Gradle](https://gradle.org) などのビルド・ツールを使用して Maven Central などのリポジトリーからライブラリーを取り出して、コードとすべての依存項目を組み込んだ最終的な JAR アーカイブをビルドできます。

Gradle によって `com.google.zxing` ライブラリーを利用した Java アクションをビルドする例を以下に示します。このライブラリーは、QR コード・イメージを生成する機能を提供します。

1. ファイル `build.gradle` を作成して、依存項目を指定します。

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

2. `gradle jar` コマンドを実行します。ディレクトリー `build/libs/` に JAR アーカイブが生成されます。
この JAR アーカイブを使用して、前のセクションで説明されているようにアクションをデプロイします。
詳しくは、Gradle の資料 [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies) を参照してください。

入力として `text` を取り込んで QR コード・イメージを生成する Java Web アクションの例を以下に示します。ディレクトリー `java_example/src/main/java/qr` にファイル `Generate.java` を作成します。

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

3. `java_example` (ファイル `build.gradle` が入っているディレクトリー) から以下のコマンドを実行して、Web アクション JAR をビルドします。

  ```bash
  gradle jar
  ```
{: pre}

4. JAR `build/libs/java_example-1.0.jar` を使用して Web アクションをデプロイします。

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. Web アクション・エンドポイントのパブリック URL を取得して環境変数に割り当てます。

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. この `URL` を使用して Web ブラウザーを開くことができます。QR イメージにエンコードして組み込むメッセージを、その照会パラメーター `text` に追加します。`curl` などの HTTP クライアントを使用して QR イメージをダウンロードすることもできます。

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
