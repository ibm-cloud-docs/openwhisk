---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-10"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# Gerenciando acesso
{: #iam}

O {{site.data.keyword.openwhisk}} suporta o Identity and Access Management (IAM). Agora é possível definir políticas do IAM para seus recursos, como namespaces.
{: shortdesc}

</br>

## Mapeando funções do IAM para {{site.data.keyword.openwhisk_short}}
{: #user-roles}

No {{site.data.keyword.openwhisk_short}}, o namespace é considerado um recurso do {{site.data.keyword.Bluemix_notm}} que permite trabalhar com funções e políticas do IAM para gerenciamento de acesso. Todas as políticas que você configura para um namespace também se aplicam às entidades do {{site.data.keyword.openwhisk_short}}, como ações ou acionadores, que o namespace contém.
{: shortdesc}

O {{site.data.keyword.openwhisk_short}} usa as funções de gerenciamento de plataforma e de serviço. É possível configurar políticas sobre quem pode criar namespaces no nível da plataforma, ao usar as funções de serviço para gerenciar a interação com os namespaces em si.

Deseja aprender mais sobre os conceitos principais do IAM? Confira [os documentos do IAM](/docs/iam?topic=iam-iamconcepts#iamconcepts).
{: tip}

</br>

### Funções de gerenciamento de plataforma

A tabela a seguir detalha as ações que são mapeadas para funções de gerenciamento de plataforma. As funções de gerenciamento de plataforma permitem que os usuários executem tarefas em recursos de serviço no nível de plataforma. Por exemplo, designe acesso de usuário para o serviço, crie ou exclua IDs de serviço, crie instâncias e ligue instâncias aos aplicativos.
{: shortdesc}

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
      <td>Os usuários são capazes de criar namespaces.</td>
    </tr>
  </tbody>
</table>

Você precisa ter a função de administrador para o gerenciamento de plataforma a fim de trabalhar com o serviço. Para obter mais informações sobre as funções, confira as [Funções de gerenciamento da plataforma](/docs/iam?topic=iam-userroles).

</br>

### Funções específicas do serviço
{: #service_specific_roles}

As funções específicas do serviço determinam o escopo de uma política de acesso dentro de um serviço específico. Para o {{site.data.keyword.openwhisk_short}}, as funções podem se aplicar à capacidade de um usuário para usar o serviço, como acessar a UI ou executar chamadas de API.
{: shortdesc}


É importante observar que as permissões se baseiam umas nas outras. Por exemplo, qualquer operação que a função `writer` é capaz de executar, a função `manager` também pode. No entanto, a função `manager` teria mais permissões incluídas. Para ver as permissões gerais para cada função, confira [Funções de acesso de serviço](/docs/iam?topic=iam-userroles).

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
    <td>Visualize as entidades disponíveis, como regras, dentro de namespaces.</td>
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

Para obter informações sobre como designar funções de usuário na IU, consulte [Gerenciando o acesso ao IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

</br>


## Configurando políticas de acesso do IAM para um namespace
{: #set-iam}

Ao gerenciar um namespace ou as entidades dentro, é possível conceder acesso a outros usuários usando as [funções específicas de serviço](#service_specific_roles) listadas acima. Durante a criação do namespace, um ID de serviço é criado, representando o namespace juntamente com um ID de usuário funcional. Por padrão, o ID do usuário funcional é designado à função de Leitor. Os Leitores podem ler entidades de namespace e chamar ações. A função de Leitor é usada por acionadores para chamar ações. Para controlar o tráfego de entrada, talvez você queira conceder acesso a outros usuários, como a designação de função de Leitor para chamar ações.
{: shortdesc}

Para obter informações sobre como designar, editar, revisar ou excluir políticas de acesso de recurso, consulte [Gerenciando o acesso do IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Acessando outros recursos por meio de um namespace
{: #namespace-access}

As ações geralmente chamam outros recursos e serviços do {{site.data.keyword.Bluemix_notm}} que requerem a autenticação apropriada. Se esses serviços forem ativados pelo IAM e aceitarem tokens do IAM, será possível alavancar o ID funcional do namespace para comunicação de saída. Conforme descrito em [Gerenciando o acesso do IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), para cada namespace, um ID de serviço é criado, representando o namespace. É possível conceder acesso a outros serviços e recursos para esse ID de serviço, designando as funções apropriadas usando o gerenciamento de política do IAM.

No tempo de execução, o {{site.data.keyword.openwhisk_short}} transmite uma chave de API do ID de serviço de namespace para o código de ação como o valor da variável de ambiente `__OW_IAM_NAMESPACE_API_KEY`. O código de ação pode usar essa chave de API para gerar um token do IAM. A maioria dos SDKs do {{site.data.keyword.openwhisk_short}} suportados, como Cloudant, {{site.data.keyword.watson}} e {{site.data.keyword.cos_full_notm}} é autenticada com a própria chave de API do IAM. Para outros serviços ou recursos gerenciados do IAM que usam uma API de REST, é possível autenticar com o token derivado da chave de API do IAM. Para obter mais informações, consulte [Criar um token de acesso do IAM para um usuário ou um ID de serviço](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Não tem certeza de como as chaves de API e os tokens se ajustam? Saiba mais em [os docs do IAM](/docs/iam?topic=iam-iamapikeysforservices).

