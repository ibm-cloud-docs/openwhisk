---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: package, cognitive, functions

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


# {{site.data.keyword.speechtotextshort}}
{: #pkg_speech_to_text}

## Pacotes

| Nome | Disponibilidade | Descrição |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](#speech_to_text) | Instalável | Trabalha com o serviço {{site.data.keyword.speechtotextshort}} V1. |
| [`/whisk.system/watson-speechToText`](#preinstall_speechtotext) | Pré-instalado (Não disponível em Tóquio) | Chame as APIs do Watson para converter a fala em texto |

## Serviço {{site.data.keyword.speechtotextshort}}
{: #speech_to_text}

O serviço instalável {{site.data.keyword.speechtotextfull}} fornece uma [API](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} que usa os recursos de reconhecimento de voz da IBM para produzir transcrições de áudio falado.
{:shortdesc}

O serviço pode transcrever a fala de vários idiomas e formatos de áudio. Além da transcrição básica, o serviço pode produzir informações detalhadas sobre muitos aspectos do áudio. Para a maioria dos idiomas, o serviço suporta duas taxas de amostragem, banda larga e banda estreita. Ele retorna todo o conteúdo de resposta JSON no conjunto de caracteres UTF-8. Para obter mais informações sobre o serviço, consulte a [Documentação do IBM&reg; Cloud](/docs/services/speech-to-text?topic=speech-to-text-about).

O pacote do  {{site.data.keyword.speechtotextshort}}  contém as entidades a seguir. É possível localizar mais informações na referência da API do {{site.data.keyword.speechtotextshort}} clicando no nome da entidade.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`speech-to-text-v1`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html){: external} | Pacote | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Trabalhe com o serviço  {{site.data.keyword.speechtotextshort}}  V1. |
| [`get-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `model_id` | Obtenha um modelo. |
| [`list-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-models){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listar modelos. |
| [`recognize-sessionless`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#recognize-sessionless){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Reconhecer áudio (sem ter sessionless). |
| [`check-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-job){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id`  | Verificar uma tarefa. |
| [`check-jobs`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#check-jobs){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Verifique as tarefas. |
| [`create-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-job){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `audio`, `content_type`, `model`, `callback_url`, `events`, `user_token`, `results_ttl`, `customization_id`, `acoustic_customization_id`, `base_model_version`, `customization_weight`, `inactivity_timeout`, `keywords`, `keywords_threshold`, `max_alternatives`, `word_alternatives_threshold`, `word_confidence`, `timestamps`, `profanity_filter`, `smart_formatting`, `speaker_labels` | Crie uma atividade. |
| [`delete-job`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-job){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `id` | Excluir uma tarefa. |
| [`register-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#register-callback){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url`, `user_secret` | Registre um retorno de chamada. |
| [`unregister-callback`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#unregister-callback){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `callback_url` | Cancelar o registro de um retorno de chamada. |
| [`create-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-language-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`, `dialect`, `description` | Crie um modelo de idioma customizado. |
| [`delete-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-language-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Exclua um modelo de idioma customizado. |
| [`get-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-language-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Obtenha um modelo de idioma customizado. |
| [`list-language-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-language-models){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`,  `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Liste os modelos de idioma customizado. |
| [`reset-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-language-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reconfigurar um modelo de idioma customizado. |
| [`train-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-language-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type_to_add`, `customization_weight` | Treinar um modelo de idioma customizado. |
| [`upgrade-language-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-language-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Faça upgrade de um modelo de idioma customizado. |
| [`add-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-corpus){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name`, `corpus_file`, `allow_overwrite` | Incluir um corpus. |
| [`delete-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-corpus){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Excluir um corpus. |
| [`get-corpus`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-corpus){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `corpus_name` | Obtenha um corpus. |
| [`list-corpora`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-corpora){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listar corpo. |
| [`add-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-word){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name`, `word`, `sounds_like`, `display_as` | Inclua uma palavra customizada. |
| [`add-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-words){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words` | Incluir palavras customizadas. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-word){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Exclua uma palavra customizada. |
| [`get-word`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-word){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_name` | Obtenha uma palavra customizada. |
| [`list-words`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-words){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word_type`, `sort` | Listar palavras customizadas. |
| [`create-acoustic-model](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#create-acoustic-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `base_model_name`,` description` | Crie um modelo acústico customizado. |
| [`delete-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-acoustic-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Exclua um modelo acústico customizado. |
| [`get-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-acoustic-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Obtenha um modelo acústico customizado. |
| [`list-acoustic-models`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-acoustic-models){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Liste modelos acústicos customizados. |
| [`reset-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#reset-acoustic-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Reconfigurar um modelo acústico customizado. |
| [`train-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#train-acoustic-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Treinar um modelo de acústica customizado. |
| [`upgrade-acoustic-model`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#upgrade-acoustic-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `custom_language_model_id` | Faça upgrade de um modelo acústico customizado. |
| [`add-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#add-audio){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name`, `audio_resource`, `content_type`, `contained_content_type`, `allow_overwrite` | Incluir um recurso de áudio. |
| [`delete-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-audio){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Exclua um recurso de áudio. |
| [`get-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#get-audio){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `audio_name` | Obtenha um recurso de áudio. |
| [`list-audio`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#list-audio){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listar recursos de áudio. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/curl.html?curl#delete-user-data){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Exclua os dados rotulados. |

### Criando uma instância de serviço do  {{site.data.keyword.speechtotextshort}}
{: #service_instance_speechtotext}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.speechtotextshort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.speechtotextshort}}](https://cloud.ibm.com/catalog/services/speech_to_text){: external}.
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

### Instalando o pacote do  {{site.data.keyword.speechtotextshort}}
{: #install_speechtotext}

Depois que você tem uma instância de serviço do {{site.data.keyword.speechtotextshort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.speechtotextshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #speechtotext_cli}

**Antes de iniciar**

[Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do  {{site.data.keyword.speechtotextshort}} :

1. Clone o repo do pacote do  {{site.data.keyword.speechtotextshort}} .

    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.

    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/speech-to-text-v1/manifest.yaml
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
    /myOrg_mySpace/speech-to-text-v1                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.speechtotextshort}} que você criou ao pacote.

    ```
    ibmcloud fn service bind speech_to_text UNK-to-text-v1
    ```
    {: pre}

    Dependendo da região em que você criou a instância de serviço, a instância de serviço poderá ser nomeada de forma diferente porque ela é um serviço do IAM. Se o comando falhar, use o nome de serviço a seguir para o comando bind:
    ```
    ibmcloud fn service bind speech-to-text speech-to-text-v1
    ```
    {: pre}

    **Saída de exemplo**

    ```
    Credentials 'Credentials-1' from 'speech_to_text' service instance 'Watson Speech to Text' bound to 'speech-to-text-v1'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.speechtotextshort}}.

    ```
    ibmcloud fn package get UNK-to-text-v1 parameters
    ```
    {: pre}

    **Saída de exemplo**

    ```
    ok: got package speech-to-text-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "speech_to_text": {
            "credentials": "Credentials-1",
            "instance": "Watson Speech to Text",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/speech_to_text/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #speechtotext_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Selecione o namespace no qual você deseja instalar o pacote usando o menu suspenso de namespace.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Fala para texto**.

5. Clique em  ** Instalar **.

6. Depois que o pacote é instalado, você é redirecionado para a página de ações e pode procurar seu novo pacote, que é denominado `speech-to-text-v1`.

7. Para usar as ações no pacote **speech-to-text-v1**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 4 e 5 nas [instruções da CLI](#speechtotext_cli).
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. 
  
  Deve-se concluir as etapas a seguir para cada ação que você deseja usar.
  {: note}

    1. Clique em uma ação a partir do pacote **discurso-para-texto-v1**que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo **parâmetro**. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

### Usando o pacote do  {{site.data.keyword.speechtotextshort}}
{: #usage_speechtotext}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke speech-to-text-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Experimente a ação  ` list-models ` .
```
ibmcloud fn action invoke UNK-to-text-v1/list-models -b
```
{: pre}

## Watson: fala para texto
{: #preinstall_speechtotext}

Esse pacote pré-instalado não está disponível na região de Tóquio. Consulte o [Voz para texto](#install_speechtotext) instalável.
{: tip}

O pacote `/whisk.system/watson-speechToText` oferece uma maneira conveniente de chamar APIs do Watson para converter a fala em texto.
{: shortdesc}

O pacote inclui as ações a seguir.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | Pacote | `username`, `password` | Pacote para converter a fala em texto. |
| `/whisk.system/watson-speechToText/speechToText` | Ação | `payload`, `content_type`, `encoding`, `username`, `password`, `continuous`, `inactivity_timeout`, `interim_results`, `keywords`, `keywords_threshold`, `max_alternatives`, `model`, `timestamps`, `watson-token`, `word_alternatives_threshold`, `word_confidence`, `X-Watson-Learning-Opt-Out` | Converter áudio em texto. |

O pacote `/whisk.system/watson` está descontinuado, incluindo a ação `/whisk.system/watson/speechToText`. Em vez disso, consulte o [pacote do {{site.data.keyword.speechtotextshort}}](#setting-up-the-watson-speech-to-text-package-in-ibm-cloud).
{: deprecated}

### Configurando o pacote Watson Speech to Text no {{site.data.keyword.cloud_notm}}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.cloud_notm}}, as ligações de pacote serão criadas automaticamente
para suas instâncias de serviço do {{site.data.keyword.cloud_notm}} Watson.

1. Crie uma instância de serviço do Watson Speech to Text em seu {{site.data.keyword.cloud_notm}} painel do [](https://cloud.ibm.com){: external}. Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.cloud_notm}} no qual você se encontra.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Watson que você criou.
  ```
  Atualização do pacote ibmcloud fn
  ```
  {: pre}

  **Saída de exemplo**

  ```
  Ligações criadas:
  Watson_SpeechToText_Credentials-1
  ```
  {: screen}

3. Listar pacotes para ver se a ligação foi criada.

  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Saída de exemplo**

  ```
  Pacotes
  /myOrg_mySpace/Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

### Configurando um pacote Watson Speech to Text fora do {{site.data.keyword.cloud_notm}}

Se você não está usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.cloud_notm}} ou se deseja configurar o Watson Speech to Text fora do {{site.data.keyword.cloud_notm}}, deve-se criar manualmente uma ligação de pacote para o serviço Watson Speech to Text. Você precisa do nome do usuário e da senha do serviço de Fala do Watson para Texto.

Crie uma ligação de pacote que esteja configurada para o seu serviço de Fala do Watson para Texto.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

### Convertendo fala para texto

A ação `/whisk.system/watson-speechToText/speechToText` converte fala de áudio em texto. Os parâmetros a seguir são suportados.

| Parâmetros | Descrição |
| --- | --- |
| `username` | O nome do usuário da API do Watson. |
| `password` | A senha da API do Watson. |
| `payload` | Os dados binários de fala codificados para transformar em texto. |
| `content_type` | O tipo MIME do áudio. |
| `encoding` | A codificação dos dados binários da fala. |
| `continuous` | Indica se vários resultados finais que representam frases consecutivas que são separadas por pausas longas são retornados. |
| `inactivity_timeout` | O tempo, em segundos, após o qual a conexão será encerrada se for detectado apenas silêncio no áudio enviado. |
| `interim_results` | Indica se o serviço é para retornar resultados provisórios. |
| `keywords` | Uma lista de palavras-chave para detectar no áudio. |
| `keywords_threshold` | Um valor de confiança que é o limite inferior para identificar uma palavra-chave. |
| `max_alternatives` | O número máximo de transcrições alternativas a serem retornadas. |
| `model` | O identificador do modelo a ser usado para a solicitação de reconhecimento. |
| `timestamps` | Indica se o alinhamento de hora é retornado para cada palavra. |
| `watson-token` | Fornece um token de autenticação para o serviço como uma alternativa para fornecer credenciais de serviço. |
| `word_alternatives_threshold` | Um valor de confiança que é o limite inferior para identificar uma hipótese como uma possível alternativa de palavra. |
| `word_confidence` | Indica se uma medida de confiança no intervalo de 0 a 1 deve ser retornada para cada palavra. |
| `X-Watson-Learning-Opt-Out` | Indica se deve ser feito opt-out da coleta de dados para a chamada. |

Chame a ação `speechToText` em sua ligação do pacote para converter o áudio codificado.

```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

**Saída de exemplo**
```
{
  "data": "Hello Watson"
}
```
{: screen}


