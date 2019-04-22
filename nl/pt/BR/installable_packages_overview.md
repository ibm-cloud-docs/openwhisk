---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

keywords: packages, installable packages

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Usando pacotes instaláveis
{: #installable-packages-overview}

Os pacotes instaláveis representam um novo design para gerenciamento e interação com pacotes no
{{site.data.keyword.openwhisk}}.
{: shortdesc}

## visão geral
{: #overview}

** O que são pacotes instaláveis? **

Os pacotes são conjuntos em pacote configurável de ações e feeds relacionados. Cada pacote é projetado
para interação específica com serviços e provedores de eventos. Os pacotes instaláveis são pacotes que estão
disponíveis para escolha, instalação e edição com base em suas necessidades.

**Como os pacotes instaláveis são diferentes dos pacotes pré-instalados?**

[Os pacotes pré-instalados](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#browse-packages) são registrados
automaticamente no {{site.data.keyword.openwhisk_short}} no namespace `/whisk.system`. Para armazenar credenciais ou outros parâmetros em um pacote pré-instalado, deve-se criar
[ligações de pacotes](/docs/openwhisk?topic=cloud-functions-openwhisk_packages#openwhisk_package_bind).

Os pacotes instaláveis não residem dentro do sistema {{site.data.keyword.openwhisk_short}}. Em
vez disso, os pacotes instaláveis são hospedados externamente em repositórios do Github individuais. É possível instalar esses pacotes diretamente em seu próprio namespace usando o comando `ibmcloud fn deploy` e fornecer um nome customizado a um pacote. Como o pacote é instalado em seu próprio namespace, é
possível modificar as ações e os feeds no pacote, conforme necessário.

## Instalando pacotes usando wskDeploy
{: #installing}

Antes de Iniciar:

  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar um pacote:

1. Clone o repo do pacote. Os repositórios do pacote podem ser localizados em páginas individuais de
cada pacote neste conjunto de documentação.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. Navegue até o diretório do pacote que contém um arquivo `manifest.yaml` ou
`manifest.yml`. O arquivo `manifest.yaml` especifica a estrutura geral do pacote, incluindo o pacote e as ações a serem instalados em seu namespace e quaisquer metadados que devam ser incluídos com o comando `ibmcloud fn deploy`. Para saber mais sobre arquivos
`manifest.yaml`, consulte a
[Documentação
do wskdeploy![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Implemente o pacote. Alguns pacotes requerem determinadas variáveis de ambiente para permitir que o
pacote funcione corretamente.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Exemplo usando o pacote  {{site.data.keyword.cos_full_notm}}
{: #example}

Para ver um exemplo de como instalar um pacote, confira o [pacote do {{site.data.keyword.cos_short}}](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions). O
{{site.data.keyword.cos_full}} é um serviço que permite que os usuários armazenem todos os tipos de
arquivos, como imagens, vídeos, música e texto. Para interagir com os arquivos, um armazenamento de dados
baseado em nuvem de pares de chave-valor é guardado em um depósito. Portanto, para usar o [pacote do {{site.data.keyword.cos_short}}](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions), deve-se primeiro criar uma instância de serviço do {{site.data.keyword.cos_short}} e, em seguida, criar um depósito. O depósito é usado como uma variável de ambiente que é necessária para instalar esse pacote.

Depois de criar a instância de serviço e o depósito, a instalação do pacote requer os comandos a seguir:

1. Clone o repo do pacote.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue até o diretório do pacote que contém o `manifest.yaml`. Neste exemplo, a
versão de tempo de execução do Node.js do pacote do {{site.data.keyword.cos_short}} é usada.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Implemente o pacote, usando seu depósito como uma variável de ambiente. A dependência da variável de
ambiente `PACKAGE_NAME` permite que você forneça um nome customizado para esse pacote.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}
