---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Cloudant
{: #cloudant_actions}

O pacote `/whisk.system/cloudant` permite trabalhar com um banco de dados [{{site.data.keyword.cloudant}}](/docs/services/Cloudant/getting-started.html#getting-started-with-cloudant) e inclui as ações e os feeds a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacote | dbname, host, username, password | Trabalhe com um banco de dados Cloudant. |
| `/whisk.system/cloudant/read` | ação | dbname, id | Leia um documento de um banco de dados. |
| `/whisk.system/cloudant/write` | ação | dbname, overwrite, doc | Grave um documento em um banco de dados. |
| `/whisk.system/cloudant/changes` | Feed | Dbname, filtro, query_params, maxTriggers | Acione eventos de disparo nas mudanças em um banco de dados. |
{: shortdesc}

As seções a seguir conduzem você pela configuração de um banco de dados {{site.data.keyword.cloudant_short_notm}} e como ler e gravar nele.
Para obter mais informações sobre como usar feeds com o pacote `/whisk.system/cloudant`, veja [Origem de eventos do {{site.data.keyword.cloudant_short_notm}}](./openwhisk_cloudant.html).

## Configurando um banco de dados {{site.data.keyword.cloudant_short_notm}} no {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_cloud}

Se você está usando o {{site.data.keyword.openwhisk}} do {{site.data.keyword.Bluemix_notm}}, o {{site.data.keyword.openwhisk_short}} cria automaticamente ligações de pacote para suas instâncias de serviço {{site.data.keyword.cloudant_short_notm}}. Se você não está usando o {{site.data.keyword.openwhisk_short}} e o {{site.data.keyword.cloudant_short_notm}} por meio do {{site.data.keyword.Bluemix_notm}}, vá para a próxima seção.

1. Crie uma instância de serviço {{site.data.keyword.cloudant_short_notm}} em seu [painel do {{site.data.keyword.Bluemix_notm}}](http://console.bluemix.net).

  Certifique-se de criar uma chave de credencial para cada nova instância de serviço.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para cada instância de serviço {{site.data.keyword.cloudant_short_notm}} com uma chave de credencial definida.
  ```
  ibmcloud wsk package refresh
  ```
  {: pre}

  Exemplo de Saída:
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```
  {: screen}

  ```
  ibmcloud wsk package list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```
  {: screen}

  Sua ligação de pacote agora contém as credenciais que estão associadas à sua instância de serviço {{site.data.keyword.cloudant_short_notm}}.

3. Verifique se a ligação do pacote que foi criada anteriormente está configurada com seu host e credenciais da instância de serviço {{site.data.keyword.cloudant_short_notm}} {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters

  [
      {
          "key": "username",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix"
      },
      {
          "key": "host",
          "value": "cdb18832-2bbb-4df2-b7b1-Bluemix.cloudant.com"
      },
      {
          "key": "password",
          "value": "c9088667484a9ac827daf8884972737"
      }
  ]
  ```
  {: screen}

## Configurando um banco de dados {{site.data.keyword.cloudant_short_notm}} fora do {{site.data.keyword.Bluemix_notm}}
{: #cloudantdb_nocloud}

Se você não está usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar o banco de dados {{site.data.keyword.cloudant_short_notm}} fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para sua conta do {{site.data.keyword.cloudant_short_notm}}. Você precisa do nome do host, nome do usuário e senha da conta do {{site.data.keyword.cloudant_short_notm}}.

1. Crie uma ligação de pacote que esteja configurada para sua conta do {{site.data.keyword.cloudant_short_notm}}.
  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}


2. Verifique se a ligação do pacote existe.
  ```
  wsk package list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacotes
  /myNamespace/myCloudant private binding
  ```
  {: screen}

## Lendo de um banco de dados {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_read}

É possível usar uma ação para buscar um documento de um banco de dados {{site.data.keyword.cloudant_short_notm}} chamado **testdb**. Certifique-se de que esse banco de dados exista em sua conta do {{site.data.keyword.cloudant_short_notm}}.

- Busque um documento usando a ação **read** na ligação do pacote anteriormente criada. Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.
  ```
  ibmcloud wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
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

## Gravando em um banco de dados {{site.data.keyword.cloudant_short_notm}}
{: #cloudant_write}

É possível usar uma ação para armazenar um documento em um banco de dados {{site.data.keyword.cloudant_short_notm}} chamado **testdb**. Certifique-se de que esse banco de dados exista em sua conta do {{site.data.keyword.cloudant_short_notm}}.

1. Armazene um documento usando a ação **write** na ligação do pacote anteriormente criada. Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.
  ```
  ibmcloud wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
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
