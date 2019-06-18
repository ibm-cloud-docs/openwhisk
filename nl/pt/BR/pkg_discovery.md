---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: watson discovery, functions, watson, cognitive

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

# {{site.data.keyword.discoveryshort}}
{: #pkg_discovery}

O serviço instalável do {{site.data.keyword.discoveryfull}} é um mecanismo de procura cognitiva e de analítica de conteúdo que você pode incluir em aplicativos para identificar padrões, tendências e insights acionáveis a fim de conduzir uma melhor tomada de decisão. Unifique dados estruturados e
não estruturados de forma segura com conteúdo pré-enriquecido e use uma linguagem de consulta simplificada para eliminar a
necessidade de filtragem manual de resultados.
{: shortdesc}

O pacote do  {{site.data.keyword.discoveryshort}}  contém as entidades a seguir. É possível
localizar detalhes adicionais na referência da API do {{site.data.keyword.discoveryshort}} clicando
no nome da entidade.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`discovery-v1`](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html) | pacote | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabalhe com o serviço  {{site.data.keyword.discoveryshort}} . |
| [create-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-environment) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, description, size,  | Criar um ambiente. |
| [delete-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-environment) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id,  | Excluir um ambiente. |
| [get-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-environment) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id,  | Obtenha informações sobre um ambiente. |
| [list-environments](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-environments) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    name,  | Listar ambientes. |
| [list-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-fields) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,  | Listar campos ao longo de coleções. |
| [update-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-environment) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description,  | Atualizar um ambiente. |
| [create-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-configuration) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, conversions, enrichments, normalizations,  | Incluir uma configuração. |
| [delete-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-configuration) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, configuration_id,  | Excluir uma configuração. |
| [get-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-configuration) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, configuration_id,  | Obter detalhes de configuração. |
| [list-configurations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-configurations) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, name,  | Listar configurações. |
| [update-configuration](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-configuration) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration_id,    name, description, conversions, enrichments, normalizations,  | Atualizar uma configuração. |
| [test-configuration-in-environment](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#test-configuration-in-environment) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     configuration,     step,     configuration_id,     file,     metadata,     file_content_type,  | Teste uma configuração. |
| [create-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-collection) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,    name, description, configuration_id, language,  | Criar uma coleção. |
| [delete-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-collection) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Excluir uma coleção. |
| [get-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-collection) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Obter detalhes da coleta. |
| [list-collection-fields](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collection-fields) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Listar campos de coleta. |
| [list-collections](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-collections) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, name,  | Listar coleções. |
| [update-collection](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-collection) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    name, description, configuration_id,  | Atualizar uma coleção. |
| [create-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-expansions) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    expansions,  | Crie ou atualize a lista de expansão. |
| [delete-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-expansions) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Exclua a lista de expansão. |
| [list-expansions](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-expansions) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Obtenha a lista de expansão. |
| [add-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-document) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     file,     metadata,     file_content_type,  | Incluir um documento. |
| [delete-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-document) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, document_id,  | Excluir um documento. |
| [get-document-status](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-document-status) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, document_id,  | Obtenha detalhes do documento. |
| [update-document](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-document) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     document_id,     file,     metadata,     file_content_type,  | Atualizar um documento. |
| [federated-query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consultar documentos em múltiplas coleções. |
| [federated-query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#federated-query-notices) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_ids,     filter,     query,     natural_language_query,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consultar múltiplos avisos do sistema de coleta. |
| [query](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Consulte sua coleção. |
| [query-entities](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-entities) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    feature, entity, context, count, evidence_count,  | Consulte uma entidade do Gráfico de Conhecimento. |
| [query-notices](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-notices) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     filter,     query,     natural_language_query,     passages,     aggregation,     count,     return_fields,     offset,     sort,     highlight,     passages_fields,     passages_count,     passages_characters,     deduplicate_field,     similar,     similar_document_ids,     similar_fields,  | Avisos do sistema de consulta. |
| [query-relations](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#query-relations) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    entities, context, sort, filter, count, evidence_count,  | Consulte um relacionamento do Gráfico de Conhecimento. |
| [add-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#add-training-data) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,    natural_language_query, filter, examples,  | Incluir uma consulta nos dados de treinamento. |
| [create-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#create-training-example) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,    document_id, cross_reference, relevance,  | Incluir um exemplo na consulta de dados de treinamento. |
| [delete-all-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-all-training-data) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Excluir todos os dados de treinamento. |
| [delete-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-data) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, query_id,  | Excluir uma consulta de dados de treinamento. |
| [delete-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-training-example) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, query_id, example_id,  | Excluir um exemplo para consulta de dados de treinamento. |
| [get-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-data) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, query_id,  | Obter detalhes sobre uma consulta. |
| [get-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#get-training-example) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, query_id, example_id,  | Obter detalhes de um exemplo de dados de treinamento. |
| [list-training-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-data) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id,  | Listar dados de treinamento. |
| [list-training-examples](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#list-training-examples) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, environment_id, collection_id, query_id,  | Listar exemplos para uma consulta de dados de treinamento. |
| [update-training-example](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#update-training-example) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    environment_id,     collection_id,     query_id,     example_id,    cross_reference, relevance,  | Mudar um rótulo ou uma referência cruzada para um exemplo. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html?curl#delete-user-data) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customer_id,  | Exclua os dados rotulados. |

## Criando uma instância de serviço do  {{site.data.keyword.discoveryshort}}
{: #service_instance_discovery}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do
{{site.data.keyword.discoveryshort}}.

1. [Crie uma instância de serviço do {{site.data.keyword.discoveryshort}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/discovery).
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.discoveryshort}}
{: #install_discovery}

Depois que você tem uma instância de serviço do {{site.data.keyword.discoveryshort}}, use a CLI do
{{site.data.keyword.openwhisk}} para instalar o pacote do
{{site.data.keyword.discoveryshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #discovery_cli}

Antes de começar:
  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do  {{site.data.keyword.discoveryshort}} :

1. Clone o repo do pacote do  {{site.data.keyword.discoveryshort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/discovery-v1/manifest.yaml
    ```
    {: pre}

3. Verifique se o pacote foi incluído em sua lista de pacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Saída:
    ```
    packages
    /myOrg_mySpace/discovery-v1                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.discoveryshort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind discovery discovery-v1
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'discovery' service instance 'Watson Discovery' bound to 'discovery-v1'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.discoveryshort}}.
    ```
    ibmcloud fn package get discovery-v1 parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package discovery-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "descoberta": {
            "credentials": "Credentials-1",
            "instance": "Watson Discovery",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/discovery/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #discovery_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create).

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote. 

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes do  ** Watson ** .

5. Clique no pacote  ** Discovery ** .

5. Clique em  ** Instalar **.

6. Depois que o Pacote tiver sido instalado, você será redirecionado para a página Ações e poderá
procurar pelo seu novo Pacote, que é denominado **discovery-v1**.

7. Para usar as ações no pacote **discovery-v1**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, conclua a etapa 4 nas instruções da CLI listadas acima.
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma ação por meio do Pacote **discovery-v1** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.discoveryshort}}
{: #usage_discovery}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke discovery-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Todas as ações exigirão um parâmetro de versão no formato AAAA-MM-DD. Quando a API mudar de uma maneira inversa de forma incompatível, uma nova data da versão será liberada. Consulte mais detalhes na
[Referência da
API](https://www.ibm.com/watson/developercloud/discovery/api/v1/curl.html#versioning).

As funções desse pacote usam a versão atual do Discovery, 2018-03-05. Experimente a ação  ` list-environments ` .
```
ibmcloud fn action invoke discovery-v1/list-environments -b -p version 2018-03-05
```
{: pre}

