---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-05"

keywords: cloudant, database, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Pacote Cloudant
{: #cloudant_actions}

O pacote `/whisk.system/cloudant` permite trabalhar com um banco de dados do
[{{site.data.keyword.cloudant}}](/docs/services/Cloudant?topic=cloudant-getting-started#getting-started)
e inclui as ações e os feeds a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacote | dbname, host, username, password | Trabalhe com um banco de dados Cloudant. |
| `/whisk.system/cloudant/read` | ação | dbname, id | Leia um documento de um banco de dados. |
| `/whisk.system/cloudant/write` | ação | dbname, overwrite, doc | Grave um documento em um banco de dados. |
| `/whisk.system/cloudant/changes` | alimentação | dbname, iamApiKey, iamUrl, filter, query_params, maxTriggers | Acione eventos de disparo nas mudanças em um banco de dados. |
{: shortdesc}

As seções a seguir o guiam por meio da configuração de um banco de dados do {{site.data.keyword.cloudant_short_notm}} e como ler e gravar nele.
Para obter mais informações sobre como usar os feeds com o pacote `/whisk.system/cloudant`, consulte a [origem de eventos do {{site.data.keyword.cloudant_short_notm}}](/docs/openwhisk?topic=cloud-functions-openwhisk_cloudant).

## Configurando um Banco de Dados {{site.data.keyword.cloudant_short_notm}}no {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.Bluemix_notm}}, será possível usar o [plug-in da CLI do {{site.data.keyword.openwhisk}}](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli) para ligar um serviço a uma ação ou a um pacote.

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
  

## Lendo por meio de um banco de dados do {{site.data.keyword.cloudant_short_notm}}
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

## Gravando em um banco de dados do {{site.data.keyword.cloudant_short_notm}}
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
