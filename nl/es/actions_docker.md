---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: docker, actions, serverless

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

# Creación de acciones Docker
{: #creating-docker-actions}


Con acciones Docker de {{site.data.keyword.openwhisk_short}}, puede escribir sus acciones en cualquier lenguaje.
{: shortdesc}

Su código se compila en un binario ejecutable y se incluye en una imagen Docker. El programa binario interactúa con el sistema
aceptando la entrada desde `stdin` y respondiendo por medio de `stdout`.   Podrá obtener más información sobre la creación de acciones de Docker en la sección
[Entornos de ejecución](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Puede utilizar acciones para llamar a imágenes de registros públicos únicamente, como una imagen que esté disponible públicamente en Docker Hub. No se admiten registros privados.
{: tip}


## Creación e invocación de una acción Docker

**Antes de empezar:**

Debe tener una cuenta de Docker Hub. Puede configurar una cuenta y un ID de Docker gratuito en [Docker Hub ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://hub.docker.com).

**Para configurar una acción Docker:**

1. Descargue e instale el esqueleto de Docker. El esqueleto es una plantilla de contenedor de Docker en la que puede inyectar código en forma de binarios personalizados.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configure su binario personalizado en el esqueleto de caja negra. El esqueleto incluye un programa en C que puede usar. Parte del archivo `example.c` está compilado como parte del proceso de compilación de imagen de Docker, por lo que no necesita C compilado en su máquina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Salida de ejemplo:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Opcional: Añada código y dependencias adicionales a la imagen de Docker modificando el `Dockerfile` para compilar el ejecutable. Tenga en cuenta los siguientes requisitos y límites:
  * El binario debe estar en el contenedor en `/action/exec`.
  * El ejecutable recibe un único argumento de la línea de mandatos. Este argumento la serialización de una cadena del objeto JSON que representa los argumentos para la acción.
  * El programa puede redirigir sus registros a `stdout` o `stderr`.
  * Por convenio, la última línea de la salida debe ser un objeto JSON en forma de cadena que represente el resultado de la acción.

4. Construya la imagen de Docker y suba dicha imagen mediante un script proporcionado.
    1. Inicie la sesión en Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Vaya al directorio `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Ejecute el script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Utilice el contenedor Docker para crear una acción.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Invoque la acción.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Salida de ejemplo:
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Para actualizar la acción de Docker, cargue la imagen más reciente en Docker Hub. Esto permite al sistema extraer la nueva imagen de Docker la próxima vez que ejecute el código para la acción.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. Si hay un contenedor reciente que utiliza una versión anterior de la imagen de Docker, las nuevas invocaciones siguen utilizando esta imagen. Actualice la acción para que las nuevas invocaciones empiecen a utilizar la nueva imagen.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Opcional: Puede utilizar el argumento `--native` como abreviatura de `--docker openwhisk/dockerskeleton`. Este argumento hace que sea más fácil crear y desplegar un ejecutable que se ejecuta dentro del SDK de acción Docker estándar.
    1. Los pasos anteriores crean un ejecutable binario dentro del contenedor ubicado en `/action/exec`. Copie el archivo `/action/exec` en su sistema de archivos locales y empaquételo como zip en `exec.zip`.
    2. Cree una acción de Docker que reciba el ejecutable como datos de inicialización. El argumento `--native` sustituye al argumento `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
