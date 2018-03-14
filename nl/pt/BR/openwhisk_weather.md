---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote Weather
{: #openwhisk_catalog_weather}

O pacote `/whisk.system/weather` oferece uma maneira conveniente de chamar o Weather Company Data para a API do {{site.data.keyword.Bluemix}}.
{: shortdesc}

O pacote inclui a ação a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/weather` | Pacote | username, password | Os serviços do Weather Company Data para a API do {{site.data.keyword.Bluemix_notm}}  |
| `/whisk.system/weather/forecast` | Ação | latitude, longitude, timePeriod | previsão para o período especificado|

É sugerido criar uma ligação de pacote com os valores `username`
e `password`. Dessa forma, você não precisa especificar as credenciais toda vez que chama as ações no pacote.

## Configurando o pacote Weather no {{site.data.keyword.Bluemix_notm}}

Se você estiver usando o OpenWhisk do {{site.data.keyword.Bluemix_notm}}, o OpenWhisk criará automaticamente as ligações de pacotes para suas instâncias de serviço do {{site.data.keyword.Bluemix_notm}} Weather.

1. Crie uma instância de serviço do Weather Company Data em seu [painel](http://console.ng.Bluemix.net) do {{site.data.keyword.Bluemix_notm}}.
  
  Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do {{site.data.keyword.Bluemix_notm}} no qual você se encontra.
  
2. Atualize os pacotes em seu namespace. A atualização cria automaticamente uma ligação de pacote para a instância de serviço Weather Company Data que você criou.
  
  ```
  wsk package refresh
  ```
  {: pre}
  ```
  Ligações criadas: Bluemix_Weather_Company_Data_Credentials-1
  ```
  ```
  wsk package list
  ```
  {: pre}
  ```
  Os pacotes /myBluemixOrg_myBluemixSpace / Clima Bluemix_Weather_Company_Data_Credentials-1 privado
  ```
  
 
## Configurando um pacote Weather fora do {{site.data.keyword.Bluemix_notm}}

Se você não está usando o OpenWhisk no {{site.data.keyword.Bluemix_notm}} ou se deseja configurar seu serviço Weather Company Data fora do {{site.data.keyword.Bluemix_notm}}, deve-se criar manualmente uma ligação de pacote para seu serviço Weather Company Data. São necessários o nome do usuário e a senha do serviço Weather Company Data.

- Crie uma ligação de pacote que esteja configurada para o seu serviço de Tradutor do Watson.

  ```
  wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}


## Obtendo uma previsão de tempo para um local
{: #openwhisk_catalog_weather_forecast}

A ação `/whisk.system/weather/forecast` retorna uma previsão de tempo para um local, chamando uma API da The Weather Company. Os parâmetros são os seguintes:

- `username`: o nome do usuário para The Weather Company Data for {{site.data.keyword.Bluemix_notm}} que está autorizado a chamar a API de previsão.
- `password`: a senha para The Weather Company Data for {{site.data.keyword.Bluemix_notm}} que está autorizada a chamar a API de previsão.
- `latitude`: a coordenada de latitude do local.
- `longitude`: a coordenadas de longitude do local.
- `timePeriod`: período para a previsão. Os valores
válidos são:
  - `10day` - (padrão) Retorna uma previsão diária de 10 dias
  - `48hour` - Retorna uma previsão de hora em hora de 2 dias
  - `current` - Retorna as condições metereológicas atuais
  - `timeseries` - Retorna as observações atuais e até 24 horas de observações passadas, a partir da data e hora atuais.


O exemplo a seguir mostra como criar uma ligação de pacote e, em seguida, obter uma previsão de 10 dias.

- Chame a ação `forecast` em sua ligação de pacote para obter a previsão de tempo.
  ```
  wsk action invoke myWeather/forecast --result \
  --param latitude 43.7 \
  --param longitude -79.4
  ```
  {: pre}
  
  ```json
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
  
