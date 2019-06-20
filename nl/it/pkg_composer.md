---

copyright:
years: 2017, 2019
lastupdated: "2019-05-15"

keywords: composer, openwhisk, compositions, sequence, branch

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

# Composer
{: #pkg_composer}

{{site.data.keyword.openwhisk}} ora supporta Composer per Apache OpenWhisk come un'anteprima tecnica. Composer estende le sequenze Apache OpenWhisk con ulteriori combinatori ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md), [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)), che ti permettono di creare flussi più complessi che includono loop, gestione dell'errore e diramazione condizionale. La documentazione completa e le specifiche tecniche per Composer sono disponibili nel [repository Composer Git](https://github.com/apache/incubator-openwhisk-composer).

Puoi anche utilizzare il progetto open source [Kui](https://github.com/ibm/kui) per rendere più semplice creare, distribuire e visualizzare il tuo codice di origine Composer. Vedi [Kui on Github](https://github.com/ibm/kui) per ulteriori informazioni su come utilizzare Kui con Composer.
{: note}

## Installazione della libreria Composer per JavaScript o Python 3
{: #install_composer}

Puoi utilizzare Composer con le azioni scritte in qualsiasi linguaggio, ma devi indicare la composizione in JavaScript o Python 3. Dopo l'installazione puoi utilizzare i comandi `compose/pycompose` e `deploy/pydeploy` Composer per [configurare ed eseguire una composizione](#run).
{: shortdesc}

**Per JavaScript**:
1. Installa il [pacchetto Node.js](https://github.com/apache/incubator-openwhisk-composer) della libreria Composer utilizzando il npm (Node Package Manager).

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Conferma di aver installato la libreria eseguendo help per i comandi Composer.

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    Output di esempio:
    ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Per Python 3**:
Installa la libreria [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python) utilizzando `pip3`.

1.  Clona il repository GitHub Composer for Python 3.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  Passa alla directory composer.
    ```
    cd composer-python
    ```
    {: pre}
3.  Installa la libreria Composer. Includi il punto (`.`) in modo che il comando cerchi nella directory in cui sei.
    ```
    pip3 install -e .
    ```
    {: pre}
4.  Conferma di aver installato la libreria eseguendo help per i comandi Composer.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    Output di esempio:
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## Configurazione ed esecuzione delle composizioni in IBM Cloud Functions
{: #run}

Puoi utilizzare le librerie Composer JavaScript o Python 3 per creare le tue composizioni in {{site.data.keyword.openwhisk}}. Utilizza `compose` o `pycompose` per compilare il tuo codice di origine della composizione, quindi utilizza `deploy` o `pydeploy` per distribuire la tua composizione a {{site.data.keyword.openwhisk}}. Dopo aver configurato la composizione, puoi eseguirla in {{site.data.keyword.openwhisk}}.
{: shortdesc}

**Prima di cominciare**:
Per impostazione predefinita, le distribuzioni utilizzano i valori impostati in `~/.wskprops`. Sovrascrivi il valore predefinito impostando due parametri con input per il comando `deploy` o `pydeploy` Composer.

1.  Imposta l'host API sull'endpoint {{site.data.keyword.openwhisk}}.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  Aggiungi la tua chiave di autenticazione CLI `wsk`. 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**Per eseguire una composizione**:

1.  Crea un codice di origine Composer con le librerie NodeJS o Python 3. Ad esempio, crea un file `demo.js`.
2.  Compila il codice di origine Composer in un file JSON.
    *   In JavaScript:
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   In Python 3:
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  Distribuisci il codice a {{site.data.keyword.openwhisk}}.
    *   In JavaScript: includi l'indicatore `-w` per sovrascrivere tutte le distribuzioni esistenti denominate `demo`.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   In Python 3: includi l'indicatore `-w` per sovrascrivere tutte le distribuzioni esistenti denominate `demo`.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  Esegui la composizione nello stesso modo in cui [richiami altre azioni](/docs/openwhisk?topic=cloud-functions-triggers) in {{site.data.keyword.openwhisk}}.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} esegue il codice che hai distribuito come un tipo di azione speciale. Per ulteriori dettagli, vedi la documentazione su [conductor actions](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md).

## Estensione delle sequenze con Composer
{: #extending}

Apache OpenWhisk ti consente di concatenare le funzioni tra loro in una sequenza (`sequence`), in cui l'output di un'azione diventa l'input di un'altra azione.

### Sequenze senza Composer
{: #sequences-without-composer}
Puoi concatenare tra loro due funzioni denominate `action1` e `action2` in {{site.data.keyword.openwhisk_short}}:

`ibmcloud fn action create --sequence mysequence action1 action2`.

Il risultato di questo comando è una funzione denominata `mysequence`, che è un composito di `action1` e `action2`.  Puoi utilizzare `mysequence` allo stesso modo di qualsiasi altra funzione in OpenWhisk.

### Sequenze con Composer
{: #sequences-with-composer}
In Composer, puoi specificare sequenze più articolate utilizzando il codice di origine invece della riga di comando.

Per JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

Per Python 3:
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="Sequenza semplice" alt="Sequenza con due azioni" style="width:250px; border-style: none"/></br> _Figura 1. Sequenza con due azioni_

Non hai limiti per la concatenazione tra loro delle funzioni in Composer. Composer include una famiglia di combinatori basati su [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) o [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md) che migliorano l'espressività delle sequenze. Puoi vedere degli esempi comuni nelle seguenti sezioni.

### Gestione degli errori
{: #error-handling}
Puoi aggiungere la gestione dell'errore in una sequenza utilizzando i blocchi `try-catch-finally`. In questo esempio, racchiudi la sequenza con try. Il codice `handleError` viene eseguito se l'azione restituisce un errore.

Per JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

Per Python 3:
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Sequenza try" alt="Sequenza con gestione dell'errore" style="width:400px; border-style: none"/></br>
_Figura 2. Sequenza con gestione dell'errore_

### Diramazione condizionale
{: #conditional-branch}
Puoi creare una sequenza diramata utilizzando `if-then-else`. Questo esempio illustra un `if-then-else`. `action1` deve restituire un valore booleano. Se è `true`, viene poi eseguito `action2`, altrimenti `action3`. Tieni presente che `action3` è facoltativo e può essere omesso per `if-then`.

Per JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

Per Python 3:
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="Sequenza if" alt="Sequenza con diramazione condizionale" style="width:250px; border-style: none"/></br>
_Figura 3. Sequenza con diramazione condizionale_

### Loop
{: #loop}
Puoi creare dei costrutti di loop in Composer. In questo esempio, `action2` viene eseguito finché `action1` restituisce `true`. Composer limita il numero totale di passi che puoi eseguire in una sequenza composta. Il limite corrente è 20.

Per JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

Per Python 3:
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="Sequenza while" alt="Sequenza con loop while" style="width:250px; border-style: none"/></br>
_Figura 4. Sequenza con loop `while`_

### Definizione incorporata delle azioni
{: #inline-def}
Puoi definire le azioni all'interno del codice di composizione stesso. In questo esempio, crei la definizione incorporata dell'azione con la composizione denominata `hello` utilizzando `composer.action()`.

Per JavaScript:
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

Per Python 3:
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="Sequenza while" alt="Sequenza con definizione dell'azione incorporata" style="width:250px; border-style: none"/></br>
_Figura 5. Sequenza con definizione dell'azione incorporata_

## Utilizzo di altre definizioni del combinatore
{: #combinator-def}
Vedi la documentazione di Composer su Apache OpenWhisk ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) o [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)) per l'elenco completo delle definizioni dei combinatori.

