---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-01"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilisation du package Alarm
{: #openwhisk_catalog_alarm}

Le package `/whisk.system/alarms` peut être utilisé pour exécuter un déclencheur à une fréquence spécifiée. Les alarmes sont utiles pour configurer des tâches ou des travaux récurrents, comme l'appel d'une action de sauvegarde du système toutes les heures.
{: shortdesc}

Le package comprend notamment les flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | package | - | Alarmes et utilitaire périodique. |
| `/whisk.system/alarms/interval` | flux | minutes, trigger_payload, startDate, stopDate | Exécuter un événement déclencheur selon une planification basée sur des intervalles. |
| `/whisk.system/alarms/once` | flux | date, trigger_payload | Exécuter un événement déclencheur une fois à une date spécifique. |
| `/whisk.system/alarms/alarm` | flux | cron, trigger_payload, startDate, stopDate | Exécuter un événement déclencheur selon une planification horaire à l'aide de cron. |


## Exécution régulière d'un événement déclencheur selon une planification basée sur des intervalles
{: #openwhisk_catalog_alarm_fire}

Le flux `/whisk.system/alarms/interval` configure le service Alarm pour exécuter un événement déclencheur selon une planification basée sur des intervalles. Les paramètres sont les suivants :

- `minutes` : entier représentant la longueur de l'intervalle (en minutes) entre les exécutions de déclencheur. 

- `trigger_payload` : la valeur de ce paramètre devient le contenu du déclencheur à chaque fois que celui-ci est exécuté. 

- `startDate` : date d'exécution du premier déclencheur. Les exécutions suivantes se produiront en fonction de la longueur d'intervalle spécifiée par le paramètre `minutes`.    

- `stopDate` : date de fin d'exécution du déclencheur. Les déclencheurs ne se déclencheront plus une fois cette date atteinte. 

  **Remarque** : les paramètres `startDate` et `stopDate` prennent en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC et la valeur de chaîne doit être spécifiée au format ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

L'exemple ci-dessous crée un déclencheur qui est exécuté toutes les 2 minutes. Le déclencheur s'exécute dès que possible et cessera de s'exécuter le 31 janvier 2019, à 23:59:00 UTC.

  ```
  wsk trigger create interval \
    --feed /whisk.system/alarms/interval \
    --param minutes 2 \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

Chaque événement généré inclut des paramètres, qui sont les propriétés spécifiées par la valeur `trigger_payload`. Dans ce cas, chaque événement déclencheur possède les paramètres `name=Odin` et `place=Asgard`.

## Exécution ponctuelle d'un événement déclencheur  

Le flux `/whisk.system/alarms/once` configure le service Alarm pour exécuter un événement déclencheur à une date spécifiée. Les paramètres sont les suivants :

- `date` : date d'exécution du déclencheur. Le déclencheur sera exécuté une seule fois au moment spécifié.  

  **Remarque** : le paramètre `date` prend en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC et la valeur de chaîne doit être spécifiée au format ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

- `trigger_payload` : la valeur de ce paramètre devient le contenu du déclencheur lorsque celui-ci est exécuté.  

Voici un exemple de création d'un déclencheur qui sera exécuté une seule fois le 25 décembre 2017, à 12:30:00 UTC. 

  ```
  wsk trigger create fireOnce \
    --feed /whisk.system/alarms/once \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param date "2017-12-25T12:30:00.000Z"
  ```
  {: pre}
    
## Exécution d'un déclencheur selon une planification horaire à l'aide de cron

Le flux `/whisk.system/alarms/alarm` configure le service Alarm pour exécuter un événement déclencheur à une fréquence spécifiée. Les paramètres sont les suivants :

- `cron` : chaîne basée sur la syntaxe crontab UNIX indiquant quand exécuter le déclencheur (en temps universel coordonné). Il s'agit d'une séquence de cinq zones séparées par un espace : `X X X X X`.
Pour plus d'informations, voir http://crontab.org. Les chaînes suivantes sont des exemples qui utilisent des fréquences différentes. 

  - `* * * * *` : le déclencheur s'exécute au début de chaque minute.
  - `0 * * * *` : le déclencheur s'exécute au début de chaque heure. 
  - `0 */2 * * *` : le déclencheur s'exécute toutes les 2 heures (c'est-à-dire 02:00:00, 04:00:00, ...).
  - `0 9 8 * *` : le déclencheur s'exécute à 9:00:00 du matin (UTC) le huitième jour de chaque mois.

  **Remarque** : le paramètre `cron` ne prend en charge que 5 zones. 
    
- `trigger_payload` : la valeur de ce paramètre devient le contenu du déclencheur à chaque fois que celui-ci est exécuté. 

- `startDate` : date de début d'exécution du déclencheur. Le déclencheur s'exécute en fonction de la planification spécifiée par le paramètre cron.   

- `stopDate` : date de fin d'exécution du déclencheur. Les déclencheurs ne s'exécutent plus une fois cette date atteinte.

  **Remarque** : les paramètres `startDate` et `stopDate` prennent en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC, et la valeur de chaîne doit être spécifiée au format ISO 8601 (http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

Voici un exemple de création d'un déclencheur qui s'exécute toutes les 2 minutes avec les valeurs `name` et `place` dans l'événement déclencheur. L'exécution du déclencheur ne démarrera pas avant le 1er janvier 2019, à 00:00:00 UTC et cessera le 31 janvier 2019, à 23:59:00 UTC.

  ```
  wsk trigger create periodic \
    --feed /whisk.system/alarms/alarm \
    --param cron "*/2 * * * *" \
    --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
    --param startDate "2019-01-01T00:00:00.000Z" \
    --param stopDate "2019-01-31T23:59:00.000Z"
  ```
  {: pre}

 **Remarque** : le paramètre `maxTriggers` est obsolète sera bientôt retiré. Pour arrêter le déclencheur, utilisez le paramètre `stopDate`. 
