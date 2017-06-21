---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote Clima
{: #openwhisk_catalog_weather}

O pacote `/whisk.system/weather` oferece uma maneira conveniente
de chamar a API do Weather Company Data for IBM Bluemix.

O pacote inclui a ação a seguir.

| Entidade | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/weather` | pacote | username, password | Serviços da API do Weather Company Data for IBM Bluemix  |
| `/whisk.system/weather/forecast` | ação | latitude, longitude, timePeriod | previsão para o período especificado|

É sugerido criar uma ligação de pacote com os valores `username`
e `password`. Dessa forma, não será necessário especificar as
credenciais toda vez que chamar as ações no pacote.

## Configurando o pacote Weather no Bluemix

Se você estiver usando o OpenWhisk do Bluemix, o OpenWhisk criará automaticamente as ligações de pacote para suas instâncias de serviço Bluemix Weather.

1. Crie uma instância de serviço Weather Company Data em seu [painel](http://console.ng.Bluemix.net) do Bluemix.
  
  Certifique-se de lembrar do nome da instância de serviço e da organização e do espaço do Bluemix nos quais você está.
  
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
  
 
## Configurando um pacote Weather fora do Bluemix

Se você não estiver usando o OpenWhisk no Bluemix ou se desejar configurar seu serviço Weather Company Data fora do Bluemix, deve-se criar manualmente uma ligação de pacote para seu serviço Weather Company Data. São necessários o nome do usuário e a senha do serviço Weather Company Data.

- Crie uma ligação de pacote que esteja configurada para o seu serviço de Tradutor do Watson.

  ```
  wsk package bind /whisk.system/weather myWeather -p username MYUSERNAME -p password MYPASSWORD
  ```
  {: pre}


## Obtendo uma previsão de tempo para um local
{: #openwhisk_catalog_weather_forecast}

A ação `/whisk.system/weather/forecast` retorna uma previsão do tempo para um local, chamando uma API a partir da The Weather Company. Os parâmetros são como segue:

- `username`: nome do usuário do The Weather Company Data for IBM Bluemix que está autorizado a chamar a API de previsão.
- `password`: senha para o The Weather Company Data for IBM Bluemix que está autorizado a chamar a API de previsão.
- `latitude`: a coordenada de latitude do local.
- `longitude`: a coordenadas de longitude do local.
- `timePeriod`: período para a previsão. As opções válidas são:
  - `10day` - (padrão) Retorna uma previsão diária de 10 dias
  - `48hour` - Retorna uma previsão de hora em hora de 2 dias
  - `current` - Retorna as condições metereológicas atuais
  - `timeseries` - Retorna as observações atuais e até 24 horas de observações passadas, a partir da data e hora atuais.


- A seguir está um exemplo de criação de uma ligação de pacote e, depois, obtenção de previsão de 10 dias. 1.Crie uma ligação de pacote com sua chave da API.
  
  ```
  wsk package bind /whisk.system/weather myWeather --param username MY_USERNAME --param password MY_PASSWORD
  ```
  {: pre}

- Chame a ação `forecast` em sua ligação do pacote para obter a previsão do tempo.
  
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
  
