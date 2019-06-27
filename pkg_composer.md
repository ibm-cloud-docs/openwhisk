---

copyright:
years: 2017, 2019
lastupdated: "2019-06-27"

keywords: composer, openwhisk, compositions, sequence, branch, functions

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

{{site.data.keyword.openwhisk}} now supports Composer for Apache OpenWhisk as a technical preview. Composer extends Apache OpenWhisk sequences with more combinators ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external}, [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md){: external}), which you can use to create more complex flows that include conditional branching, error handling, and loops. Full documentation and technical specifications for Composer are available on the [Composer Git repository](https://github.com/apache/incubator-openwhisk-composer){: external}.

You can also use the open source project [Kui](https://github.com/ibm/kui){: external} to make it easier to create, deploy, and visualize your Composer source code. For more information, see [Kui on GitHub](https://github.com/ibm/kui){: external}.
{: note}

## Installing the Composer library for JavaScript or Python 3
{: #install_composer}

You can use Composer with actions that are written in any language, but you express the composition in either JavaScript or Python 3. After the installation, you can use the Composer `compose/pycompose` and `deploy/pydeploy` commands to [configure and run a composition](#run).
{: shortdesc}

**For JavaScript**
1. Install the Composer library [Node.js package](https://github.com/apache/incubator-openwhisk-composer){: external} by using the Node Package Manager.

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Confirm that you installed the library by running help for the Composer commands.

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    **Example output**
    ```
    Usage:
        compose composition.js [flags]

    Usage:
        deploy composition composition.json [flags]
    ```
    {: screen}

**For Python 3**
Install the [Composer for Python 3](https://github.com/apache/incubator-openwhisk-composer-python){: external} library by using `pip3`.

1.  Clone the Composer for Python 3 GitHub repo.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}

2.  Navigate to the composer directory.
    ```
    cd composer-python
    ```
    {: pre}

3.  Install the Composer library. Include the period (`.`) so that the command looks within the directory that you are in.
    ```
    pip3 install -e .
    ```
    {: pre}

4.  Confirm that you installed the library by running help for the Composer commands.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    **Example output**
    ```
    usage: pycompose composition.py command [flags]
    usage: pydeploy composition composition.json [flags]
    ```
    {: screen}

## Configuring and running compositions in IBM Cloud Functions
{: #run}

You can use the JavaScript or Python 3 Composer libraries to create your compositions in {{site.data.keyword.openwhisk}}. Use `compose` or `pycompose` to compile your composition source code, then use `deploy` or `pydeploy` to deploy the composition to {{site.data.keyword.openwhisk}}. After you configure the composition, you can run it in {{site.data.keyword.openwhisk}}.
{: shortdesc}

**Before you begin**
By default, deployments use the values set in `~/.wskprops`. Override the default by setting two parameters as input for the Composer `deploy` or `pydeploy` command.

1.  Set the API host to the {{site.data.keyword.openwhisk}} endpoint.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  Add your `wsk` CLI authentication key. 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**To run a composition**

1.  Create Composer source code with the nodeJS or Python 3 libraries. For example, create a `demo.js` file.
2.  Compile the Composer source code into a JSON file.
    *   **JavaScript**
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   **Python 3**
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  Deploy the code to {{site.data.keyword.openwhisk}}.
    *   In JavaScript, include the `-w` flag to overwrite any existing deployment that is named `demo`.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   In Python 3, include the `-w` flag to overwrite any existing deployment that is named `demo`.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  Execute the composition in the same way you [invoke other actions](/docs/openwhisk?topic=cloud-functions-triggers) in {{site.data.keyword.openwhisk}}.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} executes the code that you deployed as a special action. For more information, see the documentation on [conductor actions](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md){: external}.

## Extending sequences with Composer
{: #extending}

With Apache OpenWhisk, you can chain functions together in a `sequence`, where the output of one action becomes the input to another action.

### Sequences without Composer
{: #sequences-without-composer}
You can chain together two functions that are named `action1` and `action2` in  {{site.data.keyword.openwhisk_short}}:

```
ibmcloud fn action create --sequence mysequence action1 action2
```
{: pre}

The result of this command is a function that is called `mysequence`, which is a composite of `action1` and `action2`.  You can use `mysequence` in the same manner as any function in OpenWhisk.

### Sequences with Composer
{: #sequences-with-composer}
In Composer, you can specify richer sequences by using source code rather than the command line.

**For JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

**For Python 3**
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}

</br>
<img src="images/composer-sequence.png" width="35%" title="Simple Sequence" alt="Sequence with two actions" style="width:250px; border-style: none"/></br>
_Figure 1. Sequence with two actions_

You aren't limited to chaining together functions in Composer. Composer includes a family of [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external} or [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md){: external} based combinators that enhance the expressiveness of sequences. You can see common examples in the following sections.

### Error handling
{: #error-handling}
You can add error handling to a sequence by using `try-catch-finally` blocks. In this example, you surround the sequence with a try. The `handleError` code executes if either action returns an error.

**For JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

**For Python 3**
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}

</br>
<img src="images/composer-error.png" width="400" title="Try Sequence" alt="Sequence with error handling" style="width:400px; border-style: none"/></br>
_Figure 2. Sequence with error handling_

### Conditional branching
{: #conditional-branch}
You can create a branched sequence by using `if-then-else`. This example demonstrates an `if-then-else`. `action1` returns a boolean. If `true`, then `action2` is executed, otherwise `action3` is executed. Note `action3` is optional and can be omitted for `if-then`.

**For JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

**For Python 3**
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="If Sequence" alt="Sequence with conditional branching" style="width:250px; border-style: none"/></br>
_Figure 3. Sequence with conditional branching_

### Loops
{: #loop}
You can create looping constructs in Composer. In this example, `action2` executes as long as `action1` returns `true`. Composer limits the total number of steps you can execute in a composed sequence. The current limit is 20.

**For JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

**For Python 3**
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="While Sequence" alt="Sequence with while loop" style="width:250px; border-style: none"/></br>
_Figure 4. Sequence with `while` loop_

### Inline definition of Actions
{: #inline-def}
You can define actions within the composition code itself. In this example, you create the action definition inline with the composition that is named `hello` by using the `composer.action()`.

**For JavaScript**
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

**For Python 3**
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}

</br>
<img src="images/composer-inline.png" width="250" title="While Sequence" alt="Sequence with inline action definition" style="width:250px; border-style: none"/></br>
_Figure 5. Sequence with inline action definition_

## Using other combinator definitions
{: #combinator-def}
See the documentation for Composer on Apache OpenWhisk ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md){: external} or [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md){: external}) for the full list of combinators definitions.

