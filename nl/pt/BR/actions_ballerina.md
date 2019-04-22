---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

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

# Criando ações do Ballerina
{: #ballerina-actions}

As seções a seguir orientam você na criação e na chamada de uma ação do Ballerina única e na inclusão de parâmetros nessa ação. Uma ação é uma função do Ballerina de nível superior que aceita e retorna um objeto JSON. 

As ações do Ballerina são executadas no Ballerina [0.990.2](https://ballerina.io/downloads). Você precisará de uma versão compatível do compilador localmente disponível para gerar o executável. Sem o compilador do Ballerina, não é possível criar uma ação.

## Criando e chamando uma ação do Ballerina

**Antes de iniciar:** crie um arquivo chamado `hello.bal` com o código-fonte a seguir.

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" }; } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

O método de entrada para a ação é `main` por padrão. É possível especificar essa variável quando você cria a ação com a CLI `wsk` usando `--main`. 

**Nota:** o compilador do Ballerina espera a presença de uma função chamada `main` para gerar o executável, portanto, seu arquivo de origem deve incluir um marcador chamado `main`.

Para criar uma ação chamada `hello`, conclua as etapas a seguir.

1. Gere o arquivo .balx.
  ```
  ballerina build hello.bal
  ```
{: pre}

2. Crie a ação usando o arquivo .balx.
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. A CLI ainda não determina o tipo de ação da extensão do arquivo de origem. Deve-se especificar o tipo explicitamente. Para arquivos de origem `.balx`, a ação é executada usando o tempo de execução do Ballerina 0.990.2.
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

Saída de exemplo.
```json
{
  "greeting": "Hello World!"
}
```
{: screen}
