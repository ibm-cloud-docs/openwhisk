---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Pacote Watson: Translator
{: #openwhisk_catalog_watson_translator}

O pacote `/whisk.system/watson-translator` oferece uma maneira conveniente de chamar APIs do Watson para traduzir.
{: shortdesc}

O pacote inclui as ações a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` | pacote | username, password | Pacote para tradução de texto e identificação de idioma  |
| `/whisk.system/watson-translator/translator` | ação | payload, translateFrom, translateTo, translateParam, username, password | Traduzir texto |
| `/whisk.system/watson-translator/languageId` | ação | payload, username, password | Identificar idioma |

**Nota**: o pacote `/whisk.system/watson` está descontinuado, incluindo as ações `/whisk.system/watson/translate` e `/whisk.system/watson/languageId`.

## Configurando o pacote do Tradutor do Watson no {{site.data.keyword.Bluemix_notm}}

Se você estiver usando o {{site.data.keyword.openwhisk}} por meio do {{site.data.keyword.Bluemix_notm}}, as ligações de pacote serão criadas automaticamente
para suas instâncias de serviço do {{site.data.keyword.Bluemix_notm}} Watson.

1. Crie uma instância de serviço Watson Translator em seu [painel](http://console.bluemix.net) do {{site.data.keyword.Bluemix_notm}}. Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.Bluemix_notm}} no qual você se encontra.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço do Watson que você criou.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Exemplo de Saída:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
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
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## Configurando um pacote do Tradutor do Watson fora do {{site.data.keyword.Bluemix_notm}}

Se você não está usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar o Watson Translator fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para o seu serviço Watson Translator. Você precisa do nome do usuário e da senha do serviço de Tradutor do Watson.

- Crie uma ligação de pacote que esteja configurada para o seu serviço de Tradutor do Watson.
  ```
  ibmcloud fn package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## Traduzindo texto

A ação `/whisk.system/watson-translator/translator` traduz o texto de um idioma para outro. Os parâmetros são os seguintes:

- `username`: o nome do usuário da API do Watson.
- `password`: a senha da API do Watson.
- `payload`: o texto a ser traduzido.
- `translateParam`: o parâmetro de entrada que indica o texto a ser traduzido. Por exemplo, se `translateParam=payload`, o valor do
parâmetro `payload` que é passado à ação será traduzido.
- `translateFrom`: um código de dois dígitos do idioma de origem.
- `translateTo`: um código de dois dígitos do idioma de destino.

Chame a ação **translator** em sua ligação de pacote para traduzir algum texto do inglês para o francês.
```
ibmcloud fn action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

Exemplo de Saída:
```
{
    "payload": "Ciel bleu a venir"
}
```
{: screen}

## Identificando o idioma de algum texto

A ação `/whisk.system/watson-translator/languageId` identifica o idioma de algum texto. Os parâmetros são os seguintes:

- `username`: o nome do usuário da API do Watson.
- `password`: a senha da API do Watson.
- `payload`: o texto para identificar.

Chame a ação **languageId** em sua ligação do pacote para identificar o idioma.
```
ibmcloud fn action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

Exemplo de Saída:
```
{
  "payload": "Ciel bleu a venir",
  "language": "fr",
  "confidence": 0.710906
}
```
{: screen}
