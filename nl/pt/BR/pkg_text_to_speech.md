---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: cognitive, functions, packages

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


# Text to Speech
{: #pkg_text_to_speech}

## Opções de pacote

| Pacote | Disponibilidade | Descrição |
| --- | --- | --- | --- |
| [`/whisk.system/watson-textToSpeech`](#text_to_speech) | Pré-instalado (Não disponível em Tóquio) | Pacote para converter texto em fala |
| [`text-to-speech-v1`](#text_to_speech_ins) | Instalável | Trabalhe com o serviço  {{site.data.keyword.texttospeechshort}} . |

## Watson Text to Speech
{: #text_to_speech}

Esse pacote pré-instalado não está disponível na região de Tóquio. Consulte o pacote instalável do [Text to Speech](#text_to_speech_ins) usando a autenticação do IAM.
{: tip}

O pacote `/whisk.system/watson-textToSpeech` oferece uma maneira conveniente de chamar APIs do Watson para converter o texto em fala.
{: shortdesc}

O pacote inclui as ações a seguir.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Pacote | `username`, `password` | Pacote para converter texto em fala. |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Ação | `payload`, `voice`, `accept`, `encoding`, `username`, `password` | Converter texto em áudio. |

O pacote `/whisk.system/watson` está descontinuado, incluindo a ação `/whisk.system/watson/textToSpeech`. Em vez disso, consulte o [pacote instalável do {{site.data.keyword.texttospeechshort}}](#text_to_speech_ins).
{: deprecated}

### Configurando o pacote do {{site.data.keyword.texttospeechshort}} no {{site.data.keyword.cloud_notm}}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.cloud_notm}}, as ligações de pacote serão criadas automaticamente
para suas instâncias de serviço do {{site.data.keyword.cloud_notm}} Watson.

1. Crie uma instância de serviço do {{site.data.keyword.texttospeechshort}} em seu {{site.data.keyword.cloud_notm}} painel do [](https://cloud.ibm.com){: external}. Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.cloud_notm}} no qual você se encontra.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Watson que você criou.
  ```
  Atualização do pacote ibmcloud fn
  ```
  {: pre}

  **Saída de exemplo**
  ```
  Ligações criadas:
  Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Listar pacotes para ver se a ligação de pacote foi criada.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Saída de exemplo**
  ```
  Pacotes
  /myOrg_mySpace/Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

### Configurando um pacote do {{site.data.keyword.texttospeechshort}} fora do {{site.data.keyword.cloud_notm}}

Se você não estiver usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.cloud_notm}} ou se desejar configurar o {{site.data.keyword.texttospeechshort}} fora do {{site.data.keyword.cloud_notm}}, deverá criar manualmente uma ligação de pacote para seu serviço do {{site.data.keyword.texttospeechshort}}. Você precisa do nome do usuário do serviço do {{site.data.keyword.texttospeechshort}} e da senha.

Crie uma ligação de pacote configurada para o seu serviço {{site.data.keyword.texttospeechshort}}.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username <username> -p password <password>
```
{: pre}

### Convertendo texto para fala

A ação `/whisk.system/watson-textToSpeech/textToSpeech` converte texto em um discurso de áudio. Os parâmetros a seguir são suportados.

| `username` | O nome do usuário da API do Watson. |
| `password` | A senha da API do Watson. |
| `payload` | O texto para converter em fala. |
| `voice` | A voz do falante. |
| `accept` | O formato do arquivo de fala. |
| `encoding` | A codificação dos dados binários de fala. |

Teste a ação `textToSpeech` em sua ligação de pacote para converter o texto.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

**Saída de exemplo**
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}


## {{site.data.keyword.texttospeechshort}}
{: #text_to_speech_ins}

O serviço instalável do {{site.data.keyword.texttospeechfull}} fornece uma API com os recursos de síntese de discurso da IBM para sintetizar texto em um fala de sotaque natural em vários idiomas, dialetos e vozes.
{:shortdesc}

O serviço suporta pelo menos uma voz masculina ou feminina, às vezes ambas, para cada idioma. O áudio é transmitido de volta para o cliente com um atraso mínimo. Para obter mais informações sobre o serviço, consulte a [Documentação do IBM Cloud](/docs/services/text-to-speech?topic=text-to-speech-about).

O pacote do  {{site.data.keyword.texttospeechshort}}  contém as entidades a seguir. É possível localizar mais informações na referência da API do {{site.data.keyword.texttospeechshort}} clicando no nome da entidade.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html){: external} | Pacote | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Trabalhe com o serviço  {{site.data.keyword.texttospeechshort}} . |
| [`get-voice`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `voice`, `customization_id` | Obter uma voz. |
| [`list-voices`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url` | Listar vozes. |
| [`synthesize`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `accept`, `voice`, `customization_id` | Sincronizar áudio. |
| [`get-pronunciation`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `text`, `voice`, `format`, `customization_id` | Obtenha pronúncia. |
| [`create-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `name`, `language`, `description` | Crie um modelo customizado. |
| [`delete-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Exclua um modelo customizado. |
| [`get-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Obtenha um modelo customizado. |
| [`list-voice-models`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `language` | Liste modelos customizados. |
| [`update-voice-model`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `name`, `description`, `words` | Atualizar um modelo customizado. |
| [`add-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word`, `translation`, `part_of_speech` | Inclua uma palavra customizada. |
| [`add-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `words`  | Incluir palavras customizadas. |
| [`delete-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Exclua uma palavra customizada. |
| [`get-word`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id`, `word` | Obtenha uma palavra customizada. |
| [`list-words`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customization_id` | Listar palavras customizadas. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data){: external} | Ação | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id` | Exclua os dados rotulados. |

### Criando uma instância de serviço do  {{site.data.keyword.texttospeechshort}}
{: #service_instance_texttospeech}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.texttospeechshort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.texttospeechshort}}](https://cloud.ibm.com/catalog/services/text_to_speech){: external}.
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

### Instalando o pacote do  {{site.data.keyword.texttospeechshort}}
{: #install_texttospeech}

Depois que você tem uma instância de serviço do {{site.data.keyword.texttospeechshort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.texttospeechshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

**Antes de iniciar** [Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do {{site.data.keyword.texttospeechshort}}, execute os comandos a seguir.

1. Clone o repo do pacote do  {{site.data.keyword.texttospeechshort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/text-to-speech-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.texttospeechshort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind text_to_fala text-to-fala-v1
    ```
    {: pre}

    Dependendo da região em que você criou a instância de serviço, a instância de serviço poderá ser nomeada de forma diferente porque ela é um serviço do IAM. Se o comando acima falhar, use o nome do serviço a seguir para o comando bind:
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-fala-v1 parameters
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ok: got package text-to-speech-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "text_to_UNK": {
            "credentials": "Credentials-1",
            "instance": "Watson Text to Speech",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/text_to_speech/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #texttospeech_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Selecione o namespace no qual você deseja instalar o pacote usando o menu suspenso de namespace.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Texto para fala**.

5. Clique em  ** Instalar **.

6. Depois que o pacote tiver sido instalado, você será redirecionado para a página de ações e poderá procurar por seu novo pacote, que é denominado `text-to-speech-v1`.

7. Para usar as ações no pacote `text-to-speech-v1`, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas [4 e 6 nas instruções da CLI](#texttospeech_cli).
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU.

  Deve-se concluir as etapas a seguir para cada ação que você deseja usar.
  {: note}

    1. Clique em uma ação no pacote `text-to-speech-v1` que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo parâmetro. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.texttospeechshort}}
{: #usage_texttospeech}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke text-to-speech-v1/<action_name> -b -p <param name> <param>
```
{: pre}

Experimente a ação  ` list-vozes ` .
```
ibmcloud fn action invoke text-to-fala-v1/list-vozes -b
```
{: pre}


