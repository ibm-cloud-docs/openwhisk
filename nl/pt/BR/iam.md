---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# Configurando políticas de acesso
{: #iam}

## Mapeando funções do IAM para {{site.data.keyword.openwhisk_short}}
{: #user-roles}

No {{site.data.keyword.openwhisk_short}}, os namespaces são recursos do {{site.data.keyword.cloud_notm}} que você pode usar para trabalhar com funções e políticas do IAM para gerenciamento de acesso. Todas as políticas que você configura para um namespace também se aplicam às entidades do {{site.data.keyword.openwhisk_short}}, como ações ou acionadores, que o namespace contém.
{: shortdesc}

O {{site.data.keyword.openwhisk_short}} usa as funções de gerenciamento de Plataforma e Serviço. É possível configurar políticas sobre quem pode criar namespaces no nível da plataforma e usar as funções de serviço para gerenciar a interação com os próprios namespaces.

Deseja aprender mais sobre os conceitos principais do IAM? Confira [os documentos do IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Funções de gerenciamento de plataforma

A tabela a seguir detalha as ações que são mapeadas para funções de gerenciamento de plataforma. As funções de gerenciamento de plataforma permitem que os usuários executem tarefas em recursos de serviço no nível de plataforma. Por exemplo, designe acesso de usuário para o serviço, crie ou exclua IDs de serviço, crie instâncias e ligue instâncias aos aplicativos.
{: shortdesc}

Para obter mais informações sobre como designar, editar, revisar ou excluir políticas de acesso de recurso, consulte [Gerenciando o acesso do IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}

<table>
  <thead>
    <tr>
      <th>Função da plataforma</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Administrador</td>
      <td>Os usuários podem criar namespaces. A função de Administrador é necessária para executar as operações de `service id` e `apikey lock` durante a criação do serviço.</td>
    </tr>
  </tbody>
</table>

Como o serviço não precisa ser provisionado, a função de editor é a única função da plataforma que é necessária para trabalhar com o serviço. Para obter mais informações sobre as outras funções, consulte as [Funções de gerenciamento da plataforma](/docs/iam?topic=iam-userroles).

</br>

### Funções específicas do serviço
{: #service_specific_roles}

As funções específicas do serviço determinam o escopo de uma política de acesso dentro de um serviço específico. Para o {{site.data.keyword.openwhisk_short}}, as funções podem se aplicar à capacidade de um usuário para usar o serviço, como acessar a UI ou executar chamadas de API.
{: shortdesc}

Permissões se baseiam umas nas outras. Por exemplo, qualquer operação que a função `writer` é capaz de executar, a função `manager` também pode. No entanto, a função `manager` teria mais permissões incluídas. Para ver as permissões gerais para cada função, confira [Funções de acesso de serviço](/docs/iam?topic=iam-userroles).

Para ver quais funções são necessárias para executar cada operação, confira a tabela a seguir:

<table><caption>Quais funções podem executar quais operações?</caption>
  <tr>
    <th style="width:150px">Ação</th>
    <th style="width:2500px">Descrição</th>
    <th style="width:50px">Leitor</th>
    <th style="width:50px">Gravador</th>
    <th style="width:50px">Gerente</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>Atualize um namespace.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>Exclua um namespace.</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>Visualize os namespaces disponíveis.</td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Crie uma entidade, como uma ação, dentro de um namespace do Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Atualize uma entidade, como um pacote, dentro de um namespace do Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Exclua uma entidade, como um acionador, de um namespace do Functions.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>Visualize as entidades disponíveis, como regras, dentro de um namespace.</td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>Ative uma entidade, como uma ação, dentro de um namespace.</td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Recurso disponível" style="width:32px;" /></td>
  </tr>
</table>

</br>

### Configurando políticas por meio da CLI
{: #cli-set}

Para fornecer um recurso, como uma ação, em um acesso de serviço baseado no IAM a um serviço baseado no IAM, é possível criar uma política de acesso do IAM para o namespace no qual o recurso está.

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os componentes de comando <code>ibmcloud iam service-policy-create</code></th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>O ID do serviço para o namespace. Para ver todos os IDs de serviço, execute <code>ibmcloud iam service-ids</code>.</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>O tipo de função de acesso ao serviço do IAM que a ação deve ter para usar o serviço de destino. Para ver as funções suportadas para o outro serviço, execute <code>ibmcloud iam roles --service SERVICE_NAME</code>. Para obter mais informações, consulte [Funções de acesso do IAM](/docs/iam?topic=iam-userroles#service-access-roles).</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>O nome do outro tipo de serviço do {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>O GUID da outra instância de serviço à qual você deseja que a ação tenha acesso. Para obter o GUID da instância de serviço, execute <code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code>.</td>
    </tr>
  </tbody>
</table>

</br>

**Próximas etapas**
Para obter mais informações sobre o gerenciamento de credenciais de serviço, consulte o blog [Gerenciar credenciais de serviço para aplicativos sem servidor](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.



