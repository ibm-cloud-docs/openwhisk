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

Le package `/whisk.system/alarms` peut être utilisé pour exécuter un déclencheur à une fréquence spécifiée. Les alarmes sont utiles pour configurer des tâches ou des travaux récurrents, comme l'appel d'une sauvegarde du système toutes les heures.
{: shortdesc}

Le package comprend les flux suivants :

| Entité | Type | Paramètres | Description |
| --- | --- | --- | --- |
| `/whisk.system/alarms` | Package | - | Alarmes et utilitaire périodique. |
| `/whisk.system/alarms/once` | Flux | `date`, `trigger_payload`, `deleteAfterFire` | Exécution d'un événement déclencheur une fois à une date spécifique. |
| `/whisk.system/alarms/interval` | Flux | `minutes`, `trigger_payload`, `startDate`, `stopDate` | Exécution d'un événement déclencheur selon une planification basée sur des intervalles. |
| `/whisk.system/alarms/alarm` | Flux | `cron`, `timezone`, `trigger_payload`, `startDate`, `stopDate` | Exécution d'un événement déclencheur selon une planification horaire à l'aide de cron. |



## Exécution ponctuelle d'un événement déclencheur
{: #pkg_alarms_one}

Le flux `/whisk.system/alarms/once` configure le service Alarm pour exécuter un événement déclencheur une fois à une date spécifiée. Pour créer une alarme à exécution unique, utilisez la commande suivante. 

```
ibmcloud fn trigger create fireOnce --feed /whisk.system/alarms/once --param date "<date>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param deleteAfterFire "<delete_option>"
```
{: pre}
</br>

<table>
<caption>Description des composants de la commande <code>trigger create fireOnce</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de la commande <code>trigger create fireOnce</code></th>
</thead>
<tbody>
<tr>
<td><code>fireOnce</code></td>
<td>Type de déclencheur d'alarme que vous créez.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/once</code></td>
<td>Chemin du fichier de package d'alarme pour le flux fireOnce.</td>
</tr>
<tr>
<td><code>--param date</code></td>
<td>Remplacez <code>&lt;date&gt;</code> par la date à laquelle vous prévoyez d'exécuter le déclencheur. Le déclencheur s'exécute une seule fois à l'heure spécifiée. Le paramètre `date` prend en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC (`1 January 1970 00:00:00`) et la valeur de chaîne doit être spécifiée au <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">format ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Facultatif) Remplacez <code>&lt;key&gt;</code> et <code>&lt;value&gt;</code> par les paramètres du déclencheur lorsque celui-ci est exécuté. </td>
</tr>
<tr>
<td><code>--param deleteAfterFire</code></td>
<td>(Facultatif) Indique si le déclencheur et toute règle associée sont supprimés une fois le déclencheur exécuté. Remplacez <code>&lt;delete_option&gt;</code> par l'une des valeurs suivantes. <ul><li><code>false</code> -(valeur par défaut) Aucune action n'est prise après l'exécution du déclencheur.</li><li><code>true</code> - Le déclencheur est supprimé après son exécution. </li><li><code>rules</code> - Le déclencheur et toutes les règles associées sont supprimés après l'exécution du déclencheur.</li></ul></td>
</tr>
</tbody></table>

La commande suivante est un exemple de création d'un déclencheur exécuté une seule fois le 25 décembre 2019 à 12:30:00 UTC. Chaque événement déclencheur possède les paramètres `name=Odin` et `place=Asgard`. Une fois le déclencheur exécuté, ce dernier ainsi que toutes les règles associées sont supprimés.

```
ibmcloud fn trigger create fireOnce \
  --feed /whisk.system/alarms/once \
  --param date "2019-12-25T12:30:00.000Z" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param deleteAfterFire "rules"
```
{: pre}


## Exécution régulière d'un événement déclencheur selon une planification basée sur des intervalles
{: #pkg_alarms_int}

Le flux `/whisk.system/alarms/interval` configure le service Alarm pour exécuter un événement déclencheur selon une planification basée sur des intervalles. Pour créer une alarme basée sur des intervalles, utilisez la commande suivante.
```
ibmcloud fn trigger create interval --feed /whisk.system/alarms/interval --param minutes "<minutes>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>Description des composants de la commande <code>trigger create interval</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de la commande <code>trigger create interval</code></th>
</thead>
<tbody>
<tr>
<td><code>interval</code></td>
<td>Type de déclencheur d'alarme que vous créez.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/interval</code></td>
<td>Chemin du fichier de package d'alarme pour le flux d'intervalle.</td>
</tr>
<tr>
<td><code>--param minutes</code></td>
<td>Remplacez <code>&lt;minutes&gt;</code> par un entier représentant la longueur de l'intervalle, en minutes, entre les exécutions de déclencheur.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Facultatif) Remplacez <code>&lt;key&gt;</code> et <code>&lt;value&gt;</code> par les paramètres du déclencheur lorsque celui-ci est exécuté. </td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(Facultatif) Remplacez <code>&lt;startDate&gt;</code> par la date à laquelle vous prévoyez l'exécution du premier déclencheur. Les exécutions suivantes se produisent en fonction de la longueur d'intervalle spécifiée par le paramètre minutes. Ce paramètre prend en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC (`1 January 1970 00:00:00`) et la valeur de chaîne doit être spécifiée au <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">format ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(Facultatif) Remplacez <code>&lt;stopDate&gt;</code> par la date à laquelle vous prévoyez que le déclencheur s'arrête. Les déclencheurs ne se déclenchent plus une fois cette date atteinte. Ce paramètre prend en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC (`1 January 1970 00:00:00`) et la valeur de chaîne doit être spécifiée au <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">format ISO 8601</a>.</td>
</tr>
</tbody></table>

L'exemple ci-dessous crée un déclencheur qui est exécuté toutes les 2 minutes. Le déclencheur s'exécute dès que possible et cessera de s'exécuter le 31 janvier 2019, à 23:59:00 UTC (`January 31, 2019, 23:59:00`). Chaque événement déclencheur possède les paramètres `name=Odin` et `place=Asgard`.

```
ibmcloud fn trigger create interval \
  --feed /whisk.system/alarms/interval \
  --param minutes 2 \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}



## Exécution d'un déclencheur selon une planification horaire à l'aide de cron
{: #pkg_alarms_cron}

Le flux `/whisk.system/alarms/alarm` configure le service Alarm pour exécuter un événement déclencheur à une fréquence spécifiée. Pour créer une alarme basée sur une heure, exécutez la commande suivante. 
```
ibmcloud fn trigger create periodic --feed /whisk.system/alarms/alarm --param cron "<cron>" --param trigger_payload "{<key>:<value>,<key>:<value>}" --param startDate "<start_date>" --param stopDate "<stop_date>"
```
{: pre}
</br>

<table>
<caption>Description des composants de la commande <code>trigger create periodic</code></caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composants de la commande <code>trigger create periodic</code></th>
</thead>
<tbody>
<tr>
<td><code>periodic</code></td>
<td>Type de déclencheur d'alarme que vous créez.</td>
</tr>
<tr>
<td><code>--feed /whisk.system/alarms/alarm</code></td>
<td>Chemin du fichier de package d'alarme pour le flux d'alarme périodique.</td>
</tr>
<tr>
<td><code>--param cron</code></td>
<td>Remplacez <code>&lt;cron&gt;</code> par une chaîne indiquant l'heure d'exécution du déclencheur en Temps Universel Coordonné (UTC). La chaîne repose sur la <a href="http://crontab.org">syntaxe UNIX crontab</a> et figure dans une séquence comportant cinq zones au maximum. Les zones sont séparées par des espaces au format <code>X X X X X</code>. Les chaînes suivantes sont des exemples qui utilisent différentes durées fréquence. <ul><li><code>\* \* \* \* \*</code> - Le déclencheur s'exécute au début de chaque minute. </li><li><code>\* \* \* \* \*</code> - Le déclencheur s'exécute au début de chaque heure. </li><li><code>0 \*/2 \* \* \*</code> - Le déclencheur s'exécute toutes les 2 heures (02:00:00, 04:00:00, et ainsi de suite).</li><li><code>0 9 8 \* \*</code> - Le déclencheur s'exécute à 9:00 (UTC) le huitième jour de chaque mois. </li></ul></td>
</tr>
<tr>
<tr>
<td><code>--param timezone</code></td>
<td>(Facultatif) Remplacez <code>&lt;timezone&gt;</code> par une chaîne spécifiant le fuseau horaire. L'heure réelle d'exécution du déclencheur est modifiée en fonction du fuseau horaire indiqué. Si le fuseau horaire n'est pas valide, une erreur est générée. Vous pouvez consulter tous les fuseaux horaires disponibles sur le [site Web Moment Timezone](http://momentjs.com/timezone/docs/#/data-loading/getting-zone-names){: external}.</td>
</tr>
<tr>
<td><code>--param trigger_payload</code></td>
<td>(Facultatif) Remplacez <code>&lt;key&gt;</code> et <code>&lt;value&gt;</code> par les paramètres du déclencheur lorsque celui-ci est exécuté. </td>
</tr>
<tr>
<td><code>--param startDate</code></td>
<td>(Facultatif) Remplacez <code>&lt;startDate&gt;</code> par la date à laquelle vous prévoyez l'exécution du premier déclencheur. Les exécutions suivantes se produisent en fonction de la longueur d'intervalle spécifiée par le paramètre minutes. Ce paramètre prend en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC (`1 January 1970 00:00:00`) et la valeur de chaîne doit être spécifiée au <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">format ISO 8601</a>.</td>
</tr>
<tr>
<td><code>--param stopDate</code></td>
<td>(Facultatif) Remplacez <code>&lt;stopDate&gt;</code> par la date à laquelle vous souhaitez que le déclencheur cesse de s'exécutzer. Les déclencheurs ne se déclenchent plus une fois cette date atteinte. Ce paramètre prend en charge une valeur de type entier ou chaîne. La valeur entière représente le nombre de millisecondes depuis le 1er janvier 1970 00:00:00 UTC (`1 January 1970 00:00:00`) et la valeur de chaîne doit être spécifiée au <a href="http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15">format ISO 8601</a>.</td>
</tr>
</tbody></table>

La commande ci-dessous crée un déclencheur qui s'exécute toutes les 2 minutes. L'exécution du déclencheur ne démarre pas avant le
1er janvier 2019 (`January 1, 2019, 00:00:00`) UTC et cesse au 31 janvier 2019 (`January 31, 2019, 23:59:00`) UTC. Chaque événement déclencheur possède les paramètres `name=Odin` et `place=Asgard`.

```
ibmcloud fn trigger create periodic \
  --feed /whisk.system/alarms/alarm \
  --param cron "*/2 * * * *" \
  --param trigger_payload "{\"name\":\"Odin\",\"place\":\"Asgard\"}" \
  --param startDate "2019-01-01T00:00:00.000Z" \
  --param stopDate "2019-01-31T23:59:00.000Z"
```
{: pre}




