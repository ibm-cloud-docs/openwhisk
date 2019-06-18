---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: tone analyzer, functions, serverless, watson

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

# {{site.data.keyword.toneanalyzershort}}
{: #pkg_tone_analyzer}

O serviço instalável do {{site.data.keyword.toneanalyzerfull}} usa a análise linguística para detectar tons emocionais e linguísticos no texto escrito.
{:shortdesc}

O serviço pode analisar o tom tanto no nível do documento quanto no da sentença. É possível usar o serviço para entender como suas comunicações por escrito são percebidas e, em seguida, melhorar o tom de suas comunicações. As empresas podem usar o serviço para aprender o tom das comunicações dos clientes e para responder a cada cliente apropriadamente ou para entender e melhorar suas conversas com o cliente.

**Nota:** a criação de log de solicitação está desativada para o serviço do Tone Analizer. O serviço não registra nem retém dados de solicitações e respostas, independentemente de o cabeçalho de solicitação `X-Watson-Learning-Opt-Out` estar configurado.

O pacote do  {{site.data.keyword.toneanalyzershort}}  contém as entidades a seguir. É possível localizar detalhes adicionais na referência da API do {{site.data.keyword.toneanalyzershort}} clicando no nome da entidade.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`tone-analyzer-v3`](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html) | pacote | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabalhe com o serviço  {{site.data.keyword.toneanalyzershort}} . |
| [tone](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    tone_input,     content_type,     sentences,     tones,     content_language,     accept_language,  | Analisar o tom geral. |
| [tone-chat](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#tone-chat) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   utterances,     content_language,     accept_language,  | Analise o tom de engajamento do cliente |

## Criando uma instância de serviço do  {{site.data.keyword.toneanalyzershort}}
{: #service_instance_tone}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.toneanalyzershort}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/tone_analyzer).
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.toneanalyzershort}}
{: #install_tone}

Depois que você tem uma instância de serviço do {{site.data.keyword.toneanalyzershort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.toneanalyzershort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #toneanalyzer_cli}

Antes de começar:
  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do  {{site.data.keyword.toneanalyzershort}} :

1. Clone o repo do pacote do  {{site.data.keyword.toneanalyzershort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/tone-analyzer-v3/manifest.yaml
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
    /myOrg_mySpace/tone-analyzer-v3                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.toneanalyzershort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind tone_analyzer tone-analyzer-v3
    ```
    {: pre}

    Dependendo da região em que você criou a instância de serviço, a instância de serviço poderá ser nomeada de forma diferente porque ela é um serviço do IAM. Se o comando acima falhar, use o nome do serviço a seguir para o comando BIND:
    ```
    ibmcloud fn service bind tone-analyzer tone-analyzer-v3
    ```
    {: pre}
    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'tone_analyzer' service instance 'Watson Tone Analyzer' bound to 'tone-analyzer-v3'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.toneanalyzershort}}.
    ```
    ibmcloud fn package get tone-analyzer-v3 parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package tone-analyzer-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "tone_analyzer": {
            "credentials": "Credentials-1",
            "instance": "Watson Tone Analyzer",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #toneanalyzer_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/openwhisk/create).

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote. 

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Analisador de sinal**.

5. Clique em  ** Instalar **.

6. Depois que o pacote tiver sido instalado, você será redirecionado para a página de ações e poderá procurar por seu novo pacote, que é denominado **tone-analyzer-v3**.

7. Para usar as ações no pacote **tone-analyzer-v3**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima.
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma ação por meio do pacote **tone-analyzer-v3** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.toneanalyzershort}}
{: #usage_tone}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke tone-analyzer-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas as ações exigirão um parâmetro de versão no formato AAAA-MM-DD. Quando a API mudar de uma maneira inversa de forma incompatível, uma nova data da versão será liberada. Consulte mais detalhes na [Referência da API](https://www.ibm.com/watson/developercloud/tone-analyzer/api/v3/curl.html?curl#versioning).

As funções deste pacote usam a versão atual de Tone Analyzer, 2017-09-21. Experimente a ação  ` tone ` .
```
ibmcloud fn action invoke tone-analyzer-v3/tone -b -p version 2017-09-21 -p text "i hope you're having a wonderful day"
```
{: pre}
