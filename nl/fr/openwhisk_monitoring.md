---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-16"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Surveillance d'activité à l'aide du tableau de bord
{: #openwhisk_monitoring}

Le [tableau de bord {{site.data.keyword.openwhisk}}](https://console.bluemix.net/openwhisk/dashboard/) fournit un récapitulatif graphique de votre activité. Utilisez-le pour déterminer les performances et la santé de vos actions {{site.data.keyword.openwhisk_short}}.
{:shortdesc}

Cliquez sur **Rechargement** à tout moment pour mettre à jour le tableau de bord avec les données de journal d'activation les plus récentes.

## Récapitulatif de l'activité
{: #summary}

Cette vue fournit un récapitulatif général de votre environnement {{site.data.keyword.openwhisk_short}}. Utilisez la vue **Récapitulatif de l'activité** pour surveiller la santé et les performances générales de votre service activé pour {{site.data.keyword.openwhisk_short}}. A partir des métriques affichées dans cette vue, vous pouvez :
* Déterminer la fréquence d'utilisation des actions activées pour votre service {{site.data.keyword.openwhisk_short}} en affichant le nombre de fois qu'elles ont été appelées.
* Déterminer le taux d'échec global pour toutes les actions. Si vous détectez une erreur, vous pouvez déterminer quels sont les services ou quelles sont les actions qui présentent des erreurs en affichant la vue de l'**histogramme de l'activité**. Isolez les erreurs proprement dites en affichant le **journal d'activité**.
* Déterminer si vos actions s'exécutent correctement en affichant le temps d'exécution moyen associé à chaque action.

<!-- For tips on improving performance, see troubleshooting? -->

## Chronologie de l'activité
{: #timeline}

La vue de la **chronologie de l'activité** affiche un graphique à barres verticales qui présente l'activité des actions passées et présentes. Le rouge indique des erreurs dans des actions spécifiques. Corrélez cette vue avec le **journal d'activité** pour plus de détails sur les erreurs.

## Histogramme de l'activité
{: #histogram}

La vue de l'**histogramme de l'activité** affiche un graphique à barres horizontales qui présente l'activité des actions passées et présentes. Le rouge indique des erreurs dans des actions spécifiques. Corrélez cette vue avec le **journal d'activité** pour plus de détails sur les erreurs.

## Journal d'activité
{: #log}

Cette vue affiche une version formatée du journal d'activation. Elle affiche les détails de chaque activation et recherche les nouvelles activations toutes les minutes. Cliquez sur une action pour afficher un journal détaillé.

Pour obtenir la sortie affichée dans le journal d'activité via l'interface de ligne de commande, utilisez la commande suivante :
```
ibmcloud fn activation poll
```
{: pre}

## Option de filtrage
{: #filtering}

Sélectionnez les journaux d'actions que vous souhaitez afficher et la période d'activité consignée.

Ces filtres sont appliqués à toutes les vues du tableau de bord.
{: tip}
