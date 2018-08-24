---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.texttospeechshort}} pacote 

O serviço do {{site.data.keyword.texttospeechfull}} fornece uma API que usa os recursos de síntese de discurso da IBM para sintetizar texto em discurso de som natural em uma variedade de idiomas, dialetos e vozes.
{:shortdesc}

O serviço suporta pelo menos uma voz masculina ou feminina, às vezes ambas, para cada idioma. O áudio é transmitido de volta para o cliente com um atraso mínimo. Para obter mais informações sobre o serviço, consulte a [Documentação do IBM Cloud](https://console.bluemix.net/docs/services/text-to-speech/index.html).

O pacote do  {{site.data.keyword.texttospeechshort}}  contém as entidades a seguir. É possível localizar detalhes adicionais na referência da API do {{site.data.keyword.texttospeechshort}} clicando no nome da entidade.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`text-to-speech-v1`](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html) | pacote | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabalhe com o serviço  {{site.data.keyword.texttospeechshort}} . |
| [get-voice](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    voice,     customization_id,  |Obter uma voz. |
| [list-voices](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voices) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, | Listar vozes. |
| [synthesize](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#synthesize) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   text,     accept,     voice,     customization_id,  | Sincronizar áudio. |
| [get-pronunciation](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-pronunciation) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    text,     voice,     format,     customization_id,  | Obtenha pronúncia. |
| [create-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#create-voice-model) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,   name, language, description,  | Crie um modelo customizado. |
| [delete-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-voice-model) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customization_id,  | Exclua um modelo customizado. |
| [get-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-voice-model) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customization_id,  | Obtenha um modelo customizado. |
| [list-voice-models](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-voice-models) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, language,  | Liste modelos customizados. |
| [update-voice-model](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#update-voice-model) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,    name, description, words,  |Atualizar um modelo customizado. |
| [add-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-word) | ação |  username, password,  iam_access_token, iam_apikey, iam_url,  headers, headers[X-Watson-Learning-Opt-Out], url,    customization_id,     word,    translation, part_of_speech,  | Inclua uma palavra customizada. |
| [add-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#add-words) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customization_id, words,  | Incluir palavras customizadas. |
| [delete-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-word) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customization_id, word,  | Exclua uma palavra customizada. |
| [get-word](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#get-word) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customization_id, word,  | Obtenha uma palavra customizada. |
| [list-words](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#list-words) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customization_id,  | Listar palavras customizadas. |
| [delete-user-data](https://www.ibm.com/watson/developercloud/text-to-speech/api/v1/curl.html?curl#delete-user-data) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, customer_id,  | Exclua os dados rotulados. |

## Criando uma instância de serviço do  {{site.data.keyword.texttospeechshort}}
{: #service_instance}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.texttospeechshort}}.
{: shortdesc}

1. [Criar uma instância de serviço do {{site.data.keyword.texttospeechshort}}![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog/services/text_to_speech).
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.texttospeechshort}}
{: #install}

Depois que você tem uma instância de serviço do {{site.data.keyword.texttospeechshort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.texttospeechshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #texttospeech_cli}

Antes de Iniciar:
  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](bluemix_cli.html#cloudfunctions_cli).
  2. Instale o comando [`wskdeploy`![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) e inclua o binário transferido por download em seu PATH.

Para instalar o pacote do  {{site.data.keyword.texttospeechshort}} :

1. Clone o repo do pacote do  {{site.data.keyword.texttospeechshort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    wskdeploy -m openwhisk-sdk/packages/text-to-fala-v1/manifest.yaml
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
    /myOrg_mySpace/text-to-speech-v1                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.texttospeechshort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind text_to_fala text-to-fala-v1
    ```
    {: pre}

    Dependendo da região em que você criou a instância de serviço, a instância de serviço poderá ser nomeada de forma diferente porque ela é um serviço do IAM. Se o comando acima falhar, use o nome do serviço a seguir para o comando BIND:
    ```
    ibmcloud fn service bind text-to-speech text-to-speech-v1
    ```
    {: pre}
    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'text_to_speech' service instance 'Watson Text to Speech' bound to 'text-to-speech-v1'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.texttospeechshort}}.
    ```
    ibmcloud fn package get text-to-fala-v1 parameters
    ```
    {: pre}

    Exemplo de Saída:
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

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/create).

2. Usando as listas de **Organizações do Cloud Foundry** e **Espaços do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_short}}. Os namespaces são formados por meio da organização combinada e nomes de espaço.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes do  ** Watson ** .

5. Clique no Pacote **Text To Speech**.

5. Clique em  ** Instalar **.

6. Depois que o Pacote tiver sido instalado, você será redirecionado para a página Ações e poderá procurar pelo seu novo Pacote, que é denominado **text-to-speech-v1**.

7. Para usar as Ações no pacote **text-to-speech-v1**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima. 
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma Ação no Pacote **text-to-speech-v1** que você deseja usar. A página de detalhes para essa Ação é aberta. 
    2. Na navegação à esquerda, clique na seção **Parâmetros**. 
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.texttospeechshort}}
{: #usage}

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
