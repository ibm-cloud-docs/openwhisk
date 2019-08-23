---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: packages, installable packages, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Incorporando pacotes
{: #pkg_ov}

Os pacotes são conjuntos em pacote configurável de ações e feeds relacionados. Cada pacote é projetado
para interação específica com serviços e provedores de eventos. Alguns pacotes já são instalados com o {{site.data.keyword.openwhisk}} para que você use, mas também é possível instalar outros.
{: shortdesc}

## visão geral
{: #pkg_overview}

[Os pacotes pré-instalados](/docs/openwhisk?topic=cloud-functions-pkg_ov#pkg_browse) são registrados
automaticamente no {{site.data.keyword.openwhisk_short}} no namespace `/whisk.system`. É possível usá-los sem concluir nenhuma etapa de instalação.

Os pacotes instaláveis são pacotes que estão disponíveis para você instalar, editar e usar com base em suas necessidades. Os pacotes instaláveis não residem dentro do sistema {{site.data.keyword.openwhisk_short}}. Em vez disso, os pacotes instaláveis são hospedados externamente em repositórios GitHub individuais.

É possível instalar esses pacotes ou o seu próprio diretamente em seu namespace e é possível fornecer um nome customizado para um pacote. Como o pacote é instalado em seu próprio namespace, é
possível modificar as ações e os feeds no pacote, conforme necessário.



## Procurando pacotes pré-instalados
{: #pkg_browse}

Vários pacotes já são registrados com o {{site.data.keyword.openwhisk_short}} para você. É possível obter uma lista de pacotes em um namespace, listar as entidades em um pacote e obter uma descrição das entidades individuais em um pacote.
{: shortdesc}

1. Obtenha uma lista de pacotes no namespace `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Saída da lista de pacotes:
  ```
  packages
  /whisk.system/cloudant                                                 compartilhado
  /whisk.system/alarms                                                   compartilhado
  /whisk.system/watson                                                   compartilhado
  /whisk.system/websocket                                                compartilhado
  /whisk.system/weather                                                  compartilhado
  /whisk.system/system                                                   compartilhado
  /whisk.system/utils                                                    compartilhado
  /whisk.system/slack                                                    compartilhado
  /whisk.system/samples                                                  compartilhado
  /whisk.system/github                                                   compartilhado
  /whisk.system/pushnotifications                                        compartilhado
  ```
  {: screen}

2. Obtenha uma lista de entidades em um pacote.

  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  **Exemplo**
  ```
  ibmcloud fn package get -- summary /whisk.system/cloudant
  ```
  {: pre}

  **Saída de exemplo**
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.cloud_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Essa saída mostra que o pacote do {{site.data.keyword.cloudant_short_notm}} inclui ações e um feed. Por exemplo, duas ações, `read` e `write` e um feed do acionador chamado `changes`. O feed `changes` faz com que os acionadores sejam disparados quando os documentos
são incluídos no banco de dados do {{site.data.keyword.cloudant_short_notm}}.

  O pacote do {{site.data.keyword.cloudant_short_notm}} também define os parâmetros `username`, `password`, `host` e `port`. Esses parâmetros devem ser especificados para que as ações e os feeds sejam significativos. Os parâmetros permitem que as ações operem em uma conta específica do {{site.data.keyword.cloudant_short_notm}}, por exemplo.

3. Obtenha uma descrição de uma ação ou feed para ver os parâmetros que são necessários.

  **Exemplo**
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  **Saída de exemplo**
  ```
  action /whisk.system/cloudant/read: ler documento do banco de dados
     (parâmetros: dbname includeDoc id)
  ```
  {: screen}

  Essa saída mostra que a ação `read` do {{site.data.keyword.cloudant_short_notm}} requer três parâmetros, incluindo o ID do banco de dados e do documento para recuperação.



## Ligando parâmetros a pacotes pré-instalados
{: #pkg_bind}

Embora seja possível usar as entidades em um pacote diretamente, você pode observar que está passando os mesmos parâmetros para a ação toda vez. É possível simplificar o processo ligando a um pacote e especificando parâmetros padrão, que são herdados pelas ações no pacote.
{: shortdesc}

Por exemplo, no pacote `/whisk.system/cloudant`, é possível configurar valores padrão de `username`, `password` e `dbname` em uma ligação de pacote e esses valores serão passados automaticamente a qualquer ação no pacote.

No exemplo a seguir, você liga ao pacote `/whisk.system/samples`.

1. Faça a ligação com o pacote `/whisk.system/samples` e configure um valor de parâmetro `place` padrão.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  **Saída de exemplo**
  ```
  ok: ligação valhallaSamples criada
  ```
  {: screen}

2. Obtenha uma descrição da ligação do pacote.
  ```
  ibmcloud fn package get -- summary valhallaSamples
  ```
  {: pre}

  **Saída de exemplo**
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Retorna uma saudação amistosa
   action /myNamespace/valhallaSamples/wordCount: Contar palavras em uma sequência
   action /myNamespace/valhallaSamples/helloWorld: Demonstra recursos de criação de log
   action /myNamespace/valhallaSamples/curl: Enrolar uma url de host
  ```
  {: screen}

  Observe que todas as ações no pacote `/whisk.system/samples` estão disponíveis na ligação do pacote `valhallaSamples`.

3. Chame uma ação na ligação do pacote.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  **Saída de exemplo**
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Observe no resultado que a ação herda o parâmetro `place` que você configurou quando criou a ligação de pacote `valhallaSamples`.

4. Chame uma ação e sobrescreva o valor de parâmetro padrão.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  **Saída de exemplo**
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Observe que o valor do parâmetro `place` especificado com a chamada da ação sobrescreve o valor padrão configurado na ligação do pacote `valhallaSamples`.



## Incluindo seus próprios pacotes
{: #pkg_add}

É possível criar um pacote de código local ou um clone de qualquer repositório GitHub.
{: shortdesc}

**Antes de iniciar**
- [Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).
- Crie um arquivo `manifest.yaml` ou `manifest.yml` para seu app e armazene-o no diretório-raiz. O arquivo `manifest.yaml` especifica a estrutura geral do pacote, incluindo quaisquer metadados que devam ser incluídos com o comando `ibmcloud fn deploy`. Para saber mais sobre os arquivos `manifest.yaml`, consulte a [documentação do <ph class="ignoreSpelling">wskdeploy</ph>](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example){: external}.

Para incluir um pacote:

1. Clone o repo do pacote.
    ```
    git clone https://github.com/ORG_NAME/REPOSITORY_NAME
    ```
    {: pre}

2. Navegue para o diretório que contenha o arquivo `manifest.yaml`.
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Implemente o pacote.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

    Alguns pacotes requerem determinadas variáveis de ambiente para permitir que o
pacote funcione corretamente. Se sim, inclua as variáveis de ambiente com o comando `deploy`. Por exemplo, é possível escolher um nome para o pacote e especificá-lo com a variável PACKAGE_NAME.

    ```
    PACKAGE_NAME=CUSTOM_PACKAGE_NAME VARIABLE_NAME=VARIABLE_VALUE ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

### Exemplo de pacote do {{site.data.keyword.cos_full_notm}}
{: #pkg_ex}

Para ver um exemplo de como instalar um pacote, confira o [pacote do {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage). O
{{site.data.keyword.cos_full}} é um serviço que permite que os usuários armazenem todos os tipos de
arquivos, como imagens, vídeos, música e texto. Para interagir com os arquivos, um armazenamento de dados baseado em nuvem de pares chave-valor é armazenado em um depósito. Portanto, para usar o [pacote do {{site.data.keyword.cos_full_notm}}](/docs/openwhisk?topic=cloud-functions-pkg_obstorage), deve-se primeiro criar uma instância de serviço do {{site.data.keyword.cos_full_notm}} e, em seguida, criar um depósito. O depósito é usado como uma variável de ambiente que é necessária para instalar esse pacote.

Depois de criar a instância de serviço e o depósito, será possível instalar o pacote usando os comandos a seguir:

1. Clone o repo do pacote.
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue até o diretório do pacote que contém o `manifest.yaml`. Neste exemplo, a
versão de tempo de execução do Node.js do pacote do {{site.data.keyword.cos_full_notm}} é usada.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Implemente o pacote, usando seu depósito como uma variável de ambiente.  É possível fornecer um nome customizado ao pacote, usando a variável de ambiente `PACKAGE_NAME`.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> ibmcloud fn deploy
    ```
    {: pre}



