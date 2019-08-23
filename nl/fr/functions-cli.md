---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

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




# Interface de ligne de commande de {{site.data.keyword.openwhisk_short}}
{: #functions-cli}

Exécutez les commandes suivantes pour gérer les entités qui constituent vos fonctions.
{: shortdec}

<br />

## Commandes d'action
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

Créez une action.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>Nom de l'action. Pour inclure l'action dans un package, entrez le nom au format Nom du package/Nom de l'action (`PACKAGE_NAME`/`ACTION_NAME`). Cette valeur est obligatoire. </dd>

   <dt>`APP_FILE`</dt>
   <dd>Chemin d'accès au package ou au fichier d'application à exécuter en tant qu'action. Cette option est obligatoire.</dd>
   
   <dt>`--copy`</dt>
   <dd>Traite l'action comme un nom d'action existante.</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>Nom d'utilisateur de Docker Hub (DOCKER_HUB_USERNAME) et nom de l'image Docker (IMAGE_NAME) dans Docker Hub utilisés pour exécuter l'action. Cet indicateur est obligatoire pour créer des actions à partir d'images Docker.</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>Contexte d'exécution de votre application. Cet indicateur est facultatif. Si aucune `valeur` n'est indiquée, la version par défaut du contexte d'exécution détecté est utilisée.
     Valeurs `VALUES` possibles pour l'option `--kind`.
     <table>
  <tr>
    <th>Langage</th>
    <th>Identificateur de type</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (valeur par défaut), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>D'autres langages sont pris en charge en utilisant des actions Docker.</td>
  </tr>
</table>
{: caption="Tableau 1. Environnements d'exécution pris en charge" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>Taille maximale de journal en Mo pour l'action. La valeur par défaut est 10 Mo.</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>Si la méthode d'entrée (ENTRY_METHOD_NAME) de l'action ne correspond pas à `main`, indiquez le nom personnalisé. Cet indicateur est obligatoire lorsque la méthode n'a pas la valeur `main`. Pour certains contextes d'exécution, comme par exemple Java, ce nom doit correspondre au nom qualifié complet de la méthode. </dd>

   <dt>`--native`</dt>
   <dd>Vous pouvez utiliser l'argument `--native` comme abréviation de `--docker openwhisk/dockerskeleton`. En utilisant cet argument, vous pouvez créer et déployer un exécutable qui s'exécute dans le SDK d'action Docker standard.
<ol><li>Lorsque vous créez une image Docker, un exécutable est généré dans le conteneur situé dans `/action/exec`. Copiez le fichier `/action/exec` sur votre système de fichiers local et compressez-le dans `exec.zip`.</li>
       <li>Créez une action Docker qui reçoit l'exécutable sous forme de données d'initialisation. L'argument `--native` remplace l'argument `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>Créez une séquence d'actions et incluez les noms des actions associées. Séparez les `ACTION_NAME` par des virgules.</dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>Valeur limite (`LIMIT`) du délai d'attente en millisecondes. La valeur par défaut est 60000 millisecondes. Lorsque ce délai est écoulé, l'action prend fin.</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>Traite l'action en tant qu'action Web, action Web HTTP brute ou en tant qu'action standard. Indiquez `yes` ou `true` pour une action Web, `raw` pour une action Web HTTP brute ou `no` ou `false` pour une action standard. Pour sécuriser votre action Web, incluez également l'option `--web-secure`.</dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>Sécurise l'action Web. La `valeur` de `SECRET` peut être `true`, `false` ou n'importe quelle chaîne. Cette option ne peut être utilisée qu'avec l'option `--web`.</dd>
   </dl>

<br />**Exemple**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **Sortie**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

Vous pouvez nettoyer votre espace de nom en supprimant les actions que vous ne souhaitez plus utiliser.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **Sortie**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

Obtenez les métadonnées qui décrivent une action spécifique.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Nom de l'action. Cette valeur est obligatoire.</dd>

   <dt>`--save`</dt>
   <dd>Vous pouvez obtenir et sauvegarder en local le code associé à une action existante, à l'exception des séquences et des actions Docker. `FILENAME` correspond à un nom d'action existant dans le répertoire de travail en cours et l'extension du fichier correspond à la valeur du paramètre kind de l'action. Pour le code d'action dans un fichier archive, une extension .zip est utilisée. Cet indicateur est facultatif.</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>Sauvegardez le code des actions dans un fichier avec un nom personnalisé en fournissant le chemin d'accès au fichier, le nom du fichier (`FILENAME`) et l'extension. Cet indicateur est facultatif.</dd>

  <dt>`--summary`</dt>
  <dd>Permet d'obtenir un récapitulatif des détails de l'action. Les paramètres avec le préfixe "*" sont liés ; les paramètres avec le préfixe "**" sont liés et finalisés. Cet indicateur est facultatif.</dd>

  <dt>`--url`</dt>
  <dd>Obtenez uniquement l'URL de l'action. Cet indicateur est facultatif.</dd>
   </dl>

<br />**Exemple**

```
ibmcloud fn action get hello
```
{: pre}

**Sortie**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

Exécutez une action pour la tester.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Nom de l'action. Cette valeur est obligatoire. </dd>

   <dt>`--blocking, -b`</dt>
   <dd>Les appels bloquants utilisent un style de demande et de réponse à attendre pour que le résultat de l'activation soit disponible. La période d'attente est inférieure à 60 secondes ou correspond à la [valeur limite (`LIMIT` `VALUE` de durée](/docs/openwhisk?topic=cloud-functions-limits)) de l'action. Cet indicateur est facultatif.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

   <dt>`--result, -r`</dt>
   <dd>Le résultat du code d'application est affiché sous forme de sortie de la commande. Si cette option n'est pas indiquée, l'ID d'activation est affiché. L'appel est bloquant lorsque cette option est spécifiée. Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `ibmcloud fn action list`
{: #cli_action_list}

Répertoriez toutes les actions que vous avez créées ou un nombre d'actions spécifique.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>Nom d'un package d'actions. Cette valeur est facultative. Si ce nom n'est pas spécifié, la liste de toutes les actions est affichée.</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>Affiche la liste d'un nombre d'actions (NUMBER_OF_ACTIONS) spécifié. La valeur par défaut est 30 actions.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Trie la liste des actions renvoyées par nom, autrement la liste est triée par date de création.</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>Exclut du résultat le nombre spécifié d'actions correspondant aux dernières actions créées.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

Mettez à jour une action ou l'application qui se trouve dans une action.

Lorsque vous mettez à jour des paramètres pour un package, une action ou un déclencheur, vous devez spécifier tous les paramètres précédemment créés. Sinon, les paramètres précédemment créés sont supprimés. Pour les packages, tous les services liés au package sont également retirés. Par conséquent, lorsque vous mettez à jour d'autres paramètres, vous devez à nouveau [lier les services](/docs/openwhisk?topic=cloud-functions-services) à votre package.
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Options de commande**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Nom de l'action. Pour inclure l'action dans un package, entrez le nom au format Nom du package/Nom de l'action (`PACKAGE_NAME`/`ACTION_NAME`). Cette valeur est obligatoire. </dd>

  <dt>`APP_FILE`</dt>
  <dd>Chemin d'accès au package ou au fichier d'application à exécuter en tant qu'action. Cette option est obligatoire lorsque vous souhaitez mettre à jour votre application au sein de l'action.</dd>

  <dt>`--copy`</dt>
  <dd>Traite l'action comme un nom d'action existante.</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>Nom d'utilisateur de Docker Hub (DOCKER_HUB_USERNAME) et nom de l'image Docker (IMAGE_NAME) dans Docker Hub utilisés pour exécuter l'action. Cet indicateur est obligatoire pour créer des actions à partir d'images Docker.</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>Contexte d'exécution de votre application. Cet indicateur est facultatif. Si aucune valeur (VALUE) n'est spécifiée, la version par défaut de l'environnement d'exécution détecté est utilisée.
    Valeurs possibles pour l'option `--kind`.
    <table>
  <tr>
    <th>Langage</th>
    <th>Identificateur de type</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (valeur par défaut), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (valeur par défaut)</td>
  </tr>
  <tr>
    <td>D'autres langages sont pris en charge en utilisant des actions Docker.</td>
  </tr>
</table>
{: caption="Tableau 1. Environnements d'exécution pris en charge" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>Taille maximale de journal en Mo pour l'action. La valeur par défaut est 10 Mo.</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>Si la méthode d'entrée (ENTRY_METHOD_NAME) de l'action ne correspond pas à `main`, indiquez le nom personnalisé. Cet indicateur est obligatoire lorsque la méthode n'a pas la valeur `main`. Pour certains contextes d'exécution, comme par exemple Java, ce nom doit correspondre au nom qualifié complet de la méthode. </dd>

  <dt>`--native`</dt>
  <dd>Vous pouvez utiliser l'argument `--native` comme abréviation de `--docker openwhisk/dockerskeleton`. En utilisant cet argument, vous pouvez créer et déployer un exécutable qui s'exécute dans le SDK d'action Docker standard.       <ol><li>Lorsque vous créez une image Docker, un exécutable est généré dans le conteneur situé dans `/action/exec`. Copiez le fichier `/action/exec` sur votre système de fichiers local et compressez-le dans `exec.zip`.</li>
      <li>Créez une action Docker qui reçoit l'exécutable sous forme de données d'initialisation. L'argument `--native` remplace l'argument `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>Crée une séquence d'actions en spécifiant le nom (ACTION_NAME) des actions associées.</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>Durée limite du délai d'attente en millisecondes. La valeur par défaut est 60000 millisecondes. Lorsque ce délai est écoulé, l'action prend fin.</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>Traite l'action en tant qu'action Web, action Web HTTP brute ou en tant qu'action standard. Indiquez `yes` ou `true` pour une action Web, `raw` pour une action Web HTTP brute ou `no` ou `false` pour une action standard. Pour sécuriser votre action Web, incluez également l'option `--web-secure`.</dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>Sécurise l'action Web. La `valeur` de `SECRET` peut être `true`, `false` ou n'importe quelle chaîne. Cette option ne peut être utilisée qu'avec l'option `--web`.</dd>
  </dl>

<br />**Exemple**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Commandes d'activation
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

Obtenez les métadonnées qui décrivent une activation spécifique.

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**Options de commande**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>ID d'une activation spécifique. Utilisez `ibmcloud fn activation list` pour extraire la liste des ID disponibles. Cette valeur est obligatoire, sauf si l'option `--last` ou `-l` est spécifiée.</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>Zone dans les métadonnées dont vous voulez afficher les informations. Par exemple, pour afficher la zone des journaux (logs), exécutez `ibmcloud fn activation get ACTIVATION_ID logs`. Cette valeur est facultative.</dd>

  <dt>`--last, -l`</dt>
  <dd>Affiche les métadonnées de l'activation la plus récente. Cet indicateur est facultatif.</dd>

  <dt>`--summary, -s`</dt>
  <dd>Affiche la réponse des résultats à partir des détails de l'activation. Cet indicateur est facultatif.</dd>
  </dl>


<br />**Exemple**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list`
{: #cli_activation_list}

Répertoriez tous les ID d'activation de toutes les actions d'un package.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**Options de commande**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>Affiche la description de toutes les activations.</dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Affiche la liste d'un nombre d'activations (NUMBER_OF_ACTIVATIONS) spécifié. La valeur par défaut est 30 activations et le nombre maximal est fixé à 200 activations.</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>Affiche la liste des activations créées depuis la date indiquée. La durée est exprimée en millisecondes depuis le 1er janvier 1970. Exemple : `1560371263` correspond au 12 juin 2019 08:27:43 UTC.</dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Exclut du résultat le nombre spécifié d'activations correspondant aux activations les plus récentes.</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>Affiche la liste des activations créées avant la date indiquée. La durée est exprimée en millisecondes depuis le 1er janvier 1970. Exemple : `1560371263` correspond au 12 juin 2019 08:27:43 UTC.</dd>
  </dl>

<br />**Exemple**
```
ibmcloud fn activation list
```
{: pre}

**Sortie**
```
activations
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

Obtenez les journaux d'une activation spécifique.

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**Options de commande**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>ID d'une activation spécifique. Utilisez `ibmcloud fn activation list` pour extraire la liste des ID disponibles. Cette valeur est obligatoire, sauf si l'option `--last` ou `-l` est spécifiée.</dd>

  <dt>`--last, -l`</dt>
  <dd>Affiche les journaux de l'activation la plus récente. Cet indicateur est facultatif.</dd>

  <dt>`--strip, -r`</dt>
  <dd>Affiche le message de journal uniquement ; exclut les informations d'horodatage et de flux. Cet indicateur est facultatif.</dd>
  </dl>

<br />**Exemple**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

Affichez un flux de données, la liste d'activations en direct d'une action ou d'un espace de nom. Vous pouvez appuyer sur `CTRL + C` pour quitter l'interrogation.

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**Options de commande**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>Espace de nom commençant par /. Interroge les activations d'un espace de nom, d'une action ou d'un espace. Cette valeur est facultative. Si aucun espace de nom ou aucune action n'est indiqué, l'interrogation porte sur l'espace.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Interroge les activations d'un espace de nom, d'une action ou d'un espace. Cette valeur est facultative. Si aucun espace de nom ou aucune action n'est indiqué, l'interrogation porte sur l'espace.</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>Interroge les activations durant un nombre de secondes indiqué avant de quitter. Cet indicateur est facultatif.</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>Début de l'interrogation des activations depuis un certain nombre de jours. Cet indicateur est facultatif.</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>Début de l'interrogation des activations depuis un certain nombre d'heures. Cet indicateur est facultatif.</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>Début de l'interrogation des activations depuis un certain nombre de minutes. Cet indicateur est facultatif.</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>Début de l'interrogation des activations depuis un certain nombre de secondes. Cet indicateur est facultatif.</dd>
  </dl>

<br />**Exemple**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

Obtenez le résultat d'une activation spécifique.

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**Options de commande**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>ID d'une activation spécifique. Utilisez `ibmcloud fn activation list` pour extraire la liste des ID disponibles. Cette valeur est obligatoire, sauf si l'option `--last` ou `-l` est spécifiée.</dd>

  <dt>`--last, -l`</dt>
  <dd>Affiche le résultat de l'activation la plus récente. Cet indicateur est facultatif.</dd>

  </dl>

<br />**Exemple**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## Commandes d'API
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

Créez une API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Chemin de base de l'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Nom de l'API. Ce nom doit être identique au chemin de base.</dd>

   <dt>`API_VERB`</dt>
   <dd>Instruction de l'API, par exemple `get` ou `post`.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>Nom de l'action.</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>Nom de l'API. Cet indicateur est ignoré lorsqu'un fichier de configuration est spécifié. Le nom par défaut est `BASE_PATH`. Cet indicateur est facultatif.</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>Fichier JSON contenant la configuration de l'API Swagger. Lorsque cet indicateur est utilisé, l'indicateur du nom d'API est ignoré. Cet indicateur est obligatoire.</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>Définit le type de réponse de l'action Web sous la forme `html`, `http`, `json`, `text` ou `svg`. La valeur par défaut est `json`. Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Sortie**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

Supprimez une API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Chemin de base de l'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Nom de l'API. Ce nom doit être identique au chemin de base.</dd>

   <dt>`API_PATH`</dt>
   <dd>Chemin d'accès à l'API.</dd>

   <dt>`API_VERB`</dt>
   <dd>Instruction de l'API, par exemple `GET` ou `POST`.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Indique le type de sortie de l'API sous la forme `json` ou `yaml`. La valeur par défaut est `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Affiche les détails de la configuration complète de l'API.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

Obtenez les métadonnées d'une API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Chemin de base de l'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Nom de l'API. Ce nom doit être identique au chemin de base.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Indique le type de sortie de l'API sous la forme `json` ou `yaml`. La valeur par défaut est `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Affiche les détails de la configuration complète de l'API.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `ibmcloud fn api list`
{: #cli_api_list}

Répertoriez toutes les API que vous avez créées ou un nombre d'API spécifique. Si aucun nom ou chemin de base n'est indiqué, la liste de toutes les API est affichée.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>Chemin de base de l'API.</dd>

   <dt>`API_NAME`</dt>
   <dd>Nom de l'API. Ce nom doit être identique au chemin de base.</dd>

   <dt>`API_PATH`</dt>
   <dd>Chemin d'accès à l'API.</dd>

   <dt>`API_VERB`</dt>
   <dd>Instruction de l'API, par exemple `GET` ou `POST`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Affiche tous les détails de l'API. Cet indicateur est facultatif. </dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>Affiche un nombre d'API (NUMBER_OF_APIS) spécifié. La valeur par défaut est 30 API. Cet indicateur est facultatif. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Trie la liste des API renvoyées par nom, autrement la liste est triée par date de création. Cet indicateur est facultatif. </dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>Exclut du résultat le nombre spécifié d'API correspondant aux dernières API créées. Cet indicateur est facultatif. </dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## Commandes de déploiement
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

Utilisez un fichier manifeste pour déployer une collection de packages, d'actions, de déclencheurs et de règles.

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>Hôte d'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>Fichier de configuration. La valeur par défaut est `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>Chemin d'accès au fichier de déploiement.</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>Chemin d'accès au fichier manifeste. Cet indicateur est obligatoire si le fichier manifest.yaml ne figure pas dans le répertoire de travail.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>Nom ou ID d'un espace de nom.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

   <dt>`--preview` </dt>
   <dd>Affichez le plan de déploiement avant le déploiement.</dd>

   <dt>`--project PATH`</dt>
   <dd>Chemin d'accès au projet sans serveur. La valeur par défaut est <code>.</code> (répertoire de travail).</dd>

   <dt>`--strict`</dt>
   <dd>Autorise une version du contexte d'exécution défini par l'utilisateur.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Affiche la sortie prolixe.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Utilisez un fichier manifeste pour annuler le déploiement d'une collection de packages, d'actions, de déclencheurs et de règles.

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>Hôte d'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>Fichier de configuration. La valeur par défaut est `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>Chemin d'accès au fichier de déploiement.</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>Chemin d'accès au fichier manifeste. Cet indicateur est obligatoire si le fichier manifest.yaml ne figure pas dans le répertoire de travail.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>Nom ou ID d'un espace de nom.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

   <dt>`--preview` </dt>
   <dd>Affiche le résultat de la commande sans exécuter la commande.</dd>

   <dt>`--project PATH`</dt>
   <dd>Chemin d'accès au projet sans serveur. La valeur par défaut est `.` (répertoire de travail).</dd>

   <dt>`--strict`</dt>
   <dd>Autorise une version du contexte d'exécution défini par l'utilisateur.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Affiche la sortie prolixe.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## Commande de liste
{: #cli_list_cmd}


### `ibmcloud fn list`
{: #cli_list}

Affichez une liste de groupes de packages, d'actions, de déclencheurs et de règles dans un espace de nom.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>Trie chaque groupe d'entités renvoyées par nom, autrement chaque groupe est trié par date de création.</dd>
   </dl>

<br />**Exemple**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Commandes d'espace de nom
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

Créez un espace de nom IAM.

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Nom d'un espace de nom. N'incluez pas de trait d'union (-) dans le nom. Cette valeur est obligatoire.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Ecrivez une description unique pour vous aider à identifier l'espace de nom. Si votre description comporte plusieurs mots, mettez-la entre guillemets ("). Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. See Beth for information about this namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

Supprimez un espace de nom IAM.

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**Exemple**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Obtenez les entités pour un espace de nom IAM ou Cloud Foundry ou les métadonnées en provenance d'un espace de nom de ce type.

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Nom ou ID d'un espace de nom. Cette valeur est obligatoire.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Trie la liste des espaces de nom renvoyés par nom, autrement la liste est triée par date de création. Cet indicateur est facultatif. </dd>

   <dt>`--properties`</dt>
   <dd>Affiche les propriétés de l'espace de nom au lieu des entités qu'il contient. Cet indicateur est facultatif. </dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **Sortie**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `ibmcloud fn namespace list`
{: #cli_namespace_list}

Affichez la liste des espaces de nom Cloud Foundry et IAM disponibles.

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--cf`</dt>
   <dd>Affiche uniquement les espaces de nom Cloud Foundry. Les espaces de nom IAM ne sont pas affichés. Cet indicateur est facultatif.</dd>

   <dt>`--iam`</dt>
   <dd>Affiche uniquement les espaces de nom IAM. Les espaces de nom Cloud Foundry ne sont pas affichés. Cet indicateur est facultatif.</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Affiche la liste d'un nombre d'espaces de nom (NUMBER_OF_NAMESPACES) spécifié. La valeur par défaut est 30 espaces de nom. Cet indicateur est facultatif. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Trie la liste des espaces de nom renvoyés par nom, autrement la liste est triée par date de création. Cet indicateur est facultatif. </dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Exclut du résultat le nombre spécifié d'espaces de nom correspondant aux derniers espaces de nom créés. Cet indicateur est facultatif. </dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

Changez le nom ou modifiez la description d'un espace de nom IAM.

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>Nom d'un espace de nom. N'incluez pas de trait d'union (-) dans le nom. Cette valeur est obligatoire.</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>Nouveau nom d'un espace de nom. N'incluez pas de trait d'union (-) dans le nom. Cette valeur est facultative.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Ecrivez une description unique pour vous aider à identifier l'espace de nom. Si votre description comporte plusieurs mots, mettez-la entre guillemets ("). Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Commandes de package
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

Liez les paramètres à un package. Toutes les actions dans le package héritent de ces paramètres sauf indication contraire.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Options de commande**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>Nom du package. Cette valeur est obligatoire. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>
  </dl>

<br />**Exemple**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

Créez un package conçu pour contenir une ou plusieurs actions. Pour ajouter une action dans le package, incluez le nom du package avec le nom de l'action lors de la création ou de la mise à jour de l'action.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Options de commande**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>Nom du package. Cette valeur est obligatoire. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Fichier JSON contenant le paramètre au format `KEYS` `VALUE`. Cet indicateur est facultatif.</dd>

  <dt>`--shared yes|no`</dt>
  <dd>Lorsque cette option n'a pas de valeur ou est spécifiée avec la valeur yes, le package est partagé avec d'autres utilisateurs.</dd>
  </dl>

<br />**Exemple**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Sortie**
  ```
  ok: created hellopkg
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

Vous pouvez procéder au nettoyage de votre espace de nom en supprimant les packages que vous ne souhaitez plus utiliser.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **Sortie**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

Obtenez les métadonnées qui décrivent un package spécifique.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**Options de commande**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>Nom d'un package. Cette valeur est obligatoire.</dd>

   <dt>`--summary`</dt>
   <dd>Obtenez un récapitulatif des détails du package. Les paramètres avec le préfixe "*" sont liés. Cet indicateur est facultatif.</dd>
   </dl>

<br />**Exemple**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `ibmcloud fn package list`
{: #cli_pkg_list}

Répertoriez tous les packages que vous avez créés ou un nombre de packages spécifique.

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>Répertoriez les packages figurant dans un espace de nom spécifique. Cette valeur est facultative. Si cette option n'est pas indiquée, la liste de tous les packages est affichée.</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>Affiche la liste d'un nombre de packages (NUMBER_OF_PACKAGES) spécifié. La valeur par défaut est 30 packages.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Trie la liste des packages renvoyés par nom, autrement la liste est triée par date de création.</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>Exclut du résultat le nombre spécifié de packages correspondant aux derniers packages créés.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Exécutez la commande `ibmcloud fn package list /whisk.system` pour afficher la liste des packages préinstallés.
  {: tip}


<br />

### `ibmcloud fn package refresh`
{: #cli_pkg_refresh}

Actualisez les liaisons de package pour tous le packages figurant dans un espace de nom spécifique.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>Espace de nom commençant par /. Cet indicateur est obligatoire. Exécutez la commande `ibmcloud fn namespace list` pour obtenir la liste des espaces de nom parmi lesquels effectuer la sélection.</dd>
   </dl>

<br />**Exemple**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

Mettez à jour un package conçu pour contenir une ou plusieurs actions. Pour ajouter une action dans le package, incluez le nom du package avec le nom de l'action lors de la création ou de la mise à jour de l'action.

Lorsque vous mettez à jour des paramètres pour un package, une action ou un déclencheur, vous devez spécifier tous les paramètres précédemment créés. Sinon, les paramètres précédemment créés sont supprimés. Pour les packages, tous les services liés au package sont également retirés. Par conséquent, lorsque vous mettez à jour d'autres paramètres, vous devez à nouveau [lier les services](/docs/openwhisk?topic=cloud-functions-services) à votre package.
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>Nom du package. Cette valeur est obligatoire. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

   <dt>`--shared yes|no`</dt>
   <dd>Lorsque cette option n'a pas de valeur ou est spécifiée avec la valeur `yes`, le package est partagé avec d'autres utilisateurs.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Sortie**
  ```
  ok: created hellopkg
  ```
  {: screen}




<br /><br />
## Commandes de propriété
{: #cli_prop}

Définissez les propriétés globales de votre environnement d'interface de ligne de commande ou affichez les propriétés de l'interface de ligne de commande `wsk`, qui s'exécute dans le cadre de l'interface de ligne de commande `ibmcloud fn`.

### `ibmcloud fn property get`
{: #cli_prop_get}

Affichez les détails des métadonnées d'une propriété de l'interface de ligne de commande `wsk`.

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`--all`</dt>
   <dd>Affiche toutes les propriétés de l'interface de ligne de commande `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`---apibuild`</dt>
   <dd>Informations de génération de l'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--apibuildno`</dt>
   <dd>Numéro de génération de l'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>Hôte d'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>Version de l'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Certificat client `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--cliversion`</dt>
   <dd>Version de l'interface de ligne de commande `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`Clé` du client `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Espace de nom IAM. Cet indicateur ne peut pas être défini pour les espaces de nom Cloud Foundry. Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn property get --auth
  ```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

Définissez une propriété. Vous devez indiquer au moins un indicateur. Une fois qu'une propriété est définie, elle est conservée sur votre poste de travail dans `<home_dir>/.bluemix/plugins/cloud-functions/config.json`. Pour retirer une propriété, exécutez [`ibmcloud fn property unset --<property>`](#cli_prop_set). 

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>Hôte d'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>Version de l'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Certificat client `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`Clé` du client `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Espace de nom IAM. Cet indicateur ne peut pas être défini pour les espaces de nom Cloud Foundry. Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **Sortie**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

Annulez la définition d'une propriété de l'interface de ligne de commande `wsk`. Vous devez indiquer au moins un indicateur.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>Hôte d'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>Version de l'API `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>`Clé` d'autorisation `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>Certificat client `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>`Clé` du client `wsk`. Cet indicateur est facultatif.</dd>

   <dt>`--namespace` `NAMESPACE`</dt>
   <dd>Espace de nom IAM. Cet indicateur ne peut pas être défini pour les espaces de nom Cloud Foundry. Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## Commandes de règle
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

Créez une règle pour associer un déclencheur à une action. Avant de créer une règle, créez d'abord un déclencheur et une action.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **Sortie**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

Pour effectuer un nettoyage de votre espace de nom, supprimez les règles dont vous n'avez plus besoin.

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Nom d'une règle. Cette valeur est obligatoire.</dd>

  <dt>`--disable`</dt>
  <dd>Désactivez la règle avant de la supprimer.</dd>
  </dl>


<br />**Exemple**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

Faites passer le statut d'une règle à 'inactive' et empêchez-la d'effectuer une action lorsqu'un déclencheur est exécuté.

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

Modifiez le statut d'une règle pour la faire passer de l'état 'inactive' à 'active'. Lorsque la règle est active, une action est effectuée lorsqu'un déclencheur est exécuté.

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

Obtenez les métadonnées qui décrivent une règle spécifique.

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Nom d'une règle. Cette valeur est obligatoire.</dd>

  <dt>`--summary`</dt>
  <dd>Obtenez un récapitulatif des détails de la règle.</dd>
  </dl>

<br />**Exemple**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `ibmcloud fn rule list`
{: #cli_rule_list}

Répertoriez toutes les règles que vous avez créées ou un nombre de règles spécifique.

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Nom d'une règle. Cette valeur est facultative. Si elle n'est pas spécifiée, la liste de toutes les règles est affichée.</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>Affiche la liste d'un nombre de règles (NUMBER_OF_RULES) spécifié. La valeur par défaut est 30 règles.</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>Trie la liste des règles renvoyées par nom, autrement la liste est triée par date de création.</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>Exclut du résultat le nombre spécifié de règles correspondant aux dernières règles créées.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

Déterminez si une règle est active ou inactive. Exécutez les commandes `ibmcloud fn rule disable` ou `ibmcloud fn run enable` pour modifier le statut.

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

Pour modifier des déclencheurs associés à certaines règles, vous pouvez mettre à jour une règle.

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Exemple**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## Commandes SDK
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

Installez un logiciel SDK.

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>Composant du logiciel SDK, par exemple `docker`, `iOS` et `bashauto`. Cette valeur est obligatoire.</dd>

   <dt>`--stdout, --s`</dt>
   <dd>Imprime les résultats de la commande bash dans `STDOUT`. Cet indicateur est facultatif.</dd>


   </dl>

<br />**Exemple**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Commandes de service
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

Lie les données d'identification de service à une action ou un package.

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>Nom du service.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>Nom du package ou de l'action auquel lier les données d'identification.</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>Nom de l'instance de service.</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>Nom des données d'identification (`KEY`) du service à lier.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

Supprime la liaison des données d'identification de service d'une action ou d'un package.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`SERVICE`</dt>
   <dd>Nom du service.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>Nom du package ou de l'action duquel déconnecter les données d'identification.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Commandes de déclencheur
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

Créez un déclencheur.

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Nom du déclencheur. Cette valeur est obligatoire. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>Définit le type de déclencheur en tant que flux. Cet indicateur est facultatif.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>


   </dl>

<br />**Exemple**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

Supprimez un déclencheur.

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**Exemple**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

Testez un déclencheur en l'exécutant, plutôt qu'en attendant son exécution automatique.

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Options de commande**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>Nom du déclencheur. Cette valeur est obligatoire. </dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>`Valeurs` des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

Obtenez les métadonnées qui décrivent un déclencheur spécifique.

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Nom d'un déclencheur. Cette valeur est obligatoire.</dd>

  <dt>`--summary`</dt>
  <dd>Permet d'obtenir un récapitulatif des détails du déclencheur.</dd>
  </dl>

<br />**Exemple**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `ibmcloud fn trigger list`
{: #cli_trigger_list}

Répertoriez tous les déclencheurs que vous avez créés ou un nombre de déclencheurs spécifique.

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>Nom d'un déclencheur. Cette valeur est facultative. Si elle n'est pas spécifiée, la liste de tous les déclencheurs est affichée.</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Affiche la liste d'un nombre de déclencheurs (NUMBER_OF_TRIGGERS) spécifié. La valeur par défaut est 30 déclencheurs.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Trie la liste des déclencheurs renvoyés par nom, autrement la liste est triée par date de création.</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Exclut du résultat le nombre spécifié de déclencheurs correspondant aux derniers déclencheurs créés.</dd>

   </dl>

<br />**Exemple**

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

Mettez à jour un déclencheur.

Lorsque vous mettez à jour des paramètres pour un package, une action ou un déclencheur, vous devez spécifier tous les paramètres précédemment créés. Sinon, les paramètres précédemment créés sont supprimés. Pour les packages, tous les services liés au package sont également retirés. Par conséquent, lorsque vous mettez à jour d'autres paramètres, vous devez à nouveau [lier les services](/docs/openwhisk?topic=cloud-functions-services) à votre package.
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Options de commande**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>Nom du déclencheur. Cette valeur est obligatoire. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>Les annotations sont spécifiées au format clé valeur  (`KEY` `VALUE`). Pour inclure plus d'une annotation, indiquez cette option pour chaque annotation. Cet indicateur est facultatif.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Fichier JSON contenant une annotation au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valeurs des paramètres au format clé valeur  (`KEY` `VALUE`). Cet indicateur est facultatif.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Fichier JSON contenant la clé (`KEY`) et la valeur (`VALUE`) de chaque paramètre. Cet indicateur est facultatif.</dd>
   </dl>

<br />**Exemple**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






