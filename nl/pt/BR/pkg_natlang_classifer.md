---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: machine learning, functions

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


# {{site.data.keyword.nlclassifiershort}}
{: #pkg_natlang_classifier}

O {{site.data.keyword.nlclassifierfull}} usa algoritmos de aprendizado de máquina para retornar as classes predefinidas correspondentes para entrada de texto curto. É possível criar e treinar um classificador para conectar classes predefinidas a textos de exemplo para que o serviço possa aplicar essas classes a novas entradas.
{: shortdesc}

O pacote do  {{site.data.keyword.nlclassifiershort}}  contém as entidades a seguir. É possível
localizar detalhes adicionais na referência da API do {{site.data.keyword.nlclassifiershort}}
clicando no nome da entidade.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`natural-language-classifier-v1`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html){: external} | Pacote | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Trabalhe com o serviço  {{site.data.keyword.nlclassifiershort}} . |
| [`classify`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `text`,  | Classificar uma frase. |
| [`classify-collection`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#classify-collection){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `collection` | Classificar múltiplas frases. |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#create-classifier){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `metadata`, `training_data` | Crie um classificador. |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#delete-classifier){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id` | Excluir um classificador. |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#get-classifier){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id` | Obtenha informações sobre um classificador. |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/natural-language-classifier/api/v1/curl.html?curl#list-classifiers){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listar classificadores. |


## Criando uma instância de serviço do  {{site.data.keyword.nlclassifiershort}}
{: #service_instance_classifier}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.nlclassifiershort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.nlclassifiershort}}](https://cloud.ibm.com/catalog/services/natural_language_classifier){: external}.
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.nlclassifiershort}}
{: #install_classifier}

Depois de ter uma instância de serviço do {{site.data.keyword.nlclassifiershort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.nlclassifiershort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #nlclassifier_cli}

**Antes de iniciar** [Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do {{site.data.keyword.nlclassifiershort}}, execute o comando a seguir.

1. Clone o repo do pacote do  {{site.data.keyword.nlclassifiershort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-classifier-v1/manifest.yaml
    ```
    {: pre}

3. Verifique se o pacote foi incluído em sua lista de pacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Saída**
    ```
    packages
    /myOrg_mySpace/natural-language-classifier-v1                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.nlclassifiershort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind natural_language_classifier natural-language-classifier-v1
    ```
    {: pre}

    Dependendo da região em que você criou a instância de serviço, a instância de serviço poderá ser nomeada de forma diferente porque ela é um serviço do IAM. Se o comando falhar, use o nome de serviço a seguir para o comando bind:
    ```
    ibmcloud fn service bind natural-language-classifier natural-language-classifier-v1
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Credentials 'Credentials-1' from 'natural_language_classifier' service instance 'Watson Natural Language Classifier' bound to 'natural-language-classifier-v1'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.nlclassifiershort}}.
    ```
    ibmcloud fn package get parameters-language-classifier-v1 parameters
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ok: got package natural-language-classifier-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "natural_language_classifier": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Classifier",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural_language_classifier/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #nlclassifier_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote. 

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Classificador de língua natural**.

5. Clique em  ** Instalar **.

6. Depois que o pacote é instalado, você é redirecionado para a página de ações e pode procurar seu novo pacote, que é denominado **natural-language-classifier-v1**.

7. Para usar as ações no pacote **natural-language-classifier-v1**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 4 e 5 nas [instruções da CLI](#nlclassifier_cli).
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. 
  
  Deve-se concluir as etapas a seguir para cada ação que você deseja usar.
  {: note}

    1. Clique em uma ação por meio do Pacote **natural-language-classifier-v1** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo **parâmetro**. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.nlclassifiershort}}
{: #usage_classifier}

Para usar as ações nesse pacote, execute comandos no formato a seguir.

```
ibmcloud fn action invoke natural-language-classifier-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Experimente a ação  ` list-classificfiers ` .
```
ibmcloud fn action invoke natural-language-classifier-v1/list-classifiers -b
```
{: pre}



