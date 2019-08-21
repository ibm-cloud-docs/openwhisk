---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: namespaces, iam, cloud foundry, classic namespaces, functions

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


# Gestione degli spazi dei nomi
{: #namespaces}

Con {{site.data.keyword.openwhisk}}, puoi creare spazi dei nomi gestiti IAM (Identity and Access Management) per raggruppare tra loro le entità, quali azioni o trigger. Successivamente, puoi creare delle politiche di accesso IAM per lo spazio dei nomi. Per una panoramica di IAM, consulta il [blog degli annunci di abilitazione IAM {{site.data.keyword.openwhisk_short}}](https://www.ibm.com/cloud/blog/ibm-cloud-functions-is-now-identity-and-access-management-enabled){: external}.
{: shortdesc}

## Cos'è uno spazio dei nomi?

Gli spazi dei nomi contengono entità {{site.data.keyword.openwhisk_short}}, quali azioni e trigger, e appartengono a un gruppo di risorse. Puoi consentire agli utenti di accedere alle tue entità concedendo loro l'accesso allo spazio dei nomi.

Il nome completo di un'entità è `/namespaceName/packageName/entityName`.

### Cosa succede quando creo uno spazio dei nomi?

Gli spazi dei nomi creati in {{site.data.keyword.openwhisk_short}} vengono identificati come istanza di servizio IAM.
Durante la creazione di uno spazio dei nomi, puoi specificare il [gruppo di risorse](/docs/resources?topic=resources-rgs) in cui aggiungere l'istanza del servizio.

Quando crei uno spazio dei nomi, vengono creati i seguenti componenti:

| Componente | Descrizione |
| --- | --- | 
| Un ID servizio | Puoi utilizzare l'ID servizio come ID funzionale quando effetti chiamate in uscita. Tutte le azioni create in questo spazio dei nomi possono utilizzare questo ID servizio per accedere ad altre risorse. L'utente funzionale ottiene il ruolo Lettore per impostazione predefinita. L'accesso come lettore significa che può leggere le entità dello spazio dei nomi e richiamare le azioni. Il ruolo Lettore viene utilizzato dai trigger per richiamare le azioni. Per controllare il traffico in entrata, potresti voler concedere l'accesso ad altri utenti, ad esempio assegnando il ruolo Lettore per richiamare delle azioni. |
| Una chiave API | Una chiave API per l'ID servizio che può essere utilizzata per generare token IAM. Puoi utilizzare i token per autenticare lo spazio dei nomi con altri servizi {{site.data.keyword.cloud_notm}}. La chiave API viene fornita alle azioni come variabile di ambiente `__OW_IAM_NAMESPACE_API_KEY`. |

Visualizza tutti i tuoi ID servizio.
```
ibmcloud iam service-ids
```
{: pre}

Visualizza le chiavi API associate a un ID servizio. 
```
ibmcloud iam service-api-keys <ServiceID-12345678-1234-abcd-1234-123456789abc>
```
{: pre}

</br>

Non eliminare chiavi API.
{: tip}

### Ci sono delle limitazioni per gli spazi dei nomi?

[La creazione di API con il gateway API](/docs/openwhisk?topic=cloud-functions-apigateway) e l'[SDK mobile](/docs/openwhisk?topic=cloud-functions-pkg_mobile_sdk) non sono supportati per gli spazi dei nomi gestiti da IAM. 

I nomi di tutte le entità, inclusi azioni, trigger, regole, pacchetti e spazi dei nomi, sono una sequenza di caratteri aventi il seguente formato:
* Il primo carattere deve essere un carattere alfanumerico o un carattere di sottolineatura.
* I caratteri successivi possono essere alfanumerici, spazi o uno qualsiasi dei seguenti valori: `_`, `@`, `.`, `-`.
* L'ultimo carattere non può essere uno spazio.

### Cosa faccio se ho uno spazio dei nomi basato su Cloud Foundry?

I tuoi spazi dei nomi basati su Cloud Foundry funzionano ancora. Tuttavia, per poter usufruire delle nuove funzioni, devi creare uno spazio dei nomi abilitato a IAM.


## Creazione di uno spazio dei nomi basato su IAM nell'IU
{: #create_iam_ui}

1. Nella [console {{site.data.keyword.openwhisk_short}} ](https://cloud.ibm.com/openwhisk){: external}, fai clic sul menu a discesa dello spazio dei nomi.

2. Fai clic su **Crea spazio dei nomi**.

3. Immetti un nome di visualizzazione per lo spazio dei nomi e una breve descrizione, ad esempio i tipi di azioni o pacchetti che prevedi di creare in questo spazio dei nomi.

4. Scegli il gruppo di risorse in cui vuoi creare lo spazio dei nomi e un'ubicazione in cui distribuire la risorsa dello spazio dei nomi.

5. Fai clic su **Crea**.

6. Per visualizzare l'istanza del servizio per la risorsa dello spazio dei nomi, vai al [dashboard {{site.data.keyword.cloud_notm}} ](https://cloud.ibm.com/resources){: external} e trova il nome del tuo spazio dei nomi nel riquadro **Spazi dei nomi Functions**.

Se necessario, puoi aggiornare il nome o la descrizione dello spazio dei nomi nella pagina **Impostazioni spazio dei nomi** della console {{site.data.keyword.openwhisk_short}}.

## Creazione di uno spazio dei nomi basato su IAM con la CLI
{: #namespaces_create}

Puoi creare uno spazio dei nomi gestito da IAM come parte di un gruppo di risorse e gestire le politiche di accesso per le tue risorse specificando il gruppo di risorse durante la creazione di uno spazio dei nomi. Se hai altri utenti che richiedono l'accesso al tuo spazio dei nomi o se vuoi accedere ad altre risorse dalle azioni del tuo spazio dei nomi, assicurati di configurare le politiche IAM dopo la creazione del tuo spazio dei nomi.
{: shortdesc}

1. Seleziona il gruppo di risorse in cui vuoi creare lo spazio dei nomi. Se non hai ancora creato un [gruppo di risorse](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puoi selezionare il gruppo predefinito (`default`).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crea uno spazio dei nomi abilitato a IAM. Facoltativo: includi una descrizione per il tuo spazio dei nomi utilizzando l'indicatore `-n` o `--description`. Se la tua descrizione contiene più parole, deve essere racchiusa tra virgolette.

  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description of your namespace">]
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;namespace_name&gt;</code></td>
        <td>Il nome di visualizzazione per lo spazio dei nomi basato su IAM.</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Facoltativo: aggiungi una descrizione allo spazio dei nomi, ad esempio quale tipo di azioni o pacchetti prevedi di creare. Se la descrizione contiene più parole, deve essere racchiusa tra virgolette.</td>
      </tr>
      <tr>
        <td><code>--description &lt;description&gt;</code></td>
        <td>Facoltativo: aggiungi una descrizione allo spazio dei nomi, ad esempio quale tipo di azioni o pacchetti prevedi di creare. Se la descrizione contiene più parole, deve essere racchiusa tra virgolette.</td>
      </tr>
    </tbody>
  </table>

  Output di esempio:

  ```
  ok: created namespace myNamespace
  ```
  {: screen}

3. Verifica che il tuo nuovo spazio dei nomi sia stato creato.

  ```
  ibmcloud fn namespace get <namespace_name_or_id> --properties
  ```
  {: pre}

  Output di esempio:

  ```
  Details of namespace: myNamespace
  Description: short description
  Resource Plan Id: functions-base-plan
  Location: jp-tok
  ID: 05bae599-ead6-4ccb-9ca3-94ce8c8b3e43
  ```
  {: screen}

  Puoi anche elencare tutti gli spazi dei nomi, inclusi quelli basati su IAM e Cloud Foundry:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}

4. Prima di poter creare entità nello spazio dei nomi, devi impostare il contesto della tua CLI sullo spazio dei nomi specificandolo come destinazione.

  ```
  ibmcloud fn property set --namespace <namespace_name_or_id>
  ```
  {: pre}

Dopo aver impostato una proprietà, come la proprietà `--namespace`, questa viene mantenuta finché non ne annulli manualmente l'impostazione. Se vuoi passare da uno spazio dei nomi IAM all'altro o tra Cloud Foundry e IAM, devi annullare l'impostazione della proprietà dello spazio dei nomi e reimpostarla. Per ulteriori informazioni, vedi [`ibmcloud fn property set`]
{: note}

## Creazione di uno spazio dei nomi con l'API
{: #namespaces_create_api}

Puoi creare uno spazio dei nomi gestito da IAM come parte di un gruppo di risorse e gestire le politiche di accesso per le tue risorse specificando il gruppo di risorse durante la creazione di uno spazio dei nomi. Se hai altri utenti che richiedono l'accesso al tuo spazio dei nomi o se vuoi accedere ad altre risorse dalle azioni del tuo spazio dei nomi, assicurati di configurare le politiche IAM dopo la creazione del tuo spazio dei nomi.
{: shortdesc}

1. Seleziona il gruppo di risorse in cui vuoi creare lo spazio dei nomi. Se non hai ancora creato un [gruppo di risorse](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_resource_group_create), puoi selezionare il gruppo predefinito (`default`).

  ```
  ibmcloud target -g default
  ```
  {: pre}

2. Crea uno spazio dei nomi abilitato a IAM.

  ```
  curl --request POST \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>' \
    --data '{"description":"string","name":"string","resource_group_id":"string","resource_plan_id":"string"}'
  ```
  {: pre}

  <table>
    <thead>
      <tr>
        <th colspan=2><img src="images/idea.png" alt="Icona Idea"/> Descrizione dei componenti di questo comando</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>&lt;IAM_token&gt;</code></td>
        <td>Il tuo token Identity and Access Management (IAM) {{site.data.keyword.cloud_notm}}. Per richiamare il tuo token IAM, esegui <code>ibmcloud iam oauth-tokens</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;name&gt;</code></td>
        <td>Il nome dello spazio dei nomi.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_group_id&gt;</code></td>
        <td>L'ID del gruppo di risorse in cui vuoi creare lo spazio dei nomi. Per visualizzare gli ID del gruppo di risorse, esegui <code>ibmcloud resource groups</code>.</td>
      </tr>
      <tr>
        <td><code>-n &lt;resource_plan_id&gt;</code></td>
        <td>L'ID del piano di risorse, ad esempio functions-base-plan</td>
      </tr>
      <tr>
        <td><code>-n &lt;description&gt;</code></td>
        <td>Facoltativo: aggiungi una descrizione allo spazio dei nomi, ad esempio quale tipo di azioni o pacchetti conterrà.</td>
      </tr>
    </tbody>
  </table>

  **Output di esempio**

  ```
  {
    "description": "My new namespace for packages X, Y, and Z.",
      "id": "12345678-1234-abcd-1234-123456789abc",
      "location": "jp-tok",
      "crn": "crn:v1:functions:jp-tok:a/1a22bb3c44dd1a22bb3c44dd1a22:12345678-1234-abcd-1234-123456789abc::",
      "name": "mynamespace",
      "resource_group_id": "1a22bb3c44dd1a22bb3c44dd1a22",
      "resource_plan_id": "functions-base-plan"
    }
  ```
  {: screen}

3. Verifica che il tuo nuovo spazio dei nomi sia stato creato.

  ```
  curl --request GET \
      --url 'https://us-south.functions.cloud.ibm.com/api/servicebroker/api/v1/namespaces/{id} \
      --header 'accept: application/json' \
      --header 'authorization: <IAM_token>'
  ```
  {: pre}

  Puoi anche elencare tutti gli spazi dei nomi, inclusi quelli basati su IAM e Cloud Foundry:
  ```
  curl --request GET \
    --url 'https://jp-tok.functions.cloud.ibm.com/api/v1/namespaces?limit=0&offset=0' \
    --header 'accept: application/json' \
    --header 'authorization: <IAM_token>'
  ```
  {: pre}

  **Output di esempio**

  ```
  {
    "limit": 10,
      "offset": 0,
      "total_Count": 2,
      "namespaces": [
        {
        "id": "12345678-1234-abcd-1234-123456789abc",
          "location": "jp-tok"
      },
      {
        "id": "BobsOrg_dev",
          "classic_type": 1,
          "location": "jp-tok"
        }
    ]
  }
  ```
  {: screen}

Per ulteriori informazioni sull'utilizzo di REST HTTP, consulta la [documentazione API {{site.data.keyword.openwhisk_short}}](/apidocs/functions).
{: tip}

### Accesso ad altre risorse da uno spazio dei nomi
{: #namespace-access}

Le azioni in genere richiamano altre risorse e servizi {{site.data.keyword.cloud_notm}} che richiedono un'autenticazione appropriata. Se questi servizi sono abilitati a IAM e accettano i token IAM, puoi sfruttare l'ID funzionale dello spazio dei nomi per le comunicazioni in uscita.
{: shortdesc}

Come descritto in [Gestione dell'accesso IAM](/docs/iam?topic=iam-iammanidaccser#iammanidaccser), per ciascuno spazio dei nomi, viene creato un ID servizio che rappresenta lo spazio dei nomi. Puoi concedere l'accesso ad altri servizi e risorse per questo ID servizio assegnando i ruoli appropriati utilizzando la gestione delle politiche IAM. Per ulteriori informazioni sulla creazione di ID servizio per accedere ad altri servizi abilitati a IAM, vedi [Creazione e gestione degli ID servizio](/docs/iam?topic=iam-serviceids#serviceids).

Durante il runtime, {{site.data.keyword.openwhisk_short}} passa una chiave API dell'ID servizio dello spazio dei nomi al codice dell'azione come variabile di ambiente `__OW_IAM_NAMESPACE_API_KEY`. Il codice dell'azione può utilizzare questa chiave API per generare un token IAM. La maggior parte degli SDK {{site.data.keyword.openwhisk_short}} supportati, quali Cloudant, {{site.data.keyword.watson}} e {{site.data.keyword.cos_full_notm}} eseguono l'autenticazione con la chiave API IAM stessa. Per altri servizi o risorse gestiti da IAM che utilizzano un'API REST, puoi eseguire l'autenticazione con il token derivato dalla chiave API IAM. Per ulteriori informazioni, consultare [Crea un token di accesso IAM per un ID utente o di servizio](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i).

Non sei sicuro di come le chiavi API e i token si integrano tra loro? Trova ulteriori informazioni nella [documentazione IAM](/docs/iam?topic=iam-iamapikeysforservices).

## Passi successivi
{: #namespaces_next}

Ora che hai creato uno spazio dei nomi, puoi creare le politiche di accesso IAM per proteggerlo. Per iniziare, consulta [Gestione dell'accesso](/docs/openwhisk?topic=cloud-functions-iam). 

Per ulteriori informazioni su come gestire gli spazi dei nomi basati su IAM, vedi la [Guida di riferimento API REST {{site.data.keyword.openwhisk_short}}](/apidocs/functions).








