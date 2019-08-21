---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: object storage, bucket, package, functions

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
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Object Storage
{: #pkg_obstorage}

É possível estender a funcionalidade de seu app {{site.data.keyword.openwhisk}} integrando com uma instância do {{site.data.keyword.cos_full}}.

**Antes de iniciar** Para saber mais sobre o {{site.data.keyword.cos_full_notm}}, consulte [Sobre o Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Para obter mais informações sobre como configurar a instância do {{site.data.keyword.cos_full_notm}}, consulte [Fornecer uma instância do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Pacotes
{: #obstorage_packages}
| Pacote | Disponibilidade | Descrição |
| --- | --- | --- |
| [Pacote do {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_install)| Instalável | Leitura, gravação e exclusão de uma instância do {{site.data.keyword.cos_full_notm}}. |
| [Origem de eventos (Experimental) do {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_ev) | Pré-instalado (somente para o Sul dos EUA) | Ouça as mudanças em uma instância do {{site.data.keyword.cos_full_notm}}. |

## Criando uma instância de serviço do IBM Cloud Object Storage
{: #pkg_obstorage_service}

Antes de poder usar qualquer um dos pacotes, deve-se solicitar uma instância do {{site.data.keyword.cos_full_notm}} e criar pelo menos um depósito.

1. [Crie uma instância de serviço do {{site.data.keyword.cos_full_notm}} ](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Crie um conjunto de credenciais de serviço HMAC ](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) para a instância de serviço do {{site.data.keyword.cos_full_notm}}. No campo **Incluir parâmetros de configuração sequenciais (opcional)**, inclua `{"HMAC":true}`.

3. [ Crie pelo menos um depósito ](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Instalando o pacote do {{site.data.keyword.cos_full_notm}} por meio da CLI do {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_install}

Depois de ter uma instância de serviço do {{site.data.keyword.cos_full_notm}}, é possível usar a CLI ou a UI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.cos_full_notm}} em seu namespace.
{: shortdesc}
{: #pkg_obstorage_cli}

O pacote instalável do {{site.data.keyword.cos_full_notm}} implementa um conjunto de ações que podem ser usadas para trabalhar com sua instância do {{site.data.keyword.cos_full_notm}}. Elas são executadas em Node.js ou Python. Depois de instalar o pacote, será possível selecionar um tempo de execução. Para obter uma lista das ações no pacote `cloud-object-storage`, consulte [Entidades disponíveis](#pkg_obstorage_actions).
{: note}

**Antes de iniciar**

[Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do  {{site.data.keyword.cos_full_notm}} :

1. Clone o repo do pacote do  {{site.data.keyword.cos_full_notm}} .
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue para o diretório `runtimes/nodejs` ou `runtimes/python` para selecionar um tempo de execução para as ações no pacote.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

4. Implemente o pacote `cloud-object-storage`.
    ```
    ibmcloud fn deploy
    ```
    {: pre}

    **Resposta**
    ```
    Sucesso: implementação concluída com êxito.
    ```
    {: scree}

5. Verifique se o pacote `cloud-object-storage` está incluído em sua lista depacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Saída**
    ```
    packages
    /<org_space>/cloud-object-storage         private
    ```
    {: screen}

6. Ligue as credenciais da instância do {{site.data.keyword.cos_full_notm}} que você criou ao pacote. É possível incluir o sinalizador `--keyname` para ligar credenciais de serviço específicas. Para obter mais informações sobre como ligar serviços, consulte [Comandos de serviço](/docs/cloud-functions-cli-plugin?topic=cloud-functions-cli-plugin-functions-cli#cli_service).

    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage --keyname `<service_key>`
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

7. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.cos_full_notm}}.
    ```
    ibmcloud fn package get /<org_space>/cloud-object-storage parameters
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ok: got package /<org_space>/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service.bluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

## Ligando parâmetros

É possível usar o comando [`package update`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_pkg_bind) para ligar o terminal de um depósito a uma ação específica ou ao pacote `cloud-object-storage`. Substitua `<bucket_endpoint>` pelo terminal de seu depósito.

Ao atualizar os parâmetros para um pacote, uma ação ou um acionador, deve-se especificar todos os parâmetros criados anteriormente. Caso contrário, os parâmetros criados anteriormente serão removidos. Todos os serviços que foram ligados ao pacote também serão removidos, portanto, depois de atualizar outros parâmetros, deve-se [ligar os serviços](/docs/openwhisk?topic=cloud-functions-services) ao seu pacote novamente.
{: important}

**Ligar parâmetros a todas as ações em um pacote**
```
ibmcloud fn package update cloud-object-storage --param endpoint <bucket_endpoint>
```
{: pre}

**Ligar parâmetros a uma ação específica**
```
ibmcloud fn action update cloud-object-storage/object-write --param endpoint <bucket_endpoint>
```
{: pre}

Também é possível ligar parâmetros a ações usando a guia **Parâmetros** na IU. Para incluir parâmetros na IU, navegue para a página [**Ações**](https://cloud.ibm.com/openwhisk/actions){: external} e clique em uma de suas ações. Em seguida, clique em **Parâmetros** > **Incluir parâmetro**. Deve-se incluir parâmetros nos pares `<key>` e `<value>`.
{: tip}


## Instalando o pacote do {{site.data.keyword.cos_full_notm}} por meio da IU do {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_full_notm}} usando o menu suspenso de namespace

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **IBM Cloud Object Storage** e, em seguida, clique no pacote **IBM Cloud Object Storage**.

5. Na seção **Tempos de Execução Disponíveis**, selecione `Node.JS`ou `Python`a partir da lista suspensa. Em seguida, clique em **Instalar**.

6. Depois que o pacote é instalado, você é redirecionado para a página **Ações** e pode procurar por seu novo pacote, que é denominado `cloud-object-storage`.

7. Para usar as ações no pacote `cloud-object-storage`, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga a etapa 5 nas [instruções da CLI](#pkg_obstorage_cli).
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. 
  
Se você ligar o serviço de suas credenciais de serviço a ações individuais, deverá concluir as etapas a seguir para cada ação que deseja usar.
{: note}

1. Clique em uma ação por meio do pacote `cloud-object-storage` que você deseja usar. A página de detalhes para essa ação é aberta.
2. Na navegação à esquerda, clique em **Parâmetros**.
3. Insira um novo parâmetro. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.


Para obter mais informações sobre parâmetros de ligação, consulte [Ligando parâmetros a pacotes](/docs/openwhisk?topic=cloud-functions-actions#actions_pkgs_params).


## Entidades disponíveis
{: #pkg_obstorage_actions}

O pacote do  {{site.data.keyword.cos_full_notm}}  inclui as ações a seguir:

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/cloud-object-storage` | Pacote | `apikey`, `resource_instance_id`, `cos_hmac_keys.access_key_id`, `cos_hmac_keys.secret_access_key` | Trabalhe com uma instância do  {{site.data.keyword.cos_full_notm}} . |
| `/cloud-object-storage/object-write` | Ação | `bucket`, `key`, `body`, `endpoint`, `ibmAuthEndpoint` | Gravar um objeto em um depósito. |
| `/cloud-object-storage/object-read` | Ação | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Ler um objeto a partir de um depósito. |
| `/cloud-object-storage/object-delete` | Ação | `bucket`, `key`, `endpoint`, `ibmAuthEndpoint` | Excluir um objeto de um depósito. |
| `/cloud-object-storage/bucket-cors-put` | Ação | `bucket`, `corsConfig`, `endpoint`, `ibmAuthEndpoint` | Designar uma configuração do CORS a um depósito. |
| `/cloud-object-storage/bucket-cors-get` | Ação | `bucket`, `endpoint`, `ibmAuthEndpoint` | Ler a configuração de CORS de um depósito. |
| `/cloud-object-storage/bucket-cors-delete` | Ação | `bucket`, `endpoint`, `ibmAuthEndpoint` | Excluir a configuração de CORS de um depósito. |
| `/cloud-object-storage/client-get-signed-url` | Ação | `bucket`, `key`, `operation`, `expires`, `endpoint`, `ibmAuthEndpoint` | Obter uma URL assinada para restringir a Gravação, a Leitura e a Exclusão de um objeto em um depósito. |

Para obter uma lista completa de entidades disponíveis, execute `ibmcloud fn package get cloud-object-storage`.
{: note}

### Parâmetros do Pacote
{: #pkg_obstorage_pkgparams}

Espera-se que os parâmetros de pacote a seguir sejam ligados ao pacote e estejam disponíveis automaticamente para todas as ações. Também é possível especificar esses parâmetros quando você chama uma das ações.

| Parâmetro de pacote | Descrição |
| --- | --- |
| `apikey` | O parâmetro `apikey` é a chave de API do IAM para a instância do {{site.data.keyword.cos_full_notm}}. |
| `resource_instance_id` | O parâmetro `resource_instance_id` é o identificador de instância do {{site.data.keyword.cos_full_notm}}. |
| `cos_hmac_keys` | O parâmetro `cos_hmac_keys` são as credenciais HMAC da instância do {{site.data.keyword.cos_full_notm}}, que incluem os valores `access_key_id` e `secret_access_key`. Essas credenciais são usadas exclusivamente pela ação `client-get-signed-url`.  Consulte [Usando credenciais HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) para obter instruções sobre como gerar credenciais HMAC para sua instância do {{site.data.keyword.cos_full_notm}}. |
 
### Parâmetros de ação
{: #pkg_obstorage_actparams}

Os parâmetros de ação a seguir são especificados quando você chama as ações individuais. Nem todos esses parâmetros são suportados por cada ação. Consulte a tabela [Entidades disponíveis](#pkg_obstorage_actions) para ver quais parâmetros são suportados por qual ação.

| Parâmetro de ação | Descrição |
| --- | --- |
| `bucket` | O parâmetro `bucket` é o nome do depósito do {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | O parâmetro `endpoint` é o terminal do {{site.data.keyword.cos_full_notm}} que é usado para se conectar à sua instância do {{site.data.keyword.cos_full_notm}}. É possível localizar seu terminal na [documentação do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `expires` | O parâmetro `expires` é o número de segundos para expirar a operação de URL pré-assinada. O valor `expires` padrão é 15 minutos. |
| `ibmAuthEndpoint` | O parâmetro `ibmAuthEndpoint ` é o terminal de autorização do IBM Cloud que é usado pelo {site.data.keyword.cos_short}} para gerar um token por meio de `apikey`. O terminal de autorização padrão funciona para todas as regiões do IBM Cloud. |
| `key` | O parâmetro `key` é a chave do objeto do depósito. |
| `operation` | O parâmetro `operation` é a operação pré-assinada da URL a ser chamada. |
| `corsConfig` | O parâmetro `corsConfig` é uma configuração CORS de um depósito. |


## Gravando objetos em um depósito
{: #pkg_obstorage_write}

É possível usar a ação `object-write` para gravar um objeto em um depósito do {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

Nas etapas a seguir, o nome `test-bucket` é usado como um exemplo. Os depósitos no {{site.data.keyword.cos_full_notm}} devem ser globalmente exclusivos, portanto, deve-se substituir `test-bucket` por um nome de depósito exclusivo.
{: note}

### Gravando um objeto em um depósito usando a CLI
{: #pkg_obstorage_write_cli}
Grave um objeto em seu depósito usando a ação `object-write`.
{: shortdesc}


Chame a ação `object-write` para gravar um objeto em seu depósito. Se você ligar seu terminal de depósito a seu pacote ou à ação `object-write`, não será necessário incluir o terminal como um parâmetro. Substitua `<org_space>` pelo nome de sua Organização e Espaço do Cloud Foundry, `<test-bucket>` pelo nome de seu depósito e `<test.txt>` pelo nome do objeto que você deseja gravar.

```
ibmcloud fn action invoke /<org_space>/cloud-object-storage/object-write --blocking --result --param bucket <test-bucket> --param key <test.txt> --param body <test> --param endpoint <bucket_endpoint>
```
{: pre}

**Saída de exemplo**

```
{
  "body": {
      "ETag": "\" 32cef9b573122b1cf8fd9aec5fdb898c \""
  },
  "bucket": "test-bucket",
  "key": "test.txt"
}
```
{: screen}

### Gravando um objeto em um depósito usando a IU
{: #pkg_obstorage_write_ui}


1. Acesse a [página Ações](https://cloud.ibm.com/openwhisk/actions){: external} no console do {{site.data.keyword.openwhisk_short}}.

2. No pacote `cloud-object-storage`, clique na ação **object-write**.

3. Na área de janela de Código, clique em **Mudar entrada**.

4. Insira um objeto JSON que contenha o depósito, a chave e o corpo como chaves de objeto.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": "test"
    }
    ```
    {: pre}

5. Clique em  ** Salvar **.

6. Clique em  ** Invoke **.

7. Verifique se a saída é semelhante à seguinte:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09 Activation ID: bb6eba3cf69wereaeba3cf691a1aad8 Results: {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\"" }
    } Logs: []
    ```
    {: screen}

## Lendo objetos por meio de um depósito
{: #pkg_obstorage_read}

É possível usar a ação `object-read` para ler em um objeto em um depósito do {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

Nas etapas a seguir, o nome `test-bucket` é usado como um exemplo. Os depósitos no {{site.data.keyword.cos_full_notm}} devem ser globalmente exclusivos, portanto, deve-se substituir `test-bucket` por um nome de depósito exclusivo.
{: note}

### Lendo um objeto por meio de um depósito usando a CLI
{: #pkg_obstorage_read_cli}

Leia em um objeto em seu depósito usando a ação `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket test-bucket --param key data.txt
```
{: pre}

**Saída de exemplo**
```
{
  "body": "test",
  "bucket": "test-bucket,
  "key": "data.txt"
}
```
{: screen}

### Lendo um objeto por meio de um depósito usando a IU
{: #pkg_obstorage_read_ui}

1. Acesse a [página Ações](https://cloud.ibm.com/openwhisk/actions){: external}.

2. No pacote `cloud-object-storage`, clique na ação `object-read`.

3. Na área de janela de Código, clique em **Mudar entrada**.

4. Insira um objeto JSON que contenha seu depósito e sua chave como chaves de objeto.
    ```
    {
      "bucket": "test-bucket",
      "key": "test.txt",
    }
    ```
    {: pre}

5. Clique em  ** Salvar **.

6. Clique em  ** Invoke **.

7. Verifique se a saída é semelhante à seguinte:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09 Activation ID: bb6eba3cf69wereaeba3cf691a1aad8 Results: {
      "bucket": "test-bucket",
      "key": "test.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\"" }
    } Logs: []
    ```
    {: screen}


## Atendendo a mudanças em um depósito usando a origem de eventos do Object Storage (Experimental)
{: #pkg_obstorage_ev}

O pacote `/whisk.system/cos-experimental` pode ser instável, mudar frequentemente de maneiras que não sejam compatíveis com versões anteriores e pode ser descontinuado com um aviso breve. Esse pacote não é recomendado para uso em ambientes de produção. Esse pacote experimental está disponível apenas na região Sul dos EUA.
{: important}

É possível usar o {{site.data.keyword.openwhisk}} para detectar mudanças em um depósito do [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) e usar uma ação para processar um ou mais objetos do depósito.

<br>

**Caso de uso de amostra:** com o pacote `/whisk.system/cos-experimental`, é possível atender a mudanças nos dados da rua do GPS armazenados em um depósito do {{site.data.keyword.cos_full_notm}}. Em seguida, quando ocorrerem mudanças, será possível acionar a regeneração automática de um mapa de GPS para que os usuários possam ter acesso aos dados de rua mais recentes para seu aplicativo GPS.

### (Experimental) Parâmetros de origem de eventos de Object Storage
{: #pkg_obstorage_ev_ch}

Com o pacote `/whisk.system/cos-experimental`, é possível configurar eventos por meio de uma instância do {{site.data.keyword.cos_full_notm}} e inclui o feed a seguir:

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | Pacote | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Pacote que contém a ação de feed `changes`. |
| `/whisk.system/cos-experimental/changes` | Feed | `apikey`, `auth_endpoint`, `bucket`, `endpoint`, `interval` | Disparar eventos acionadores em mudanças em um depósito do {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

É possível usar o feed `changes` para configurar o serviço de origem de eventos do {{site.data.keyword.cos_full_notm}} para disparar um acionador em cada mudança em um depósito em sua instância do {{site.data.keyword.cos_full_notm}}.

Parâmetros que são usados neste exemplo:

| Parâmetro | Descrição |
| --- | --- |
| `apikey` | (Obrigatório, a menos que ligado ao pacote). O parâmetro `apikey` é a chave de API do IAM para a instância do {{site.data.keyword.cos_full_notm}}.  Normalmente, esse valor é ligado ao pacote. No entanto, se o valor `apikey` for especificado ao usar a ação de feed `changes`, o valor especificado será usado para as credenciais em vez da chave de API de credenciais de limite. |
| ` auth_endpoint` | (Opcional). O parâmetro `auth_endpoint` é o terminal de autorização que é usado pelo {{site.data.keyword.cos_full_notm}} para gerar um token por meio de `apikey`. O terminal padrão é o terminal do {{site.data.keyword.cloud}}. |
| `bucket` | (Obrigatório). O parâmetro `bucket` é o nome do depósito do {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | (Obrigatório). O parâmetro `endpoint` é o terminal do {{site.data.keyword.cos_full_notm}} usado para se conectar à sua instância do {{site.data.keyword.cos_full_notm}}. É possível localizar seu terminal na [documentação do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints). |
| `interval` | (Opcional). O parâmetro `interval`é o intervalo de pesquisa do depósito, em minutos inteiros. O valor `interval` deve ser de pelo menos 1 minuto e é configurado como 1 minuto por padrão. |

## Criando um acionador para responder ao feed de mudanças
{: #pkg_obstorage_ev_trig}

Ao criar o acionador, é possível evitar passar suas credenciais {{site.data.keyword.cos_full_notm}}para a ação de feed `mudanças`ligando suas credenciais diretamente para o pacote `cos-experimental`.
{: shortdesc}

 1. Primeiro, crie uma ligação de pacote que possa ser modificada para conter suas credenciais. O seguinte cria uma ligação de pacote, `myCosPkg`, em seu namespace.

  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}

 2. Ligue suas credenciais do {{site.data.keyword.cos_full_notm}} ao pacote. A ligação de suas credenciais do {{site.data.keyword.cos_full_notm}} ao pacote liga o valor `apikey` ao pacote para que você não precise especificar o valor `apikey` quando a ação de feed `changes` for chamada.

  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}

 3. Crie um acionador denominado `cosTrigger` com o feed `changes` na ligação de pacote que você criou. Use o nome do depósito e os valores de parâmetro de terminal do {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

  **Saída de exemplo**

    ```
    ok: created trigger feed cosTrigger
    ```
  {: pre}
 

Crie uma ação para verificar se o acionador, o feed de mudanças e a regra estão todos configurados e funcionando corretamente.
 
  1. Salve o código JavaScript a seguir como `cosChange.js`. 

  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}

  2. Crie uma ação chamada `cosChange` usando o código `cosChange.js`.

  ```
  ibmcloud fn action create cosChange <filepath>/cosChange.js
  ```
  {: pre}

  3. Crie uma regra para conectar a ação `cosChange` ao acionador `cosTrigger`.

  ```
  ibmcloud fn rule create cosRule cosTrigger cosChange
  ```
  {: pre}

  4. Em uma janela separada, comece a pesquisar ativações para dar visibilidade clara do que está acontecendo. Quando o acionador é disparado e a ação é executada, esse comando lista os registros de ativação para cada uma dessas operações à medida que elas ocorrem.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}
  
  5. Em seu painel do {{site.data.keyword.cos_full_notm}}, modifique um objeto do depósito existente ou crie um. Para saber como incluir um objeto em seu depósito, veja [Incluir alguns objetos em seu depósito](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).
  
  6. Para cada mudança de objeto do depósito, observe as novas ativações para o acionador `cosTrigger` e a ação `cosChange`. É possível visualizar essas ativações executando o comando `ibmcloud fn activation poll` dentro do intervalo de pesquisa do depósito configurado.
  
  7. Se você não for capaz de observar novas ativações, verifique se os valores de parâmetro `apikey`, `endpoint` e `bucket` estão corretos.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}


### Estrutura de dados de uma ativação do acionador do Object Storage
{: #pkg_obstorage_ev_data}

O conteúdo dos eventos gerados tem os parâmetros a seguir:

| Parâmetro | Descrição |
| --- | --- |
| `file` | Os metadados do arquivo ou do objeto. Essa estrutura é descrita em [Listar objetos em um depósito específico](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets). |
| `status` | A mudança detectada. Esse valor é `added`, `modified` ou `deleted`. |
| `bucket`| O nome do depósito do {{site.data.keyword.cos_full_notm}}. |
| `endpoint` | O terminal do {{site.data.keyword.cos_full_notm}} usado para conexão com a instância do {{site.data.keyword.cos_full_notm}}. |
| `key` | O identificador do objeto de depósito mudado. Esse valor é o mesmo que `file.Key`, mas disponível na parte superior do JSON do evento acionador. |

**Representação JSON de exemplo da ativação do acionador de mudança do depósito**

```json
{
  "file": {
    "ETag": "\"fb47672a6f7c34339ca9f3ed55c6e3a9\"",
    "Key": "file-86.txt",
    "LastModified": "2018-12-19T08:33:27.388Z",
    "Owner": {
      "DisplayName": "80a2054e-8d16-4a47-a46d-4edf5b516ef6",
      "ID": "80a2054e-8d16-4a47-a46d-4edf5b516ef6"
    },
    "Size": 25,
    "StorageClass": "STANDARD"
  },
  "status": "added",
  "bucket": "myBucket",
  "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
  "key": "file-86.txt"
}
```
{: codeblock}

## Criando uma ação para processar um objeto mudado
{: #pkg_obstorage_ev_act}

É possível criar uma única ação que recupere e processe o objeto. Ou é possível criar uma sequência que use uma ação para recuperar o objeto e outra ação para processar o objeto.

### Criando uma ação para recuperar e processar o objeto
{: #pkg_obstorage_ev_act_ret}

Este código de ação de amostra recupera e processa o documento de notificação de mudança de depósito. É possível passar os parâmetros `apikey` e `serviceInstanceId` diretamente para a ação durante a chamada de ação manual, mas, quando essa ação é chamada por um acionador, esses valores devem ser obtidos de seu {{site.data.keyword.cos_full_notm}}, que deve ser ligado à ação com o comando `ibmcloud fn service bind`. 

1. Salve o código a seguir em um arquivo .zip chamado `myCosAction.zip`.

  ```javascript
  const COS = require('ibm-cos-sdk')

  function main(params){
  const apikey = params.apikey || params.__bx_creds['cloud-object-storage'].apikey
  const serviceInstanceId = params.serviceInstanceId || params.__bx_creds['cloud-object-storage'].resource_instance_id
  const ibmAuthEndpoint = params.ibmAuthEndpoint
  const endpoint = params.endpoint
  const bucket = params.bucket
  const file = params.key

    const cos_config = { endpoint: endpoint, apiKeyId: apikey, ibmAuthEndpoint: ibmAuthEndpoint, serviceInstanceId: serviceInstanceId }
  const client = new COS.S3(cos_config);

    return new Promise(function(resolve, reject) {
      client.getObject( { Bucket: bucket, Key: file }, (err, results) = >{
        if (err != null) {
          console.log(err)
        reject({ err: err })
        } else {
          console.log(results)
        resolve({ contents: Buffer.from(results.Body).toString() })
      }
      })
    });
}
exports.main = main;
  ```
  {: codeblock}

  Como essa ação usa o pacote NPM `ibm-cos-sdk`, a ação deve ser empacotada como um [módulo Node.js](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) ou em um [pacote configurável único](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

2. Crie a ação para recuperar e processar o objeto por meio do {{site.data.keyword.cos_full_notm}}:

  ```
  ibmcloud fn action create myCosAction <filepath>/myCosAction.zip --kind nodejs:10
  ```
  {: pre}



### Criando uma sequência de ações para recuperar e processar o objeto
{: #pkg_obstorage_ev_act_seq}

Em vez de incluir o código de recuperação de objeto em sua ação, é possível usar a ação `object-read` por meio do pacote `cloud-object-storage`, que deve ser [instalado manualmente](#pkg_obstorage_install).  Seu código de ação só precisa processar os resultados retornados de `object-read`.
{: shortdesc}

Para criar uma ação que processe apenas o objeto de depósito:

1. Salve o código a seguir como `myCosAction.js`.

  ```javascript
  function main(data) {
    if (data) {
      // Process the object
  }
  }
  ```
  {: codeblock}

2. Crie a ação para processar apenas o objeto do {{site.data.keyword.cos_full_notm}}.

  ```
  ibmcloud fn action create myCosProcessObjectAction <filepath>/myCosAction.js
  ```
  {: pre}

3. Ligar suas credenciais do {{site.data.keyword.cos_full_notm}} ao pacote `cloud-object-storage` manualmente instalado.

  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}

4. A ação `object-read` pode ser composta por `myCosProcessObjectAction` para criar uma sequência de ações.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

Além da ação `object-read`, é possível usar outras ações que são incluídas no pacote instalável `cloud-object-storage`.
{: tip}

Para obter uma lista das entidades disponíveis, execute o comando a seguir.
```
ibmcloud fn package get cloud-object-storage
```
{: pre}

### Ligando credenciais à sua ação
{: #pkg_obstorage_ev_bind}

É possível evitar passar credenciais sensíveis durante a chamada ligando suas credenciais do {{site.data.keyword.cos_full_notm}} à ação usando o comando a seguir:
  ```
  ibmcloud fn service bind cloud-object-storage myCosAction
  ```
  {: pre}

### Criando uma regra para associar a ação ao acionador de mudança
{: #pkg_obstorage_ev_rule}

É possível usar uma ação ou sequência de ações em uma [regra](/docs/openwhisk?topic=cloud-functions-rules) para buscar e processar o objeto que está associado a um evento de mudança do {{site.data.keyword.cos_full_notm}}.

Crie uma regra que ativa a ação `MyCosAction` nos novos eventos acionadores do {{site.data.keyword.cos_full_notm}}.
  ```
  Regra ibmcloud fn create myRule myCosTrigger myCosAction
  ```
  {: pre}



