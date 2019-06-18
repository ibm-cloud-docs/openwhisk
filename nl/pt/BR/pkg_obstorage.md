---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: object storage, bucket, package

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
{:gif: data-image-type='gif'}

# Object Storage
{: #pkg_obstorage}

É possível estender a funcionalidade de seu app {{site.data.keyword.openwhisk}} integrando com uma instância do {{site.data.keyword.cos_full}}.

**Antes de iniciar:** para saber mais sobre o {{site.data.keyword.cos_full_notm}}, consulte [Sobre o Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api). Para obter mais informações sobre como configurar a instância do {{site.data.keyword.cos_full_notm}}, consulte [Fornecer uma instância do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-gs-dev#gs-dev-provision).

## Pacotes
{: #obstorage_packages}
| Pacote | Disponibilidade | Descrição |
| --- | --- | --- |
| [Pacote {{site.data.keyword.cos_full_notm}}](#pkg_obstorage)| Instalável | Leitura, gravação e exclusão de uma instância do {{site.data.keyword.cos_full_notm}}. |
| [Origem de eventos (Experimental) do {{site.data.keyword.cos_full_notm}}](#pkg_obstorage_ev) | Pré-instalado (somente para o Sul dos EUA) | Ouça as mudanças em uma instância do {{site.data.keyword.cos_full_notm}}. |

## Criando uma instância de serviço do IBM Cloud Object Storage
{: #pkg_obstorage_service}

Antes de poder usar qualquer um dos pacotes, deve-se solicitar uma instância do {{site.data.keyword.cos_full_notm}} e criar pelo menos um depósito.

1. [Crie uma instância de serviço do {{site.data.keyword.cos_full_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage?topic=cloud-object-storage-gs-dev#gs-dev-provision).

2. [Criar um conjunto de credenciais de serviço do HMAC![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) para a instância de serviço do {{site.data.keyword.cos_full_notm}}. No campo **Incluir parâmetros de configuração sequenciais (opcional)**, inclua `{"HMAC":true}`.

3. [Criar pelo menos um depósito ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-create-buckets).

## Lendo e gravando em um depósito com o pacote do {{site.data.keyword.cos_full_notm}}
{: #pkg_obstorage_install}

Depois de ter uma instância de serviço do {{site.data.keyword.cos_full_notm}}, é possível usar a CLI ou a UI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.cos_full_notm}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_cli}

Antes de começar:

[ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do  {{site.data.keyword.cos_full_notm}} :

1. Clone o repo do pacote do  {{site.data.keyword.cos_full_notm}} .
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue até o diretório `runtimes/nodejs` ou `runtimes/python`. As ações no pacote do {{site.data.keyword.cos_full_notm}} são implementadas no tempo de execução quevocê escolher.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Implemente o pacote. É possível repetir as etapas anteriores para reimplementar o pacote em outro tempo de execução.
    ```
    ibmcloud fn deploy -m manifest.yaml
    ```
    {: pre}

4. Verifique se o pacote `cloud-object-storage` está incluído em sua lista depacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Saída:
    ```
    packages
    /myOrg_mySpace/cloud-object-storage private
    ```
    {: screen}

5. Ligue as credenciais da instância do {{site.data.keyword.cos_full_notm}} que você criou ao pacote.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.cos_full_notm}}.
    ```
    ibmcloud fn package get /myBluemixOrg_myBluemixSpace/cloud-object-storage parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package /myBluemixOrg_myBluemixSpace/cloud-object-storage, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "cloud-object-storage": {
            "apikey": "sdabac98wefuhw23erbsdufwdf7ugw",
            "credentials": "Credentials-1",
            "endpoints": "https://cos-service-s.us-south.containers.mybluemix.net/endpoints",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:cloud-object-storage:global:a/ddkgdaf89uawefoujhasdf:sd8238-sdfhwej33-234234-23423-213d::",
            "iam_apikey_name": "auto-generated-apikey-sduoiw98wefuhw23erbsdufwdf7ugw",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
            "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e::serviceid:ServiceId-sd8238-sdfhwej33-234234-23423-213d",
            "instance": "Cloud Object Storage-r1",
            "resource_instance_id": "crn:v1:staging:public:cloud-object-storage:global:a/dd166ddkjadf89uawefoujhasdf3bc1f8e4d6818b8da577757528e:sd8238-sdfhwej33-234234-23423-213d::"
          }
         }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #pkg_obstorage_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create).

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_full_notm}}. 

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **IBM Cloud Object Storage** e, em seguida, clique no pacote **IBM Cloud Object Storage**.

5. Na seção **Tempos de Execução Disponíveis**, selecione `Node.JS`ou `Python`a partir da lista suspensa. Em seguida, clique em **Instalar**.

6. Depois que o pacote tiver sido instalado, você será redirecionado para a página Ações e poderá procurar por seu novo Pacote, que é denominado **cloud-object-storage**.

7. Para usar as ações no pacote **cloud-object-storage**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima.
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma ação por meio do pacote **cloud-object-storage** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.


### Ações disponíveis
{: #pkg_obstorage_actions}

O pacote do  {{site.data.keyword.cos_full_notm}}  inclui as ações a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/cloud-object-storage` | pacote | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Trabalhe com uma instância do  {{site.data.keyword.cos_full_notm}} . |
| `/cloud-object-storage/object-write` | ação | bucket, key, body, endpoint, ibmAuthEndpoint | Gravar um objeto em um depósito. |
| `/cloud-object-storage/object-read` | ação | bucket, key, endpoint, ibmAuthEndpoint | Ler um objeto a partir de um depósito. |
| `/cloud-object-storage/object-delete` | ação | bucket, key, endpoint, ibmAuthEndpoint | Excluir um objeto de um depósito. |
| `/cloud-object-storage/bucket-cors-put` | ação | bucket, corsConfig, endpoint, ibmAuthEndpoint | Designar uma configuração do CORS a um depósito. |
| `/cloud-object-storage/bucket-cors-get` | ação | bucket, endpoint, ibmAuthEndpoint | Ler a configuração de CORS de um depósito. |
| `/cloud-object-storage/bucket-cors-delete` | ação | bucket, endpoint, ibmAuthEndpoint | Excluir a configuração de CORS de um depósito. |
| `/cloud-object-storage/client-get-signed-url` | ação | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Obter uma URL assinada para restringir a Gravação, a Leitura e a Exclusão de um objeto em um depósito. |


### Parâmetros do Pacote
{: #pkg_obstorage_pkgparams}

Os parâmetros a seguir devem ser ligados ao pacote; isso os tornará automaticamente disponíveis para todas as ações. Também é possível especificar esses parâmetros ao chamar uma das ações.

**apikey**: o parâmetro `apikey` é a chave de API do IAM para a instância do {{site.data.keyword.cos_full_notm}}.

**resource_instance_id**: o parâmetro `resource_instance_id` é o identificador da instância do {{site.data.keyword.cos_full_notm}}.

**cos_hmac_keys**: o parâmetro `cos_hmac_keys` são as credenciais HMAC da instância do {{site.data.keyword.cos_full_notm}}, que inclui os valores `access_key_id` e `secret_access_key`.  Essas credenciais são usadas exclusivamente pela ação `client-get-signed-url`.  Consulte [Usando credenciais HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) para obter instruções sobre como gerar credenciais HMAC para sua instância do {{site.data.keyword.cos_full_notm}}.

#### Parâmetros de ação
{: #pkg_obstorage_actparams}

Os parâmetros a seguir são especificados ao chamar as ações individuais.  Nem todos esses parâmetros são suportados por todas as ações. Consulte a tabela acima para ver quais parâmetros são suportados por cada ação.

**bucket**: o parâmetro `bucket` é o nome do depósito do {{site.data.keyword.cos_full_notm}}.

**endpoint**: O parâmetro `endpoint`é o {{site.data.keyword.cos_full_notm}}terminal usado para se conectar à sua instância do {{site.data.keyword.cos_full_notm}}. É possível localizar seu terminal na [documentação do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints).

**expires**: o parâmetro `expires` é o número de segundos para expirar a operação de URL pré-assinada.  O valor `expires` padrão é 15 minutos.

**ibmAuthEndpoint**: o parâmetro `ibmAuthEndpoint ` é o terminal de autorização do IBM Cloud usado pelo {site.data.keyword.cos_short}} para gerar um token do `apikey`. O terminal de autorização padrão deve funcionar para todas as regiões do IBM Cloud.

**key**: o parâmetro `key` é a chave do objeto de depósito.

**operation**: o parâmetro `operation` é a operação da URL pré-assinada para chamada.

**corsConfig**: o parâmetro `corsConfig` é uma configuração CORS de um depósito.


### Gravando em um  {{site.data.keyword.cos_full_notm}}  bucket
{: #pkg_obstorage_write}

É possível usar a ação `object-write` para gravar um objeto em um depósito do {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

**Nota**: nas etapas a seguir, o nome `testbucket` é usado como um exemplo. Os depósitos no {{site.data.keyword.cos_full_notm}} devem ser globalmente exclusivos; portanto, deve-se substituir `testbucket` por um nome de depósito exclusivo.

#### Gravar em um depósito na CLI
{: #pkg_obstorage_write_cli}

Grave um objeto em seu depósito usando a ação `object-write`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-write --blocking --result --param bucket testbucket --param key data.txt --param body "my_test_data"
```
{: pre}

Exemplo de Saída:
```
{
  "body": {
      "ETag": "\" 32cef9b573122b1cf8fd9aec5fdb898c \""
  },
  "bucket": "testbucket",
  "key": "data.txt"
}
```
{: screen}

### Gravar em um depósito na UI
{: #pkg_obstorage_write_ui}

1. Acesse a página [Ações no console do {{site.data.keyword.openwhisk_short}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/actions).

2. No pacote `cloud-object-storage`, clique na ação **object-write**.

3. Na área de janela de Código, clique em **Mudar entrada**.

4. Insira um objeto JSON que contenha o depósito, a chave e o corpo como chaves de objeto.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": "my_test_data"
    }
    ```
    {: pre}

5. Clique em  ** Salvar **.

6. Clique em  ** Invoke **.

7. Verifique se a saída é semelhante à seguinte:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09 Activation ID: bb6eba3cf69wereaeba3cf691a1aad8 Results: {
      "bucket": "testbucket",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\"" }
    } Logs: []
    ```
    {: screen}

### Lendo a partir de um  {{site.data.keyword.cos_full_notm}}  bucket
{: #pkg_obstorage_read}

É possível usar a ação `object-read` para ler em um objeto em um depósito do {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

**Nota**: nas etapas a seguir, o nome `testbucket` é usado como um exemplo. Os depósitos no {{site.data.keyword.cos_full_notm}} devem ser globalmente exclusivos; portanto, deve-se substituir `testbucket` por um nome de depósito exclusivo.

#### Ler em um depósito na CLI
{: #pkg_obstorage_read_cli}

Leia em um objeto em seu depósito usando a ação `object-read`.
```
ibmcloud fn action invoke /_/cloud-object-storage/object-read --blocking --result --param bucket testbucket --param key data.txt
```
{: pre}

Exemplo de Saída:
```
{
  "body": "my_test_data",
  "bucket": "testbucket,
  "key": "data.txt"
}
```
{: screen}

#### Ler em um depósito na UI
{: #pkg_obstorage_read_ui}

1. Acesse a página [Ações no console do {{site.data.keyword.openwhisk_short}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/actions).

2. No pacote `cloud-object-storage`, clique na ação **object-read**.

3. Na área de janela de Código, clique em **Mudar entrada**.

4. Insira um objeto JSON que contenha seu depósito e sua chave como chaves de objeto.
    ```
    {
      "bucket": "testbucket",
      "key": "data.txt",
    }
    ```
    {: pre}

5. Clique em  ** Salvar **.

6. Clique em  ** Invoke **.

7. Verifique se a saída é semelhante à seguinte:
    ```
    object-write 3855 ms 6/7/2018, 14:56:09 Activation ID: bb6eba3cf69wereaeba3cf691a1aad8 Results: {
      "bucket": "testbucketeweit",
      "key": "data.txt",
      "body": {
        "ETag": "\"af1c16ea127ab52040d86472c45978fd\"" }
    } Logs: []
    ```
    {: screen}


## Atendendo mudanças em um depósito com a origem de eventos do Object Storage (Experimental)
{: #pkg_obstorage_ev}

O pacote `/whisk.system/cos-experimental` pode ser instável, mudar frequentemente de maneiras que não sejam compatíveis com versões anteriores e pode ser descontinuado com um aviso breve. Esse pacote não é recomendado para uso em ambientes de produção. Esse pacote experimental está atualmente disponível apenas na região Sul dos EUA.
{: important}

É possível usar o {{site.data.keyword.openwhisk}} para detectar mudanças em um depósito do [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) e usar uma ação para processar um ou mais objetos do depósito.

<br>

**Caso de uso de amostra:**Com o pacote `/whisk.system/cos-experimental`, é possível detectar mudanças nos dados da rua do GPS armazenadas em um {{site.data.keyword.cos_full_notm}}depósito. Em seguida, quando ocorrerem mudanças, será possível acionar a regeneração automática de um mapa de GPS, para que os usuários possam ter acesso aos dados de rua mais recentes para seu aplicativo GPS.

### (Experimental) Parâmetros de origem de eventos de Object Storage
{: #pkg_obstorage_ev_ch}

O pacote `/whisk.system/cos-experimental` permite que você configure eventos de uma instância do {{site.data.keyword.cos_full_notm}} e inclui o feed a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | pacote | apikey, auth_endpoint, bucket, endpoint, interval | Pacote que contém a ação de feed `changes`. |
| `/whisk.system/cos-experimental/changes` | alimentação | apikey, auth_endpoint, bucket, endpoint, interval | Disparar eventos acionadores sobre mudanças em um depósito do {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

É possível usar o feed `changes` para configurar o serviço de origem de eventos do {{site.data.keyword.cos_full_notm}} para disparar um acionador em cada mudança em um depósito em sua instância do {{site.data.keyword.cos_full_notm}}.

Parâmetros que são usados neste exemplo:

**apikey**: _(obrigatório, a menos que ligado ao pacote)_. O parâmetro `apikey` é a chave de API do IAM para a instância do {{site.data.keyword.cos_full_notm}}.  Normalmente, esse valor é ligado ao pacote. No entanto, se o valor `apikey` for especificado ao usar a ação de feed `changes`, o valor especificado será usado para as credenciais em vez da chave de API de credenciais de limite.

**auth_endpoint**: _(opcional)_. O parâmetro `auth_endpoint` é o terminal de autorização usado pelo {{site.data.keyword.cos_full_notm}} para gerar um token por meio do `apikey`.  O terminal padrão é o terminal do {{site.data.keyword.Bluemix}}.

**bucket**: _(obrigatório)_. O parâmetro `bucket` é o nome do depósito do {{site.data.keyword.cos_full_notm}}.

**endpoint**: _(obrigatório)_. O parâmetro `endpoint` é o terminal do {{site.data.keyword.cos_full_notm}} usado para se conectar à sua instância do {{site.data.keyword.cos_full_notm}}. É possível localizar seu terminal na [documentação do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints).

**interval**: _(opcional)_. O parâmetro `interval`é o intervalo de pesquisa do depósito, em minutos inteiros. O valor `interval` deve ser de pelo menos 1 minuto e é configurado como 1 minuto por padrão.

### Criando um acionador para responder ao feed de mudanças
{: #pkg_obstorage_ev_trig}

Ao criar o acionador, é possível evitar passar suas credenciais {{site.data.keyword.cos_full_notm}}para a ação de feed `mudanças`ligando suas credenciais diretamente para o pacote `cos-experimental`.
 {: shortdesc}

 1. Primeiro, crie uma ligação de pacote que possa ser modificada para conter suas credenciais. O seguinte cria uma ligação de pacote, `myCosPkg`, em seu namespace.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Ligue suas credenciais do {{site.data.keyword.cos_full_notm}} ao pacote.
 A ligação de suas credenciais do {{site.data.keyword.cos_full_notm}} ao pacote ligará o valor `apikey` ao pacote para que você não precise especificar o valor `apikey` quando a ação de feed `changes` for chamada.
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Crie um acionador denominado `myCosTrigger` com o feed `changes` na ligação de pacote que você criou. Use o nome do depósito e os valores de parâmetro de terminal do {{site.data.keyword.cos_full_notm}}.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
--param bucket myBucket \
--param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}

    Exemplo de Saída:
    ```
    ok: created trigger feed myCosTrigger
    ```
  {: pre}
 4. Crie uma ação simples que sirva apenas para verificar se o acionador, o feed de mudança e a regra estão todos configurados e funcionando corretamente. Por exemplo, crie uma ação chamada `showCosChange` contendo o código JavaScript `showCosChange.js` a seguir:
  ```javascript
  function main(data) {
      console.log(data);
  }
  ```
  {: codeblock}
  ```
  ibmcloud fn action create showCosChange showCosChange.js
  ```
  {: pre}
Exibição de código de amostra
 5. Crie uma regra para conectar a ação `showCosChange` ao acionador `myCosTrigger`:
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 6. Em uma janela separada, comece a pesquisar ativações para dar visibilidade clara do que está acontecendo. Quando o acionador for disparado e a ação for executada, esse comando listará os registros de ativação para cada uma dessas operações à medida que elas ocorrerem.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 7. Em seu painel do {{site.data.keyword.cos_full_notm}}, modifique um objeto do depósito existente ou crie um. Para saber como incluir um objeto em seu depósito, veja [Incluir alguns objetos em seu depósito](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started#gs-add-objects).

 8. Para cada mudança de objeto do depósito, observe as novas ativações para o acionador `myCosTrigger` e a ação `showCosChange`. Essas ativações aparecem em sua janela executando o comando `ibmcloud fn activation poll` dentro do intervalo de pesquisa do depósito configurado.

Se você não for capaz de observar novas ativações, verifique se os valores de parâmetro `apikey`, `endpoint` e `bucket` estão corretos.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Estrutura de dados de um evento acionador do Object Storage
{: #pkg_obstorage_ev_data}

O conteúdo dos eventos gerados tem os parâmetros a seguir:

  - `file`: os metadados do arquivo ou do objeto. Essa estrutura é descrita em [Listar objetos em um depósito específico](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations#compatibility-api-list-buckets).
  - `status`: a mudança detectada.  Esse valor é `added`, `modified` ou `deleted`.
  - `bucket`: O nome do depósito {{site.data.keyword.cos_full_notm}}.
  - `endpoint`: o terminal do {{site.data.keyword.cos_full_notm}} usado para conexão com a instância do {{site.data.keyword.cos_full_notm}}.
  - `key`: o identificador do objeto de depósito mudado. Esse valor é o mesmo que `file.Key`, mas disponível na parte superior do JSON do evento acionador.

Exemplo de representação JSON do evento acionador de mudança de depósito:
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

### Criando uma ação para processar o objeto mudado
{: #pkg_obstorage_ev_act}

É possível criar uma única ação que recupere e processe o objeto. Ou é possível criar uma sequência que use uma ação para recuperar o objeto e outra ação para processar o objeto.

### Criando uma ação para recuperar e processar o objeto
{: #pkg_obstorage_ev_act_ret}

Este código de ação de amostra recupera e processa o documento de notificação de mudança de depósito. É possível passar os parâmetros `apikey`e `serviceInstanceId`diretamente para a ação durante a chamada de ação manual, mas quando essa ação chamada por um acionador, esses valores devem ser obtidos de seu {{site.data.keyword.cos_full_notm}}que deve ser ligado à ação com o comando `ibmcloud fn service bind`.

Código de exemplo:

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

Como essa ação usa o pacote npm `ibm-cos-sdk`, a ação deve ser empacotada como um [módulo Node.js](/docs/openwhisk?topic=cloud-functions-prep#prep_js_npm) ou um [pacote configurável único](/docs/openwhisk?topic=cloud-functions-prep#prep_js_pkg).

Depois de empacotar essa ação em um arquivo .zip, `myCosAction.zip`, crie a ação para recuperar e processar o objeto por meio de {{site.data.keyword.cos_full_notm}}:

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

### Criando uma sequência de ações para recuperar e processar o objeto
{: #pkg_obstorage_ev_act_seq}

Em vez de incluir o código de recuperação de objeto em sua ação, é possível usar a ação `object-read` por meio do pacote `cloud-object-storage`, que deve ser [instalado manualmente](#pkg_obstorage_install). Seu código de ação só precisaria processar os resultados retornados de `object-read`.

Exemplo de `myCosAction.js` de um código de ação que processa somente o objeto do depósito:
```javascript
function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. Crie a ação para processar apenas o objeto a partir de {{site.data.keyword.cos_full_notm}}:
  ```
  ibmcloud fn action create myCosProcessObjectAction myCosAction.js
  ```
  {: pre}
2. Ligar suas credenciais do {{site.data.keyword.cos_full_notm}} ao pacote `cloud-object-storage` manualmente instalado.
  ```
  ibmcloud fn service bind cloud-object-storage cloud-object-storage
  ```
  {: pre}
3. A ação `object-read` pode ser composta por `myCosProcessObjectAction` para criar uma sequência de ações.
  ```
  ibmcloud fn action create myCosAction --sequence cloud-object-storage/object-read,myCosProcessObjectAction
  ```
  {: pre}

Além da ação `object-read`, é possível usar outras ações incluídas no pacote instalável `cloud-object-storage`.

### Ligando credenciais à sua ação
{: #pkg_obstorage_ev_bind}

É possível evitar passar credenciais sensíveis durante a chamada vinculando as credenciais do {{site.data.keyword.cos_full_notm}} à ação com o comando a seguir:
```
ibmcloud fn service bind cloud-object-storage myCosAction
```
{: pre}

### Criando uma regra para associar a ação ao acionador de mudança
{: #pkg_obstorage_ev_rule}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

É possível usar uma ação ou sequência de ações em uma [regra](/docs/openwhisk?topic=cloud-functions-rules) para buscar e processar o objeto que está associado a um evento de mudança do {{site.data.keyword.cos_full_notm}}.

Crie uma regra que ativa a ação `MyCosAction` nos novos eventos acionadores do {{site.data.keyword.cos_full_notm}}.
```
Regra ibmcloud fn create myRule myCosTrigger myCosAction
```
{: pre}

