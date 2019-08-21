---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

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



# Associando acionadores e ações por meio de regras
{: #rules}

Toda vez que o acionador for disparado, a regra usará o evento acionador como entrada e chamará a ação associada. Com o conjunto apropriado de regras, é possível que um único acionador chame diversas ações ou para que uma ação seja chamada como uma resposta a eventos de vários acionadores.
{: shortdesc}


## Criação de regra por meio da IU
{: #rules_ui}

Na interface com o usuário, uma regra para associar uma ação e um acionador é criada para você.
{: shortdesc}

Ao criar ou acessar os detalhes para uma ação ou um acionador, você tem a opção de conectar uma ação ou um acionador novos ou existentes. Quando você faz a conexão, uma regra é criada para você e é nomeada no formato `ACTION_NAME-TRIGGER_NAME`.

Na CLI, é possível executar `ibmcloud fn rule list` para verificar se a regra foi criada para você.


## Criando regras por meio da CLI
{: #rules_create}

As regras são usadas para associar um acionador a uma ação. Cada vez que um evento acionador é
disparado, a ação é chamada com os parâmetros do evento acionador.

Antes de iniciar, crie [uma ação](/docs/openwhisk?topic=cloud-functions-actions) e [um acionador](/docs/openwhisk?topic=cloud-functions-triggers).


Crie uma regra para associar um acionador a uma ação. As regras devem ser criadas diretamente dentro de um namespace e não podem
ser criadas dentro de pacotes.
```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}


Para desativar a regra, é possível executar o comando a seguir.
```
ibmcloud fn rule disable RULE_NAME
```
{: pre}


## Criando regras para sequências de ações
{: #rules_seq}

É possível usar regras para associar acionadores com sequências de ações.

Antes de iniciar, crie [uma sequência de ações](/docs/openwhisk?topic=cloud-functions-actions#actions_seq) e [um acionador](/docs/openwhisk?topic=cloud-functions-triggers).

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_SEQUENCE_NAME
```
{: pre}


## Associando múltiplos acionadores e ações
{: #rules_assoc}

É possível usar combinações diferentes de acionadores e ações, criando uma regra para cada combinação. Não é necessário ter uma proporção de um para um para ações e acionadores.

Por exemplo, considere as ações a seguir.

| Ação | Descrição |
| --- | --- |
| `classifyImage` | Uma ação que detecta os objetos em uma imagem e os classifica. |
| `thumbnailImage` | Uma ação que cria uma versão em miniatura de uma imagem. |

Além disso, suponha que duas origens de eventos estejam disparando os acionadores a seguir.

| Acionador | Descrição |
| --- | --- |
| `newTweet` | Um acionador que é disparado quando um novo tweet é postado. |
| `imageUpload` | Um acionador que é disparado quando uma imagem é transferida por upload para um website. |

É possível configurar regras para que um único evento acionador chame múltiplas ações e que múltiplos
acionadores chamem a mesma ação.
- Regra ` newTweet-> classifyImage `
- Regra ` imageUpload-> classifyImage `
- Regra ` imageUpload-> thumbnailImage `

As três regras estabelecem o comportamento a seguir.
- As imagens em ambos os tweets são classificadas.
- As imagens transferidas por upload são classificadas
- Uma versão miniatura é gerada.

