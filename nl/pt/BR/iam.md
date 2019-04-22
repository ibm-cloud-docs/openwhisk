---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-29"

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

As políticas do IAM estão disponíveis na região de Tóquio somente para {{site.data.keyword.openwhisk_short}}. Se você operar na região de Tóquio, deverá usar políticas do IAM para controlar o acesso.
{: tip}

</br>

## Mapeando funções do IAM para {{site.data.keyword.openwhisk_short}}
{: #user-roles}

No {{site.data.keyword.openwhisk_short}}, os namespaces são considerados um recurso do IBM Cloud que permite trabalhar com funções e políticas do IAM para gerenciamento de acesso. Todas as políticas que você configura para um namespace também se aplicam às entidades do {{site.data.keyword.openwhisk_short}}, como ações ou acionadores, que o namespace contém.
{: shortdesc}

O {{site.data.keyword.openwhisk_short}} usa as funções de gerenciamento de Plataforma e Serviço. É possível configurar políticas sobre quem pode criar namespaces no nível da plataforma, ao usar as funções de serviço para gerenciar a interação com os namespaces em si.

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


## Configurando políticas de acesso do IAM
{: #set-iam}

Quando um serviço chama uma ação, ela tem uma resposta. Como a resposta é enviada do namespace ou a ação para um serviço, ela é considerada informações de saída. Se desejar limitar a quantidade de influência que seu namespace tem em outros serviços, talvez você queira criar uma política de acesso.
{: shortdesc}

Para obter informações sobre como designar, editar, revisar ou excluir políticas de acesso de recurso, consulte [Gerenciando o acesso do IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).
{: tip}




</br>
</br>

## Acessando outros recursos por meio de um namespace
{: #namespace-access}

É possível acessar outros recursos por meio de um namespace gerenciado do IAM usando um token do IAM. Um token representa a autenticação e verifica a identidade do recurso. O token do IAM é necessário para autenticar ao acessar serviços ou recursos gerenciados do IAM.
{: shortdesc}

Semelhante a como um ID do usuário identifica um usuário, um ID de serviço representa um recurso específico. Isso significa que as políticas do IAM podem ser aplicadas a esses recursos que gerenciam as permissões de acesso. Assim como um usuário, um recurso deve autenticar para verificar sua identidade. Dentro do Functions, isso pode ser aproveitado pela implementação das ações ao acessar outros serviços ou recursos.

Quando você cria um novo namespace gerenciado do IAM, o Functions cria automaticamente um ID de serviço correspondente que identifica o namespace e uma chave de API. No tempo de execução, o Cloud Functions passa a chave de API para o código de ação como o valor da variável de ambiente `__OW_IAM_NAMESPACE_KEY`. O código de ação pode usar essa chave de API para gerar um token do IAM. A maioria dos SDKs suportados, como Cloudant, Watson e COS, é autenticada com a própria chave do IAM em si. Outros serviços ou recursos gerenciados do IAM que usam uma API de REST, autenticam-se com o token que é derivado da chave do IAM.

Não tem certeza de como as chaves de API e os tokens se ajustam? Saiba mais em [os docs do IAM](/docs/iam?topic=iam-iamapikeysforservices).
