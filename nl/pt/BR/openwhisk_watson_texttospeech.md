---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote de Texto do Watson para Fala
{: #openwhisk_catalog_watson_texttospeech}

O pacote `/whisk.system/watson-textToSpeech` oferece uma maneira conveniente de chamar APIs do Watson para converter o texto em fala.
{: shortdesc}

O pacote inclui as Ações a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | Pacote | username, password | Pacote para converter texto em fala |
| `/whisk.system/watson-textToSpeech/textToSpeech` | Ação | payload, voice, accept, encoding, username, password | Converter texto em áudio |

**Nota**: o pacote `/whisk.system/watson` está descontinuado, incluindo a ação `/whisk.system/watson/textToSpeech`.

## Configurando o pacote Watson Text to Speech no {{site.data.keyword.Bluemix_notm}}

Se estiver usando o OpenWhisk pelo {{site.data.keyword.Bluemix_notm}}, o OpenWhisk criará automaticamente ligações de pacotes para suas instâncias de serviço Watson do {{site.data.keyword.Bluemix_notm}}.

1. Crie uma instância de serviço do Watson Text to Speech em seu [painel](http://console.ng.Bluemix.net) do {{site.data.keyword.Bluemix_notm}}.
  
  Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.Bluemix_notm}} no qual você se encontra.
  
2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Watson que você criou.
  ```
  wsk package refresh
  ```
  
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  
  ```
  wsk package list
  ```
  {: pre}
  
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  
  
## Configurando um pacote Watson Text to Speech fora do {{site.data.keywrod.Bluemix_notm}}

Se você não está usando o OpenWhisk no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar o Watson Text to Speech fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para o seu serviço Watson Text to Speech. Você precisa do nome do usuário e da senha do serviço de Texto do Watson para Fala.

- Crie uma ligação de pacote que esteja configurada para o seu serviço de Fala do Watson para Texto.
  
  ```
  wsk package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}
  

## Converter algum texto para fala

A ação `/whisk.system/watson-textToSpeech/textToSpeech` converte texto em uma fala de áudio. Os parâmetros são os seguintes:

- `username`: o nome do usuário da API do Watson.
- `password`: a senha da API do Watson.
- `payload`: o texto para converter em fala.
- `voice`: a voz do orador.
- `accept`: o formato do arquivo de fala.
- `encoding`: a codificação dos dados binários de fala.


- Chame a ação `textToSpeech` em sua ligação de pacote para converter o texto.
  
  ```
  wsk action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
  ```
  {: pre}
  
  ```json
  {
    "payload": "<base64 encoding of a .wav file>"
  }
  ```
  
