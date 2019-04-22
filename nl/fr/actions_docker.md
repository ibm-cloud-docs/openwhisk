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

# Création d'actions Docker
{: #creating-docker-actions}


Avec les actions Docker {{site.data.keyword.openwhisk_short}}, vous pouvez écrire vos actions dans n'importe quel langage.
{: shortdesc}

Votre code est compilé dans un fichier binaire exécutable et imbriqué dans une image Docker. Le programme binaire interagit avec le système en prenant l'entrée dans `stdin` et en répondant par le biais de `stdout`. Des informations sur la création d'actions Docker sont disponibles dans la section [Contextes d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Vous pouvez utiliser des actions pour appeler des images à partir de registres publics uniquement, par exemple une image qui est disponible publiquement sur Docker Hub. Les registres privés ne sont pas pris en charge.
{: tip}


## Création et appel d'une action Docker

**Avant de commencer :**

Vous devez disposer d'un compte Docker Hub. Vous pouvez configurer un ID et un compte Docker gratuits sur le site [Docker Hub ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com).

**Pour configurer une action Docker :**

1. Téléchargez et installez le squelette Docker. Le squelette est un modèle de conteneur Docker dans lequel vous pouvez injecter votre code sous la forme de fichiers binaires personnalisés.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configurez votre fichier binaire personnalisé dans le squelette de boîte noire. Le squelette inclut un programme C que vous pouvez utiliser. Une partie du fichier `example.c` est compilée dans le cadre du processus de génération de l'image Docker ; par conséquent, il n'est pas nécessaire que C soit compilé sur votre machine.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Exemple de sortie :
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Facultatif : ajoutez du code et des dépendances supplémentaires à l'image Docker en modifiant `Dockerfile` pour générer votre exécutable. Notez les exigences et limites suivantes :
  * Le fichier binaire doit se trouver dans le conteneur dans le répertoire `/action/exec`.
  * Le fichier exécutable reçoit un argument simple depuis la ligne de commande. Cet argument est une sérialisation de chaîne de l'objet JSON représentant les arguments de l'action.
  * Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
  * Par convention, la dernière ligne de la sortie doit être un objet JSON transformé en chaîne qui représente le résultat de l'action.

4. Générez l'image Docker et téléchargez-la à l'aide d'un script fourni.
    1. Connectez-vous à Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Accédez au répertoire `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Exécutez le script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Utilisez votre conteneur Docker pour créer une action.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Appelez l'action.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Exemple de sortie :
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Afin de mettre à jour l'action Docker, télécharger l'image la plus récente dans le dockerhub. Ainsi, le système pourra extraire votre nouvelle image Docker à la prochaine exécution du code pour votre action.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. S'il existe un conteneur en cours d'exécution utilisant une version précédente de votre image Docker, les nouveaux appels continuent d'utiliser cette image. Mettez à jour l'action de sorte que les nouveaux appels commencent à utiliser la nouvelle image.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Facultatif : vous pouvez utiliser l'argument `--native` comme abréviation de `--docker openwhisk/dockerskeleton`. Cet argument simplifie la création et le déploiement d'un exécutable qui s'exécute au sein du SDK d'action Docker standard.
    1. Les étapes précédentes créent un exécutable binaire dans le conteneur situé dans `/action/exec`. Copiez le fichier `/action/exec` sur votre système de fichiers local et compressez-le dans `exec.zip`.
    2. Créez une action Docker qui reçoit l'exécutable sous forme de données d'initialisation. L'argument `--native` remplace l'argument `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
