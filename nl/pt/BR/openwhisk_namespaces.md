---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: namespaces, actions, create

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Criando namespaces
{: #openwhisk_namespaces}

Na região de Tóquio, o {{site.data.keyword.openwhisk_short}} usa os namespaces gerenciados de Identity and Access (IAM) para agrupar entidades, como ações ou acionadores, juntos. Em seguida, é possível criar políticas de acesso para o namespace.
{: shortdesc}

Quando você cria um namespace {{site.data.keyword.openwhisk_short}}, ele é identificado como uma instância de serviço do IAM. As instâncias de serviço gerenciado do IAM devem ser criadas dentro de um [grupo de recursos](/docs/resources?topic=resources-rgs). É possível criar seu próprio grupo de recursos ou destinar o padrão. Para ver as instâncias de serviço do IAM que você tem em sua conta, é possível executar `ibmcloud resource service-instances`.

Os artefatos a seguir são criados em conjunto com seu namespace. Não os exclua.

* É criado um ID de Serviço que pode ser utilizado como um ID funcional quando você faz chamadas de saída. Todas as ações que são criadas nesse namespace podem usar esse ID de serviço para acesso a outros recursos. Para ver todos os IDs de serviço, execute `ibmcloud iam service-ids`.

* Uma chave de API é criada para o ID de serviço acima que pode ser usado para gerar tokens do IAM. Em seguida, é possível usar os tokens para autenticar o namespace com outros serviços do IBM Cloud. A chave de API é fornecida para as ações como variável de ambiente.


## Limitações
{: #limitations}

[Criar APIs com o API Gateway](/docs/openwhisk?topic=cloud-functions-openwhisk_apigateway) e usar o [SDK móvel](/docs/openwhisk?topic=cloud-functions-openwhisk_mobile_sdk) não são suportados para namespaces gerenciados pelo IAM neste momento.

</br>

Para destinar o serviço de back-end do {{site.data.keyword.openwhisk_short}} no local de Tóquio, você tem que anexar o `apihost` a todas as chamadas da CLI, como `ibmcloud fn namespace list --apihost jp-tok.functions.cloud.ibm.com`. Isso é temporário até que o local possa ser destinado por `ibmcloud target -r jp-tok`.
{: tip}



</br>
</br>


## Criando um namespace com a CLI
{: #create_iam_cli}

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
          <td>O nome de exibição para o novo namespace baseado em IAM.</td>
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
    ibmcloud fn namespace get <namespace_name_or_id> --no-entities
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
    ibmcloud fn namespace list --iam
    ```
    {: pre}

4. Antes de criar entidades no novo namespace, configure seu contexto da CLI para o namespace, destinando-o.
    ```
    ibmcloud fn property set --namespace <namespace_name_or_id>
    ```
    {: pre}

</br>

## Criando um Espaço de Nomes com a API
{: #create_iam_api}

É possível criar um namespace gerenciado do IAM como parte de um grupo de recursos e gerenciar políticas de acesso para seus recursos, destinando o grupo de recursos quando um namespace é criado. Se você tiver outros usuários que requeiram acesso ao seu namespace ou se desejar acessar outros recursos por meio de suas ações do namespace, certifique-se de configurar políticas do IAM depois que seu namespace for criado.
{: shortdesc}



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
          <td>Seu token do IBM Cloud Identity and Access Management (IAM). Para recuperar seu token do IAM, execute <code>ibmcloud iam oauth-tokens</code>.</td>
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


Para obter mais informações sobre como trabalhar com HTTP REST, verifique os [dos da API do Cloud Functions](https://cloud.ibm.com/apidocs/functions).
{: tip}

</br>
</br>


## Próximas etapas
{: #next}

Agora que você criou um namespace, é possível criar políticas de acesso do IAM para ajudar a protegê-lo. Para iniciar, confira [Gerenciando acesso](/docs/openwhisk?topic=cloud-functions-iam#iam). Para obter mais informações sobre como é possível gerenciar os namespaces baseados em IAM, consulte a [referência de API de REST do {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/apidocs/functions).
