---

copyright:
  years: 2019, 2019
lastupdated: "2019-04-04"

keywords: object storage, bucket, event, action, trigger

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# (Experimental) Origem de eventos de armazenamento de objeto
{: #cloud_object_storage}

O pacote `/whisk.system/cos-experimental` pode ser instável, mudar frequentemente de maneiras que não sejam compatíveis com versões anteriores e pode ser descontinuado com um aviso breve. Esse pacote não é recomendado para uso em ambientes de produção. Esse pacote experimental está atualmente disponível apenas na região Sul dos EUA.
{: important}

Neste exemplo, você aprenderá como: 
* [Detectar mudanças](#listening_to_cos_bucket_changes) em uma instância do {{site.data.keyword.cos_full}}.
* [Criar um acionador](#creating_a_trigger_cos) para responder a essas mudanças.
* [Criar ações](#creating_action_to_process_object) para recuperar e processar as mudanças.
* [Criar uma regra](#associating_action_with_change_trigger) para associar sua ação ao acionador de mudança.
<br>

**Caso de uso de amostra:**Com o pacote `/whisk.system/cos-experimental`, é possível detectar mudanças nos dados da rua do GPS armazenadas em um {{site.data.keyword.cos_full_notm}}depósito. Em seguida, quando ocorrerem mudanças, será possível acionar a regeneração automática de um mapa de GPS, para que os usuários possam ter acesso aos dados de rua mais recentes para seu aplicativo GPS.

## Sobre o IBM Cloud Object Storage
{: #cloud_object_storage_info}

**Antes de iniciar:** para saber mais sobre o {{site.data.keyword.cos_full_notm}}, consulte [Sobre o Object Storage](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-object-storage#about-object-storage). Para obter mais informações sobre como configurar a instância do {{site.data.keyword.cos_full_notm}}, consulte [Fornecer uma instância do {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-for-developers#provision-an-instance-of-ibm-cloud-object-storage).

## Atendendo as mudanças em um depósito do IBM Cloud Object Storage
{: #listening_to_cos_bucket_changes}

É possível usar o {{site.data.keyword.openwhisk}} para detectar mudanças em um depósito do [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage/api-reference?topic=cloud-object-storage-about-ibm-cloud-object-storage#about-ibm-cloud-object-storage) e usar uma ação para processar um ou mais objetos do depósito. 

O pacote `/whisk.system/cos-experimental` permite que você configure eventos de uma instância do {{site.data.keyword.cos_full_notm}} e inclui o feed a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cos-experimental` | pacote | apikey, auth_endpoint, bucket, endpoint, interval | Pacote que contém a ação de feed `changes`. |
| `/whisk.system/cos-experimental/changes` | alimentação | apikey, auth_endpoint, bucket, endpoint, interval | Disparar eventos acionadores sobre mudanças em um depósito do {{site.data.keyword.cos_full_notm}}. |
{: shortdesc}

É possível usar o feed `changes` para configurar o serviço de origem de eventos do {{site.data.keyword.cos_full_notm}} para disparar um acionador em cada mudança em um depósito em sua instância do {{site.data.keyword.cos_full_notm}}.

Parâmetros que são usados neste exemplo:

**apikey**: _(obrigatório, a menos que ligado ao pacote)_. O parâmetro `apikey` é a chave de API do IAM para a instância do {{site.data.keyword.cos_full_notm}}. Normalmente, esse valor é ligado ao pacote. No entanto, se o valor `apikey` for especificado ao usar a ação de feed `changes`, o valor especificado será usado para as credenciais em vez da chave de API de credenciais de limite.

**auth_endpoint**: _(opcional)_. O parâmetro `auth_endpoint` é o terminal de autorização usado pelo {{site.data.keyword.cos_full_notm}} para gerar um token por meio do `apikey`. O terminal padrão é o terminal do {{site.data.keyword.Bluemix}}.

**bucket**: _(obrigatório)_. O parâmetro `bucket` é o nome do depósito do {{site.data.keyword.cos_full_notm}}.

**endpoint**: _(obrigatório)_. O parâmetro `endpoint` é o terminal do {{site.data.keyword.cos_full_notm}} usado para se conectar à sua instância do {{site.data.keyword.cos_full_notm}}. É possível localizar seu terminal na [documentação do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-select_endpoints#select_endpoints).

**interval**: _(opcional)_. O parâmetro `interval`é o intervalo de pesquisa do depósito, em minutos inteiros. O valor `interval` deve ser de pelo menos 1 minuto e é configurado como 1 minuto por padrão.

## Criando um acionador para responder ao feed de mudanças
{: #creating_a_trigger_cos}

Ao criar o acionador, é possível evitar passar suas credenciais {{site.data.keyword.cos_full_notm}}para a ação de feed `mudanças`ligando suas credenciais diretamente para o pacote `cos-experimental`.
 {: shortdesc}
 
 1. Primeiro, crie uma ligação de pacote que possa ser modificada para conter suas credenciais. O seguinte cria uma ligação de pacote, `myCosPkg`, em seu namespace.
  ```
  ibmcloud fn package bind /whisk.system/cos-experimental myCosPkg
  ```
  {: pre}
 2. Ligue suas credenciais do {{site.data.keyword.cos_short}} ao pacote.
 A ligação de suas credenciais do {{site.data.keyword.cos_short}} ao pacote ligará o valor `apikey` ao pacote para que você não precise especificar o valor `apikey` quando a ação de feed `changes` for chamada. 
  ```
  ibmcloud fn service bind cloud-object-storage myCosPkg
  ```
  {: pre}
 3. Crie um acionador denominado `myCosTrigger` com o feed `changes` na ligação de pacote que você criou. Use o nome do depósito e os valores de parâmetro de terminal do {{site.data.keyword.cos_short}}.
  ```
  ibmcloud fn trigger create myCosTrigger --feed myCosPkg/changes \
  --param bucket myBucket
  --param endpoint s3.us-south.cloud-object-storage.appdomain.cloud
  ```
  {: pre}
    Exemplo de Saída:
    ```
    ok: created trigger feed myCosTrigger
    ```
    {: screen}
  4. Inicie a pesquisa de ativações para dar visibilidade clara do que está acontecendo.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}
 5. Crie uma ação para observar o feed de mudanças. Por exemplo, uma ação chamada `showCosChange`contendo o código JavaScript a seguir:
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
 6. Crie uma regra para conectar a ação `showCosChange` ao acionador `myCosTrigger`:
  ```
  ibmcloud fn rule create myCosRule myCosTrigger showCosChange
  ```
  {: pre}
 7. Em seu painel do {{site.data.keyword.cos_short}}, modifique um objeto do depósito existente ou crie um. Para saber como incluir um objeto em seu depósito, veja [Incluir alguns objetos em seu depósito](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started-tutorial#gs-add-objects).
 
 8. Para cada mudança de objeto do depósito, observe as novas ativações para o acionador `myCosTrigger` e a ação `showCosChange`. Essas ativações aparecem dentro do intervalo de pesquisa do depósito configurado.

Se você não for capaz de observar novas ativações, verifique se os valores de parâmetro `apikey`, `endpoint` e `bucket` estão corretos.
  ```
  ibmcloud fn trigger get myCosTrigger
  ```
  {: pre}
{: tip}

### Estrutura de dados de um evento acionador
{: #data_structure_trigger_event}

O conteúdo dos eventos gerados tem os parâmetros a seguir:

  - `file`: os metadados do arquivo ou do objeto.
  - `status`: a mudança detectada. Esse valor é `added`, `modified` ou `deleted`.
  - `bucket`: O nome do depósito {{site.data.keyword.cos_short}}.
  - `endpoint`: o terminal do {{site.data.keyword.cos_short}} usado para conexão com a instância do {{site.data.keyword.cos_short}}.
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

## Criando uma ação para processar o objeto mudado
{: #creating_action_to_process_object}

É possível criar uma única ação que recupere e processe o objeto. Ou é possível criar uma sequência que use uma ação para recuperar o objeto e outra ação para processar o objeto.

### Criando uma ação para recuperar e processar o objeto
{: #creating_action_to_retrieve_object}

Este código de ação de amostra recupera e processa o documento de notificação de mudança de depósito. É possível passar os parâmetros `apikey`e `serviceInstanceId`diretamente para a ação durante a chamada de ação manual, mas quando essa ação chamada por um acionador, esses valores devem ser obtidos de seu {{site.data.keyword.cos_short}}que deve ser ligado à ação com o comando `ibmcloud fn service bind`.

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
```
{: codeblock}

Como essa ação usa o pacote npm `ibm-cos-sdk`, a ação deve ser empacotada como um [módulo Node.js](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_packaged_action) ou um [pacote configurável único](/docs/openwhisk?topic=cloud-functions-creating-javascript-actions#openwhisk_js_webpack_action).

Depois de empacotar essa ação em um arquivo .zip, crie a ação para recuperar e processar o objeto do {{site.data.keyword.cos_short}}:

```
ibmcloud fn action create myCosAction myCosAction.zip --kind nodejs:10
```
{: pre}

[Ligue](#cos_binding_credentials_to_action) as credenciais do {{site.data.keyword.cos_short}} a essa ação. Em seguida, [crie uma regra](#associating_action_with_change_trigger) para chamar essa ação quando o acionador for disparado.

### Criando uma sequência de ações para recuperar e processar o objeto

Em vez de incluir o código de recuperação de objeto em sua ação, é possível usar a ação `object-read` por meio do pacote do {{site.data.keyword.cos_short}}, que deve ser [instalado manualmente](/docs/openwhisk?topic=cloud-functions-cloud_object_storage_actions#cloud_object_storage_installation).  Seu código de ação só precisaria processar os resultados retornados de `object-read`.

Código de exemplo de uma ação que processa somente o objeto de depósito:
```javascript
function main(data) {
  if (data) {
    // Process the object
  }
}
```
{: codeblock}

1. Crie a ação para processar apenas o objeto a partir de {{site.data.keyword.cos_short}}:
```
ibmcloud fn action create myCosProcessObjectAction myCosAction.js
```
{: pre}
2. Ligue suas credenciais do {{site.data.keyword.cos_short}} à sua ligação de pacote `cos-experimental`.
```
ibmcloud fn service bind cloud-object-storage myCloudObjectStoragePackage
```
{: pre}
3. A ação `object-read` pode ser composta por `myCosProcessObjectAction` para criar uma sequência de ações.
```
ibmcloud fn action create myCosAction --sequence myCloudObjectStoragePackage/object-read,myCosProcessObjectAction
```
{: pre}

Além da ação `object-read`, é possível usar outras ações incluídas no pacote instalável do {{site.data.keyword.cos_short}}.

[Ligue](#cos_binding_credentials_to_action) as credenciais do {{site.data.keyword.cos_short}} a essa ação. Em seguida, [crie uma regra](#associating_action_with_change_trigger) para chamar essa ação quando o acionador for disparado.

 ## Ligando credenciais à sua ação
 {: #cos_binding_credentials_to_action}
 
 É possível evitar passar credenciais sensíveis durante a chamada vinculando as credenciais do {{site.data.keyword.cos_short}} à ação com o comando a seguir:
 ```
 ibmcloud fn service bind cloud-object-storage myCosAction
 ```
 {: pre}

## Criando uma regra para associar a ação ao acionador de mudança
{: #associating_action_with_change_trigger}

{: #openwhisk_catalog_cloud_object_storage_read_change notoc}

É possível usar uma ação ou sequência de ações em uma [regra](/docs/openwhisk?topic=cloud-functions-openwhisk_triggers#openwhisk_rules_use) para buscar e processar o objeto que está associado a um evento de mudança do {{site.data.keyword.cos_short}}.

Crie uma regra que ativa a ação `MyCosAction` nos novos eventos acionadores do {{site.data.keyword.cos_short}}.
```
Regra ibmcloud fn create myRule myCosTrigger myCosAction
```
{: pre}



