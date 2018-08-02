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

# Origem de eventos do Cloudant
{: #openwhisk_cloudant}

Saiba como receber mudanças para um banco de dados {{site.data.keyword.cloudant}}, filtrar os eventos de mudança do banco de dados e usar uma sequência de ações para processar um documento de um banco de dados {{site.data.keyword.cloudant_short_notm}}. O pacote `/whisk.system/cloudant` permite trabalhar com um banco de dados {{site.data.keyword.cloudant_short_notm}} e inclui as ações e os feeds a seguir:

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/cloudant` | pacote | dbname, host, username, password | Trabalhe com um banco de dados Cloudant. |
| `/whisk.system/cloudant/read` | ação | dbname, id | Leia um documento de um banco de dados. |
| `/whisk.system/cloudant/write` | ação | dbname, overwrite, doc | Grave um documento em um banco de dados. |
| `/whisk.system/cloudant/changes` | Feed | Dbname, filtro, query_params, maxTriggers | Acione eventos de disparo nas mudanças em um banco de dados. |
{: shortdesc}

As seções a seguir conduzem você pela configuração de um pacote associado e como usar ações e feeds no pacote `/whisk.system/cloudant`. Para obter mais informações sobre como configurar o banco de dados {{site.data.keyword.cloudant_short_notm}} e ler ou gravar nele, veja [Ações do {{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html).

## Criar um acionador usando a função de filtro

É possível usar o feed `changes` para configurar um serviço para disparar um acionador em cada mudança para seu banco de dados {{site.data.keyword.cloudant_short_notm}}.

Os parâmetros usados neste exemplo são como a seguir:

**dbname**: o nome do banco de dados {{site.data.keyword.cloudant_short_notm}} _(necessário)_.

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

Se você não puder observar novas ativações, veja o tópico [{{site.data.keyword.cloudant_short_notm}}](./cloudant_actions.html) que demonstra como ler de um banco de dados {{site.data.keyword.cloudant_short_notm}} e gravar nele. Teste as etapas de leitura e gravação para ajudar a verificar se suas credenciais do {{site.data.keyword.cloudant_short_notm}} estão corretas.
{: tip}

## Estrutura de dados de um evento acionador

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

## Eventos de mudança de dados do Filtro

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

## Usando uma sequência de ações e um acionador de mudança para processar um documento de um banco de dados {{site.data.keyword.cloudant_short_notm}}
{: #openwhisk_catalog_cloudant_read_change notoc}

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

É possível recriar os acionadores criados anteriormente com o `includeDoc`. Sigas estas etapas para recriar o acionador:
```
ibmcloud fn trigger delete myCloudantTrigger
```
{: pre}

```
ibmcloud fn trigger create myCloudantTrigger --feed /_/myCloudant/changes --param dbname testdb
```
{: pre}

O exemplo pode ser usado para criar uma sequência de ações para ler o documento mudado e chamar as ações existentes. Lembre-se de desativar quaisquer regras que não são mais válidas e criar novas usando o padrão de sequência de ações.
