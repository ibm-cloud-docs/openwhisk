---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: cloudant, event, action, trigger, sequence

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


# Cloudant
{: #pkg_cloudant}

O pacote `/whisk.system/cloudant` pré-instalado permite que você trabalhe com um banco de dados [{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started). Nenhuma ligação de serviço é necessária para usar esse pacote.


## Ações e feed disponíveis
{: #cloudant_available}

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacote | dbname, host, username, password | Trabalhe com um banco de dados Cloudant. |
| `/whisk.system/cloudant/read` | ação | dbname, id | Leia um documento de um banco de dados. |
| `/whisk.system/cloudant/write` | ação | dbname, overwrite, doc | Grave um documento em um banco de dados. |
| `/whisk.system/cloudant/changes` | alimentação | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Acione eventos de disparo nas mudanças em um banco de dados. |
{: shortdesc}

### Configurando um Banco de Dados {{site.data.keyword.cloudant_short_notm}}no {{site.data.keyword.Bluemix_notm}}
{: #cloudant_db}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.Bluemix_notm}}, será possível usar o plug-in da CLI do {{site.data.keyword.openwhisk}} para ligar um serviço a uma ação ou a um pacote.

Deve-se primeiro criar manualmente uma ligação de pacote à sua conta do {{site.data.keyword.cloudant_short_notm}}.

1. Crie uma ligação de pacote que esteja configurada para sua conta do {{site.data.keyword.cloudant_short_notm}}.
  ```
  ibmcloud fn package bind /whisk.system/cloudant myCloudant
  ```
  {: pre}

2. Verifique se a ligação do pacote existe.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  packages
  /myNamespace/myCloudant private
  ```
  {: screen}

3. Obtenha o nome da instância de serviço que você deseja ligar a uma ação ou um pacote.
    ```
    ibmcloud resource service-instances
    ```
    {: pre}

    Exemplo de Saída:
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

    Exemplo de Saída:
    ```
    Name                    State    Created At
    Service credentials-1   active   Sat Oct 27 03:26:52 UTC 2018
    Service credentials-2   active   Sun Jan 27 22:14:58 UTC 2019
    ```
    {: screen}

5. Ligue o serviço ao pacote que você criou na etapa 1.
    ```
    ibmcloud fn service bind cloudantnosqldb myCloudant --instance Cloudant-gm --keyname 'Service credentials-1'
    ```
    {: pre}

6. Verifique se as credenciais foram ligadas com êxito.
    ```
    ibmcloud fn package get myCloudant parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package myCloudant, displaying field parameters
    {
        "parameters":[ {
                "key": "bluemixServiceName",
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

    Neste exemplo, as credenciais para o serviço Cloudant pertencem a um parâmetro denominado `__bx_creds`.


### Lendo por meio de um banco de dados do {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

É possível usar uma ação para buscar um documento por meio de um banco de dados do {{site.data.keyword.cloudant_short_notm}} chamado **testdb**. Certifique-se de que esse banco de dados exista em sua conta do {{site.data.keyword.cloudant_short_notm}}.

- Busque um documento usando a ação **read** na ligação do pacote anteriormente criada. Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.
  ```
  ibmcloud fn action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```
  {: screen}

### Gravando em um banco de dados do {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

É possível usar uma ação para armazenar um documento em um banco de dados do {{site.data.keyword.cloudant_short_notm}} chamado **testdb**. Certifique-se de que esse banco de dados exista em sua conta do {{site.data.keyword.cloudant_short_notm}}.

1. Armazene um documento usando a ação **write** na ligação do pacote anteriormente criada. Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.
  ```
  ibmcloud fn action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287

  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```
  {: screen}

2. Verifique se o documento existe procurando-o em seu painel do {{site.data.keyword.cloudant_short_notm}}.

  A URL do painel para o banco de dados **testdb** é semelhante à seguinte: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


### Criar um acionador usando a função de filtro
{: #cloudant_trigger}

É possível usar o feed `changes` para configurar um serviço para disparar um acionador
em cada mudança em seu banco de dados do {{site.data.keyword.cloudant_short_notm}}.

Os parâmetros usados neste exemplo são como a seguir:

**dbname**: o nome do banco de dados {{site.data.keyword.cloudant_short_notm}} _(necessário)_.

**iamApiKey**: a chave de API do IAM para o banco de dados Cloudant.  Se especificado, será usado como as credenciais em vez do nome do usuário e da senha _(opcional)_.

**iamUrl**: A URL do serviço de token do IAM que é usada quando `iamApiKey`é especificado.  Padronizado para `https://iam.bluemix.net/identity/token`_(opcional)_.

**maxTriggers**: parar de disparar acionadores quando esse limite for atingido _(opcional)_. O padrão é definido como infinite.

**filter**: a função de filtro que está definida em um documento de design _(opcional)_

**query_params**: os parâmetros de consulta extras para a função de filtro _(opcional)_.

1. Crie um acionador nomeado **myCloudantTrigger** com o feed `changes` feed na ligação de pacote que você criou anteriormente. Incluindo as funções `filter` e `query_params` para disparar o acionador quando um documento é incluído (ou modificado) quando o status for `new`.

  Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.
  ```
  ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: feed do acionador myCloudantTrigger criado
  ```
  {: screen}

2. Inicie a pesquisa de ativações para dar visibilidade clara do que está acontecendo.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

3. Crie uma ação que podemos usar para observar o efeito do feed de mudança. Por exemplo, uma ação chamada **showCloudantChange** contendo o código JavaScript a seguir:
  ```javascript
  function main(data) {
    console.log(data);
  }
  ```
  {: codeblock}

4. Crie uma regra para conectar a ação **showCloudantChange** ao acionador criado anteriormente:
  ```
  ibmcloud fn rule update aCloudantRule myCloudantTrigger showCloudantChange
  ```
  {: pre}

5. Crie ações e uma regra para associá-las ao acionador **myCloudantTrigger**.

6. No painel do {{site.data.keyword.cloudant_short_notm}}, modifique um documento existente ou crie um novo. O documento deve ter um campo _status_, que é configurado para **new**.

7. Observe novas ativações para o acionador **myCloudantTrigger** para cada mudança de documento somente se o status do documento for **new** com base na função de filtro e no parâmetro de consulta.

Teste as etapas de leitura e gravação para ajudar a verificar se suas credenciais do {{site.data.keyword.cloudant_short_notm}} estão corretas.

### Estrutura de dados de um evento acionador
{: #cloudant_struct}

O conteúdo dos eventos gerados tem os parâmetros a seguir:

  - `id`: o ID do documento.
  - `seq`: o identificador de sequência que é gerado pelo {{site.data.keyword.cloudant_short_notm}}.
  - `changes`: uma matriz de objetos, cada um dos quais tendo um campo `rev` que contém o ID da revisão do documento.

A representação JSON do evento acionador é a seguinte:
```json
{
    "dbname": "testdb",
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

### Eventos de mudança de dados do Filtro
{: #cloudant_filter}

Você pode opcionalmente definir uma função de filtro para evitar ter eventos de mudança desnecessários que disparam o acionador.

Para criar uma nova função de filtro, é possível usar uma ação.

Crie um arquivo do documento json `design_doc.json` com a função de filtro a seguir:
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

Crie um documento de design no banco de dados com a função de filtro a seguir:
```
ibmcloud fn action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
{: pre}

As informações para o novo documento de design são impressas na tela:
```
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```
{: screen}

### Usando uma sequência de ações e um acionador de mudanças para processar um documento por meio de um banco de dados do {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_seq}

É possível usar uma sequência de ações em uma regra para buscar e processar o documento que está associado a um evento de mudança do {{site.data.keyword.cloudant_short_notm}}.

Código de amostra de uma ação que manipula um documento:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```
{: codeblock}

Crie a ação para processar o documento do {{site.data.keyword.cloudant_short_notm}}:
```
ibmcloud fn action create myAction myAction.js
```
{: pre}

Para ler um documento do banco de dados, é possível usar a ação `read` do pacote do {{site.data.keyword.cloudant_short_notm}}.
A ação `read` pode ser composta por `myAction` para criar uma sequência de ações.
```
ibmcloud fn action create sequenceAction --sequence /_/myCloudant/read,myAction
```
{: pre}

A ação `sequenceAction` pode ser usada em uma regra que ativa a ação em novos eventos acionadores do {{site.data.keyword.cloudant_short_notm}}.
```
ibmcloud fn rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota:** o acionador `changes` do {{site.data.keyword.cloudant_short_notm}} usado para suportar o parâmetro `includeDoc` que não é mais suportado.

É possível recriar acionadores criados anteriormente com `includeDoc`. Siga estas etapas para recriar o acionador:
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

O exemplo pode ser usado para criar uma sequência de ações para ler o documento mudado e chamar as ações existentes. Lembre-se de desativar quaisquer regras que não são mais válidas e criar novas usando o padrão de sequência de ações.

