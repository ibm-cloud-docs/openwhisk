---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: cloudant, event, action, trigger, sequence, functions

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


# Cloudant
{: #pkg_cloudant}

Com o pacote `/whisk.system/cloudant` pré-instalado, é possível trabalhar com um banco de dados [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). Nenhuma ligação de serviço é necessária para usar esse pacote.
{: shortdesc}


## Entidades disponíveis
{: #cloudant_available}
A tabela a seguir mostra uma seleção das entidades disponíveis no pacote `whisk.system/cloudant`. É possível usar o pacote `whisk.system/cloudant` para ler, gravar, atualizar ou excluir documentos. Também é possível usar o feed `changes` para atender a mudanças em um banco de dados do {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

Para obter uma lista completa das entidades que estão disponíveis no pacote `/whisk.system/cloudant`, execute `ibmcloud fn package get /whisk.system/cloudant`.
{: note}

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Pacote | `dbname`, `host`, `username`, `password` | Trabalhe com um banco de dados Cloudant. |
| `/whisk.system/cloudant/read` | Ação | `dbname`, `id` | Leia um documento de um banco de dados. |
| `/whisk.system/cloudant/write` | Ação | `dbname`, `overwrite`, `doc` | Grave um documento em um banco de dados. |
| `/whisk.system/cloudant/update-document` | Ação | `dbname`, `doc` | Atualizar um documento em um banco de dados. |
| `/whisk.system/cloudant/changes` | Feed | `dbname`, `iamApiKey`, `iamUrl`, `filter`, `query_params`, `maxTriggers` | Acione eventos de disparo nas mudanças em um banco de dados. |

O parâmetro `includeDoc` não é mais suportado para uso com o feed `/whisk.system/cloudant/changes`. Se você criou acionadores que usam esse parâmetro, deverá recriá-los sem o parâmetro `includeDoc`.
{: deprecated}

## Ligar o pacote `/whisk.system/cloudant` ao seu banco de dados do {{site.data.keyword.cloudant_short_notm}}.
Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.cloud_notm}}, será possível usar o plug-in da CLI do {{site.data.keyword.openwhisk}} para ligar um serviço a uma ação ou a um pacote.
{: #cloudant_db}

**Antes de iniciar**, deve-se ter uma instância do {{site.data.keyword.cloudant_short_notm}}. Para criar uma instância, consulte [Introdução ao {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started).

1. Crie uma ligação de pacote `/whisk.system/cloudant` que esteja configurada para a sua conta do {{site.data.keyword.cloudant_short_notm}}. Neste exemplo, o nome do pacote é `myCloudant`.

  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Verifique se a ligação do pacote existe.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Saída de exemplo**

  ```
  packages
  /<namespace>/myCloudant private
  ```
  {: screen}

3. Obtenha o nome da instância de serviço que você deseja ligar a uma ação ou um pacote.

    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Name          Location   State    Type
    Cloudant-gm   us-south   active   service_instance
    ```
    {: screen}

4. Obtenha o nome das credenciais que estão definidas para a instância de serviço que você obteve na etapa anterior.

    ```
    ibmcloud resource service-keys --instance-name Cloudant-gm
    ```
    {: pre}

    **Saída de exemplo**

    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Ligar o serviço ao pacote que você criou na etapa um.

    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verifique se as credenciais foram ligadas com êxito.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    **Saída de exemplo**

    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters":[ {
                "key": "serviceName",
                "value": "cloudantNoSQLDB"
            },
            {
                "key": "apihost",
                "value": "us-south.functions.cloud.ibm.com"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "cloudantnosqldb": {
                        "apikey": "[Service apikey]",
                        "credentials": "Service credentials-1",
                        "iam_apikey_description": "[Service description]",
                        "iam_apikey_name": "[Service apikey name]",
                        "iam_role_crn": "[Service role crn]",
                        "iam_serviceid_crn": "[Service id crn]",
                        "instance": "Cloudant-gm",
                        "url": "[Service url]",
                        "username": "[Service username]"
                    }
                }
            }
        ],
    }
    ```
    {: screen}

Neste exemplo, as credenciais para o serviço do {{site.data.keyword.cloudant_short_notm}} pertencem a um parâmetro denominado `__bx_creds`.

## Trabalhando com documentos em um banco de dados do {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

É possível usar uma ação para ler, gravar, atualizar, excluir um documento de um banco de dados do {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

### Lendo um documento
É possível usar a ação `/whisk.system/cloudant/read` para ler um documento por meio do banco de dados do {{site.data.keyword.cloudant_short_notm}}.

**Antes de iniciar** Se você não tiver um documento em seu banco de dados do {{site.data.keyword.cloudant_short_notm}}, será possível criar um usando o painel do {{site.data.keyword.cloudant_short_notm}}. A URL para o painel é `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/<database_name>/_all_docs?limit=100`.

Busque um documento usando a ação `read`. Substitua `/_/myCloudant` por seu nome de pacote, `<database_name>` por seu nome do banco de dados e `<document_id>` pelo ID do arquivo. Chame a ação para testar a busca de um documento.

**Sintaxe do Comando**

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id <document_id>
  ```
  {: pre}

**Exemplo de ação de leitura de um banco de dados `test`** Chame a ação para testar a leitura de um arquivo. Esse exemplo lê um arquivo com o `id` de `9f86f4955e7a38ab0169462e6ac0f476`, que é um arquivo vazio.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id 9f86f4955e7a38ab0169462e6ac0f476
  ```
  {:pre}

**Saída de exemplo**
  ```
  {
      "_id": "9f86f4955e7a38ab0169462e6ac0f476",
      "_rev": "1-967a00dff5e02add41819138abb3284d"
  }
  ```
  {: screen}

### Gravando um documento em um banco de dados do {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

É possível usar uma ação para criar ou atualizar documentos em um banco de dados do {{site.data.keyword.cloudant_short_notm}}.
{: shortdesc}

**Antes de iniciar** Crie uma [ligação de pacote](#cloudant_db) `/whisk.system/cloudant` que esteja configurada para a sua conta do {{site.data.keyword.cloudant_short_notm}}.

1. Armazene um documento usando a ação `write` na ligação de pacote que você criou. Substitua `/_/myCloudant` por seu nome de pacote, substitua `<database_name>` pelo nome de seu banco de dados, `<document_id>` por seu ID do documento e `<test_name>` por um nome.

  **Sintaxe do Comando**
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param <database_name> test --param doc "{\"_id\":\"<document_id>\",\"name\":\"<test_name>\"}"
  ```
  {: pre}

  **Exemplo de ação de gravação em um banco de dados `test`**

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\"}"
  ```
  {: pre}

  **Saída de exemplo**

  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "color",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verifique se o documento existe no painel do {{site.data.keyword.cloudant_short_notm}}. A URL do painel para o banco de dados `test` está no formato a seguir: `https://<mycloudantaccount>.cloudant.com/dashboard.html#database/test/_all_docs?limit=100`.

  **Exemplo de documento no painel do {{site.data.keyword.cloudant_short_notm}}**
  ```
  {
  "_id": "color",
  "_rev": "1-f413f4b74a724e391fa5dd2e9c8e9d3f",
  "name": "blue"
  }
  ```
  {: screen}

### Atualizando um documento
É possível usar a ação `/update-document` para atualizar um documento em seu banco de dados do {{site.data.keyword.cloudant_short_notm}}.
{: short desc}

**Antes de iniciar** Crie uma [ligação de pacote](#cloudant_db) `/whisk.system/cloudant` que esteja configurada para a sua conta do {{site.data.keyword.cloudant_short_notm}}.

O exemplo a seguir atualiza o documento que foi criado na seção [Gravando um documento em um banco de dados do {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

É possível atualizar um documento em seu banco de dados substituindo `<test>` pelo nome do banco de dados e substituindo o sinalizador `--param doc` pelo `id` e conteúdo do documento em seu banco de dados que você deseja atualizar.


1. É possível atualizar um documento no banco de dados `test` executando o comando a seguir. Esse exemplo inclui o valor `shade` no documento `color`. 

  ```
  ibmcloud fn action invoke /_/myCloudant/update-document --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}"
  ```
  {: pre}

  **Saída**
  ```
  {
    "id": "color",
    "ok": true,
    "rev": "2-8b904347bfe52e0f388ef6f39d6ba84f"
    }
  ```
  {: screen}

2. Para ver a atualização, busque o documento novamente.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname test --param id color
  ```
  {: pre}

  **Exemplo de documento**
  ```
  {
    "_id": "color",
    "_rev": "2-8b904347bfe52e0f388ef6f39d6ba84f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

## Criar um acionador usando a função de filtro
{: #cloudant_trigger}

É possível usar o feed `changes` para configurar um serviço para disparar um acionador
em cada mudança em seu banco de dados do {{site.data.keyword.cloudant_short_notm}}.

**Antes de iniciar** Crie uma [ligação de pacote](#cloudant_db) `/whisk.system/cloudant` que esteja configurada para a sua conta do {{site.data.keyword.cloudant_short_notm}}.

Parâmetros que são usados neste exemplo.

| Parâmetro | Descrição |
| --- | --- |
| `dbname` | (Obrigatório) O nome do banco de dados do {{site.data.keyword.cloudant_short_notm}}. |
| `iamApiKey` | (Opcional) A chave de API do IAM para o banco de dados Cloudant. Se especificado, esse valor será usado como as credenciais em vez do nome do usuário e da senha. |
| `iamUrl` | (Opcional) A URL do serviço de token do IAM que é usada quando `iamApiKey` é especificado. O valor padrão é `https://iam.cloud.ibm.com/identity/token`. | 
| `maxTriggers` | (Opcional) Parar de disparar acionadores quando esse limite for atingido. O padrão é definido como infinite. |
| `filter` | (Opcional) A função de filtro que é definida em um documento de design. |
| `query_params` | (Opcional) Quaisquer parâmetros de consulta adicionais que possam ser necessários para a função de filtro. |
| `includeDoc` | (Descontinuado) O parâmetro `includeDoc` não é mais suportado para uso com o feed `/whisk.system/cloudant/changes`. |

</br>

1. Crie um acionador denominado `cloudantTrigger` com o feed `changes` na ligação de pacote que você criou anteriormente. Incluindo as funções `filter` e `query_params` para disparar o acionador quando um documento é incluído (ou modificado) quando o status for `new`.

  Substitua `/_/myCloudant` pelo nome de seu pacote. Este exemplo usa um banco de dados chamado `test`.
  ```
  ibmcloud fn trigger create cloudantTrigger --feed /_/myCloudant/changes \ --param dbname test
  ```
  {: pre}

  **Saída de exemplo**

  ```
  ok: created trigger feed cloudantTrigger
  ```
  {: screen}

2. Salve o código JavaScript a seguir como `cloudantChange.js`.

  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

3. Crie uma ação chamada `cloudantChange` que você pode usar para observar as mudanças de feed. Substitua `<file_path>` pelo caminho de arquivo para o arquivo `cloudantChange.js` em seu computador.

  ```
  ibmcloud fn action create cloudantChange <file_path>/cloudantChange.js
  ```
  {: pre}

4. Crie uma regra denominada `cloudantRule` para conectar a ação `cloudantChange` ao `cloudantTrigger` que você criou anteriormente.

  ```
  ibmcloud fn rule create cloudantRule cloudantTrigger cloudantChange
  ```
  {: pre}

5. Em outra janela do terminal, inicie a pesquisa para que seja possível ver quando as ativações ocorrerem.

  ```
  ibmcloud fn activation poll
  ```
  {: pre}

6. Em seu painel do {{site.data.keyword.cloudant_short_notm}}, modifique um documento existente ou crie um.

7. Observe as ativações para o acionador `cloudantTrigger` para cada mudança de documento.

**Exemplo de ativação do `cloudantTrigger`**

```
Activation: 'cloudantTrigger' (ef6605cc05e04589a605cc05e04589d8)
[
    "{\"statusCode\":0,\"success\":true,\"activationId\":\"6067ed0d28774a68a7ed0d28771a684d\",\"rule\":\"<namespace>/cloudantRule\",\"action\":\"<namespace>/cloudantChange\"}"
]

Activation: 'cloudantChange' (6067ed0d28774a68a7ed0d28771a684d)
[
    "2019-06-24T16:46:10.428643Z    stdout: { changes: [ { rev: '19-f7f6d8607d6381d224321acfcfb8887e' } ],",
    "2019-06-24T16:46:10.428693Z    stdout: dbname: 'test',",
    "2019-06-24T16:46:10.428697Z    stdout: id: '6ca436c44074c4c2aa6a40c9a188b348',",
    "2019-06-24T16:46:10.428700Z    stdout: seq: '103-g1AAAAeLeJy91M9NwzAUBnCrrVQqDvTKCa4gpcT5YycnugFsAH5' }"
```
{: screen}

### Estrutura de dados de uma ativação do acionador
{: #cloudant_struct}

O conteúdo do evento gerado tem os parâmetros a seguir.

| Parâmetro | Descrição |
| --- | --- |
| `id` | O ID do documento. |
| `seq` | O identificador de sequência que é gerado pelo {{site.data.keyword.cloudant_short_notm}}. |
| `changes` | Uma matriz de objetos, cada um dos quais possui um campo `rev` que contém o ID de revisão do documento. |

**Representação JSON da ativação do acionador**

```json
{
    "dbname": "test",
    "id": "6ca436c44074c4c2aa6a40c9a188b348",
    "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
    "changes": [
        {
            "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
    ]
}
```
{: codeblock}

## Eventos de mudança de dados do Filtro
{: #cloudant_filter}

É possível definir uma função de filtro para evitar ter eventos de mudança desnecessários que disparem seu acionador.

**Antes de iniciar** Crie uma [ligação de pacote](#cloudant_db) `/whisk.system/cloudant` que esteja configurada para a sua conta do {{site.data.keyword.cloudant_short_notm}}.

Para criar uma função de filtro, é possível usar uma ação.

1. Salve o JSON a seguir após o filtro em um arquivo chamado `design_doc.json`.

  ```json
  {
    "doc": {
      "_id": "_design/mailbox",
      "filters": {
        "by_status": "function(doc, req){if (doc.status != req.query.status){return false;} return true;}"
      }
    }
  }
  ```
  {: codeblock}

2. Crie um documento de design no banco de dados com a função de filtro a seguir. Substitua `<database_name>` pelo nome de seu banco de dados e `<file_path>` pelo caminho de arquivo de seu `design_doc.json`. Chame a ação `write` para testar a criação de um documento de design.

**Sintaxe do Comando**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname <database_name> -p overwrite true -P <file_path>/design_doc.json
```
{: pre}

**Comando de exemplo para gravar um arquivo `design_doc.json` em um banco de dados `test`**
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname test -p overwrite true -P <file_path>/design_doc.json -r
```
{: pre}

**Saída de exemplo**

```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}


Para obter mais informações sobre documentos de design do {{site.data.keyword.cloudant_short_notm}}, consulte [Documentos de design](/docs/services/Cloudant?topic=cloudant-design-documents)

## Processando um documento individual usando uma sequência de ações
{: #cloudant_seq}

É possível usar uma sequência de ações em uma regra para buscar e processar o documento que está associado a um evento de mudança do {{site.data.keyword.cloudant_short_notm}}.

**Antes de iniciar** Crie uma [ligação de pacote](#cloudant_db) `/whisk.system/cloudant` que esteja configurada para a sua conta do {{site.data.keyword.cloudant_short_notm}}. 

Este exemplo atualiza o documento que foi criado na seção [Gravando um documento em um banco de dados do {{site.data.keyword.cloudant_short_notm}}](#cloudant_write).
{: note}

### Criando uma ação para processar um documento individual

Para criar uma ação que manipule mudanças em um documento individual, execute os comandos a seguir.
{: shortdesc}

1. Salve o código a seguir como `docChange.js`

  ```javascript
  function main(doc){
    return { "isBlue:" : doc.name === "blue"};
  }
  ```
  {: codeblock}

2. Crie uma ação chamada `docChange` para processar o documento com o nome `blue` que você criou anteriormente. Substitua `<file_path>` pelo caminho de arquivo de seu `docChange.js`

  ```
  ibmcloud fn action create docChange <file_path>/docChange.js
  ```
  {: pre}

  **Saída**
  ```
  ok: created action docChange
  ```
  {: screen}

### Crie uma sequência com a ação `read` 

A ação `read` pode ser composta com sua ação `docChange` para criar uma sequência de ações.
{: shortdesc}

  ```
  ibmcloud fn action create docSequence --sequence /_/myCloudant/read,docChange
  ```
  {: pre}

  **Saída**
  ```
  ok: created action docSequence
  ```
  {: screen}

### Crie um acionador com o feed `changes`

  ```
  ibmcloud fn trigger create docTrigger --feed /_/myCloudant/changes \
  --param dbname test
  ```
  {: pre}

### Crie uma regra para associar o acionador à sequência

A ação `docSequence` pode ser usada em uma regra que ativa a ação em novos eventos acionadores do {{site.data.keyword.cloudant_short_notm}}.

  ```
  ibmcloud fn rule create docRule docTrigger docSequence
  ```
  {: pre}

  **Saída**
  ```
  ok: created rule docRule
  ```

  **Exemplo de ativação**
  ```
  "{\"statusCode\":0,\"success\":true,\"activationId\":\"144a4f95198a49ec8a4f95198a79ecc8\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ```
  {: screen}

### Teste a sequência

1. Teste a `docSequence` fazendo uma mudança no arquivo `blue` que você criou anteriormente. Neste exemplo, o valor `shade` é mudado para `indigo`.

  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname test --param doc "{\"_id\":\"color\",\"name\":\"blue\",\"shade\":\"indigo\"}" -p overwrite true
  ```
  {: pre}

  **Exemplo de ativação**

  ```
  Activation: 'docChange' (aa3e8fc3030446b2be8fc3030406b2eb)
  []

  Activation: 'docSequence' (23e0a17bebd3486ca0a17bebd3186c8d)
  [
      "8d42679127f3400382679127f300039d",
      "aa3e8fc3030446b2be8fc3030406b2eb"
  ]

  Activation: 'docTrigger' (db6de778bb084366ade778bb08036685)
  [
      "{\"statusCode\":0,\"success\":true,\"activationId\":\"23e0a17bebd3486ca0a17bebd3186c8d\",\"rule\":\"<namespace>/docRule\",\"action\":\"<namespace>/docSequence\"}"
  ]
  ```
  {: screen}

2. Verifique se o arquivo foi atualizado para incluir o valor `shade`, chamando a ação `read`. Substitua o nome `<database>` pelo nome de seu banco de dados.

  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname <database_name> --param id color
  ```
  {: pre}

  **Saída**
  ```
  {
    "_id": "color",
    "_rev": "3-6845b04618338f717676f16edf32a78f",
    "name": "blue",
    "shade": "indigo"
  }
  ```
  {: screen}

### Próximas etapas
Agora que você está atendendo as mudanças em um documento em seu banco de dados do {{site.data.keyword.cloudant_short_notm}}, é possível acionar notificações do Slack para as mudanças usando o pacote [`/whisk.system/slack`](/docs/openwhisk?topic=cloud-functions-pkg_slack).


