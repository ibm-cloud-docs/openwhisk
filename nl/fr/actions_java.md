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

# Création d'actions Java
{: #creating-java-actions}

Les sections ci-après vous expliquent comment créer et appeler une action Java unique, et comment ajouter des paramètres à cette action.

Pour pouvoir compiler, tester et archiver des fichiers Java, vous devez avoir installé un logiciel [JDK 8 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://openjdk.java.net/install) en local.

## Création et appel d'une action Java
{: #creating_java_actions}
{: #openwhisk_actions_java_invoke}

Une action Java est un programme Java comportant une méthode appelée `main`. `main` doit avoir la signature suivante. 

```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Créez une action Java. 

1. Sauvegardez le code suivant dans un fichier nommé `Hello.java`.

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

2. Compressez `Hello.java` dans un fichier JAR nommé `hello.jar`. **Remarque **: [google-gson ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/google/gson) doit exister dans votre CLASSPATH Java.

    ```
    javac Hello.java
    ```
    {: pre}

    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Créez une action.

    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}

  * Vous devez spécifier le nom de la classe main à l'aide de `--main`. Une classe main est éligible si elle implémente une méthode `main` statique. Si la classe ne se trouve pas dans le package par défaut, utilisez le nom de classe qualifié complet Java, par exemple, `--main com.example.MyMain`.
  * Vous pouvez personnaliser le nom de méthode de votre action Java. Pour cela, spécifiez le nom de méthode qualifié complet de votre action, par exemple, `--main com.example.MyMain#methodName`.
  * Le type d'action est déterminé à l'aide de l'extension du fichier source. 

4. Appelez l'action.

    ```
    ibmcloud fn action invoke --result helloJava --param name World
    ```
    {: pre}

    Exemple de sortie : 

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

## Inclusion de dépendances dans les actions Java

Pour utiliser une dépendance tierce dans votre action Java, l'archive JAR de l'action doit inclure toutes les bibliothèques.
Vous pouvez utiliser un outil de génération tel que [Gradle](https://gradle.org) pour extraire les bibliothèques d'un référentiel comme Maven Central et générer une archive JAR finale incluant votre code et toutes les dépendances. 

Voici un exemple d'utilisation de Gradle pour générer une action Java exploitant la bibliothèque `com.google.zxing` qui permet de générer une image de code QR. 

1. Créez un fichier `build.gradle` et indiquez les dépendances. 

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

2. Exécutez la commande `gradle jar`, qui génère une archive JAR dans le répertoire `build/libs/`.
Utilisez cette archive JAR pour déployer l'action, conformément à la description de la section précédente.
Pour plus d'informations, lisez la documentation Gradle [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies).

Voici un exemple d'action Web Java qui utilise `text` comme entrée et génère une image de code QR. Créez un fichier `Generate.java` dans le répertoire `java_example/src/main/java/qr`. 

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

3. Créez le fichier JAR de l'action Web en exécutant la commande suivante à partir du répertoire `java_example`, où se trouve le fichier `build.gradle`. 

  ```bash
  gradle jar
  ```
{: pre}

4. Déployez l'action Web à l'aide du fichier JAR `build/libs/java_example-1.0.jar`. 

  ```bash
  ibmcloud fn action update QRGenerate build/libs/java_example-1.0.jar --main qr.Generate -m 128 --web true
  ```
{: pre}

5. Extrayez l'URL publique du noeud final de l'action Web et affectez-la à une variable d'environnement. 

  ```bash
  ibmcloud fn action get QRGenerate --url
  URL=$(ibmcloud fn action get QRGenerate --url | tail -1)
  ```
{: pre}

6. Vous pouvez ouvrir un navigateur Web à l'aide de cette `URL` et lui ajouter le paramètre de requête `text` avec le message à coder dans l'image QR. Vous pouvez également utiliser un client HTTP comme `curl` pour télécharger une image QR. 

  ```bash
  curl -o QRImage.png $URL\?text=https://cloud.ibm.com
  ```
{: pre}
