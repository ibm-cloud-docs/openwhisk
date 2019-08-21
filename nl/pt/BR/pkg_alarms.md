---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: alarms, serverless, triggers, functions

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



# Alarmes
{: #pkg_alarms}

O pacote `/whisk.system/alarms` pode ser usado para disparar um
acionador em uma frequência especificada. Os alarmes são úteis para a configuração de tarefas ou tarefas recorrentes, como chamar um backup de sistema a cada hora.
{: shortdesc}

O pacote inclui os feeds a seguir.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Pacote | - | Alarmes e utilitário periódico. |
| `/whisk.system/alarms/once` | Feed | `date`, `trigger_payload`, `deleteAfterFire` | Disparar evento acionador uma vez em uma data específica. |
| `/whisk.system/alarms/interval` | Feed | `minutes`, `trigger_payload`, `startDate`, `stopDate` | Disparar evento acionador em um planejamento baseado em intervalo. |
| `/whisk.system/alarms/alarm` | Feed | `cron`, `timezone`, `trigger_payload`, `startDate`, `stopDate` | Disparar evento acionador em um planejamento baseado em tempo usando cron. |



## Disparando um evento acionador uma vez
{: #pkg_alarms_one}

O feed `/whisk.system/alarms/once` configura o serviço Alarme para disparar um
evento acionador uma vez em uma data especificada. Para criar um alarme de disparo único, execute o comando a seguir.

```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

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
<td>O caminho de arquivo do pacote de alarme para o feed fireOnce.</td>
</tr>
<tr>
<td><code> -- param date </code></td>
<td>Substitua <code>&lt;date&gt;</code> pela data em que você planeja disparar o acionador. O acionador é disparado uma vez no horário especificado. O parâmetro `date` suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde 1 `January 1970 00:00:00` UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
<tr>
<td><code> -- param trigger_payload </code></td>
<td>(Opcional) Substitua o <code>&lt;key&gt;</code> e o <code>&lt;value&gt;</code> pelos parâmetros do acionador quando o acionador for disparado.</td>
</tr>
<tr>
<td><code> -- param deleteAfterFire </code></td>
<td>(Opcional) Se o acionador e quaisquer regras associadas forem excluídos após o acionador ser disparado. Substitua <code>&lt;delete_option&gt;</code> por um dos seguintes.<ul><li><code>false</code> - (padrão) Nenhuma ação é executada depois que o acionador é disparado.</li><li><code>true</code> - O acionador é excluído após ser disparado.</li><li><code>rules</code> - O acionador e todas as suas regras associadas são excluídos após ele ser disparado.</li></ul></td>
</tr>
</tbody></table>

O comando a seguir é um exemplo da criação de um acionador que é disparado uma vez em 25 de dezembro de 2019, 12:30:00 UTC. Cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`. Depois que o acionador é disparado, o acionador e todas as regras associadas são excluídos.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}


## Disparando um evento acionador periodicamente em um planejamento baseado em intervalo
{: #pkg_alarms_int}

O feed `/whisk.system/alarms/interval` configura o serviço Alarme para disparar um
evento acionador em um planejamento baseado em intervalo. Para criar um alarme baseado em intervalo, execute o comando a seguir.
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

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
<td>Substitua <code>&lt;minutes&gt;</code> por um número inteiro que representa o comprimento do intervalo, em minutos, entre os disparos do acionador.</td>
</tr>
<tr>
<td><code> -- param trigger_payload </code></td>
<td>(Opcional) Substitua o <code>&lt;key&gt;</code> e o <code>&lt;value&gt;</code> pelos parâmetros do acionador quando o acionador for disparado.</td>
</tr>
<tr>
<td><code> -- param startDate </code></td>
<td>(Opcional) Substitua <code>&lt;startDate&gt;</code> pela data que você planeja que o primeiro acionador seja disparado. Os disparos subsequentes ocorrem com base no comprimento do intervalo que é especificado pelo parâmetro minutes. Esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde `1 January 1970 00:00:00` UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
<tr>
<td><code> -- param stopDate </code></td>
<td>(Opcional) Substitua <code>&lt;stopDate&gt;</code> pela data em que você planeja que o acionador seja interrompido. Os acionadores não são disparados quando essa data é atingida. Esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde `1 January 1970 00:00:00` UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
</tbody></table>

O exemplo a seguir cria um acionador que é disparado uma vez a cada 2 minutos. O acionador é disparado assim que possível e para o disparo `31 de janeiro, 2019, 23:59:00` UTC. Cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## Disparando um acionador em um planejamento baseado em tempo usando cron
{: #pkg_alarms_cron}

O feed `/whisk.system/alarms/alarm` configura o serviço de Alarme para disparar um evento acionador a uma frequência especificada. Para criar um alarme baseado em tempo, execute o comando a seguir.
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

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
<td>Substitua <code>&lt;cron&gt;</code> por uma sequência que indica quando disparar o acionador em Hora Universal Coordenada (UTC). A sequência é baseada na <a href="http://crontab.org">sintaxe de crontab UNIX</a> e é uma sequência de cinco campos no máximo. Os campos são separados por espaços no formato <code>X X X X X</code>. As sequências a seguir são exemplos que usam durações variadas de frequência.<ul><li><code>\* \* \* \* \*</code> - O acionador dispara no início de cada minuto.</li><li><code>0 \* \* \* \*</code> - O acionador dispara no início de cada hora.</li><li><code>0 \*/2 \* \* \*</code> - O acionador dispara a cada 2 horas (isto é, 02:00:00, 04:00:00...).</li><li><code>0 9 8 \* \*</code> - O acionador dispara às 9h (UTC) no oitavo dia de cada mês.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>(Opcional) Substitua <code>&lt;timezone&gt;</code> por uma sequência que especifica o fuso horário. O horário real para disparar o acionador é modificado em relação ao fuso horário especificado. Se o fuso horário for inválido, um erro será lançado. É possível verificar todos os fusos horários disponíveis no [website Moment Timezone](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}.</td>
</tr>
<tr>
<td><code> -- param trigger_payload </code></td>
<td>(Opcional) Substitua o <code>&lt;key&gt;</code> e o <code>&lt;value&gt;</code> pelos parâmetros do acionador quando o acionador for disparado.</td>
</tr>
<tr>
<td><code> -- param startDate </code></td>
<td>(Opcional) Substitua o <code>&lt;startDate&gt;</code> pela data em que você planeja disparar o primeiro acionador. Os disparos subsequentes ocorrem com base no comprimento do intervalo que é especificado pelo parâmetro minutes. Esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde `1 January 1970 00:00:00` UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
<tr>
<td><code> -- param stopDate </code></td>
<td>(Opcional) Substitua <code>&lt;stopDate&gt;</code> pela data em que você deseja que o acionador pare a execução. Os acionadores não são disparados quando essa data é atingida. Esse parâmetro suporta um valor de número inteiro ou de sequência. O valor de número inteiro representa o número de milissegundos desde `1 January 1970 00:00:00` UTC e o valor de sequência deve estar no formato <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">ISO 8601</a>.</td>
</tr>
</tbody></table>

O comando a seguir é um exemplo da criação de um acionador que é disparado uma vez a cada 2 minutos. O acionador não inicia o disparo até `January 1, 2019, 00:00:00` UTC e para o disparo em `January 31, 2019, 23:59:00` UTC. Cada evento acionador tem os parâmetros `name=Odin` e `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}




