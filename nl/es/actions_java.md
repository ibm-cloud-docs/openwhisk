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

# Creación de acciones de Java
{: #creating-java-actions}

En las secciones siguientes se proporciona una guía para la creación e invocación de una única acción Java y la adición de parámetros a dicha acción.

Para poder compilar, probar y archivar archivos Java, debe tener [JDK 8 ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](http://openjdk.java.net/install) instalado localmente.

## Creación e invocación de una acción Java
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Una acción Java es un programa Java con un método llamado `main`. `main` debe tener la firma siguiente.

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Cree una acción Java.

1. Guarde el siguiente código en un archivo denominado `Hello.java`.

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

2. Comprima `Hello.java` en un archivo JAR denominado `hello.jar`. **Nota**: [google-gson ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/google/gson) debe existir en su CLASSPATH de Java.

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Cree una acción.

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * Debe especificar el nombre de la clase principal con `--main`. Una clase principal apta es una que implemente un método `main` estático. Si la clase no está en el paquete predeterminado, utilice el nombre de clase completo de Java, por ejemplo `--main com.example.MyMain`.
  * Puede personalizar el nombre de método de la acción Java. Esto se realiza especificando el nombre completo del método de la acción, por ejemplo, `--main com.example.MyMain#methodName`.
  * El tipo de acción se determina utilizando la extensión del archivo de origen.

4. Invoque la acción.

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Salida de ejemplo.

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Inclusión de dependencias en acciones Java

Para utilizar una dependencia de terceros en la acción Java, el archivo JAR de acción debe incluir todas las bibliotecas.
Puede utilizar una herramienta de compilación como [Gradle](https://gradle.org) para captar las bibliotecas de un repositorio como Maven Central y compilar un archivo JAR final que incluya el código y todas las dependencias.

A continuación se muestra un ejemplo que utiliza Gradle para compilar una acción Java que hace uso de la biblioteca
`com.google.zxing` que proporciona la función de generar una imagen de código QR.

1. Cree un archivo `build.gradle` y especifique las dependencias.

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

2. Ejecute el mandato `gradle jar`, que genera un archivo JAR en el directorio `build/libs/`.
Utilice el archivo JAR para desplegar la acción tal como se describe en la sección anterior.
Para obtener más información, consulte [Declaración de dependencias](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies) en la documentación de Gradle.

A continuación se muestra un ejemplo de una acción web Java que acepta `text` como entrada y genera una imagen de código QR. Cree un archivo `Generate.java` en el directorio `java_example/src/main/java/qr`.

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

3. Compile el JAR de la acción web ejecutando el mandato siguiente desde el directorio `java_example` donde se encuentra el archivo `build.gradle`.

  ```bash
  gradle jar
  ```
{: pre}

4. Despliegue la acción web utilizando el JAR `build/libs/java_example-1.0.jar`.

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. Recupere el URL público del punto final de acción web y asígnelo a una variable de entorno.

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. Puede abrir un navegador web utilizando este `URL` y añadir el parámetro de consulta
`text` con el mensaje a codificar en la imagen QR. También puede utilizar un cliente HTTP como
`curl` para descargar una imagen QR.

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
