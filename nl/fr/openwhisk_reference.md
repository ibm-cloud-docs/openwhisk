---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: limits, details, entities, packages, runtimes, semantics, ordering actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:deprecated: .deprecated}

# Détails et limites du système
{: #openwhisk_reference}

Les sections suivantes fournissent des détails techniques sur les paramètres de limite et les paramètres système {{site.data.keyword.openwhisk}}.
{: shortdesc}

## Entités {{site.data.keyword.openwhisk_short}}
{: #openwhisk_entities}

### Espaces de nom et packages
{: #openwhisk_entities_namespaces}

Les actions, les déclencheurs et les règles {{site.data.keyword.openwhisk_short}} appartiennent à un espace de nom et parfois à un package. 

Un package peut contenir des actions et des flux. Un package ne peut pas contenir un autre package ; par conséquent, l'imbrication de packages n'est pas autorisée. De plus, les entités ne doivent pas obligatoirement se trouver dans un package.

Dans {{site.data.keyword.Bluemix_notm}}, une paire organisation+espace correspond à un espace de nom {{site.data.keyword.openwhisk_short}}. Par exemple, l'organisation `BobsOrg` et l'espace `dev` correspondraient à l'espace de nom {{site.data.keyword.openwhisk_short}} `/BobsOrg_dev`.



Vous pouvez créer de nouveaux espaces de nom Cloud Foundry en [créant des organisations et des espaces Cloud Foundry](/docs/openwhisk?topic=cloud-functions-cloudfunctions_cli#region_info). L'espace de nom `/whisk.system` est réservé aux entités qui sont distribuées avec le système {{site.data.keyword.openwhisk_short}}.


### Noms qualifiés complets
{: #openwhisk_entities_fullyqual}

Le nom qualifié complet d'une entité est `/nom_espace_nom/[nom_package]/nom_entité`. Notez que `/` est utilisé pour délimiter les espaces de nom, les packages et les entités. De plus, les espaces de nom doivent être précédés du préfixe `/`. 

Pour des raisons pratiques, l'espace de nom peut être omis s'il s'agit de l'espace de nom par défaut de l'utilisateur. Par exemple, imaginez un utilisateur dont l'espace de nom par défaut est `/myOrg`. Voici des exemples de nom qualifié complet pour plusieurs entités et leurs alias :



| Nom qualifié complet | Alias | Espace de nom | Package | Nom |
| --- | --- | --- | --- | --- |
| `/whisk.system/cloudant/read` |  | `/whisk.system` | `cloudant` | `read` |
| `/myOrg/video/transcode` | `video/transcode` | `/myOrg` | `video` | `transcode` |
| `/myOrg/filter` | `filter` | `/myOrg` |  | `filter` |

Vous pouvez utiliser ce schéma de dénomination dans l'interface de ligne de commande {{site.data.keyword.openwhisk_short}}, entre autres.

### Noms d'entité
{: #openwhisk_entities_names}

Les noms de toutes les entités, notamment les actions, les déclencheurs, les règles, les packages et les espaces de nom, forment une séquence de caractères au format suivant : 

* Le premier caractère doit être alphanumérique ou un trait de soulignement.
* Les caractères qui suivent doivent être des caractères alphanumériques, des espaces ou l'un des caractères suivants : `_`, `@`, `.`, `-`.
* Le dernier caractère ne peut pas être un espace.

Plus précisément, un nom doit se conformer à l'expression régulière suivante (exprimée d'après la syntaxe de métacaractères Java) : `\A([\w]|[\w][\w@ .-]*[\w@.-]+)\z`.

## Sémantique d'action
{: #openwhisk_semantics}

Les sections ci-après présentent les actions {{site.data.keyword.openwhisk_short}} en détail.

### Caractéristique "sans état"
{: #openwhisk_semantics_stateless}

Les implémentations d'action sont sans état, ou *idempotentes*. Si le système n'applique pas cette propriété, il n'est pas garanti qu'un état conservé par une action soit disponible pour tous les appels.

De plus, plusieurs instanciations d'une action peuvent exister, chacune possédant son propre état. Un appel d'action peut être envoyé dans n'importe laquelle de ces instanciations.

### Entrée et sortie d'appel
{: #openwhisk_semantics_invocationio}

L'entrée et la sortie d'une action sont un dictionnaire de paires clé-valeur. La clé est une chaîne et la valeur est une valeur JSON valide.

### Ordre des appels d'action
{: #openwhisk_ordering}

Les appels d'une action ne sont pas ordonnés. Si l'utilisateur appelle une action deux fois depuis la ligne de commande ou l'API REST, il est possible que le deuxième appel soit exécuté avant le premier. Si les actions ont des effets secondaires, ceux-ci peuvent être observés dans n'importe quel ordre.

En outre, l'exécution automatique des actions n'est pas garantie. Deux actions peuvent s'exécuter simultanément et leurs effets secondaires peuvent s'imbriquer. OpenWhisk ne garantit aucun modèle de cohérence particulier quant aux effets secondaires. Les effets secondaires liés à la simultanéité dépendent de l'implémentation.

### Garanties de l'exécution des actions
{: #openwhisk_atmostonce}

Lorsqu'une demande d'appel est reçue, le système enregistre la demande et attribue une activation.

Le système renvoie un ID d'activation (avec un appel non bloquant) qui confirme que l'appel a été reçu.
Si une défaillance du réseau, ou toute autre panne, survient avant que vous ne receviez une réponse HTTP, il est tout de même possible que {{site.data.keyword.openwhisk_short}} ait reçu et traité la demande.

Le système tente d'appeler l'action une fois, ce qui génère l'un des quatre résultats suivants :
- *success* : l'appel de l'action a abouti.
- *application error* : l'appel de l'action a abouti, mais l'action a volontairement renvoyé une valeur d'erreur, par exemple lorsqu'une précondition relative aux arguments n'a pas été satisfaite.
- *action developer error* : l'action a été appelée mais ne s'est pas terminée normalement ; par exemple, si l'action n'a pas détecté une exception ou en cas d'erreur de syntaxe.
- *whisk internal error* : le système n'est pas parvenu à appeler l'action.
Le résultat est enregistré dans la zone `status` de l'enregistrement d'activation, tel que documenté dans une section ultérieure.

Il existe un enregistrement d'activation pour chaque appel reçu et pour lequel l'utilisateur peut être facturé.

Dans le cas d'une erreur de type *action developer error*, il se peut que l'action ait été exécutée partiellement et qu'elle ait généré des effets secondaires externes visibles. Il revient à l'utilisateur de vérifier si de tels effets secondaires ont été générés et d'émettre une logique de relance s'il le souhaite. Certaines erreurs de type *whisk internal errors* indiquent que l'exécution d'une action a bien commencé mais qu'elle a échoué avant la fin.

## Enregistrement d'activation
{: #openwhisk_ref_activation}

Chaque appel d'action et chaque exécution de déclencheur génère un enregistrement d'activation.

Un enregistrement d'activation contient les zones suivantes :

- *activationId* : l'ID d'activation.
- *start* et *end* : les horodatages enregistrant le début et la fin de l'activation. Les valeurs sont au [format de temps UNIX](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_15).
- *namespace* et `name` : espace de nom et nom de l'entité.
- *logs* : tableau de chaînes indiquant les journaux qui sont générés par l'action au cours de son activation. Chaque élément de tableau correspond à une ligne générée dans `stdout` ou `stderr` par l'action et inclut l'horodatage et le flux de la sortie journal. La structure est la suivante : `HORODATAGE FLUX : JOURNAL_SORTIE`.
- *response* : dictionnaire qui définit les clés `success`, `status` et `result` :
  - *status* : résultat de l'activation, qui peut être "success", "application error", "action developer error", "whisk internal error".
  - *success* : `true` si et seulement si le statut est `"success"`.
- *result* : dictionnaire contenant le résultat de l'activation. Si l'activation a abouti, le résultat contient la valeur qui est renvoyée par l'action. Si l'activation a échoué, `result` contient la clé `error`, généralement accompagnée d'une explication de l'échec.

## API REST
{: #openwhisk_ref_restapi}

Vous trouverez des informations sur l'API REST {{site.data.keyword.openwhisk_short}} dans le [document de référence sur l'API REST](https://cloud.ibm.com/apidocs/functions).

## Limites du système
{: #openwhisk_syslimits}

### Actions
{{site.data.keyword.openwhisk_short}} présente quelques limites relatives au système, notamment la quantité de mémoire qu'une action peut utiliser et le nombre d'appels d'action autorisés par minute.

Le tableau ci-dessous répertorie les limites par défaut pour les actions.

| Limite | Description | Par défaut | Min | Max |
| ----- | ----------- | :-------: | :---: | :---: |
| [codeSize](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_codesize) | Taille maximale (en Mo) du code d'action. | 48 | 1 | 48 |
| [concurrent](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_concurrent) | N activations au maximum peuvent être soumises par espace de nom, que ce soit pour être exécutées ou être placées en file d'attente pour exécution. | 1000 | 1 | 1000* |
| [logs](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_logs) | Un conteneur ne peut pas écrire plus de N Mo de données dans la sortie standard. | 10 | 0 | 10 |
| [memory](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_memory) | Un conteneur ne peut pas allouer plus de N Mo de mémoire. | 256 | 128 | 2048 |
| [minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_minuterate) | N activations au maximum peuvent être soumises par minute par espace de nom. | 5000 | 1 | 5000* |
| [openulimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_openulimit) | Nombre maximal de fichiers ouverts pour une action. | 1024 | 0 | 1024 |
| [parameters](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_parameters) | Taille maximale (en Mo) des paramètres qui peuvent être joints. | 5 | 0 | 5 |
| [proculimit](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_proculimit) | Nombre maximal de processus disponibles pour une action. | 1024 | 0 | 1024 |
| [result](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_result) | Taille maximale (en Mo) du résultat d'un appel de l'action. | 5 | 0 | 5 |
| [sequenceMaxActions](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_sequencemax) | Nombre maximal d'actions comprenant une séquence donnée. | 50 | 0 | 50* |
| [timeout](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_timeout) | La durée d'exécution maximale d'un conteneur est limitée à N millisecondes. | 60000 | 100 | 600000 |

### Augmentation des limites fixes
{: #increase_fixed_limit}

Les valeurs limite qui se terminent par un astérisque (*) sont fixes, mais il est possible de les augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Si vous souhaitez augmenter la valeur limite, contactez le support IBM en ouvrant directement un ticket à partir de la [console Web {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk). 
  1. Sélectionnez **Support**
  2. Sélectionnez **Ajouter un ticket** dans le menu déroulant.
  3. Sélectionnez **Technique** pour le type de ticket.
  4. Sélectionnez **Fonctions** pour le domaine technique de support.

#### codeSize (Mo) (Fixe : 48 Mo)
{: #openwhisk_syslimits_codesize}
* La taille de code maximale pour l'action est 48 Mo.
* Pour les actions JavaScript, utilisez un outil permettant de concaténer tout le code source, y compris les dépendances, dans un fichier groupé unique. 
* Cette limite est fixe et ne peut pas être modifiée.

#### concurrent (Fixe : 1000*)
{: #openwhisk_syslimits_concurrent}
* Le nombre d'activations qui sont exécutées ou mises en file d'attente pour exécution pour un espace de nom ne peut pas être supérieur à 1 000. 
* Cette valeur limite est fixe, mais il est possible de l'augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Pour obtenir des instructions détaillées relatives à l'augmentation de cette limite, voir [Augmentation des limites fixes](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit).

#### logs (Mo) (Valeur par défaut : 10 Mo)
{: #openwhisk_syslimits_logs}
* La limite de journal N est comprise dans la plage [0 Mo - 10 Mo] et est définie par action.
* Un utilisateur peut modifier la limite de journal des actions lorsqu'une action est créée ou mise à jour.
* Les journaux qui dépassent la limite définie sont tronqués, par conséquent, toute nouvelle entrée de journal est ignorée, et un avertissement est ajouté comme dernière sortie de l'activation pour indiquer que celle-ci a dépassé la limite de journal définie.

#### memory (Mo) (Valeur par défaut : 256 Mo)
{: #openwhisk_syslimits_memory}
* La mémoire limite M est comprise dans la plage [128 Mo - 2048 Mo] et est définie par action en mégaoctets.
* Un utilisateur peut modifier la limite de mémoire lorsqu'une action est créée.
* Un conteneur ne peut pas utiliser plus de mémoire que celle qui est allouée par la limite.

#### minuteRate (Fixe : 5000*)
{: #openwhisk_syslimits_minuterate}
* La limite de débit N est défini à 5000 et limite le nombre d'appels d'action dans des fenêtres d'une minute.
* Un appel d'interface de ligne de commande ou API dépassant cette limite reçoit un code d'erreur correspondant au code de statut HTTP `429: TOO MANY REQUESTS`.
* Cette valeur limite est fixe, mais il est possible de l'augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Pour obtenir des instructions détaillées relatives à l'augmentation de cette limite, voir [Augmentation des limites fixes](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_limit).

#### openulimit (Fixe : 1024:1024)
{: #openwhisk_syslimits_openulimit}
* Le nombre maximal de fichiers ouverts pour une action est 1024 (pour les limites absolues et les limites souples).
* Cette limite est fixe et ne peut pas être modifiée.
* Lorsqu'une action est appelée, la commande docker run utilise l'argument `--ulimit nofile=1024:1024` pour définir la valeur de `openulimit`.
* Pour plus d'informations, voir la documentation de référence relative à la ligne de commande [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### parameters (Fixe : 5 Mo)
{: #openwhisk_syslimits_parameters}
* La taille limite pour le nombre total de paramètres lors de la création ou de la mise à jour d'une action, d'un package ou d'un déclencheur est 5 Mo.
* Une entité comportant des paramètres dont la taille est trop élevée est rejetée lorsqu'elle fait l'objet d'une tentative de création ou de mise à jour.
* Cette limite est fixe et ne peut pas être modifiée.

#### proculimit (Fixe : 1024:1024)
{: #openwhisk_syslimits_proculimit}
* Le nombre maximal de processus disponibles pour le conteneur d'action est 1024.
* Cette limite est fixe et ne peut pas être modifiée.
* Lorsqu'une action est appelée, la commande docker run utilise l'argument `--pids-limit 1024` pour définir la valeur de `proculimit`.
* Pour plus d'informations, voir la documentation de référence relative à la ligne de commande [docker run](https://docs.docker.com/engine/reference/commandline/run).

#### result (Fixe : 5 Mo)
{: #openwhisk_syslimits_result}
* Taille de sortie maximale du résultat de l'appel d'une action en Mo.
* Cette limite est fixe et ne peut pas être modifiée.

#### sequenceMaxActions (Fixe : 50*)
{: #openwhisk_syslimits_sequencemax}
* Nombre maximal d'actions comprenant une séquence donnée.
* Cette limite est fixe et ne peut pas être modifiée.

#### timeout (ms) (Valeur par défaut : 60s)
{: #openwhisk_syslimits_timeout}
* La limite de délai d'exécution N est comprise dans la plage [100 ms - 600000 ms] et est définie par action en millisecondes.
* Un utilisateur peut modifier la limite de délai d'exécution lorsqu'une action est créée.
* Un conteneur dont l'exécution dure plus longtemps que N millisecondes est arrêté.

### Déclencheurs

Les déclencheurs sont soumis à un débit de déclenchement par minute, comme indiqué dans le tableau ci-dessous.

| Limite | Description | Par défaut | Min | Max |
| ----- | ----------- | :-------: | :---: | :---: |
| [minuteRate](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#openwhisk_syslimits_tminuterate) | N déclencheurs au maximum peuvent être lancés par minute par espace de nom. | 5000* | 5000* | 5000* |

### Augmentation des limites fixes
{: #increase_fixed_tlimit}

Les valeurs limite qui se terminent par un astérisque (*) sont fixes, mais il est possible de les augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Si vous souhaitez augmenter la valeur limite, contactez le support IBM en ouvrant directement un ticket à partir de la [console Web {{site.data.keyword.openwhisk_short}}](https://cloud.ibm.com/openwhisk). 
  1. Sélectionnez **Support**
  2. Sélectionnez **Ajouter un ticket** dans le menu déroulant.
  3. Sélectionnez **Technique** pour le type de ticket.
  4. Sélectionnez **Fonctions** pour le domaine technique de support.

#### minuteRate (Fixe : 5000*)
{: #openwhisk_syslimits_tminuterate}

* La limite de débit N est 5000 et limite le nombre de déclencheurs qu'un utilisateur peut activer dans des fenêtres d'une minute.
* Un utilisateur ne peut pas modifier la limite de déclencheurs lorsqu'un déclencheur est créé.
* Un appel d'interface de ligne de commande ou API dépassant cette limite reçoit un code d'erreur correspondant au code de statut HTTP `429: TOO MANY REQUESTS`.
* Cette valeur limite est fixe, mais il est possible de l'augmenter si une étude de rentabilité préconise de définir des valeurs limite de sécurité supérieure. Pour obtenir des instructions détaillées relatives à l'augmentation de cette limite, voir [Augmentation des limites fixes](/docs/openwhisk?topic=cloud-functions-openwhisk_reference#increase_fixed_tlimit).
