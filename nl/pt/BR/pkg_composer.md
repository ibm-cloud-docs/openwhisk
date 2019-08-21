---

copyright:
years: 2017, 2019
lastupdated: "2019-07-12"

keywords: composer, openwhisk, compositions, sequence, branch, functions

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


# Composer
{: #pkg_composer}

O {{site.data.keyword.openwhisk}} agora suporta o Composer for Apache OpenWhisk como uma visualização técnica. O Composer amplia as sequências do Apache OpenWhisk com mais combinadores ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external}, [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md){: external}), que é possível usar para criar fluxos mais complexos que incluem ramificação condicional, manipulação de erros e loops. A documentação completa e as especificações técnicas para o Composer estão disponíveis no [Repositório Git do Composer](https://github.com/apache/incubator-openwhisk-composer){: external}.

Também é possível usar o projeto de software livre [Kui](https://github.com/ibm/kui){: external} para tornar mais fácil criar, implementar e visualizar seu código-fonte do Composer. Para obter mais informações, consulte [Kui no GitHub](https://github.com/ibm/kui){: external}.
{: note}

## Instalando a biblioteca do Composer para JavaScript ou Python 3
{: #install_composer}

É possível usar o Composer com ações que são gravadas em qualquer idioma, mas você expressa a composição em JavaScript ou Python 3. Após a instalação, será possível usar os comandos do Composer `compose/pycompose` e `deploy/pydeploy` para [configurar e executar uma composição](#run).
{: shortdesc}

**Para JavaScript**
1. Instale a biblioteca do Composer [pacote Node.js](https://github.com/apache/incubator-openwhisk-composer){: external} usando o Node Package Manager.

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Confirme que você instalou a biblioteca, executando a ajuda para os comandos do Composer.

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    **Saída de exemplo**
    ```
    Uso:
        compor composition.js [ sinalizadores ]

    Uso:
        deploy composition composition.json [flags]
    ```
    {: screen}

**Para Python 3** Instale a biblioteca [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python){: external} usando `pip3`.

1.  Clone o Composer para o repositório GitHub do Python 3.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}

2.  Navegue para o diretório do Composer.
    ```
    cd composer-python
    ```
    {: pre}

3.  Instale a biblioteca do Composer. Inclua o ponto (`.`) para que o comando olhe dentro do diretório no qual você está.
    ```
    pip3 install -e .
    ```
    {: pre}

4.  Confirme que você instalou a biblioteca, executando a ajuda para os comandos do Composer.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    **Saída de exemplo**
    ```
    uso: pycompose composition.py command [flags]
    uso: pydeploy composition composition.json [flags]
    ```
    {: screen}

## Configurando e executando composições em IBM Cloud Functions
{: #run}

É possível usar as bibliotecas do Composer JavaScript ou Python 3 para criar suas composições no {{site.data.keyword.openwhisk}}. Use `compose` ou `pycompose` para compilar seu código-fonte de composição, em seguida, use `deploy` ou `pydeploy` para implementar a composição no {{site.data.keyword.openwhisk}}. Depois de configurar a composição, é possível executá-lo no {{site.data.keyword.openwhisk}}.
{: shortdesc}

**Antes de iniciar** Por padrão, as implementações usam os valores configurados em `~/.wskprops`. Substitua o padrão configurando dois parâmetros como entrada para o comando Composer `deploy` ou `pydeploy`.

1.  Configure o host da API para o terminal do {{site.data.keyword.openwhisk}}.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  Inclua a chave de autenticação da CLI `wsk`. 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**Para executar uma composição**

1.  Crie o código-fonte do Composer com as bibliotecas nodeJS ou Python 3. Por exemplo, crie um arquivo `demo.js`.
2.  Compile o código-fonte do Composer em um arquivo JSON.
    *   **JavaScript**
        ```
        compõe demo.js >demo.json
        ```
        {: pre}
    *   **Python 3**
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  Implemente o código no {{site.data.keyword.openwhisk}}.
    *   Em JavaScript, inclua o sinalizador `-w` para sobrescrever qualquer implementação existente que seja denominada `demo`.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   No Python 3, inclua o sinalizador `-w` para sobrescrever qualquer implementação existente que seja denominada `demo`.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  Execute a composição da mesma maneira que você [chama outras ações](/docs/openwhisk?topic=cloud-functions-triggers) no {{site.data.keyword.openwhisk}}.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

O {{site.data.keyword.openwhisk}} executa o código que você implementou como uma ação especial. Para obter mais informações, consulte a documentação sobre [ações do condutor](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md){: external}.

## Estendendo sequências com o Composer
{: #extending}

Com o Apache OpenWhisk, é possível encadear funções juntas em uma `sequence`, em que a saída de uma ação se torna a entrada para outra ação.

### Sequências sem o Composer
{: #sequences-without-composer}
É possível encadear juntas duas funções que são denominadas `action1` e `action2` no {{site.data.keyword.openwhisk_short}}:

```
ibmcloud fn action create --sequence mysequence action1 action2
```
{: pre}

O resultado desse comando é uma função que é chamada `mysequence`, que é um composto de `action1` e `action2`.  É possível usar `mysequence` da mesma maneira que qualquer função no OpenWhisk.

### Sequências com Composer
{: #sequences-with-composer}
No Composer, é possível especificar sequências mais ricas usando o código-fonte em vez da linha de comandos.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}

</br>
<img src="images/composer-sequence.png" width="35%" title="Sequência simples" alt="Sequência com duas ações" style="width:250px; border-style: none"/></br>
_Figura 1. Sequência com duas ações_

Você não está limitado a encadear funções no Composer. O Composer inclui uma família de combinadores baseados em [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external} ou [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md){: external} que aprimoram a expressividade de sequências. É possível ver exemplos comuns nas seções a seguir.

### Manipulação de erros
{: #error-handling}
É possível incluir a manipulação de erros em uma sequência usando blocos `try-catch-finally`. Neste exemplo, você cerca a sequência com uma tentativa. O código `handleError`é executado se a ação retornar um erro.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}

</br>
<img src="images/composer-error.png" width="400" title="Tentar sequência" alt="Sequência com manipulação de erros" style="width:400px; border-style: none"/></br>
_Figura 2. Sequência com manipulação de erros_

### Ramificação condicional
{: #conditional-branch}
É possível criar uma sequência ramificada usando `if-then-else`. Este exemplo demonstra um `if-then-else`. `action1` retorna um booleano. Se `true`, então `action2` será executado, caso contrário, `action3` será executado. Observe que `action3` é opcional e pode ser omitido para `if-then`.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.if ('action1', 'action2', 'action3')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="Sequência If" alt="Sequência com ramificação condicional" style="width:250px; border-style: none"/></br>
_Figura 3. Sequência com ramificação condicional_

### Loops
{: #loop}
É possível criar construções de looping no Composer. Neste exemplo, `action2` é executado desde que `action1` retorne `true`. O Composer limita o número total de etapas que podem ser executadas em uma sequência composta. O limite atual é 20.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="Sequência While" alt="Sequência com loop while" style="width:250px; border-style: none"/></br>
_Figura 4. Sequência com loop `while`_

### Definição sequencial de ações
{: #inline-def}
É possível definir ações dentro do próprio código de composição. Neste exemplo, você cria a definição de ação sequencial com a composição que é denominada `hello` usando `composer.action()`.

**Para JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

**Para Python 3**
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}

</br>
<img src="images/composer-inline.png" width="250" title="Sequência While" alt="Sequência com definição de ação sequencial" style="width:250px; border-style: none"/></br>
_Figura 5. Sequência com definição de ação sequencial_

## Usando outras definições de combinador
{: #combinator-def}
Consulte a documentação para o Composer no Apache OpenWhisk ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external} ou [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md){: external}) para obter a lista completa de definições de combinadores.






