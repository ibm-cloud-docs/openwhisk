---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-17"

keywords: namespaces, iam, cloud foundry, classic namespaces

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



# Gerenciando namespaces
{: #namespaces}

Com o {{site.data.keyword.openwhisk}}, é possível criar namespaces gerenciados do Identity and Access (IAM) para agrupar entidades, tais como ações ou acionadores, juntos. Em seguida, é possível criar políticas de acesso do IAM para o namespace.
{: shortdesc}


**O que é um namespace?**

Os namespaces contêm entidades do {{site.data.keyword.openwhisk_short}}, como ações e acionadores, e pertencem a um grupo de recursos. É possível permitir que os usuários acessem suas entidades concedendo a eles acesso ao namespace.

O nome completo de uma entidade é `/namespaceName/[packageName]/entityName`.


**O que acontece quando eu crio um namespace?**

Os namespaces que são criados no {{site.data.keyword.openwhisk_short}} são identificados como uma instância de serviço do IAM. Durante a criação de um namespace, é possível especificar o [grupo de recursos](/docs/resources?topic=resources-rgs) no qual incluir a instância de serviço.

Quando você cria seu namespace, os artefatos a seguir são criados ao mesmo tempo:

* Um ID de serviço que pode ser usado como um ID funcional quando você faz chamadas de saída. Todas as ações que são criadas nesse namespace podem usar esse ID de serviço para acesso a outros recursos. Para ver todos os seus IDs de serviço, execute `ibmcloud iam service-ids`.

* Uma chave de API para o ID de serviço que pode ser usada para gerar tokens do IAM. Em seguida, é possível usar os tokens para autenticar o namespace com outros serviços do {{site.data.keyword.Bluemix_notm}}. A chave de API é fornecida para as ações como variável de ambiente.

    Não exclua as chaves de API.
    {: tip}

**Há alguma limitação para namespaces?**

A [criação de APIs com o API Gateway](/docs/openwhisk?topic=cloud-functions-apigateway) e o uso do [SDK móvel](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) não são suportados para namespaces gerenciados pelo IAM.

O {{site.data.keyword.openwhisk_short}} tem restrições sobre nomes de namespace. Para obter mais informações, consulte a documentação [Detalhes e limites do sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities_ov).
{: tip}



**O que eu faço se tenho um namespace baseado em Cloud Foundry?**

Os namespaces baseados no Cloud Foundry continuarão a funcionar. No entanto, para aproveitar os novos recursos, deve-se [migrar seus namespaces para o IAM](/docs/resources?topic=resources-migrate).

</br>


## Criando um namespace com a CLI
{: #namespaces_create}

É possível criar um namespace gerenciado do IAM como parte de um grupo de recursos e gerenciar políticas de acesso para seus recursos, destinando o grupo de recursos quando um namespace é criado. Se você tiver outros usuários que requeiram acesso ao seu namespace ou se desejar acessar outros recursos por meio de suas ações do namespace, certifique-se de configurar políticas do IAM depois que seu namespace for criado.
{: shortdesc}

1. Destine o grupo de recursos no qual você deseja criar o namespace. Se você não tiver criado um [grupo de recursos](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) ainda, poderá destinar o grupo `default`.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crie um namespace ativado pelo IAM.

  ```
  ibmcloud fn namespace create <namespace_name> [-n <description>]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>O nome de exibição para o namespace baseado em IAM.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Opcional: inclua uma descrição no namespace, como quais tipos de ações ou pacotes ele conterá.</td>
      </tr>
    </tbody>
  </table>

  Exemplo de Saída:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Verifique se seu novo namespace foi criado.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Exemplo de Saída:

  ```
  Details of namespace: 'myNamespace'
    Description: 'short description'
    Resource Plan Id: 'functions-base-plan'
    Location: 'jp-tok'
    ID: '05bae599-ead6-4ccb-9ca3-94ce8c8b3e43'
  ```
  {: screen}

  Também é possível listar todos os namespaces, incluindo namespaces baseados em IAM e Cloud Foundry:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Antes de criar entidades no namespace, configure seu contexto da CLI para o namespace fazendo com que ele seja o destino.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

</br>

## Criando um Espaço de Nomes com a API
{: #namespaces_create_api}

É possível criar um namespace gerenciado do IAM como parte de um grupo de recursos e gerenciar políticas de acesso para seus recursos, destinando o grupo de recursos quando um namespace é criado. Se você tiver outros usuários que requeiram acesso ao seu namespace ou se desejar acessar outros recursos por meio de suas ações do namespace, certifique-se de configurar políticas do IAM depois que seu namespace for criado.
{: shortdesc}


1. Destine o grupo de recursos no qual você deseja criar o namespace. Se você não tiver criado um [grupo de recursos](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) ainda, poderá destinar o grupo `default`.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crie um namespace ativado pelo IAM.

  ```
  curl -- request POST \
    -- url ' https: //jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    -- cabeçalho 'accept: application / json' \
    -- cabeçalho 'autorização: < IAM_token>' \
    --data '{ "description": "string", "nome": ":", "resource_group_id": "cadeia", "resource_plan_id": "string" }'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>Seu token do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Para recuperar seu token do IAM, execute <code>ibmcloud iam oauth-tokens</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>O nome do namespace.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>O ID do grupo de recursos no qual você deseja criar o namespace. Para ver IDs de grupo de recursos, execute <code>ibmcloud resource groups</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>O ID do plano de recurso, como functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Opcional: inclua uma descrição no namespace, como quais tipos de ações ou pacotes ele conterá.</td>
      </tr>
    </tbody>
  </table>

  Exemplo de Saída:

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. Verifique se seu novo namespace foi criado.

  ```
  curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Também é possível listar todos os namespaces, incluindo namespaces baseados em IAM e Cloud Foundry:
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}


Para obter mais informações sobre como trabalhar com o HTTP REST, confira os [docs da API do {{site.data.keyword.openwhisk_short}}](/apidocs/functions).
{: tip}



## Próximas etapas
{: #namespaces_next}

Agora que você criou um namespace, é possível criar políticas de acesso do IAM para ajudar a protegê-lo. Para iniciar, confira [Gerenciando acesso](/docs/openwhisk?topic=cloud-functions-iam). Para obter mais informações sobre como é possível gerenciar namespaces baseados em IAM, consulte a [Referência da API de REST do {{site.data.keyword.openwhisk_short}}](/apidocs/functions).


