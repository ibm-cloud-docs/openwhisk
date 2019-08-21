---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# Gerenciando namespaces
{: #namespaces}

Com o {{site.data.keyword.openwhisk}}, é possível criar namespaces gerenciados do Identity and Access (IAM) para agrupar entidades, tais como ações ou acionadores, juntos. Em seguida, é possível criar políticas de acesso do IAM para o namespace. Para obter uma visão geral do IAM, consulte o [Blog de anúncio de ativação do IAM do {{site.data.keyword.openwhisk_short}}](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}.
{: shortdesc}

## O que é um namespace?

Os namespaces contêm entidades do {{site.data.keyword.openwhisk_short}}, como ações e acionadores, e pertencem a um grupo de recursos. É possível permitir que os usuários acessem suas entidades concedendo a eles acesso ao namespace.

O nome completo de uma entidade é `/namespaceName/packageName/entityName`.

### O que acontece quando eu crio um namespace?

Os namespaces que são criados no {{site.data.keyword.openwhisk_short}} são identificados como uma instância de serviço do IAM.
Durante a criação de um namespace, é possível especificar o [grupo de recursos](/docs/resources?topic=resources-rgs) no qual incluir a instância de serviço.

Quando você cria um namespace, os componentes a seguir são criados:

| Componente | Descrição |
| --- | --- | 
| Um ID de serviço | É possível usar o ID de serviço como um ID funcional quando você faz chamadas de saída. Todas as ações que são criadas nesse namespace podem usar esse ID de serviço para acesso a outros recursos. O usuário funcional obtém a função de Leitor por padrão. O acesso de Leitor significa que ele pode ler entidades de namespace e chamar ações. A função de Leitor é usada por acionadores para chamar ações. Para controlar o tráfego de entrada, talvez você queira conceder acesso a outros usuários, como a designação de função de Leitor para chamar ações. |
| Uma chave de API | Uma chave de API para o ID de serviço que pode ser usada para gerar tokens do IAM. É possível usar os tokens para autenticar o namespace com outros serviços do {{site.data.keyword.cloud_notm}}. A chave de API é fornecida para as ações como a variável de ambiente `__OW_IAM_NAMESPACE_API_KEY`. |

Visualizar todos os seus IDs de serviço.
```
ibmcloud iam service-ids
```
{: pre}

Visualize as chaves de API que estão associadas a um ID de serviço. 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

Não exclua as chaves de API.
{: tip}

### Há alguma limitação para namespaces?

A [criação de APIs com o gateway de API](/docs/openwhisk?topic=cloud-functions-apigateway) e o [SDK móvel](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) não são suportados para namespaces gerenciados pelo IAM. 

Os nomes de todas as entidades, incluindo ações, acionadores, regras, pacotes e
namespaces, são uma sequência de caracteres que seguem o formato a seguir:
* O primeiro caractere deve ser um caractere alfanumérico ou um sublinhado.
* Os caracteres subsequentes podem ser alfanuméricos, espaços ou qualquer um dos valores a seguir: `_`, `@`, `.`, `-`.
* O último caractere não pode ser um espaço.

### O que eu faço caso tenha um namespace baseado no Cloud Foundry?

Seus namespaces baseados no Cloud Foundry ainda funcionam. No entanto, para aproveitar os novos recursos, deve-se criar um namespace ativado pelo IAM.


## Criando um namespace baseado no IAM na IU
{: #create_iam_ui}

1. No [console do {{site.data.keyword.openwhisk_short}} ](https://cloud.ibm.com/openwhisk){: external}, clique no menu suspenso do namespace.

2. Clique em **Criar namespace**.

3. Insira um nome de exibição para o namespace e uma descrição simples, como os tipos de ações ou pacotes que você planeja criar nesse namespace.

4. Escolha o grupo de recursos no qual você deseja criar o namespace e um local no qual implementar o recurso de namespace.

5. Clique em **Criar**.

6. Para visualizar a instância de serviço para o recurso de namespace, acesse seu [painel do {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: external} e localize seu nome de namespace na área de janela **Namespaces do Functions**.

Se for necessário, será possível atualizar o nome ou a descrição do namespace na página **Configurações de namespace** no console do {{site.data.keyword.openwhisk_short}}.

## Criando um namespace baseado no IAM com a CLI
{: #namespaces_create}

É possível criar um namespace gerenciado pelo IAM como parte de um grupo de recursos e gerenciar políticas de acesso para seus recursos, direcionando o grupo de recursos quando um namespace é criado. Se você tiver outros usuários que requeiram acesso ao seu namespace ou se desejar acessar outros recursos por meio de suas ações do namespace, certifique-se de configurar políticas do IAM depois que seu namespace for criado.
{: shortdesc}

1. Destine o grupo de recursos no qual você deseja criar o namespace. Se você não tiver criado um [grupo de recursos](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create) ainda, poderá destinar o grupo `default`.

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crie um namespace ativado pelo IAM. Opcional: inclua uma descrição para seu namespace usando a sinalização `-n` ou `--description`. Se a sua descrição for maior que uma palavra, ela deverá estar entre aspas.

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
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
        <td>Opcional: inclua uma descrição no namespace, como quais tipos de ações ou pacotes você planeja criar. Se a sua descrição for maior que uma palavra, ela deverá estar entre aspas.</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>Opcional: inclua uma descrição no namespace, como quais tipos de ações ou pacotes você planeja criar. Se a sua descrição for maior que uma palavra, ela deverá estar entre aspas.</td>
      </tr>
    </tbody>
  </table>

  Saída de exemplo:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Verifique se seu novo namespace foi criado.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Saída de exemplo:

  ```
  Detalhes do namespace: myNamespace
  Descrição: short description
  ID de plano de recursos: functions-base-plan
  Localização: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  Também é possível listar todos os namespaces, incluindo namespaces baseados em IAM e Cloud Foundry:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Antes de poder criar entidades no namespace, deve-se configurar o contexto da CLI para o namespace, destinando-o.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

Depois de configurar uma propriedade, como a propriedade `--namespace`, ela será retida até você desconfigurá-la manualmente. Se você desejar alternar entre namespaces do IAM ou entre o Cloud Foundry e o IAM, deverá desconfigurar a propriedade de namespace e reconfigurá-la. Para obter mais informações, consulte [`ibmcloud fn property set`]
{: note}

## Criando um Espaço de Nomes com a API
{: #namespaces_create_api}

É possível criar um namespace gerenciado pelo IAM como parte de um grupo de recursos e gerenciar políticas de acesso para seus recursos, direcionando o grupo de recursos quando um namespace é criado. Se você tiver outros usuários que requeiram acesso ao seu namespace ou se desejar acessar outros recursos por meio de suas ações do namespace, certifique-se de configurar políticas do IAM depois que seu namespace for criado.
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
        <td>Seu token do {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Para recuperar seu token do IAM, execute <code>ibmcloud iam oauth-tokens</code>.</td>
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

  **Saída de exemplo**

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

  **Saída de exemplo**

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

### Acessando outros recursos por meio de um namespace
{: #namespace-access}

As ações geralmente chamam outros recursos e serviços do {{site.data.keyword.cloud_notm}} que requerem autenticação apropriada. Se esses serviços estiverem ativados pelo IAM e aceitarem tokens do IAM, será possível alavancar o ID funcional do namespace para comunicação de saída.
{: shortdesc}

Conforme descrito em [Gerenciando o acesso do IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), para cada namespace, um ID de serviço é criado, representando o namespace. É possível conceder acesso a outros serviços e recursos para esse ID de serviço, designando as funções apropriadas, usando o gerenciamento de política do IAM. Para obter mais informações sobre como criar IDs de serviço para acessar outros serviços ativados pelo IAM, consulte [Criando e trabalhando com IDs de serviço](/docs/iam?topic=iam-serviceids#serviceids).

No tempo de execução, o {{site.data.keyword.openwhisk_short}} transmite uma chave de API do ID do serviço de namespace para o código de ação como a variável de ambiente `__OW_IAM_NAMESPACE_KEY`. O código de ação pode usar essa chave de API para gerar um token do IAM. A maioria dos SDKs do {{site.data.keyword.openwhisk_short}} suportados, como Cloudant, {{site.data.keyword.watson}} e {{site.data.keyword.cos_full_notm}} é autenticada com a própria chave de API do IAM. Para outros serviços ou recursos gerenciados pelo IAM que usam uma API de REST, é possível autenticar com o token que é derivado da chave de API do IAM. Para obter mais informações, consulte [Criar um token de acesso do IAM para um usuário ou um ID de serviço](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Não tem certeza de como as chaves de API e os tokens se ajustam? Saiba mais em [os docs do IAM](/docs/iam?topic=iam-iamapikeysforservices).

## Próximas etapas
{: #namespaces_next}

Agora que você criou um namespace, é possível criar políticas de acesso do IAM para ajudar a protegê-lo. Para iniciar, confira [Gerenciando acesso](/docs/openwhisk?topic=cloud-functions-iam). 

Para obter mais informações sobre como gerenciar namespaces baseados no IAM, consulte a [referência de API de REST do {{site.data.keyword.openwhisk_short}}](/apidocs/functions).








