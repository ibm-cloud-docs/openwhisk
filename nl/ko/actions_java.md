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

# Java 액션 작성
{: #creating-java-actions}

다음 절에서는 단일 Java 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다.

Java 파일을 컴파일, 테스트하고 아카이브하려면 [JDK 8 ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](http://openjdk.java.net/install)이 로컬로 설치되어 있어야 합니다.

## Java 액션 작성 및 호출
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Java 액션은 `main`이라고 하는 메소드가 포함된 Java 프로그램입니다. `main`에는 다음 서명이 있어야 합니다.

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Java 액션을 작성하십시오.

1. 이름이 `Hello.java`인 파일에 다음 코드를 저장하십시오.

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

2. `Hello.java`를 이름이 `hello.jar`인 Java 파일로 압축하십시오. **참고**: [google-gson ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/google/gson)이 Java CLASSPATH에 존재해야 합니다.

    ```
javac Hello.java
    ```
    {: pre}

    ```
jar cvf hello.jar Hello.class
    ```
    {: pre}

3. 액션을 작성하십시오.

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * `--main`을 사용하여 main 클래스의 이름을 지정해야 합니다. 자격을 갖춘 main 클래스는 정적 `main` 메소드를 구현하는 클래스입니다. 클래스가 기본 패키지에 없는 경우에는 완전한 Java 클래스 이름(예: `--main com.example.MyMain`)을 사용하십시오.
  * Java 액션의 메소드 이름을 사용자 정의할 수 있습니다. 이는 액션의 완전한 메소드 이름(예: `--main com.example.MyMain#methodName`)을 지정하여 수행됩니다.
  * 소스 파일 확장자를 사용하여 액션의 유형이 판별됩니다. 

4. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    예제 출력. 

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Java 액션에 종속 항목 포함

Java 액션에 서드파티 종속성을 사용하려면 액션 JAR 아카이브에 모든 라이브러리가 포함되어야 합니다.
[Gradle](https://gradle.org) 같은 빌드 도구를 사용하여 Maven Central 같은 저장소에서 라이브러리를 페치하고 코드 및 모든 종속 항목이 포함된 최종 JAR 아카이브를 빌드할 수 있습니다.

다음은 Gradle을 사용하여 QR 코드 이미지 생성 기능을 제공하는 `com.google.zxing` 라이브러리를 활용하는 Java 액션을 빌드하는 예입니다.

1. `build.gradle` 파일을 작성하고 종속 항목을 지정하십시오. 

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

2. `build/libs/` 디렉토리에 JAR 아카이브를 생성하는 `gradle jar` 명령을 실행하십시오.
이 JAR 아카이브를 사용하여 이전 섹션에서 설명한 대로 액션을 배치하십시오.
자세한 정보는 Gradle 문서 [종속 항목 선언](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies)을 읽어보십시오.

다음은 `text`를 입력으로 사용하고 QR 코드 이미지를 생성하는 Java 웹 액션의 예입니다. `java_example/src/main/java/qr` 디렉토리에 `Generate.java` 파일을 작성하십시오. 

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

3. `build.gradle` 파일이 위치해 있는 `java_example` 디렉토리에서 다음 명령을 실행하여 웹 액션 JAR을 빌드하십시오. 

  ```bash
  gradle jar
  ```
{: pre}

4. JAR `build/libs/java_example-1.0.jar`을 사용하여 웹 액션을 배치하십시오.

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. 웹 액션 엔드포인트의 공용 URL을 검색하고 이를 환경 변수에 지정하십시오.

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. 이 `URL`을 사용하고 QR 이미지에 인코딩할 메시지와 함께 조회 매개변수 `text`를 추가하여 웹 브라우저를 열 수 있습니다. 또한 `curl` 같은 HTTP 클라이언트를 사용하여 QR 이미지를 다운로드할 수도 있습니다.

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
