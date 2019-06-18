---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: watson, translator, cognitive, translating text, language

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

# Translator

## {{site.data.keyword.languagetranslatorshort}} pacote

{: #pkg_translator}

O pacote instalável do {{site.data.keyword.languagetranslatorfull}} traduz texto de um idioma para outro. O serviço oferece múltiplos modelos de tradução fornecidos pela IBM que podem ser customizados com base em sua terminologia e idioma exclusivos.
{: shortdesc}

O pacote do  {{site.data.keyword.languagetranslatorshort}}  contém as entidades a seguir. É possível localizar detalhes adicionais na referência da API do {{site.data.keyword.languagetranslatorshort}} clicando no nome da entidade.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`language-translator-v3`](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html) | pacote | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabalhe com o serviço  {{site.data.keyword.languagetranslatorshort}} . |
| [translate](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#translate) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text, model_id, source, target,  | Converter texto. |
| [identify](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#identify) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,  | Identifique a linguagem do texto. |
| [list-identifiable-languages](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-identifiable-languages) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, | Liste os idiomas identificáveis. |
| [create-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#create-model) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    base_model_id,     name,     forced_glossary,     parallel_corpus,  | Criar um modelo. |
| [delete-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#delete-model) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, model_id,  | Excluir um modelo. |
| [get-model](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#get-model) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, model_id,  | Obter detalhes do modelo. |
| [list-models](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#list-models) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    source,     target,     default_models,  | Listar modelos. |

## Criando uma instância de serviço do {{site.data.keyword.languagetranslatorshort}}
{: #service_instance_translator}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.languagetranslatorshort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.languagetranslatorshort}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/language_translator).
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.languagetranslatorshort}}
{: #install_translator}

Depois que você tem uma instância de serviço do {{site.data.keyword.languagetranslatorshort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.languagetranslatorshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_cli}

Antes de começar:
  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#cloudfunctions_cli).

Para instalar o pacote do  {{site.data.keyword.languagetranslatorshort}} :

1. Clone o repo do pacote do  {{site.data.keyword.languagetranslatorshort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/language-translator-v3/manifest.yaml
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
    /myOrg_mySpace/language-translator-v3                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.languagetranslatorshort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind language-translator language-translator-v3
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'language-translator' service instance 'Watson Language Translator' bound to 'language-translator-v3'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.languagetranslatorshort}}.
    ```
    ibmcloud fn package get language-translator-v3 parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package language-translator-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "language-translator": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Language Translator-g2",
            "url": "https://gateway.watsonplatform.net/language-translator/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #languagetranslator_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create).

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote. Os namespaces são formados por meio de nomes de organização e de espaço combinados.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Tradutor de idiomas**.

5. Clique em  ** Instalar **.

6. Depois que o pacote tiver sido instalado, você será redirecionado para a página de ações e poderá procurar por seu novo pacote, que é denominado **language-translator-v3**.

7. Para usar as ações no Pacote **language-translator-v3**, você deve ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima.
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma ação por meio do pacote **language-translator-v3** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.languagetranslatorshort}}
{: #usage_translator}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke language-translator-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas as ações exigirão um parâmetro de versão no formato AAAA-MM-DD. Quando a API mudar de uma maneira inversa de forma incompatível, uma nova data da versão será liberada. Consulte mais detalhes na [Referência da API](https://www.ibm.com/watson/developercloud/language-translator/api/v3/curl.html?curl#versioning).

As funções desse pacote usam a versão atual do Language Translator, 2018-05-01. Experimente a ação  ` identificar ` .
```
ibmcloud fn action invoke language-translator-v3/identify -b -p version 2018-05-01 -p text hola
```
{: pre}



