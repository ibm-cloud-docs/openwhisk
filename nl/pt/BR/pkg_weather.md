---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: weather package, forecast, functions, serverless

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


# Weather
{: #pkg_weather}

O pacote `/whisk.system/weather` pré-instalado oferece uma maneira conveniente de chamar o Weather Company Data para a API do {{site.data.keyword.cloud}}.
{: shortdesc}

O pacote inclui a ação a seguir.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Pacote | `username`, `password` | Serviços do Weather Company Data para a API do {{site.data.keyword.cloud_notm}}. |
| `/whisk.system/weather/forecast` | Ação | `latitude`, `longitude`, `timePeriod` | Previsão para o período de tempo especificado. |

É sugerido criar uma ligação de pacote com os valores `username`
e `password`. Dessa forma, não será necessário especificar as
credenciais toda vez que chamar as ações no pacote.

## Configurando o pacote Weather no {{site.data.keyword.cloud_notm}}

Se você estiver usando o {{site.data.keyword.openwhisk}} do {{site.data.keyword.cloud_notm}}, as ligações de pacote serão criadas automaticamente para as instâncias de serviço {{site.data.keyword.cloud_notm}} Weather.

1. Crie uma instância de serviço do Weather Company Data em seu {{site.data.keyword.cloud_notm}}painel do [](https://cloud.ibm.com){: external}.

  Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.cloud_notm}} no qual você se encontra.

2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço Weather Company Data que você criou.
  ```
  Atualização do pacote ibmcloud fn
  ```
  {: pre}

  **Saída de exemplo**
  ```
  created bindings:
  Weather_Company_Data_Credentials-1
  ```
  {: screen}

  Liste os pacotes para ver se a ligação de pacote foi criada:
  ```
  ibmcloud fn package list
  ```
  {: pre}

  **Saída de exemplo**
  ```
  packages
  /myOrg_mySpace/Weather Weather_Company_Data_Credentials-1 private
  ```
  {: screen}

## Configurando um pacote Weather fora do {{site.data.keyword.cloud_notm}}

Se você não estiver usando o {{site.data.keyword.openwhisk_short}} no {{site.data.keyword.cloud_notm}} ou se desejar configurar o serviço Weather Company Data fora do {{site.data.keyword.cloud_notm}}, deverá criar manualmente uma ligação de pacote para o serviço Weather Company Data. São necessários o nome do usuário e a senha do serviço Weather Company Data.

Crie uma ligação de pacote que esteja configurada para seu serviço Weather.
```
ibmcloud fn package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
```
{: pre}

## Obtendo uma previsão de tempo para um local
{: #pkg_weather_forecast}

A ação `/whisk.system/weather/forecast` retorna uma previsão do tempo para um local, chamando uma API a partir da The Weather Company. Os parâmetros são os seguintes.

| Parâmetro | Descrição |
| --- | --- |
| `username` | Nome de usuário para The Weather Company Data for {{site.data.keyword.cloud_notm}}, que está autorizado a chamar a API de previsão. |
| `password` | Senha para o Weather Company Data for {{site.data.keyword.cloud_notm}} que está autorizado a chamar a API de previsão. |
| `latitude` | A coordenada de latitude da localização. |
| `longitude` | A coordenada de longitude da localização. |
| `timePeriod` | O período de tempo para a previsão. |
</br>

A tabela a seguir contém os valores que podem ser usados com o parâmetro `--timePeriod`.
| Valores `--timePeriod` | Descrição |
| --- | --- |
| `10day` | (padrão) Retorna uma previsão diária de 10 dias. |
| `48hour` | Retorna uma previsão de 2 dias por hora. |
| `current` | Retorna as condições climáticas atuais. |
| `timeseries` | Retorna as observações atuais e até 24 horas de observações anteriores, a partir da data e hora atuais. |


**Exemplo** O exemplo a seguir mostra como criar uma ligação de pacote e, em seguida, obter uma previsão de 10 dias.

Chame a ação `forecast` em sua ligação do pacote para obter a previsão do tempo.
```
ibmcloud fn action invoke myWeather/forecast --result --param latitude 43.7 --param longitude -79.4
```
{: pre}

**Saída**
```
{
    "forecasts": [
          {
            "dow": "Wednesday",
              "max_temp": -1,
              "min_temp": -16,
              "narrative": "Chance of a few snow showers. Highs -2 to 0C and lows -17 to -15C.",
            ...
        },
        {
            "class": "fod_long_range_daily",
              "dow": "Thursday",
              "max_temp": -4,
              "min_temp": -8,
              "narrative": "Mostly sunny. Highs -5 to -3C and lows -9 to -7C.",
            ...
        },
        ...
    ],
}
```
{: screen}


