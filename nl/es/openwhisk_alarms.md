---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: alarms, triggers, event, schedule, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilización de alarmas para planificar desencadenantes
{: #openwhisk_catalog_alarm}

El paquete `/whisk.system/alarms` se puede utilizar para activar un desencadenante con una frecuencia especificada. Las alarmas son útiles para configurar trabajos o tareas recurrentes, como invocar una acción de copia de seguridad del sistema cada hora.
{: shortdesc}

El paquete incluye el canal de información siguiente.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | paquete | - | Utilidad de alarmas y periodicidad. |
| `/whisk.system/alarms/once` | canal de información | date, trigger_payload, deleteAfterFire | Se activa un suceso desencadenante una vez en una fecha específica. |
| `/whisk.system/alarms/interval` | canal de información | minutes, trigger_payload, startDate, stopDate | Se activa un suceso desencadenante en una planificación basada en intervalos. |
| `/whisk.system/alarms/alarm` | canal de información | cron, timezone, trigger_payload, startDate, stopDate | Se activa un suceso desencadenante en una planificación basada en el tiempo mediante cron. |

## Activación de un suceso desencadenante una vez

El canal de información `/whisk.system/alarms/once` configura el servicio de alarma para activar un suceso desencadenante una única vez en una fecha especificada. Ejecute el siguiente mandato para crear una alarma de una única activación:
```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}

<table>
<caption>Visión general de los componentes del mandato <code>trigger create fireOnce</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de los componentes del mandato <code>trigger create fireOnce</code></th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>El tipo de desencadenante de alarma que está creando.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>La vía de acceso de los archivos del paquete de alarma para el canal de información fireOnce.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>Sustituya <code>&lt;date&gt;</code> con la fecha en la que se activará el desencadenante. El desencadenante se activa una sola vez en el instante especificado. Nota: El parámetro `date` admite un valor de serie o un número entero. El valor entero representa el número de milisegundos desde el 1 de enero de 1970 00:00:00 UTC y el valor de serie debe estar en <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Opcional: Sustituya <code>&lt;key&gt;</code> y <code>&lt;value&gt;</code> con los parámetros del desencadenante cuando este se active.</td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>Opcional: Indica si se debe suprimir el desencadenante y todas las reglas asociadas una vez este se ha activado. Sustituya <code>&lt;delete_option&gt;</code> con uno de los valores siguientes:<ul><li><code>false</code> (predeterminado): No se toma acción alguna después de que el desencadenante se haya activado.</li><li><code>true</code>: El desencadenante se suprimirá después de que se haya activado.</li><li><code>rules</code>: El desencadenante y todas sus reglas asociadas se suprimirán después de que se haya activado.</li></ul></td>
</tr>
</tbody></table>

A continuación se muestra un ejemplo de creación de un desencadenante que se activará una vez el 25 de diciembre de 2019, a las 12:30:00 UTC. Cada suceso desencadenante cuenta con los parámetros `name=Odin` y `place=Asgard`. Una vez que se haya activado el desencadenante, se suprimirá el desencadenante y todas las reglas asociadas.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}

## Activación periódica de un suceso desencadenante en una planificación basada en intervalos
{: #openwhisk_catalog_alarm_fire}

El canal de información `/whisk.system/alarms/interval` configura el servicio de alarma para activar un suceso desencadenante en una planificación basada en intervalos. Ejecute el siguiente mandato para crear una alarma que se basa en intervalos:
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption>Visión general de los componentes del mandato <code>trigger create interval</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de los componentes del mandato <code>trigger create interval</code></th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>El tipo de desencadenante de alarma que está creando.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>La vía de acceso de los archivos del paquete de alarma para el canal de información interval.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>Sustituya <code>&lt;minutes&gt;</code> con un número entero que representa la longitud del intervalo, en minutos, entre las activaciones del desencadenante.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Opcional: Sustituya <code>&lt;key&gt;</code> y <code>&lt;value&gt;</code> con los parámetros del desencadenante cuando este se active.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>Opcional: Sustituya <code>&lt;startDate&gt;</code> con la fecha en la que se activará el primer desencadenante. Las activaciones siguientes se producirán de acuerdo con la duración del intervalo especificada en el parámetro minutes. Nota: Este parámetro da soporte a un valor entero o de serie. El valor entero representa el número de milisegundos desde el 1 de enero de 1970 00:00:00 UTC y el valor de serie debe estar en <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>Opcional: Sustituya <code>&lt;stopDate&gt;</code> con la fecha en la que se detendrá la ejecución del desencadenante. Los desencadenantes ya no se activarán una vez se haya alcanzado esta fecha. Nota: Este parámetro da soporte a un valor entero o de serie. El valor entero representa el número de milisegundos desde el 1 de enero de 1970 00:00:00 UTC y el valor de serie debe estar en <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
</tbody></table>

El ejemplo siguiente crea un desencadenante que se activa una vez cada 2 minutos. El desencadenante se activa tan pronto como sea posible, y se dejará de activar el 31 de enero de 2019, a las 23:59:00 UTC. Cada suceso desencadenante cuenta con los parámetros `name=Odin` y `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}

