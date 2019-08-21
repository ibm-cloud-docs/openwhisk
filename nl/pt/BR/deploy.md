---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploying actions, manifest, manifest file, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Implementando entidades com um arquivo manifest
{: #deploy}

É possível usar o {{site.data.keyword.openwhisk_short}} para descrever e implementar todas as entidades de namespace usando um arquivo manifest que é gravado em YAML. É possível usar esse arquivo para implementar todos os seus [Pacotes](/docs/openwhisk?topic=cloud-functions-pkg_ov), [Ações](/docs/openwhisk?topic=cloud-functions-actions), [Acionadores](/docs/openwhisk?topic=cloud-functions-triggers) e [Regras](/docs/openwhisk?topic=cloud-functions-rules) do Functions com um único comando.

O arquivo manifest descreve o conjunto de entidades que você gostaria de implementar e remover a implementação como um grupo. O conteúdo do arquivo manifest deve aderir à [Especificação YAML de implementação do OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification){: external}. Depois de definido, é possível usar seu arquivo manifest para implementar ou reimplementar um grupo de entidades do Functions no mesmo namespace do Functions ou em um diferente. É possível usar os comandos de plug-in do Functions `ibmcloud fn deploy` e `ibmcloud fn undeploy` para implementar e remover a implementação das entidades do Functions que estão definidas em seu arquivo manifest.

## Criando o exemplo de API Hello World
{: #deploy_helloworld_example}

Esse exemplo usa algum código Node.js simples `helloworld.js`, cria uma ação da web `hello_world` dentro de um pacote `hello_world_package` e define uma API de REST para essa ação.
{: shortdesc}

1. Crie um arquivo `helloworld.js` com o código a seguir.

    ```javascript
    function main() {
       return {body: 'Hello world'};
}
    ```
    {: codeblock}

    O arquivo manifest de implementação define as variáveis a seguir.
    * O nome do pacote.
    * O nome da ação.
    * A anotação de ação que indica que ela deve ser uma ação da web.
    * O nome do arquivo de código de ação.
    * A API com um caminho base de `/hello`.
    * O caminho do terminal de `/world`.

2. Crie o arquivo `hello_world_manifest.yml`.

    ```yaml
    packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
    ```
    {: codeblock}

3. Use o comando `deploy` para implementar o pacote, a ação e a API.

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. É possível listar as ações, os pacotes e as APIs para confirmar que as três entidades esperadas foram criadas com êxito.

    1. Liste as ações usando o comando a seguir.

      ```sh
      ibmcloud fn action list
      ```
      {: pre}

    2. Liste os pacotes usando o comando a seguir.

      ```sh
      ibmcloud fn package list
      ```
      {: pre}

    3. Liste as APIs usando o comando a seguir.

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. Chame a API.

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

Opcional: é possível remover a implementação das mesmas entidades usando o comando `undeploy`.

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## Mais exemplos de implementação do OpenWhisk
{: more_deploy_examples}

A implementação do Functions é baseada no projeto de implementação do OpenWhisk, que possui [vários exemplos de manifest de implementação](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples){: external} que podem ser usados dentro do Functions.  É possível usar o comando `ibmcloud fn deploy` em vez de `wskdeploy`.

## Especificação do manifest de implementação
{: manifest_specification}

Os manifestos de implementação do Functions devem aderir à especificação do manifest de implementação do OpenWhisk. Consulte a [documentação de especificação do manifest de implementação do OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification){: external} para obter detalhes.




