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

# Criando ações Java
{: #creating-java-actions}

As seções a seguir orientam você na criação e na chamada de uma ação Java única e na inclusão de parâmetros nessa ação.

Para compilar, testar e criar um archive de arquivos Java, deve-se ter o [JDK 8 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://openjdk.java.net/install) instalado localmente.

## Criando e chamando uma ação Java
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Uma ação Java é um programa Java com um método chamado `main`. `main` deve ter a assinatura a seguir.

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Crie uma ação Java.

1. Salve o código a seguir em um arquivo denominado `Hello.java`.

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

2. Compacte `Hello.java` em um arquivo JAR denominado `hello.jar`. **Nota:** o [google-gson ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/gson) deve existir em seu CLASSPATH Java.

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Crie uma ação.

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * Deve-se especificar o nome da classe principal usando `--main`. Uma classe principal elegível é aquela que implementa um método `main` estático. Se a classe não estiver no pacote padrão, use o nome completo de classe Java, por exemplo, `--main com.example.MyMain`.
  * É possível customizar o nome do método de sua ação Java. Isso é feito especificando o nome completo do método de sua ação, por exemplo, `--main com.example.MyMain#methodName`.
  * O tipo de ação é determinado pelo uso da extensão do arquivo de origem.

4. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Saída de exemplo.

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Incluindo dependências em ações Java

Para usar uma dependência de terceiro em sua ação Java, o archive JAR de ação deve incluir todas as bibliotecas.
É possível usar uma construção de uma ferramenta como [Gradle](https://gradle.org) para buscar as bibliotecas de um repositório como Maven Central e construir um archive JAR final que inclua seu código e todas as dependências.

Aqui está um exemplo usando Gradle para construir uma ação Java que alavanque a biblioteca `com.google.zxing` que fornece a funcionalidade para gerar uma imagem de código QR.

1. Crie um arquivo `build.gradle`e especifique as dependências.

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

2. Execute o comando `gradle jar`, que gera um archive JAR no diretório `build/libs/`.
Use esse archive JAR para implementar a ação conforme descrito na seção anterior. Para obter mais informações, leia a documentação do Gradle [Declarando dependências](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies).

Aqui está um exemplo de uma Ação da Web Java que aceita `text` como entrada e gera uma imagem de código QR. Crie um arquivo `Generate.java` no diretório `java_example/src/main/java/qr`.

  ```java
  package qr;

  import java.io.*;
  import java.util.Base64;

  import com.google.gson.JsonObject;

  import com.google.zxing.*;
  import com.google.zxing.client.j2se.MatrixToImageWriter;
  import com.google.zxing.common.BitMatrix;

  public class Generate {
    public static JsonObject principal (JsonObject args) lança Exceção {
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

3. Construa o JAR de Ação da web executando o comando a seguir por meio do diretório `java_exemple` no qual o arquivo `build.gradle` está localizado.

  ```bash
  gradle jar
  ```
{: pre}

4. Implemente a ação da web usando o JAR `build/libs/java_example-1.0.jar`.

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. Recupere a URL pública do terminal de ação da web e designe-a a uma variável de ambiente.

  ```bash
  Ação ibmcloud fn get QRGenerate -- url
  URL=$ (ação fn de ibmcloud get QRGenerate -- url | tail -1)
  ```
{: pre}

6. É possível abrir um navegador da web usando essa `URL` e anexando o parâmetro de consulta `text` com a mensagem a ser codificada na imagem de QR. Também é possível usar um cliente HTTP como `curl` para fazer download de uma imagem de QR.

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