## Activación de un desencadenante en una planificación basada en el tiempo mediante cron

El canal de información `/whisk.system/alarms/alarm` configura un servicio de alarma para activar un suceso desencadenante con una frecuencia especificada. Ejecute el siguiente mandato para crear una alarma que se basa en el tiempo:
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}

<table>
<caption>Visión general de los componentes del mandato <code>trigger create periodic</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icono de idea"/> Visión general de los componentes del mandato <code>trigger create periodic</code></th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>El tipo de desencadenante de alarma que está creando.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>La vía de acceso de los archivos del paquete de alarma para el canal de información periodic.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>Sustituya <code>&lt;cron&gt;</code> con una serie que indica cuándo se activará el desencadenante en Hora universal coordinada (UTC). La serie se basa en la <a href="http://crontab.org">sintaxis de crontab de UNIX</a> y es una secuencia con un máximo de 5 campos. Los campos se separan con espacios en el formato <code>X X X X X X</code>. Las series siguientes son ejemplos de distintas duraciones de frecuencia:<ul><li><code>\* \* \* \* \*</code>: El desencadenante se activa al principio de cada minuto.</li><li><code>0 \* \* \* \*</code>: El desencadenante se activa al principio de cada hora.</li><li><code>0 \*/2 \* \* \*</code>: El desencadenante se activa cada 2 horas (esto es, 02:00:00, 04:00:00, ...).</li><li><code>0 9 8 \* \*</code>: El desencadenante se activa a las 9:00:00 (UTC) en el octavo día de cada mes.</li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>Opcional: sustituya <code>&lt;timezone&gt;</code> por una serie que especifique el huso horario. La hora real a la que se activa el desencadenante se modificará según el huso horario especificado. Si el huso horario no es válido, se genera un error. Puede comprobar todos los husos horarios disponibles en el sitio web de Moment Timezone (http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names).</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>Opcional: Sustituya <code>&lt;key&gt;</code> y <code>&lt;value&gt;</code> con los parámetros del desencadenante cuando este se active.</td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>Opcional: Sustituya <code>&lt;startDate&gt;</code> con la fecha en la que se activará el primer desencadenante. Las activaciones siguientes se producirán de acuerdo con la duración del intervalo especificada en el parámetro minutes. Nota: Este parámetro da soporte a un valor entero o de serie. El valor entero representa el número de milisegundos desde el 1 de enero de 1970 00:00:00 UTC y el valor de serie debe estar en <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>Opcional: Sustituya <code>&lt;stopDate&gt;</code> con la fecha en la que se detendrá la ejecución del desencadenante. Los desencadenantes ya no se activarán una vez se haya alcanzado esta fecha. Nota: Este parámetro da soporte a un valor entero o de serie. El valor entero representa el número de milisegundos desde el 1 de enero de 1970 00:00:00 UTC y el valor de serie debe estar en <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">formato ISO 8601</a>.</td>
</tr>
</tbody></table>

El ejemplo siguiente crea un desencadenante que se activa una vez cada 2 minutos. El desencadenante no empezará a activarse hasta el 1 de enero de 2019, a las 00:00:00 UTC, y se dejará de activar el 31 de enero de 2019, a las 23:59:00 UTC. Cada suceso desencadenante cuenta con los parámetros `name=Odin` y `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}
