---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Usando alarmes para planejar acionadores
{: #openwhisk_catalog_alarm}

O pacote `/whisk.system/alarms` pode ser usado para disparar um
acionador em uma frequência especificada. Os alarmes são úteis para configurar tarefas ou trabalhos recorrentes, como chamar uma ação de backup do sistema a cada hora.
{: shortdesc}

O pacote inclui os feeds a seguir.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | pacote | - | Alarmes e utilitário periódico. |
| `/whisk.system/alarms/once` | alimentação | date, trigger_payload, deleteAfterFire | Disparar evento acionador uma vez em uma data específica. |
| `/whisk.system/alarms/interval` | alimentação | minutes, trigger_payload, startDate, stopDate |
Disparar evento acionador em um planejamento baseado em intervalo. |
| `/whisk.system/alarms/alarm` | alimentação | cron, trigger_payload, startDate, stopDate | Disparar evento acionador em um planejamento baseado em tempo usando cron. |

## Disparando um evento acionador uma vez

O feed `/whisk.system/alarms/once` configura o serviço Alarme para disparar um
evento acionador uma vez em uma data especificada. Para criar um alarme de disparo único, execute o comando a
seguir:
```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}

<table>
<caption>Entendendo os componentes do comando <code>trigger create fireOnce</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os componentes do comando
<code>trigger create fireOnce</code></th>
</thead>
<tbody>
<tr>
<td><code> fireOnce </code></td>
<td>O tipo de acionador de alarme que você está criando.</td>
</tr>
<tr>
<td><code> -- feed /whisk.system/alarms/once </code></td>
<td>O caminho de arquivo de pacote de alarme para o feed fireOnce.</td>
</tr>
<tr>
<td><code> -- param date </code></td>
<td>Substitua <code>&lt;date&gt;</code> pela data em que o acionador será disparado. O acionador é disparado
apenas uma vez no horário especificado. Nota: o parâmetro `date` suporta um valor de
número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00h00min00s UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
<tr>
<td><code> -- param trigger_payload </code></td>
<td>Opcional: Substitua <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> pelos parâmetros do acionador quando o acionador for disparado.</td>
</tr>
<tr>
<td><code> -- param deleteAfterFire </code></td>
<td>Opcional: se o acionador e quaisquer regras associadas serão excluídos após o acionador ser disparado. 
Substitua <code>&lt;delete_option&gt;</code> por um dos seguintes:<ul><li><code>false</code> (padrão):
nenhuma ação é executada depois que o acionador é disparado.</li><li><code>true</code>: o acionador é
excluído depois que ele é disparado.</li><li><code>rules</code>: o acionador e todas as suas regras associadas
são excluídos depois que ele é disparado.</li></ul></td>
</tr>
</tbody></table>

A seguir está um exemplo de criação de um acionador que será disparado uma vez em 25 de dezembro de 2019, 12h30 UTC. Cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`. 
Após o acionador ser disparado, o acionador e todas as regras associadas serão excluídos.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}

