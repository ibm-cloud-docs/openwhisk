---

copyright:
  years: 2018
lastupdated: "2018-10-01"

keywords: events, serverless, push notifications

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


# Visualizando eventos do {{site.data.keyword.cloudaccesstrailshort}}
{: #activity_tracker}

É possível visualizar, gerenciar e auditar as atividades iniciadas pelo usuário feitas na instância de serviço do {{site.data.keyword.openwhisk}} usando o serviço {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}


Para obter mais informações sobre como o serviço funciona, consulte os [docs do {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started).


## Onde visualizar os eventos
{: #view}

Os eventos estão disponíveis no **domínio da conta** do {{site.data.keyword.cloudaccesstrailshort}} que está disponível na região do {{site.data.keyword.Bluemix_notm}} na qual o recurso de namespace do {{site.data.keyword.openwhisk_short}} está disponível. Para obter mais informações, consulte [Visualizando eventos de conta](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-view_acc_events).

1. Efetue login em sua conta do {{site.data.keyword.Bluemix_notm}}.
2. Por meio do catálogo, provisione uma instância do serviço {{site.data.keyword.cloudaccesstrailshort}} na mesma conta da instância do {{site.data.keyword.openwhisk}}.
3. Na guia **Gerenciar** do painel do {{site.data.keyword.cloudaccesstrailshort}}, clique em **Visualizar no Kibana**.
4. Configure o prazo durante o qual deseja visualizar os logs. O padrão é 15 min.
5. Na lista **Campos disponíveis**, clique em **tipo**. Clique no ícone de lupa para o **Activity Tracker** para limitar os logs somente aos rastreados pelo serviço.
6. É possível usar os outros campos disponíveis para limitar sua procura.

Para que usuários que não sejam o proprietário da conta visualizem os logs, o plano premium deve ser usado. Para permitir que outros usuários visualizem os eventos, consulte [Concedendo permissões para ver os eventos de conta](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions).
{: tip}


## Lista de eventos
{: #events}

Confira a tabela a seguir para obter uma lista de eventos que são enviados para o {{site.data.keyword.cloudaccesstrailshort}}.
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>Ação</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>functions.namespace.create</td>
      <td>Criar um recurso de namespace baseado em IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.migrate</td>
      <td>Migrar um namespace baseado em Cloud Foundry para se tornar ativado para o IAM</td>
    </tr>
    <tr>
      <td>functions.namespace.update</td>
      <td>Atualizar propriedades de recurso de namespace baseadas em IAM, como o nome ou a descrição de exibição</td>
    </tr>
    <tr>
      <td>functions.namespace.delete</td>
      <td>Excluir um recurso de namespace</td>
    </tr>
  </tbody>
</table>
