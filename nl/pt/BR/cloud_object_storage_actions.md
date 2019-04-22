---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: object storage, bucket, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Pacote do Cloud Object Storage
{: #cloud_object_storage_actions}

O pacote do {{site.data.keyword.cos_full}} fornece um conjunto de ações para interação com instâncias do {{site.data.keyword.cos_full_notm}}. Essas ações permitem que você leia, grave e exclua dos depósitos que estão presentes em uma instância do {{site.data.keyword.cos_short}}.
{: shortdesc}

O pacote do  {{site.data.keyword.cos_short}}  inclui as ações a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/cloud-object-storage` | pacote | apikey, resource_instance_id, cos_hmac_keys.access_key_id, cos_hmac_keys.secret_access_key | Trabalhe com uma instância do  {{site.data.keyword.cos_short}} . |
| `/cloud-object-storage/object-write` | ação | bucket, key, body, endpoint, ibmAuthEndpoint | Gravar um objeto em um depósito. |
| `/cloud-object-storage/object-read` | ação | bucket, key, endpoint, ibmAuthEndpoint | Ler um objeto a partir de um depósito. |
| `/cloud-object-storage/object-delete` | ação | bucket, key, endpoint, ibmAuthEndpoint | Excluir um objeto de um depósito. |
| `/cloud-object-storage/bucket-cors-put` | ação | bucket, corsConfig, endpoint, ibmAuthEndpoint | Designar uma configuração do CORS a um depósito. |
| `/cloud-object-storage/bucket-cors-get` | ação | bucket, endpoint, ibmAuthEndpoint | Ler a configuração de CORS de um depósito. |
| `/cloud-object-storage/bucket-cors-delete` | ação | bucket, endpoint, ibmAuthEndpoint | Excluir a configuração de CORS de um depósito. |
| `/cloud-object-storage/client-get-signed-url` | ação | bucket, key, operation, expires, endpoint, ibmAuthEndpoint | Obter uma URL assinada para restringir a Gravação, a Leitura e a Exclusão de um objeto em um depósito. |

## Parâmetros de pacote e de ação

#### Parâmetros do Pacote

Os parâmetros a seguir devem ser ligados ao pacote; isso os tornará automaticamente disponíveis para todas as ações. Também é possível especificar esses parâmetros ao chamar uma das ações.

**apikey**: o parâmetro `apikey` é a chave de API do IAM para a instância do {{site.data.keyword.cos_short}}.

**resource_instance_id**: o parâmetro `resource_instance_id` é o identificador de instância do {{site.data.keyword.cos_short}}.

**cos_hmac_keys**: o parâmetro `cos_hmac_keys` são as credenciais HMAC da instância do {{site.data.keyword.cos_short}}, que inclui os valores `access_key_id` e `secret_access_key`. Essas credenciais são usadas exclusivamente pela ação `client-get-signed-url`.  Consulte [Usando credenciais HMAC](/docs/services/cloud-object-storage/hmac?topic=cloud-object-storage-service-credentials#service-credentials) para obter instruções sobre como gerar credenciais HMAC para sua instância do {{site.data.keyword.cos_short}}.

#### Parâmetros de ação

Os parâmetros a seguir são especificados ao chamar as ações individuais. Nem todos esses parâmetros são suportados por cada ação; consulte a tabela acima para ver quais parâmetros são suportados por qual ação.

**bucket**: o parâmetro `bucket` é o nome do depósito do {{site.data.keyword.cloud_object_storage_short_notm}}.

**endpoint**: O parâmetro `endpoint`é o {{site.data.keyword.cos_short}}terminal usado para se conectar à sua instância do {{site.data.keyword.cos_short}}. É possível localizar seu terminal na [documentação do {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints).

**expires**: o parâmetro `expires` é o número de segundos para expirar a operação de URL pré-assinada. O valor `expires` padrão é 15 minutos.

**ibmAuthEndpoint**: o parâmetro `ibmAuthEndpoint ` é o terminal de autorização do IBM Cloud usado pelo {site.data.keyword.cos_short}} para gerar um token do `apikey`. O terminal de autorização padrão deve funcionar para todas as regiões do IBM Cloud.

**key**: o parâmetro `key` é a chave do objeto de depósito.

**operation**: o parâmetro `operation` é a operação da URL pré-assinada para chamada.

**corsConfig**: o parâmetro `corsConfig` é uma configuração CORS de um depósito.


## Criando uma instância de serviço do IBM Cloud Object Storage
{: #cloud_object_storage_service_instance}

Antes de instalar o pacote, deve-se solicitar uma instância do {{site.data.keyword.cos_short}} e criar pelo menos um depósito.

1. [Crie uma instância de serviço do {{site.data.keyword.cos_short}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-order-storage#creating-a-new-service-instance).

2. [Criar um conjunto de credenciais de serviço do HMAC![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) para a instância de serviço do {{site.data.keyword.cos_short}}. No campo **Incluir parâmetros de configuração sequenciais (opcional)**, inclua `{"HMAC":true}`.

3. [Criar pelo menos um depósito ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-create-buckets).

## Instalando o pacote do  {{site.data.keyword.cos_short}}
{: #cloud_object_storage_installation}

Depois de ter uma instância de serviço do {{site.data.keyword.cos_short}}, é possível usar a CLI ou a UI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.cos_short}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #cloud_object_storage_cli}

Antes de Iniciar:

[ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar o pacote do  {{site.data.keyword.cos_short}} :

1. Clone o repo do pacote do  {{site.data.keyword.cos_short}} .
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navegue até o diretório `runtimes/nodejs` ou `runtimes/python`. As ações no pacote do {{site.data.keyword.cos_short}} são implementadas no tempo de execução quevocê escolher.
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

5. Ligue as credenciais da instância do {{site.data.keyword.cos_short}} que você criou ao pacote.
    ```
    ibmcloud fn service bind cloud-object-storage cloud-object-storage
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'cloud-object-storage' service instance 'Cloud Object Storage-r1' bound to 'cloud-object-storage'.
    ```
    {: screen}

6. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.cos_short}}.
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
{: #cloud_object_storage_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create).

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_short}}. Os namespaces são formados por meio de nomes combinados de `org` e `space`.

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

## Gravando em um  {{site.data.keyword.cos_short}}  bucket
{: #cloud_object_storage_write}

É possível usar a ação `object-write` para gravar um objeto em um depósito do {{site.data.keyword.cos_short}}.
{: shortdesc}

**Nota**: nas etapas a seguir, o nome `testbucket` é usado como um exemplo. Os depósitos no {{site.data.keyword.cos_full_notm}} devem ser globalmente exclusivos; portanto, deve-se substituir `testbucket` por um nome de depósito exclusivo.

### Gravar em um depósito na CLI
{: #write_bucket_cli}

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
{: #write_bucket_ui}

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

## Lendo a partir de um  {{site.data.keyword.cos_short}}  bucket
{: #cloud_object_storage_read}

É possível usar a ação `object-read` para ler em um objeto em um depósito do {{site.data.keyword.cos_short}}.
{: shortdesc}

**Nota**: nas etapas a seguir, o nome `testbucket` é usado como um exemplo. Os depósitos no {{site.data.keyword.cos_full_notm}} devem ser globalmente exclusivos; portanto, deve-se substituir `testbucket` por um nome de depósito exclusivo.

### Ler em um depósito na CLI
{: #read_bucket_cli}

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

### Ler em um depósito na UI
{: #read_bucket_ui}

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
