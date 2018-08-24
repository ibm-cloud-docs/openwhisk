---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Activité de journalisation et de surveillance
{: #openwhisk_logs}

La journalisation et la surveillance sont automatiquement activés dans {{site.data.keyword.openwhisk_short}} pour vous aider à résoudre les problèmes et améliorer l'intégrité et les performances de vos actions.

## Affichage des journaux
{: #view-logs}

Vous pouvez consulter les journaux d'activation directement depuis le tableau de bord de surveillance {{site.data.keyword.openwhisk_short}}. Les journaux sont également transférés à [{{site.data.keyword.loganalysisfull}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) où ils sont indexés, afin de permettre une recherche en texte intégral à l'aide de tous les messages générés et d'une interrogation efficace basée sur des zones spécifiques.
{:shortdesc}

1. Affiche la page de surveillance [{{site.data.keyword.openwhisk_short}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/dashboard/).

2. Facultatif : pour consulter uniquement les journaux relatifs à une action spécifique, limitez le récapitulatif de surveillance à cette action. Dans la section Options de filtrage, sélectionnez le nom de l'action dans la liste déroulante **Limiter à**.

3. Dans le volet de navigation gauche, cliquez sr **Journaux**. La page {{site.data.keyword.loganalysisshort_notm}} Kibana s'affiche.

4. Facultatif : pour afficher des journaux plus anciens, définissez la valeur de période par défaut sur 15 minutes en cliquant sur **15 dernières minutes** dans l'angle supérieur droit et en sélectionnant une période différente.

### Interrogation des journaux
{: #query-logs}

Vous pouvez trouver des journaux d'activation spécifiques dans [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) à l'aide de la syntaxe de requête de Kibana.

Les exemples de requête suivants peuvent vous aider à déboguer des erreurs :
  * Rechercher tous journaux des erreurs :
      ```
      type: user_logs AND stream_str: stderr
      ```
      {: codeblock}

  * Rechercher tous mes journaux des erreurs qui sont générés par "myAction" :
      ```
      type: user_logs AND stream_str: stderr AND action_str: "*myAction"
      ```
      {: codeblock}

### Interrogation des résultats
{: #query-results}

Outre les lignes de journaux, [{{site.data.keyword.loganalysislong_notm}}](https://console.bluemix.net/docs/services/CloudLogAnalysis/kibana/analyzing_logs_Kibana.html#analyzing_logs_Kibana) indexe également les résultats, ou les enregistrements d'activation, qui sont générés par {{site.data.keyword.openwhisk_short}}. Les résultats contiennent les métadonnées d'activation comme la durée d'activation ou le code de résultat d'activation. L'interrogation des zones de résultat peut vous aider à comprendre le comportement de vos actions {{site.data.keyword.openwhisk_short}}.

Vous pouvez trouver des journaux d'activation spécifiques à l'aide de la syntaxe de requête de Kibana. Les exemples de requête suivants peuvent vous aider à déboguer des erreurs :

* Rechercher toutes les activations ayant échoué :
    ```
    type: activation_record AND NOT status_str: 0
    ```
    {: codeblock}
    Dans les résultats, un `0` indique une action fermée avec succès et toutes les autres valeurs indiquent une erreur.

* Rechercher toutes les activations ayant échoué avec une erreur spécifique :
    ```
    type: activation_record AND NOT status_str:0 AND message: "*VerySpecificErrorMessage*"
    ```
    {: codeblock}

## Surveillance de votre activité
{: #openwhisk_monitoring}

Le tableau de bord [{{site.data.keyword.openwhisk_short}}](https://console.bluemix.net/openwhisk/dashboard/) fournit un récapitulatif graphique de votre activité. Utilisez-le pour déterminer les performances et la santé de vos actions {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Vous pouvez filtrer les journaux en sélectionnant les journaux d'actions que vous souhaitez afficher et la période d'activité consignée. Ces filtres sont appliqués à toutes les vues du tableau de bord. Cliquez sur **Rechargement** à tout moment pour mettre à jour le tableau de bord avec les données de journal d'activation les plus récentes.

### Récapitulatif de l'activité
{: #summary}

La vue **Activity Summary** fournit un récapitulatif général de votre environnement {{site.data.keyword.openwhisk_short}}. Utilisez cette vue pour surveiller la santé et les performances générales de votre service activé pour {{site.data.keyword.openwhisk_short}}. A partir des métriques affichées dans cette vue, vous pouvez :
* Déterminer la fréquence d'utilisation des actions compatibles avec {{site.data.keyword.openwhisk_short}} de votre service en affichant le
nombre de fois qu'elles ont été appelées.
* Déterminer le taux d'échec global pour toutes les actions. Si vous détectez une erreur, vous pouvez déterminer quels sont les services ou quelles sont les actions qui présentent des erreurs en affichant la vue de l'**histogramme de l'activité**. Isolez les erreurs proprement dites en affichant le **journal d'activité**.
* Déterminer si vos actions s'exécutent correctement en affichant le temps d'exécution moyen associé à chaque action.

### Chronologie de l'activité
{: #timeline}

La vue de la **chronologie de l'activité** affiche un graphique à barres verticales qui présente l'activité des actions passées et présentes. Le rouge indique des erreurs dans des actions spécifiques. Corrélez cette vue avec le **journal d'activité** pour plus de détails sur les erreurs.

<!--
### Activity Histogram
{: #histogram}

The **Activity Histogram** view displays a horizontal bar graph for viewing the activity of past and present actions. Red bars indicate errors within specific actions. Correlate this view with the **Activity Log** to find more details about errors.
-->

### Journal d'activité
{: #log}

Cette vue **Journal d'activité** affiche une version formatée du journal d'activation. Elle affiche les détails de chaque activation et recherche les nouvelles activations toutes les minutes. Cliquez sur une action pour afficher un journal détaillé.

Pour obtenir la sortie affichée dans le journal d'activité via l'interface de ligne de commande, utilisez la commande suivante :
```
ibmcloud fn activation poll
```
{: pre}
