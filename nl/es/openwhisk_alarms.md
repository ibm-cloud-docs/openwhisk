---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Uso del paquete Alarm
{: #openwhisk_catalog_alarm}

El paquete `/whisk.system/alarms` se puede utilizar para activar un desencadenante con una frecuencia especificada. Las alarmas son útiles para configurar trabajos o tareas recurrentes, como invocar una acción de copia de seguridad del sistema cada hora.
{: shortdesc}

El paquete incluye la información de entrada siguiente.

| Entidad | Tipo | Parámetros | Descripción |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Paquete | - | Utilidad de alarmas y periodicidad. |
| `/whisk.system/alarms/interval` | canal de información | minutes, trigger_payload, startDate, stopDate | Se activa un suceso desencadenante en una planificación basada en un intervalo. |
| `/whisk.system/alarms/once` | canal de información | date, trigger_payload | Se activa un suceso desencadenante una vez en una fecha específica. |
| `/whisk.system/alarms/alarm` | canal de información | cron, trigger_payload, startDate, stopDate | Se activa un suceso desencadenante en una planificación basada en el tiempo mediante cron. |


## Activación periódica de un suceso desencadenante en una planificación basada en un intervalo
{: #openwhisk_catalog_alarm_fire}

El canal de información `/whisk.system/alarms/interval` configura el servicio de alarma para activar un suceso desencadenante en una planificación basada en un intervalo. Los parámetros son según se indica a continuación:

- `minutes`: un número entero que representa la longitud del intervalo (en minutos) entre las activaciones del desencadenante.

- `trigger_payload`: el valor de este parámetro pasa a ser el contenido del desencadenante cada vez que se activa el desencadenante.

- `startDate`: fecha en la que se activará el primer desencadenante.  Las activaciones siguientes se producirán de acuerdo con la duración del intervalo especificada en el parámetro `minutes`.   

- `stopDate`: fecha en la que el desencadenante dejará de ejecutarse.  Los desencadenantes ya no se activarán una vez que se alcance esta fecha.

  **Nota**: los parámetros `startDate` y `stopDate` admiten un valor de serie o número entero.  El valor entero representa el número de milisegundos desde el 1 de enero de 1970 a las 00:00:00 UTC, y el valor de serie debe estar en formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

El ejemplo siguiente crea un desencadenante que se activará una vez cada 2 minutos. El desencadenante se activa tan pronto como sea posible, y se dejará de activar el 31 de enero de 2019, a las 23:59:00 UTC.

  ```
  wsk trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

Cada suceso generado incluye parámetros, que son las propiedades que se especifican mediante el valor `trigger_payload`. En este caso, cada suceso desencadenante cuenta con los parámetros `name=Odin` y `place=Asgard`.

## Activación de un suceso desencadenante una vez  

El canal de información `/whisk.system/alarms/once` configura el servicio de alarma para activar un suceso desencadenante en una fecha especificada. Los parámetros son según se indica a continuación:

- `date`: fecha en la que se activará el desencadenante.  El desencadenante se activará sólo una vez a la hora indicada. 

  **Nota**: el parámetro `date` admite un valor de serie o un número entero.  El valor entero representa el número de milisegundos desde el 1 de enero de 1970 a las 00:00:00 UTC, y el valor de serie debe estar en formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

- `trigger_payload`: el valor de este parámetro pasa a ser el contenido del desencadenante cuando se activa el desencadenante. 

A continuación se muestra un ejemplo de creación de un desencadenante que se activará una vez el 25 de diciembre de 2017, a las 12:30:00 UTC.

  ```
  wsk trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2017-12-25T12:30:00.000Z"
  ```
  {: pre}
    
## Activación de un desencadenante en una planificación basada en el tiempo mediante cron

El canal de información `/whisk.system/alarms/alarm` configura el servicio de alarma para activar un suceso desencadenante con una frecuencia especifica. Los parámetros son según se indica a continuación:

- `cron`: una serie, basada en la sintaxis crontab de UNIX, que indica cuándo activar el desencadenante en Hora Universal Coordinada (UTC). La serie es una secuencia de cinco campos separados por espacios: `X X X X X`.
Para obtener más información, consulte: http://crontab.org. Las series siguientes son ejemplos que utilizan frecuencias de diferente duración.

  - `* * * * *`: el desencadenante se activa al principio de cada minuto.
  - `0 * * * *`: el desencadenante se activa al principio de cada hora.
  - `0 */2 * * *`: el desencadenante se activa cada 2 horas (es decir, a las 02:00:00, a las 04:00:00...).
  - `0 9 8 * *`: el desencadenante se activa a las 9:00:00 (UTC) el octavo día de cada mes.

  **Nota**: el parámetro `cron` sólo admite 5 campos.
    
- `trigger_payload`: el valor de este parámetro pasa a ser el contenido del desencadenante cada vez que se activa el desencadenante.

- `startDate`: fecha en la que el desencadenante comenzará a ejecutarse. El desencadenante se activa de acuerdo con la planificación especificada con el parámetro cron.  

- `stopDate`: fecha en la que el desencadenante dejará de ejecutarse. Los desencadenantes no se activan una vez que se alcanza esta fecha.

  **Nota**: los parámetros `startDate` y `stopDate` admiten un valor de serie o número entero.  El valor entero representa el número de milisegundos desde el 1 de enero de 1970 00:00:00 UTC, y el valor de serie debe estar en el formato ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

A continuación se muestra un ejemplo de creación de un desencadenante que se activa una vez cada 2 minutos con los valores `name` y `place` en el suceso desencadenante.  El desencadenante no empezará a activarse hasta el 1 de enero de 2019, a las 00:00:00 UTC, y se dejará de activar el 31 de enero de 2019, a las 23:59:00 UTC.

  ```
  wsk trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **Nota**: el parámetro `maxTriggers` está en desuso y se eliminará próximamente.  Para detener el desencadenante, utilice el parámetro `stopDate`.
