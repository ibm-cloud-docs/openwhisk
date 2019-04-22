---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: text to speech, watson, cognitive, converting, package

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
# Pacote Watson: Text to Speech
{: #openwhisk_catalog_watson_texttospeech}

Esse pacote pré-instalado não está disponível na região de Tóquio. Consulte o pacote instalável [Texto para Discurso](/docs/openwhisk?topic=cloud-functions-text-to-speech-package)usando a autenticação IAM. 
{: tip}

O pacote `/whisk.system/watson-textToSpeech` oferece uma maneira conveniente de chamar APIs do Watson para converter o texto em fala.
{: shortdesc}

O pacote inclui as ações a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/watson-textToSpeech` | pacote | username, password | Pacote para converter texto em fala |
| `/whisk.system/watson-textToSpeech/textToSpeech` | ação | payload, voice, accept, encoding, username, password | Converter texto em áudio |

**Nota**: o pacote `/whisk.system/watson` está descontinuado, incluindo a ação `/whisk.system/watson/textToSpeech`. Consulte o [pacote instalável do {{site.data.keyword.texttospeechshort}}](/docs/openwhisk?topic=cloud-functions-text-to-speech-package) em vez disso.

## Configurando o pacote Watson Text to Speech no {{site.data.keyword.Bluemix_notm}}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.Bluemix_notm}}, as ligações de pacote serão criadas automaticamente
para suas instâncias de serviço do {{site.data.keyword.Bluemix_notm}} Watson.

1. Crie uma instância de serviço de Texto para fala do Watson em seu painel do {{site.data.keyword.Bluemix_notm}}[](http://cloud.ibm.com).

  Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.Bluemix_notm}} no qual você se encontra.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Watson que você criou.
  ```
  Atualização do pacote ibmcloud fn
  ```
  {: pre}

  Exemplo de Saída:
  ```
  created bindings:
  Bluemix_Watson_TextToSpeech_Credentials-1
  ```
  {: screen}

  Liste os pacotes para ver se a ligação de pacote foi criada:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_TextToSpeec_Credentials-1 private
  ```
  {: screen}

## Configurando um pacote Watson Text to Speech fora do {{site.data.keyword.Bluemix_notm}}

Se você não está usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar o seu Watson Text to Speech fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para o seu serviço Watson Text to Speech. Você precisa do nome do usuário e da senha do serviço de Texto do Watson para Fala.

Crie uma ligação de pacote que esteja configurada para o seu serviço de Fala do Watson para Texto.
```
ibmcloud fn package bind /whisk.system/watson-textToSpeech myWatsonTextToSpeech -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Converter algum texto para fala

A ação `/whisk.system/watson-textToSpeech/textToSpeech` converte texto em um discurso de áudio. Os parâmetros são os seguintes:

- `username`: o nome do usuário da API do Watson.
- `password`: a senha da API do Watson.
- `payload`: o texto para converter em fala.
- `voice`: a voz do orador.
- `accept`: o formato do arquivo de fala.
- `encoding`: a codificação dos dados binários de fala.

Chame a ação **textToSpeech** em sua ligação do pacote para converter o texto.
```
ibmcloud fn action invoke myWatsonTextToSpeech/textToSpeech --blocking --result --param payload 'Hey.' --param voice 'en-US_MichaelVoice' --param accept 'audio/wav' --param encoding 'base64'
```
{: pre}

Exemplo de Saída:
```
{
  "payload": "<base64 encoding of a .wav file>"
  }
```
{: screen}
