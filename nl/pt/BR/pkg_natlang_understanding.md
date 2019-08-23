---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: natural language, understanding, watson knowledge studio, functions

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


# {{site.data.keyword.nlushort}}
{: #pkg_natlang_understanding}

O serviço instalável do {{site.data.keyword.nlufull}} ajuda a analisar vários recursos de conteúdo de texto em escala.
{: shortdesc}

Forneça o texto, o HTML bruto ou uma URL pública e o {{site.data.keyword.nlushort}} fornecerá resultados para os recursos que forem solicitados. O serviço limpa o conteúdo HTML antes da análise por padrão, de modo que os resultados possam ignorar a maioria dos anúncios e outro conteúdo indesejado. É possível criar <a target="_blank" href="https://www.ibm.com/watson/developercloud/doc/natural-language-understanding/customizing.html">modelos customizados</a> com o Watson Knowledge Studio que podem ser usados para detectar entidades e relações customizadas no Natural Language Understanding.

O pacote do  {{site.data.keyword.nlushort}}  contém as entidades a seguir. Para obter mais informações, consulte a referência da API do {{site.data.keyword.nlushort}} clicando no nome da entidade.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`natural-language-understanding-v1`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl.html){: external} | Pacote | `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,  |  Trabalhe com o serviço  {{site.data.keyword.nlushort}} . |
| [`analyze`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#analyze){: external} | Ação |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `features`, `text`, `html`, `url`, `clean`, `xpath`, `fallback_to_raw`, `return_analyzed_text`, `language`, `limit_text_characters`,  | Analisar texto, HTML ou uma página da web pública. |
| [`delete-model`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#delete-model){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id`,  | Excluir modelo. |
| [`list-models`](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/?curl#list-models){: external} | Ação |  `username`, `password`,  `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, | Listar modelos. |

## Criando uma instância de serviço do  {{site.data.keyword.nlushort}}
{: #service_instance_understanding}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.nlushort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.nlushort}}](https://cloud.ibm.com/catalog/services/natural-language-understanding){: external}.
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.nlushort}}
{: #install_understanding}

Depois de ter uma instância de serviço do {{site.data.keyword.nlushort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.nlushort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #nlus_cli}

**Antes de iniciar** [Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do  {{site.data.keyword.nlushort}} :

1. Clone o repo do pacote do  {{site.data.keyword.nlushort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/natural-language-understanding-v1/manifest.yaml
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
    /myOrg_mySpace/natural-language-understanding-v1                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.nlushort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind natural-language-entendimento natural-language-understanding-v1
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Credentials 'Credentials-1' from 'natural-language-understanding' service instance 'Watson Natural Language Understanding' bound to 'natural-language-understanding-v1'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.nlushort}}.
    ```
    ibmcloud fn package get parameters-language-understanding-v1 parameters
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ok: got package natural-language-understanding-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "natural-language-entendimento": {
            "credentials": "Credentials-1",
            "instance": "Watson Natural Language Understanding",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/natural-language-understanding/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #nlus_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Selecione o namespace no qual você deseja instalar o pacote usando o menu suspenso de namespace. Os namespaces são formados por meio de nomes de organização e de espaço combinados.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Entendimento de língua natural**.

5. Clique em  ** Instalar **.

6. Depois que o pacote é instalado, você é redirecionado para a página de ações e pode procurar seu novo pacote, que é denominado **natural-language-understanding-v1**.

7. Para usar as ações no pacote **natural-language-understanding-v1**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 4 e 5 nas [instruções da CLI](#nlus_cli).
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. 

  Deve-se concluir as etapas a seguir para cada ação que você deseja usar.
  {: note}

    1. Clique em uma Ação no pacote **natural-language-understanding-v1** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo **parâmetro**. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.nlushort}}
{: #usage_understanding}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke natural-language-understanding-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Todas as ações requerem um parâmetro de versão no formato AAAA-MM-DD. Quando a API mudar de uma maneira inversa de forma incompatível, uma nova data da versão será liberada. Consulte mais detalhes na
[Referência da API](https://www.ibm.com/watson/developercloud/natural-language-understanding/api/v1/#versioning){: external}.

As funções deste pacote usam a versão atual do Natural Language Understanding, 2018-03-16. Experimente a ação  ` list-models ` .
```
ibmcloud fn action invoke natural-language-understanding-v1/list-models -b -p version 2018-03-16
```
{: pre}



