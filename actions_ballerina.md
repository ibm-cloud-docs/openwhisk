---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-06"

keywords: ballerina, serverless, actions

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

# Creating Ballerina actions
{: #ballerina-actions}

The following sections guide you through creating and invoking a single Ballerina action and adding parameters to that action. An action is a top-level Ballerina function which accepts and returns a JSON object. 

Ballerina actions are executed in Ballerina [0.990.2](https://ballerina.io/downloads). You will need a compatible version of the compiler locally available to generate the executable. Without the Ballerina compiler, you cannot create an action.

## Creating and invoking a Ballerina action

**Before you begin:** Create a file called `hello.bal` with the following source code.

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

The entry method for the action is `main` by default. You can specify this variable when you create the action with the `wsk` CLI by using `--main`. 

**Note:** that the Ballerina compiler expects the presence of a function called `main` to generate the executable, so your source file must include a place holder called `main`.

To create an action called `hello` complete the following steps.

1. Generate the .balx file.

```
ballerina build hello.bal
```
{: pre}

2. Create the action by using the .balx file.
```
ibmcloud fn action create bello hello.balx --kind ballerina:0.990
```
{: pre}

3. The CLI does not yet determine the type of the action from the source file extension. You must specify the kind explicitly. For `.balx` source files, the action runs by using the Ballerina 0.990.2 runtime.

```
ibmcloud fn action invoke --result bello --param name World
```
{: pre}

Example output.
```json
{
  "greeting": "Hello World!"
}
```
{: screen}
