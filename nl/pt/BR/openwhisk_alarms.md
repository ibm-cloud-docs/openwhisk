---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando o pacote Alarm
{: #openwhisk_catalog_alarm}

O pacote `/whisk.system/alarms` pode ser usado para disparar um
acionador em uma frequência especificada. Os alarmes são úteis para configurar tarefas ou trabalhos recorrentes, como chamar uma ação de backup do sistema a cada hora.
{: shortdesc}

O pacote inclui os feeds a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | pacote | - | Alarmes e utilitário periódico. |
| `/whisk.system/alarms/interval` | alimentação | minutes, trigger_payload, startDate, stopDate | Disparar evento acionador em um planejamento baseado em intervalo. |
| `/whisk.system/alarms/once` | alimentação | date, trigger_payload | Disparar evento acionador uma vez em uma data específica. |
| `/whisk.system/alarms/alarm` | alimentação | cron, trigger_payload, startDate, stopDate | Disparar evento acionador em um planejamento baseado em tempo usando cron. |


## Disparando um evento acionador periodicamente em um planejamento baseado em intervalo
{: #openwhisk_catalog_alarm_fire}

O feed `/whisk.system/alarms/interval` configura o serviço de Alarme para disparar um evento acionador em um planejamento baseado em intervalo. Os parâmetros são os seguintes:

- `minutes`: um número inteiro representando o comprimento do intervalo (em minutos) entre disparos do acionador.

- `trigger_payload`: o valor desse parâmetro torna-se o conteúdo do Acionador toda vez que o Acionador é disparado.

- `startDate`: a data em que o primeiro acionador será disparado. Os disparos subsequentes ocorrerão com base no comprimento de intervalo especificado pelo parâmetro `minutes`.   

- `stopDate`: a data em que o acionador irá parar a execução. Os acionadores não serão mais disparados assim que essa data tiver sido atingida.

  **Nota**: os parâmetros `startDate` e `stopDate` suportam um número inteiro ou valor de sequência.  O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00:00:00 UTC e o valor de sequência deve estar no formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

O exemplo a seguir cria um acionador que é disparado uma vez a cada 2 minutos. O acionador é disparado assim que possível e irá parar o disparo em 31 de janeiro de 2019, 23h59 UTC.

  ```
  wsk trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

Cada evento gerado inclui parâmetros, que são as propriedades especificadas pelo valor `trigger_payload`. Nesse caso, cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`.

## Disparando um evento acionador uma vez  

O feed `/whisk.system/alarms/once` configura o serviço de Alarme para disparar um evento acionador em uma data especificada. Os parâmetros são os seguintes:

- `date`: a data em que o acionador será disparado. O acionador será disparado apenas uma vez no horário especificado. 

  **Nota**: o parâmetro `date` suporta um valor de número inteiro ou sequência. O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00:00:00 UTC e o valor de sequência deve estar no formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

- `trigger_payload`: o valor desse parâmetro torna-se o conteúdo do acionador quando o acionador é disparado. 

A seguir está um exemplo de criação de um acionador que será disparado uma vez em 25 de dezembro de 2017, 12h30 UTC.

  ```
  wsk trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2017-12-25T12:30:00.000Z"
  ```
  {: pre}
    
## Disparando um acionador em um planejamento baseado em tempo usando cron

O feed `/whisk.system/alarms/alarm` configura o serviço de Alarme para disparar um evento acionador em uma frequência especificada. Os parâmetros são os seguintes:

- `cron`: uma sequência, baseada na sintaxe crontab do UNIX, que indica quando disparar o acionador na Hora Universal Coordenada (UTC). A sequência é composta por cinco campos separados por espaços: `X X X X X`.
Para obter mais informações, veja: http://crontab.org. As sequências a seguir são exemplos que usam durações variáveis de frequência.

  - `* * * * *`: o acionador é disparado no topo de cada minuto.
  - `0 * * * *`: o acionador é disparado no topo de cada hora.
  - `0 */ 2 * * *`: o acionador é disparado a cada 2 horas (ou seja, 2h, 4h,...).
  - `0 9 8 * *`: o acionador é disparado às 9h (UTC) no oitavo dia de cada mês.

  **Nota**: o parâmetro `cron` suporta somente 5 campos.
    
- `trigger_payload`: o valor desse parâmetro torna-se o conteúdo do Acionador toda vez que o Acionador é disparado.

- `startDate`: a data em que o acionador iniciará a execução. O acionador é disparado com base no planejamento especificado pelo parâmetro cron.  

- `stopDate`: a data em que o acionador irá parar a execução. Os acionadores não são mais disparados assim que essa data é atingida.

  **Nota**: os parâmetros `startDate` e `stopDate` suportam um número inteiro ou valor de sequência.  O valor de número inteiro representa o número de milissegundos desde 1º de janeiro de 1970 0h UTC e o valor de sequência deve estar no formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

A seguir está um exemplo de criação de um acionador que é disparado uma vez a cada 2 minutos com os valores `name` e `place` no evento acionador. O acionador não iniciará o disparo até
1º de janeiro de 2019, 0h UTC e irá parar o disparo em 31 de janeiro de 2019, 23h59 UTC.

  ```
  wsk trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **Nota**: o parâmetro `maxTriggers` foi descontinuado e será removido em breve. Para parar o acionador, use o parâmetro `stopDate`.
