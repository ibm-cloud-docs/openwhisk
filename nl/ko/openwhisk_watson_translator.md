---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Watson: Translator 패키지
{: #openwhisk_catalog_watson_translator}

`/whisk.system/watson-translator` 패키지는 Watson API를 호출하여 변환하는 편리한 방법을 제공합니다.
{: shortdesc}

패키지에는 다음 액션이 포함됩니다.

|엔티티 |유형 |매개변수 |설명 |
| --- | --- | --- | --- |
| `/whisk.system/watson-translator` |패키지 |username, password |텍스트 변환 및 언어 식별을 위한 패키지  |
|`/whisk.system/watson-translator/translator` |액션 |payload, translateFrom, translateTo, translateParam, username, password |텍스트 변환 |
|`/whisk.system/watson-translator/languageId` |액션 |payload, username, password |언어 식별 |

**참고**: `/whisk.system/watson` 패키지는 `/whisk.system/watson/translate` 및 `/whisk.system/watson/languageId` 액션을 포함하여 더 이상 사용되지 않습니다.

## {{site.data.keyword.Bluemix_notm}}에서 Watson Translator 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk}}를 사용 중인 경우 패키지 바인딩이 {{site.data.keyword.Bluemix_notm}} Watson 서비스 인스턴스에 대해 자동으로 작성됩니다.

1. {{site.data.keyword.Bluemix_notm}} [대시보드](http://console.bluemix.net)에서 Watson Translator 서비스 인스턴스를 작성하십시오. 자신이 속한 {{site.data.keyword.Bluemix_notm}} 조직과 영역 및 서비스 인스턴스의 이름을 반드시 기억하십시오.

2. 네임스페이스의 패키지를 새로 고치십시오. 새로 고치기를 수행하면 작성된 Watson 서비스 인스턴스에 대한 패키지 바인딩이 자동으로 작성됩니다.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  출력 예:
  ```
  created bindings:
  Bluemix_Watson_Translator_Credentials-1
  ```
  {: screen}

  패키지를 나열하여 패키지 바인딩이 작성되었는지 확인하십시오.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  출력 예:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Watson_Translator_Credentials-1 private
  ```
  {: screen}

## {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Translator 패키지 설정

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.openwhisk_short}}를 사용하지 않거나 {{site.data.keyword.Bluemix_notm}} 외부에서 Watson Translator를 설정하려는 경우 Watson Translator 서비스에 대한 패키지 바인딩을 수동으로 작성해야 합니다. Watson Translator 서비스 사용자 이름 및 비밀번호가 반드시 필요합니다.

- Watson Translator 서비스에 대해 구성된 패키지 바인딩을 작성하십시오.
  ```
  ibmcloud fn package bind /whisk.system/watson-translator myWatsonTranslator -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}

## 텍스트 변환

`/whisk.system/watson-translator/translator` 액션은 텍스트를 한 언어에서 다른 언어로 변환합니다. 매개변수는 다음과 같습니다.

- `username`: Watson API 사용자 이름입니다.
- `password`: Watson API 비밀번호입니다.
- `payload`: 변환할 텍스트입니다.
- `translateParam`: 변환할 텍스트를 표시하는 입력 매개변수입니다. 예를 들어, `translateParam=payload`인 경우 액션에 전달된 `payload` 매개변수의 값이 변환됩니다.
- `translateFrom`: 소스 언어의 두 자리 코드입니다.
- `translateTo`: 대상 언어의 두 자리 코드입니다.

패키지 바인딩의 **translator** 액션을 호출하여 일부 텍스트를 영어에서 프랑스어로 변환하십시오.
```
ibmcloud fn action invoke myWatsonTranslator/translator \
--blocking --result \
--param payload "Blue skies ahead" --param translateFrom "en" \
--param translateTo "fr"
```
{: pre}

출력 예:
```
{
    "payload": "Ciel bleu a venir"
  }
```
{: screen}

## 일부 텍스트의 언어 식별

`/whisk.system/watson-translator/languageId` 액션은 일부 텍스트의 언어를 식별합니다. 매개변수는 다음과 같습니다.

- `username`: Watson API 사용자 이름입니다.
- `password`: Watson API 비밀번호입니다.
- `payload`: 식별할 텍스트입니다.

패키지 바인딩의 **languageId** 액션을 호출하여 언어를 식별하십시오.
```
ibmcloud fn action invoke myWatsonTranslator/languageId \
--blocking --result \
--param payload "Ciel bleu a venir"
```
{: pre}

출력 예:
```
{
  "payload": "Ciel bleu a venir",
    "language": "fr",
    "confidence": 0.710906
  }
```
{: screen}