## Disparando um evento acionador periodicamente em um planejamento baseado em intervalo
{: #openwhisk_catalog_alarm_fire}

O feed `/whisk.system/alarms/interval` configura o serviço Alarme para disparar um
evento acionador em um planejamento baseado em intervalo. Para criar um alarme baseado em intervalo, execute o
comando a seguir:
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption>Entendendo os componentes do comando <code>trigger create interval</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os componentes do comando
<code>trigger create interval</code></th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>O tipo de acionador de alarme que você está criando.</td>
</tr>
<tr>
<td><code> -- feed /whisk.system/alarms/interval </code></td>
<td>O caminho de arquivo do pacote de alarme para o feed de intervalo.</td>
</tr>
<tr>
<td><code> -- param minutes </code></td>
<td>Substitua <code>&lt;minutes&gt;</code> por um número inteiro que representa a duração do
intervalo, em minutos, entre os disparos do acionador.</td>
</tr>
<tr>
<td><code> -- param trigger_payload </code></td>
<td>Opcional: Substitua <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> pelos parâmetros do acionador quando o acionador for disparado.</td>
</tr>
<tr>
<td><code> -- param startDate </code></td>
<td>Opcional: Substitua <code>&lt;startDate&gt;</code> pela data em que o primeiro acionador será disparado. Os disparos subsequentes ocorrem com base no comprimento do intervalo especificado pelo parâmetro de minutos. Nota: esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00h00min00s UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
<tr>
<td><code> -- param stopDate </code></td>
<td>Opcional: Substitua <code>&lt;stopDate&gt;</code> pela data em que o acionador parará de executar. Os acionadores não serão disparados assim que essa data tiver sido atingida. Nota: esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00h00min00s UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
</tbody></table>

O exemplo a seguir cria um acionador que é disparado uma vez a cada 2 minutos. O acionador é disparado assim que possível e irá parar o disparo em 31 de janeiro de 2019, 23h59 UTC. Cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}

## Disparando um acionador em um planejamento baseado em tempo usando cron

O feed `/whisk.system/alarms/alarm` configura o serviço de Alarme para disparar um evento acionador a uma frequência especificada. 
Para criar um alarme baseado em tempo, execute o comando a seguir:
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption>Entendendo os componentes do comando <code>trigger create periodic</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os componentes do comando
<code>trigger create periodic</code></th>
</thead>
<tbody>
<tr>
<td><code> periódico </code></td>
<td>O tipo de acionador de alarme que você está criando.</td>
</tr>
<tr>
<td><code> -- feed /whisk.system/alarms/alarm </code></td>
<td>O caminho de arquivo do pacote de alarme para o feed de alarme periódico.</td>
</tr>
<tr>
<td><code> -- param cron </code></td>
<td>Substitua <code>&lt;cron&gt;</code> por uma sequência que indica quando o acionador deverá ser disparado
na Hora Universal Coordenada (UTC). A sequência é baseada na <a href="http://crontab.org">sintaxe de crontab
do UNIX</a> e é uma sequência que tem no máximo 5 campos. Esses campos são separados por espaços no formato
<code>X X X X X</code>. As sequências a seguir são exemplos que usam durações variadas de frequência:<ul><li>
<code>\* \* \* \* \*</code>: o acionador é disparado no início de cada minuto.</li><li><code>0 \* \* \* \*</code>: o
acionador é disparado no início de cada hora.</li><li><code>0 \*/2 \* \* \*</code>: o acionador é
disparado a cada 2 horas (ou seja, 02:00:00, 04:00:00,...).</li><li><code> 0 9 8 \* \*</code>: o acionador
é disparado às 9h (UTC) no oitavo dia de cada mês.</li></ul></td>
</tr>
<tr>
<td><code> -- param trigger_payload </code></td>
<td>Opcional: Substitua <code>&lt;key&gt;</code> e <code>&lt;value&gt;</code> pelos parâmetros do acionador quando o acionador for disparado.</td>
</tr>
<tr>
<td><code> -- param startDate </code></td>
<td>Opcional: Substitua <code>&lt;startDate&gt;</code> pela data em que o primeiro acionador será disparado. Os disparos subsequentes ocorrem com base no comprimento do intervalo especificado pelo parâmetro de minutos. Nota: esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00h00min00s UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
<tr>
<td><code> -- param stopDate </code></td>
<td>Opcional: Substitua <code>&lt;stopDate&gt;</code> pela data em que o acionador parará de executar. Os acionadores não serão disparados assim que essa data tiver sido atingida. Nota: esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde 1 de janeiro de 1970 00h00min00s UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
</tbody></table>

A seguir está um exemplo da criação de um acionador que é disparado uma vez a cada 2 minutos. O acionador não iniciará o disparo até
1º de janeiro de 2019, 0h UTC e irá parar o disparo em 31 de janeiro de 2019, 23h59 UTC. Cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}
