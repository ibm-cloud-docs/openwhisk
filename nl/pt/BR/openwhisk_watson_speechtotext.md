---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: watson, speech to text, package, converting, cognitive

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Pacote Watson: Speech to Text
{: #openwhisk_catalog_watson_speechtotext}

Esse pacote pré-instalado não está disponível na região de Tóquio. Consulte o pacote [Fala para texto](/docs/openwhisk?topic=cloud-functions-speech-to-text-package) instalável usando a autenticação do IAM.
{: tip}

O pacote `/whisk.system/watson-speechToText` oferece uma maneira conveniente de chamar APIs do Watson para converter a fala em texto.
{: shortdesc}

O pacote inclui as ações a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/watson-speechToText` | pacote | username, password | Pacote para converter fala em texto |
| `/whisk.system/watson-speechToText/speechToText` | ação | payload, content_type, encoding, username, password, continuous, inactivity_timeout, interim_results, keywords, keywords_threshold, max_alternatives, model, timestamps, watson-token, word_alternatives_threshold, word_confidence, X-Watson-Learning-Opt-Out | Converter
áudio em texto |

**Nota**: o pacote `/whisk.system/watson` está descontinuado, incluindo a ação `/whisk.system/watson/speechToText`. Em vez disso, consulte o [pacote do {{site.data.keyword.speechtotextshort}}](/docs/openwhisk?topic=cloud-functions-speech-to-text-package).

## Configurando o pacote Watson Speech to Text no {{site.data.keyword.Bluemix_notm}}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.Bluemix_notm}}, as ligações de pacote serão criadas automaticamente
para suas instâncias de serviço do {{site.data.keyword.Bluemix_notm}} Watson.

1. Cie uma instância de serviço Fala para texto do Watson em seu painel do {{site.data.keyword.Bluemix_notm}} [](http://cloud.ibm.com).

  Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.Bluemix_notm}} no qual você se encontra.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Watson que você criou.
  ```
  Atualização do pacote ibmcloud fn
  ```
  {: pre}

  Exemplo de Saída:
  ```
  created bindings:
  Bluemix_Watson_SpeechToText_Credentials-1
  ```
  {: screen}

  Liste os pacotes para ver se a ligação foi criada:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_SpeechToText_Credentials-1 private
  ```
  {: screen}

## Configurando um pacote Watson Speech to Text fora do {{site.data.keyword.Bluemix_notm}}

Se você não está usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar o Watson Speech to Text fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para o serviço Watson Speech to Text. Você precisa do nome do usuário e da senha do serviço de Fala do Watson para Texto.

Crie uma ligação de pacote que esteja configurada para o seu serviço de Fala do Watson para Texto.
```
ibmcloud fn package bind /whisk.system/watson-speechToText myWatsonSpeechToText -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Convertendo fala para texto

A ação `/whisk.system/watson-speechToText/speechToText` converte fala de áudio em texto. Os parâmetros são os seguintes:

- `username`: o nome do usuário da API do Watson.
- `password`: a senha da API do Watson.
- `payload`: os dados binários de fala codificados para transformar em texto.
- `content_type`: o tipo MIME do áudio.
- `encoding`: a codificação dos dados binários de fala.
- `continuous`: indica se múltiplos resultados finais que representam frases consecutivas separadas por pausas longas são retornados.
- `inactivity_timeout`: o tempo em segundos após o qual, se apenas o silêncio for detectado no áudio enviado, a conexão será fechada.
- `interim_results`: indica se o serviço deve retornar resultados provisórios.
- `keywords`: uma lista de palavras-chave para detectar no áudio.
- `keywords_threshold`: um valor de confiança que é o limite inferior para detectar uma palavra-chave.
- `max_alternatives`: o número máximo de transcrições alternativas a serem retornadas.
- `model`: o identificador do modelo a ser usado para a solicitação de reconhecimento.
- `timestamps`: indica se o alinhamento de horário é retornado para cada palavra.
- `watson-token`: fornece um token de autenticação para o serviço como uma alternativa para fornecer credenciais de serviço.
- `word_alternatives_threshold`: um valor de confiança que é o limite inferior para identificar uma hipótese como uma alternativa de palavra possível.
- `word_confidence`: indica se uma medida de confiança no intervalo de 0 a 1 deve ser retornada para cada palavra.
- `X-Watson-Learning-Opt-Out`: indica se deve-se fazer opt-out de coleta de dados para a chamada.

Chame a ação **speechToText** em sua ligação do pacote para converter o áudio codificado.
```
ibmcloud fn action invoke myWatsonSpeechToText/speechToText --blocking --result --param payload <base64 encoding of a .wav file> --param content_type 'audio/wav' --param encoding 'base64'
```
{: pre}

Exemplo de Saída:
```
{
  "data": "Hello Watson"
}
```
{: screen}
