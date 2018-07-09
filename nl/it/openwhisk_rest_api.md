---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Utilizzo delle API REST OpenWhisk
{: #openwhisk_rest_api}

Una volta abilitato il tuo ambiente OpenWhisk, puoi utilizzarlo con le tue applicazioni web o mobili mediante le chiamate API REST.
{: shortdesc}

Per ulteriori dettagli sull'utilizzo delle API per azioni, attivazioni, pacchetti, regole e trigger, consulta la [documentazione API OpenWhisk](http://petstore.swagger.io/?url=https://raw.githubusercontent.com/openwhisk/openwhisk/master/core/controller/src/main/resources/apiv1swagger.json).


Tutte le capacità del sistema sono disponibili mediante un'API REST. Gli endpoint di raccolta ed entità sono disponibili per azioni, trigger, regole, pacchetti, attivazioni e spazi dei nomi.

Endpoint di raccolta disponibili:
- `https://{APIHOST}/api/v1/namespaces`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations`

`{APIHOST}` è il nome host dell'API OpenWhisk (ad esempio, openwhisk.ng.bluemix.net, 172.17.0.1, 192.168.99.100, 192.168.33.13 e così via).
Per `{namespace}`, è possibile utilizzare il carattere `_` per specificare lo *spazio dei nomi
predefinito* dell'utente.

Puoi effettuare una richiesta GET sugli endpoint di raccolta per richiamare un elenco di entità della raccolta.

Per ciascun tipo di entità sono disponibili i seguenti endpoint:
- `https://{APIHOST}/api/v1/namespaces/{namespace}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/actions/[{packageName}/]{actionName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/triggers/{triggerName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/rules/{ruleName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/packages/{packageName}`
- `https://{APIHOST}/api/v1/namespaces/{namespace}/activations/{activationName}`

Gli endpoint di spazio dei nomi e attivazione supportano le richieste GET. Gli endpoint di azioni, trigger, regole e pacchetti supportano le richieste GET, PUT e DELETE. Gli endpoint di azioni, trigger e regole supportano anche le richieste POST, che vengono utilizzate per richiamare azioni e trigger e per abilitare o disabilitare le regole. 

Tutte le API sono protette tramite autenticazione base HTTP.
Puoi utilizzare lo strumento [wskadmin ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://github.com/apache/incubator-openwhisk/tree/master/tools/admin) per generare un nuovo spazio dei nomi e autenticazione.
Le credenziali per l'autenticazione di base si trovano nella proprietà `AUTH` all'interno del file `~/.wskprops`, delimitate da due punti.
Puoi anche recuperare queste credenziali con la CLI eseguendo `wsk property get --auth`.


Nel seguente esempio, lo strumento di comando [cURL](https://curl.haxx.se) viene utilizzato per ottenere l'elenco di tutti i pacchetti nello spazio dei nomi `whisk.system`:
```bash
curl -u USERNAME:PASSWORD https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/packages
```
{: pre}

```json
[
  {
    "name": "slack",
    "binding": false,
    "publish": true,
    "annotations": [
      {
        "key": "description",
        "value": "Package that contains actions to interact with the Slack messaging service"
      }
    ],
    "version": "0.0.1",
    "namespace": "whisk.system"
  }
]
```

In questo esempio, l'autenticazione è stata passata utilizzando l'indicatore `-u`. Puoi passare questo valore anche come parte dell'URL, ad esempio, `https://$AUTH@{APIHOST}`.

L'API OpenWhisk supporta chiamate di richiesta-risposta dai client web. OpenWhisk risponde alle richieste `OPTIONS` con le intestazioni Cross-Origin Resource Sharing. Al momento, sono consentite tutte le origini (ad esempio, Access-Control-Allow-Origin è "`*`") e Access-Control-Allow-Headers produce Authorization e Content-Type.

**Attenzione:** poiché OpenWhisk attualmente supporta solo una chiave per spazio dei nomi, non è consigliabile utilizzare CORS al di là di semplici esperimenti. Utilizza le [Azioni web](./openwhisk_webactions.html) o il [Gateway API](./openwhisk_apigateway.html) per esporre le tue azioni al pubblico e non utilizzare la chiave di autorizzazione OpenWhisk per le applicazioni client che richiedono CORS.

## Utilizzo della modalità dettagliata della CLI
{: #openwhisk_rest_api_cli_v}

La CLI OpenWhisk è un'interfaccia all'API REST OpenWhisk.
Puoi eseguire la CLI in modalità dettagliata con l'indicatore `-v`, che stampa tutte le informazioni su richiesta e risposta HTTP.

Visualizza il valore dello spazio dei nomi per l'utente corrente immettendo il seguente comando:
```
wsk namespace list -v
```
{: pre}

```
REQUEST:
[GET]	https://openwhisk.ng.bluemix.net/api/v1/namespaces
Req Headers
{
  "Authorization": [
    "Basic XXXYYYY"
  ],
  "User-Agent": [
    "OpenWhisk-CLI/1.0 (2017-08-10T20:09:30+00:00)"
  ]
}
RESPONSE:Got response with code 200
Resp Headers
{
  "Content-Type": [
    "application/json; charset=UTF-8"
  ]
}
Response body size is 28 bytes
Response body received:
["john@example.com_dev"]
```

Le informazioni stampate forniscono le proprietà della richiesta HTTP ed eseguono un metodo HTTP `GET` sull'URL `https://openwhisk.ng.bluemix.net/api/v1/namespaces` utilizzando un'intestazione User-Agent `OpenWhisk-CLI/1.0 (<CLI-Build-version>)` e un'intestazione Basic Authorization `Basic XXXYYYY`.
Nota che il valore di autorizzazione è la tua stringa di autorizzazione OpenWhisk codificata in base64.
La risposta è del tipo di contenuto `application/json`.

## Azioni
{: #openwhisk_rest_api_actions}

Per creare o aggiornare un'azione, invia una richiesta HTTP con il metodo `PUT` sulla raccolta di azioni. Ad esempio, per creare un'azione `nodejs:6` con il nome `hello` utilizzando il contenuto di un unico file, usa il seguente comando:
```bash
curl -u $AUTH -d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"}}' -X PUT -H "Content-Type: application/json" https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true 
```
{: pre}

Per eseguire una chiamata bloccante su un'azione, inviare una richiesta HTTP con un metodo `POST` e il corpo che contiene il parametro di input `name`, utilizza il seguente comando:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}'  
```
{: pre}

Ricevi la seguente risposta:
```json
{
  "duration": 2,
  "name": "hello",
  "subject": "john@example.com_dev",
  "activationId": "c7bb1339cb4f40e3a6ccead6c99f804e",
  "publish": false,
  "annotations": [{
    "key": "limits",
    "value": {
      "timeout": 60000,
      "memory": 256,
      "logs": 10
    }
  }, {
    "key": "path",
    "value": "john@example.com_dev/hello"
  }],
  "version": "0.0.1",
  "response": {
    "result": {
      "payload": "Hello John"
    },
    "success": true,
    "status": "success"
  },
  "end": 1493327653769,
  "logs": [],
  "start": 1493327653767,
  "namespace": "john@example.com_dev"
}
```
Per ottenere `response.result`, esegui di nuovo il comando con il parametro di query `result=true` come nel seguente esempio:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?blocking=true&result=true" \
-X POST -H "Content-Type: application/json" \
-d '{"name":"John"}' 
```
{: pre}
Ricevi la seguente risposta:
```json
{
  "payload": "hello John"
}
```

## Annotazioni e azioni web
{: #openwhisk_rest_api_webactions}

Per creare un'azione come azione web, devi aggiungere l'[annotazione](./openwhisk_annotations.html) `web-export=true` per le azioni web. Poiché le azioni web sono accessibili pubblicamente, puoi proteggere i parametri predefiniti (ovvero, considerarli come finali) utilizzando l'annotazione `final=true`. Se crei o aggiorni un'azione che utilizza l'indicatore della CLI `--web true`, il comando aggiunge entrambe le annotazioni `web-export=true` e `final=true`.

Immetti il seguente comando curl per fornire l'elenco completo di annotazioni da impostare sull'azione.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/hello?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"hello","exec":{"kind":"nodejs:6","code":"function main(params) { return {payload:\"Hello \"+params.name}}"},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}'
```
{: pre}

Puoi ora richiamare questa azione come URL pubblico senza alcuna autorizzazione OpenWhisk. Prova a richiamare utilizzando l'URL pubblico dell'azione web e includendo un'estensione come `.json` o `.http`, ad esempio, alla fine dell'URL.
```bash
curl https://openwhisk.ng.bluemix.net/api/v1/web/john@example.com_dev/default/hello.json?name=John
```
{: pre}

```json
{
  "payload": "Hello John"
}
```

Questo codice sorgente di esempio non funziona con `.http`; vedi la documentazione [Azioni web](./openwhisk_webactions.html) per informazioni su come modificarlo.

## Sequenze
{: #openwhisk_rest_api_sequences}

Per creare una sequenza di azioni, fornisci i nomi delle azioni che compongono la sequenza nell'ordine desiderato, in modo che l'output della prima azione venga passato come input all'azione successiva.

$ wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort

Crea una sequenza con le azioni `/whisk.system/utils/split` e `/whisk.system/utils/sort`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/actions/sequenceAction?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"sequenceAction","exec":{"kind":"sequence","components":["/whisk.system/utils/split","/whisk.system/utils/sort"]},"annotations":[{"key":"web-export","value":true},{"key":"raw-http","value":false},{"key":"final","value":true}]}' 
```
{: pre}

I nomi che specifichi per le azioni devono essere completi.

## Trigger
{: #openwhisk_rest_api_triggers}

Per creare un trigger, l'informazione minima che ti serve è un nome per il trigger. Potresti anche includere i parametri predefiniti che vengono passati all'azione tramite una regola quando il trigger viene attivato.

Crea un trigger denominato `events` con un parametro predefinito `type` con il valore `webhook` impostato.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"events","parameters":[{"key":"type","value":"webhook"}]}' 
```
{: pre}

Ora ogni volta che hai un evento che deve attivare questo trigger, viene effettuata solo una richiesta HTTP con un metodo `POST` utilizzando la chiave di autorizzazione OpenWhisk.

Per attivare il trigger `events` con un parametro `temperature`, invia la seguente richiesta HTTP.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/events \
-X POST -H "Content-Type: application/json" \
-d '{"temperature":60}' 
```
{: pre}

### Trigger con azioni di feed
{: #openwhisk_rest_api_triggers_feed}

È possibile creare trigger speciali utilizzando un'azione di feed. L'azione di feed è un'azione che aiuta nella configurazione di un provider di feed che ha il compito di attivare il trigger ogni volta che si verifica un evento per il trigger. Per ulteriori informazioni su questi provider di feed, consulta la documentazione [feeds.md].

Alcuni dei trigger disponibili che sfruttano un'azione di feed sono periodic/alarms, Slack, Github, Cloudant/Couchdb e messageHub/Kafka. Puoi anche creare la tua propria azione di feed e il tuo provider di feed.

Crea un trigger denominato `periodic` da attivare a una frequenza specificata, ogni 2 ore (ad esempio, 02:00:00, 04:00:00,...).

Utilizzando la CLI, immetti il seguente comando per creare il trigger:
```bash
wsk trigger create periodic --feed /whisk.system/alarms/alarm \
  --param cron "0 */2 * * *" -v
```
{: pre}

Poiché è utilizzato l'indicatore `-v`, vengono inviate due richieste HTTP. Una è per creare un trigger chiamato `periodic` e l'altra per richiamare l'azione di feed. L'azione di feed `/whisk.system/alarms/alarm`, viene inviata con i parametri per configurare il provider di feed per l'attivazione del trigger ogni 2 ore.

Per ottenere questa attività con l'API REST, crea prima il trigger in questo modo:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"periodic","annotations":[{"key":"feed","value":"/whisk.system/alarms/alarm"}]}'  
```
{: pre}

Come puoi vedere, l'annotazione `Feed` è memorizzata nel trigger. Successivamente, questa annotazione può essere utilizzata per sapere quale azione di feed utilizzare per eliminare il trigger.

Adesso che il trigger è stato creato, richiama l'azione di feed
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"cron\":\"0 */2 * * *\",\"lifecycleEvent\":\"CREATE\",\"triggerName\":\"/_/periodic\"}" 
```
{: pre}

Il processo di eliminazione del trigger è simile a quello della sua creazione. Quindi, questa volta, elimina il trigger utilizzando l'azione di feed per configurare il provider di feed in modo da eliminare anche il gestore per il trigger.

Richiama l'azione di feed per eliminare il gestore del trigger dal provider di feed immettendo il seguente comando:
```bash
curl -u $AUTH "https://openwhisk.ng.bluemix.net/api/v1/namespaces/whisk.system/actions/alarms/alarm?blocking=true&result=false" \
-X POST -H "Content-Type: application/json" \
-d "{\"authKey\":\"$AUTH\",\"lifecycleEvent\":\"DELETE\",\"triggerName\":\"/_/periodic\"}"  
```
{: pre}

Adesso elimina il trigger con una richiesta HTTP utilizzando il metodo `DELETE`:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/triggers/periodic \
-X DELETE -H "Content-Type: application/json" 
```
{: pre}

## Regole
{: #openwhisk_rest_api_rules}

Per creare una regola che associa un trigger a un'azione, invia una richiesta HTTP con un metodo `PUT` per fornire il trigger e l'azione nel corpo della richiesta.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"name":"t2a","status":"","trigger":"/_/events","action":"/_/hello"}' 
```
{: pre}

Le regole possono essere abilitate o disabilitate e puoi modificare lo stato della regola aggiornando la sua proprietà stato. Ad esempio, per disabilitare la regola `t2a`, invia `status: "inactive"` nel corpo della richiesta con un metodo `POST`.
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/rules/t2a?overwrite=true \
-X POST -H "Content-Type: application/json" \
-d '{"status":"inactive","trigger":null,"action":null}'  
```
{: pre}

## Pacchetti
{: #openwhisk_rest_api_packages}

Per creare un'azione in un pacchetto, devi prima creare il pacchetto. Crea un pacchetto con il nome `iot` e invia una richiesta HTTP con un metodo `PUT` immettendo il seguente comando:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/packages/iot?overwrite=true \
-X PUT -H "Content-Type: application/json" \
-d '{"namespace":"_","name":"iot"}' 
```
{: pre}

## Attivazioni
{: #openwhisk_rest_api_activations}

Per ottenere l'elenco delle ultime tre attivazioni, utilizza una richiesta HTTP con un metodo `GET` passando il parametro di query `limit=3` come nel seguente esempio:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations?limit=3
```
{: pre}

Per ottenere tutti i dettagli di un'attivazione che includono i risultati e i log, invia una richiesta HTTP con un metodo `GET` passando l'identificativo di attivazione come parametro di percorso come nel seguente esempio:
```bash
curl -u $AUTH https://openwhisk.ng.bluemix.net/api/v1/namespaces/_/activations/f81dfddd7156401a8a6497f2724fec7b
```
{: pre}
