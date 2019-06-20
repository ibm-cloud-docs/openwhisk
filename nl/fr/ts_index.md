---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-08"

keywords: troubleshooting actions, functions, help, support,

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# Traitement des incidents
{: #troubleshooting}

Si vous rencontrez des problèmes lorsque vous utilisez {{site.data.keyword.openwhisk}}, les techniques suivantes peuvent vous aider à les résoudre et à obtenir de l'aide.
{: shortdesc}



## L'action échoue
{: #ts_action_fails}

{: tsSymptoms}
L'action échoue.

{: tsCauses}
Une explication possible à l'échec d'une application est l'utilisation d'un contexte d'exécution obsolète. Une action ne peut pas aboutir tant que le contexte d'exécution n'est pas mis à jour avec un contexte pris en charge.

{: tsResolve}
Pour voir le contexte d'exécution utilisé pour l'action, exécutez la commande `ibmcloud fn action get ACTION_NAME` et recherchez `deprecated=true` dans la réponse à la requête. Si l'environnement d'exécution est obsolète, [mettez-le à jour](/docs/openwhisk?topic=cloud-functions-actions#actions_update).


<br />


## Les limites du système ont été atteintes
{: #ts_limit_reached}

{: tsSymptoms}
Votre fonction ne s'exécute pas correctement et vous pouvez obtenir un message de type `{"error":"signal: killed"}`.

{: tsCauses}
Toutes les fonctions ont des [limites système](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits) minimales et maximales, telles qu'une utilisation maximale de la mémoire ou un délai d'attente imparti.

{: tsResolve}
Certaines valeurs limites peuvent être augmentées si une étude de rentabilité préconise de définir des valeurs limites de sécurité supérieures. Pour augmenter la valeur limite, contactez le support IBM en ouvrant directement un ticket sur la console Web d'IBM Cloud Functions.

1. Sélectionnez **Support**.
2. Sélectionnez **Ajouter un ticket** dans le menu déroulant.
3. Sélectionnez **Technique** pour le type de ticket.
4. Sélectionnez **Fonctions** pour le domaine technique de support.


<br />


## Aide et assistance
{: #gettinghelp}

Vous rencontrez toujours des problèmes avec votre fonction ?
{: shortdesc}

-   Pour voir si {{site.data.keyword.Bluemix_notm}} est disponible, [vérifiez la page Statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?selected=status).
-   Consultez les forums pour établir si d'autres utilisateurs ont rencontré le même problème. Lorsque vous utilisez les forums pour poser une question, balisez votre question de sorte que les équipes de développement {{site.data.keyword.Bluemix_notm}} la voient.
    -   Pour toute question technique sur le développement de fonctions avec {{site.data.keyword.openwhisk}}, publiez votre question sur [Stack Overflow ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/search?q=ibm-cloud-functions) en indiquant les balises `ibm-cloud-functions`.
    -   Pour toute question sur le service et les instructions de mise en route, utilisez le forum [IBM Developer Answers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/functions/?smartspace=bluemix). Incluez les balises `ibm-cloud` et `functions`.
    Pour plus de détails sur l'utilisation des forums, voir [Comment obtenir de l'aide](/docs/get-support?topic=get-support-getting-customer-support#using-avatar).
-   Contactez le support IBM en ouvrant un cas. Pour savoir comment ouvrir un cas de support IBM ou obtenir les niveaux de support et la gravité des cas, voir [Contacter le support](/docs/get-support?topic=get-support-getting-customer-support).
Lorsque vous signalez un problème, incluez votre ID d'activation. Pour obtenir un ID d'activation, exécutez la commande `ibmcloud fn activation list`.
{: tip}
