---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote Cloudant
{: #openwhisk_catalog_cloudant}
O pacote `/whisk.system/cloudant` permite trabalhar com um banco de dados Cloudant. Ele inclui as ações e os feeds a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | Pacote | dbname, host, username, password | Trabalhar com um banco de dados Cloudant |
| `/whisk.system/cloudant/read` | Ação | dbname, id | Ler um documento a partir de um banco de dados |
| `/whisk.system/cloudant/write` | Ação | dbname, overwrite, doc | Gravar um documento em um banco de dados |
| `/whisk.system/cloudant/changes` | Feed | Dbname, filtro, query_params, maxTriggers | Disparar eventos acionadores sobre mudanças em um banco de dados |
{: shortdesc}

Os tópicos a seguir explicam como configurar um banco de dados Cloudant, como configurar um pacote associado e como usar as ações e os feeds no pacote `/whisk.system/cloudant`.

## Configurando um banco de dados Cloudant no {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_in}

Se você estiver usando o OpenWhisk no {{site.data.keyword.Bluemix_notm}}, o OpenWhisk criará automaticamente as ligações de pacotes para suas instâncias de serviço do Cloudant. Se você não estiver usando o OpenWhisk e o Cloudant no {{site.data.keyword.Bluemix_notm}}, vá para a próxima etapa.

1. Crie uma instância de serviço do Cloudant em seu [painel](http://console.ng.Bluemix.net) do {{site.data.keyword.Bluemix_notm}}.

  Certifique-se de criar uma chave de credencial para cada nova instância de serviço.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para cada instância de serviço do Cloudant com uma chave de credencial definida.

  ```
  wsk package refresh
  ```
  {: pre}
  ```
  created bindings:
  Bluemix_testCloudant_Credentials-1
  ```

  ```
  wsk package list
  ```
  {: pre}
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 private binding
  ```

  A sua ligação de pacote agora contém as credenciais que estão associadas à sua instância de serviço do Cloudant.

3. Verifique se a ligação do pacote que foi criada anteriormente está configurada
com seu host e credenciais da instância de serviço do {{site.data.keyword.Bluemix_notm}}
Cloudant.

  ```
  wsk package get /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1 parameters
  ```
  {: pre}
  ```
  ok: got package /myBluemixOrg_myBluemixSpace/Bluemix_testCloudant_Credentials-1, displaying field parameters
  ```
  ```json
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

## Configurando um banco de dados Cloudant fora do {{site.data.keyword.Bluemix_notm}}
{: #openwhisk_catalog_cloudant_outside}

Se você não está usando o OpenWhisk no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar seu banco de dados Cloudant fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para sua conta do Cloudant. Você
precisa do nome do host, do nome do usuário e da senha da conta do Cloudant.

1. Crie uma ligação de pacote que esteja configurada para sua conta do Cloudant.

  ```
  wsk package bind /whisk.system/cloudant myCloudant -p username MYUSERNAME -p password MYPASSWORD -p host MYCLOUDANTACCOUNT.cloudant.com
  ```
  {: pre}
  

2. Verifique se a ligação do pacote existe.

  ```
  wsk package list
  ```
  {: pre}
  ```
  pacotes
  /myNamespace/myCloudant private binding
  ```


## Recebendo mudanças em um banco de dados do Cloudant
{: #openwhisk_catalog_cloudant_listen}

### Eventos de mudança de dados do Filtro

É possível definir uma função de filtro para evitar ter eventos de mudança desnecessários que disparam o acionador.

Para criar uma nova função de filtro, é possível usar uma ação.

Crie um arquivo do documento json `design_doc.json` com a função de filtro a seguir
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

Crie um documento de design no banco de dados com a função de filtro
```
wsk action invoke /_/myCloudant/write -p dbname testdb -p overwrite true -P design_doc.json -r
```
As informações para o novo documento de design são impressas na tela.
```json
{
    "id": "_design/mailbox",
    "ok": true,
    "rev": "1-5c361ed5141bc7856d4d7c24e4daddfd"
}
```

### Crie o acionador usando a função de filtro

É possível usar o feed `changes` para configurar um serviço para disparar um acionador em cada mudança em seu banco de dados Cloudant. Os parâmetros são os seguintes:

- `dbname`: nome do banco de dados do Cloudant.
- `maxTriggers`: parar de disparar acionadores quando esse limite for atingido. O padrão é definido como infinite.
- `filter`: a função de filtro que é definida em um documento de design.
- `query_params`: parâmetros de consulta adicionais para a função de filtro.


1. Crie um acionador com o feed `changes` no pacote de ligação que você criou anteriormente. Inclua as funções `filter` e `query_params` para disparar o acionador na inclusão ou modificação um documento quando o status for `new`.
Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.

  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes \
  --param dbname testdb \
  --param filter "mailbox/by_status" \
  --param query_params '{"status":"new"}'
  ```
  {: pre}
  ```
  ok: feed do acionador myCloudantTrigger criado
  ```

2. Pesquisa de ativações.

  ```
  wsk activation poll
  ```
  {: pre}

3. Em seu painel do Cloudant, modifique um documento existente ou crie um novo.

4. Observe novas ativações para o acionador `myCloudantTrigger` para cada mudança de documento somente se o status do documento for `new` com base na função de filtro e no parâmetro de consulta.
  
  **Nota**: se você não for capaz de observar novas ativações, consulte as seções subsequentes sobre leitura e gravação em um banco de dados do Cloudant. O teste das etapas de leitura e gravação a seguir ajudam a verificar se as suas credenciais do Cloudant estão corretas.
  
  Agora é possível criar regras e associá-las a ações para reagir às atualizações do documento.
  
  O conteúdo dos eventos gerados tem os parâmetros a seguir:
  
  - `id`: o ID do documento.
  - `seq`: o identificador de sequência que é gerado pelo Cloudant.
  - `changes`: uma matriz de objetos, cada um dos quais tendo um campo `rev` que contém o ID da revisão do documento.
  
  A representação JSON do evento acionador é a seguinte:
  
  ```json
  {
      "id": "6ca436c44074c4c2aa6a40c9a188b348",
      "seq": "2-g1AAAAL9aJyV-GJCaEuqx4-BktQkYp_dmIfC",
      "changes": [
          {
              "rev": "2-da3f80848a480379486fb4a2ad98fa16"
          }
      ]
  }
  ```
  
## Gravando em um banco de dados do Cloudant
{: #openwhisk_catalog_cloudant_write}

É possível usar uma ação para armazenar um documento em um banco de dados Cloudant chamado `testdb`. Certifique-se de que esse banco de dados exista em sua conta do Cloudant.

1. Armazene um documento usando a ação `write` na ligação de pacote que você criou anteriormente. Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.

  ```
  wsk action invoke /_/myCloudant/write --blocking --result --param dbname testdb --param doc "{\"_id\":\"heisenberg\",\"name\":\"Walter White\"}"
  ```
  ```
  ok: invoked /_/myCloudant/write with id 62bf696b38464fd1bcaff216a68b8287
  ```
  ```json
  {
    "id": "heisenberg",
    "ok": true,
    "rev": "1-9a94fb93abc88d8863781a248f63c8c3"
  }
  ```

2. Verifique se o documento existe procurando-o em seu painel do Cloudant.

  A URL do painel para o banco de dados `testdb` é semelhante à seguinte: `https://MYCLOUDANTACCOUNT.cloudant.com/dashboard.html#database/testdb/_all_docs?limit=100`.


## Lendo de um banco de dados do Cloudant
{: #openwhisk_catalog_cloudant_read}

É possível usar uma ação para buscar um documento de um banco de dados Cloudant chamado `testdb`. Certifique-se de que esse banco de dados exista em sua conta do Cloudant.

- Busque um documento usando a ação `read` na ligação de pacote que você criou anteriormente. Certifique-se de substituir `/_/myCloudant` por seu nome de pacote.

  ```
  wsk action invoke /_/myCloudant/read --blocking --result --param dbname testdb --param id heisenberg
  ```
  {: pre}
  ```json
  {
    "_id": "heisenberg",
    "_rev": "1-9a94fb93abc88d8863781a248f63c8c3",
    "name": "Walter White"
  }
  ```

## Usando uma sequência de ações e um acionador de mudança para processar um documento de um banco de dados Cloudant
{: #openwhisk_catalog_cloudant_read_change notoc}

É possível usar uma sequência de ações em uma regra para buscar e processar o documento que está associado a um evento de mudança do Cloudant.

Código de amostra de uma ação que manipula um documento:
```javascript
function main(doc){
  return { "isWalter:" : doc.name === "Walter White"};
}
```

Crie a ação para processar o documento do Cloudant:
```
wsk action create myAction myAction.js
```
{: pre}

Para ler um documento do banco de dados, é possível usar a ação `read` do pacote do Cloudant.
A ação `read` pode ser composta por `myAction` para criar uma sequência de ações.
```
SequenceAction criar ação wsk -- /_/myCloudant/read sequência, myAction
```
{: pre}

A ação `sequenceAction` pode ser usada em uma regra que ativa a ação em novos eventos acionadores do Cloudant.
```
wsk rule create myRule myCloudantTrigger sequenceAction
```
{: pre}

**Nota** o acionador `changes` do Cloudant usado para suportar o parâmetro `includeDoc` que não é mais suportado.
  É possível recriar os acionadores criados anteriormente com o `includeDoc`. Siga estas etapas para recriar o acionador: 
  ```
  wsk trigger delete myCloudantTrigger
  ```
  {: pre}
  ```
  wsk trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
  ```
  {: pre}

  O exemplo pode ser usado para criar uma sequência de ações para ler o documento mudado e chamar suas ações existentes.
  Lembre-se de desativar quaisquer regras que não sejam mais válidas e criar novas usando o padrão de sequência de ações.

