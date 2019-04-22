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

# Criando ações Docker
{: #creating-docker-actions}


Com as ações Docker do {{site.data.keyword.openwhisk_short}}, é possível escrever suas ações em qualquer linguagem.
{: shortdesc}

O seu código é compilado em um binário executável e integrado em uma imagem do Docker. O programa binário interage com o sistema aceitando entrada de `stdin` e respondendo por meio de `stdout`.   É possível localizar mais informações sobre a criação de ações do Docker na seção [Tempos de Execução](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

É possível usar ações para chamar imagens de registros públicos apenas, como uma imagem que está publicamente disponível no Docker Hub. Registros privados não são suportados.
{: tip}


## Criando e chamando uma ação do Docker

**Antes de Iniciar:**

Deve-se ter uma conta do Docker Hub. É possível configurar um ID do Docker grátis e uma conta no [Docker Hub ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com).

**Para configurar uma ação do Docker:**

1. Faça download e instale o esqueleto do Docker. A estrutura básica é um modelo do contêiner do Docker no qual é possível injetar seu código na forma de binários customizados.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configure seu binário customizado na estrutura básica da caixa preta. A estrutura básica inclui um programa C que pode ser usado. Parte do arquivo `example.c` é compilada como parte do processo de construção da imagem do Docker, de modo que o C não precisa ser compilado em sua máquina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Exemplo de Saída:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Opcional: inclua código e dependências adicionais na imagem do Docker modificando o `Dockerfile` para construir seu executável. Observe os requisitos e limites a seguir:
  * O binário deve estar localizado dentro do contêiner em `/action/exec`.
  * O executável recebe um único argumento a partir da linha de comandos. Esse argumento é uma serialização de sequência do objeto JSON que representa os argumentos para a ação.
  * O programa pode efetuar log em `stdout` ou em `stderr`.
  * Por convenção, a última linha de saída deve ser um objeto da JSON em sequência que represente o resultado da ação.

4. Construa a imagem do Docker e faça upload da mesma usando um script fornecido.
    1. Efetue login no Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Acesse o diretório  ` dockerSkeleton ` .
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Execute o script.
        ```
        ./buildAndPush.sh < username> /blackboxdemo
        ```
        {: pre}

5. Use seu contêiner do Docker para criar uma ação.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Invoque a ação.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "args": {
            "payload": "Rey"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```
    {: screen}

7. Para atualizar a ação do Docker, faça upload da imagem mais recente para o Docker Hub. Isso permitirá que o sistema puxe a sua nova imagem do Docker na próxima vez em que ele executar o código para a sua ação.
    ```
    ./buildAndPush.sh < username> /blackboxdemo
    ```
    {: pre}

8. Se houver um contêiner quente que usa uma versão anterior de sua imagem do Docker, quaisquer novas chamadas continuarão a usar essa imagem. Atualize a ação para que novas chamadas iniciem o uso da nova imagem.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Opcional: é possível usar o argumento `--native` como uma abreviação para `--docker openwhisk/dockerskeleton`. Esse argumento torna mais fácil criar e implementar um executável que é executado dentro do SDK de ação do Docker padrão.
    1. As etapas acima criam um executável binário dentro do contêiner localizado em `/action/exec`. Copie o arquivo `/action/exec` em seu sistema de arquivos local e compacte-o em `exec.zip`.
    2. Crie uma ação do Docker que receba o executável como dados de inicialização. O argumento `--native` substitui o argumento `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}
