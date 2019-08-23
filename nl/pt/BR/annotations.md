---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: annotations, annotate, package, parameters, actions, functions

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


# Anotações
{: #annotations}

As ações do {{site.data.keyword.openwhisk}}, os acionadores, as regras e os pacotes (coletivamente mencionados como entidades) podem incluir anotações.
{: shortdesc}

As anotações são anexadas às entidades como parâmetros. As anotações consistem em uma `key`
que define um nome e em um `value` que define o valor. As anotações são mais normalmente
utilizadas para documentar ações e pacotes. Os pacotes no catálogo do {{site.data.keyword.openwhisk_short}} transportam anotações. Essas anotações incluem descrições da funcionalidade que é oferecida por suas ações, parâmetros a serem usados no tempo de ligação de pacote, parâmetros de tempo de chamada ou se um parâmetro é um segredo. As anotações são inventadas conforme necessário, por exemplo, para permitir a integração da UI.

É possível documentar uma entidade na CLI usando a sinalização
`--annotation` ou `-a`.

## Anotações de ação
{: #annotations_action}

| Anotação | Descrição |
| --- | --- |
| `description` | Uma descrição da ação. |
| `parâmetros` | Uma matriz que descreve ações que são necessárias para executar a ação. |
| `sampleInput` | Um exemplo que mostra o esquema de entrada com valores típicos. |
| `sampleOutput` | Um exemplo que mostra o esquema de saída, geralmente para a `sampleInput`. |



O código a seguir é um conjunto de exemplo de anotações para uma ação `echo`, que
retorna seus argumentos de entrada não modificados. Essa ação é útil para registrar parâmetros de entrada, por exemplo, como parte de uma sequência ou regra.

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Anotações de ação da web
{: #annotations-specific-to-web-actions}

As anotações de ação da web a seguir devem ser configuradas explicitamente como `true` para ativar a interatividade da API.

| Anotação | Descrição |
| --- | --- | 
| `web-export` | Quando aplicada a uma ação, a ação se torna uma [ação da web](/docs/openwhisk?topic=cloud-functions-actions_web). A ação se torna acessível às chamadas REST sem
autenticação para que os usuários possam acessar as ações por meio de um navegador. O proprietário da ação da web incorre no custo de executá-las. Em outras palavras, o proprietário da ação também tem o registro de ativações. |
| `final` | Quando aplicada a uma ação, quaisquer parâmetros de ação que foram definidos anteriormente não podem ser substituídos por parâmetros que são fornecidos durante a chamada. |
| `raw-http` | Quando aplicada a uma ação que possui a anotação `web-export`, os parâmetros de corpo e de consulta de solicitação de HTTP são passados para a ação como propriedades reservadas. |
| `web-custom-options` | Ativa uma ação da web para responder a solicitações OPTIONS com cabeçalhos customizados. Caso contrário, uma  [ resposta CORS padrão ](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options)  se aplicará. |
| `require-whisk-auth` | A ação da web pode ser chamada apenas por solicitações que fornecem credenciais de autenticação apropriadas. Quando configurado como um valor booleano, ele controla se o valor de Autenticação básica da
solicitação é autenticado. Um valor de `true` autentica as credenciais e um valor de `false` chama a ação sem nenhuma autenticação. Quando configurado como um número inteiro ou uma sequência, esse valor deve corresponder ao valor
do cabeçalho `X-Require-Whisk-Auth` da solicitação. |

## Anotações do pacote
{: #annotations_package}

| Anotação | Descrição |
| --- | --- |
| `description` | Uma descrição do pacote. |
| `parâmetros` | Uma matriz que descreve os parâmetros que têm escopo definido para o pacote. |

## Anotações de parâmetro
{: #annotations_parameter}

| Anotação | Descrição |
| --- | --- |
| `name` | O nome do parâmetro. |
| `description` | Uma forte descrição do parâmetro. |
| `doclink` | Um link para documentação adicional para o parâmetro (útil para tokens OAuth). |
| `required` | True para os parâmetros necessários e false para os opcionais. |
| `bindTime` | True se o parâmetro for especificado quando um pacote estiver ligado. |
| `type` | O tipo do parâmetro, um de `password` ou `array` (mas pode ser usado de maneira mais ampla). |

## Anotações de Ativação
{: #annotations_activation}

É possível documentar registros de ativação com as anotações a seguir:

| Anotação | Descrição |
| --- | --- |
| `path` | O nome do caminho completo da ação que gerou a ativação. Se essa ativação foi o resultado de uma ação em uma ligação de pacote, o caminho se referirá ao pacote pai. |
| `kind` | O tipo de ação executada e um dos tipos de tempo de execução do {{site.data.keyword.openwhisk_short}} suportados. |
| `limits` | O tempo, a memória e os limites de log para os quais essa ativação estava sujeita. |

Para ativações relacionadas à sequência, o sistema gera as anotações a seguir:

| Anotação | Descrição |
| --- | --- |
| `topmost` | Essa anotação está presente somente para uma ação de sequência mais externa. |
| `causedBy` | Essa anotação está presente somente para ações que estão contidas em uma sequência. |
| `waitTime` | O tempo que é gasto aguardando no sistema interno do {{site.data.keyword.openwhisk_short}}. Esse tempo é aproximadamente o tempo que é gasto entre o recebimento da solicitação de ativação e quando o invocador provisionou um contêiner para a ação. Esse valor está presente apenas para ativações não relacionadas à sequência. Para sequências, essas informações podem ser derivadas do registro de ativação de sequência `topmost`. |
| `initTime` | O tempo que é gasto inicializando a função. Se esse valor estiver presente, a ação requer inicialização e representa um cold start. Uma ativação warm ignora a inicialização e, neste caso, a anotação não é gerada. |

O exemplo a seguir mostra essas anotações à medida que elas aparecem em um registro de ativação:

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}




