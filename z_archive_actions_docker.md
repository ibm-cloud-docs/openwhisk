---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

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

# Creating Docker actions
{: #creating-docker-actions}


With {{site.data.keyword.openwhisk_short}} Docker actions, you can write your actions in any language.
{: shortdesc}

Your code is compiled into an executable binary and embedded into a Docker image. The binary program interacts with the system by taking input from `stdin` and replying through `stdout`.   You can find more information about creating Docker actions in the [Runtimes](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker) section.

You can use actions to call images from public registries only, such as an image that is publicly available on Docker Hub. Private registries are not supported.
{: tip}


## Creating and invoking a Docker action

**Before you begin:**

You must have a Docker Hub account. You can set up a free Docker ID and account on [Docker Hub ![External link icon](../icons/launch-glyph.svg "External link icon")](https://hub.docker.com).

**To set up a Docker action:**

1. Download and install the Docker skeleton. The skeleton is a Docker container template where you can inject your code in the form of custom binaries.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Set up your custom binary in the black box skeleton. The skeleton includes a C program that you can use. Part of the `example.c` file is compiled as part of the Docker image build process, so you do not need C compiled on your machine.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Example output:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Optional: Add additional code and dependencies to the Docker image by modifying the `Dockerfile` to build your executable. Note the following requirements and limits:
  * The binary must be located inside the container at `/action/exec`.
  * The executable receives a single argument from the command line. This argument is a string serialization of the JSON object that represents the arguments to the action.
  * The program may log to `stdout` or `stderr`.
  * By convention, the last line of output must be a stringified JSON object which represents the result of the action.

4. Build the Docker image and upload it using a supplied script.
    1. Log in to Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Go to the `dockerSkeleton` directory.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Run the script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Use your Docker container to create an action.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Invoke the action.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Example output:
    ```
    {
        "args": {
            "payload": "Rey"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```
    {: screen}

7. To update the Docker action, upload the latest image to Docker Hub. This allows the system to pull your new Docker image the next time it runs the code for your action.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. If there is a warm container that uses a previous version of your Docker image, any new invocations continue to use that image. Update the action so that new invocations begin using the new image.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Optional: You can use the `--native` argument as shorthand for `--docker openwhisk/dockerskeleton`. This argument makes it easier to create and deploy an executable that runs inside the standard Docker action SDK.
    1. The above steps create a binary executable inside the container located at `/action/exec`. Copy the `/action/exec` file to your local file system and zip it into `exec.zip`.
    2. Create a Docker action that receives the executable as initialization data. The `--native` argument replaces the `--docker openwhisk/dockerskeleton` argument.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

