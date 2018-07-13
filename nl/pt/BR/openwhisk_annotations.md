---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Anotações
{: #openwhisk_annotations}

As ações, os acionadores, as regras e os pacotes do {{site.data.keyword.openwhisk}} (coletivamente referidos como ativos) podem ser decorados com `anotações`. As anotações são anexadas a ativos como parâmetros com uma `key` que define um nome e um `value` que define o valor. É conveniente configurá-los na interface da linha de comandos (CLI) usando a sinalização `--annotation` ou `-a` para abreviar.
{: shortdesc}

Lógica: as anotações foram incluídas no {{site.data.keyword.openWhisk_short}} para permitir experimentação sem mudar o esquema de ativo subjacente. Até o momento em que este documento foi escrito, houve um esforço deliberado para não definir quais `annotations` são permitidos. No entanto, como as anotações são mais fortemente usadas para divulgar as mudanças de semântica, é importante começar a documentá-las.

O uso mais comum de anotações até a data é documentar ações e pacotes. Muitos dos pacotes no catálogo do {{site.data.keyword.openwhisk_short}} transportam anotações, como: uma descrição da funcionalidade que é oferecida por suas ações, parâmetros para usar no tempo de ligação do pacote, parâmetros invoke-time ou se um parâmetro é um "segredo" (por exemplo, senha) ou não. As anotações são inventadas conforme necessário, por exemplo, para permitir a integração da UI.

Aqui está um conjunto de amostra de anotações para uma ação `echo`, que retorna seus argumentos de entrada não modificados (por exemplo, `function main(args) { return args }`). Essa ação é útil para registrar parâmetros de entrada, por exemplo, como parte de uma sequência ou regra.
```
ibmcloud wsk action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

As anotações que descrevem **pacotes** incluem:

- `description`: uma descrição concisa do pacote.
- `parameters`: uma matriz que descreve os parâmetros que estão com escopo definido para o pacote.

As anotações que descrevem **ações** incluem:

- `description`: uma descrição concisa da ação.
- `parameters`: uma matriz que descreve ações que são necessárias para executar a ação.
- `sampleInput`: um exemplo que mostra o esquema de entrada com valores típicos.
- `sampleOutput`: um exemplo que mostra o esquema de saída, geralmente para o `sampleInput`.

As anotações que descrevem **parâmetros** incluem:

- `name`: o nome do parâmetro.
- `description`: uma descrição concisa do parâmetro.
- `doclink`: um link para a documentação adicional para o parâmetro (útil para tokens OAuth).
- `required`: true para parâmetros necessários e false para os opcionais.
- `bindTime`: true se o parâmetro for especificado quando um pacote estiver ligado.
- `type`: o tipo do parâmetro, um de `password`, `array` (mas pode ser usado mais amplamente).

As anotações _não_ são verificadas. Portanto, embora seja possível usar as anotações para indicar se uma composição de duas ações em uma sequência é legal, por exemplo, o sistema ainda não faz isso.

## As anotações específicas para ações da web
{: #annotations-specific-to-web-actions}

Recentemente, a API principal foi estendida com novos recursos. Para permitir que os pacotes e ações participem desses recursos, novas anotações semanticamente significativas são introduzidas. Essas anotações devem ser configuradas explicitamente para `true` para terem efeito. Mudar o valor de `true` para `false` exclui o ativo conectado da nova API. As anotações não têm significado contrário no sistema. Veja as anotações a seguir:

- `web-export`: aplica-se somente a uma ação. Se presente, ela torna sua ação correspondente acessível para chamadas REST _sem_ autenticação. Elas são chamadas [_ações da web_](openwhisk_webactions.html) porque permitem usar ações do OpenWhisk em um navegador por exemplo. É importante observar que o _proprietário_ da ação da web incorre no custo de executá-la no sistema. Em outras palavras, o _proprietário_ da ação também possui o registro de ativações.
- `final`: aplica-se somente a uma ação. Isso torna todos os parâmetros de ação que já estão definidos imutáveis. Um parâmetro de uma ação que transporta a anotação não pode ser substituído por parâmetros invoke-time quando o valor de parâmetro é definido por meio de seu pacote de fechamento ou a definição de ação.
- `raw-http`: aplica-se somente a uma ação na presença de uma anotação `web-export`. Se presente, os parâmetros de consulta e corpo da solicitação de HTTP serão passados para a ação como propriedades reservadas.
- `web-custom-options`: quando configurada, essa anotação permite que uma ação da web responda às solicitações de OPTIONS com cabeçalhos customizados, caso contrário, uma [resposta de CORS padrão](openwhisk_webactions.html#options-requests) se aplica.
- `require-whisk-auth`: essa anotação protege a ação da web para que ela seja chamada somente por solicitações que fornecem as credenciais de autenticação apropriadas. Quando configurada para um valor booleano, ela controla se o valor de autenticação básica da solicitação (ou seja, a chave de autenticação do Whisk) será autenticado ou não. Um valor de `true` autentica as credenciais e um valor de `false` chama a ação sem nenhuma autenticação. Quando configurado para um número ou uma sequência, esse valor deve corresponder ao valor do cabeçalho `X-Requer-Whisk-Auth` da solicitação. Em ambos os casos, é importante observar que o _proprietário_ da ação da web incorre no custo de executá-la no sistema (ou seja, o _proprietário_ da ação também possui o registro de ativações).

## Anotações específicas para ativações

O sistema pode decorar registros de ativação com as anotações a seguir:

- `path`: o nome do caminho completo da ação que gerou a ativação. Observe que se essa ativação foi o resultado de uma ação em uma ligação de pacote, o caminho se referirá ao pacote pai.
- `kind`: o tipo de ação executada e um dos tipos de tempo de execução do OpenWhisk de suporte.
- `limits`: os limites de tempo, memória e log aos quais essa ativação estava sujeita.

Para ativações relacionadas à sequência, o sistema gera as anotações a seguir:

- `topmost`: isso está presente somente para uma ação de sequência externa.
- `causedBy`: isso está presente somente para ações que estão contidas em uma sequência.

Finalmente, e para fornecer transparência de desempenho, as ativações também registram:

- `waitTime`: o tempo gasto aguardando no sistema OpenWhisk interno. Isso é aproximadamente o tempo gasto entre o controlador receber a solicitação de ativação e quando o invocador provisionou um contêiner para a ação. Esse valor está presente atualmente para ativações sem sequência. Para sequências, essas informações podem ser derivadas do registro de ativação de sequência `topmost`.
- `initTime`: o tempo gasto para inicializar a função. Se esse valor estiver presente, a ação requer inicialização e representa um cold start. Uma ativação warm ignora a inicialização e, neste caso, a anotação não é gerada.

Um exemplo dessas anotações como apareceriam em um registro de ativação é mostrado abaixo.

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
