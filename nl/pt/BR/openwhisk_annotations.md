---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: annotations, annotate, package, parameters, actions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Anotações
{: #openwhisk_annotations}

As ações do {{site.data.keyword.openwhisk}}, os acionadores, as regras e os pacotes (coletivamente mencionados como entidades) podem incluir anotações.
{: shortdesc}

As anotações são anexadas às entidades como parâmetros. As anotações consistem em uma `key`
que define um nome e em um `value` que define o valor. As anotações são mais normalmente
utilizadas para documentar ações e pacotes. Muitos dos pacotes no catálogo do
{{site.data.keyword.openwhisk_short}} carregam anotações, como uma descrição da funcionalidade que é
oferecida por suas ações, parâmetros a serem usados em tempo de ligação de pacote, parâmetros de tempo de
chamada ou se um parâmetro é um segredo. As anotações são inventadas conforme necessário, por exemplo, para permitir a integração da UI.

É possível documentar uma entidade na CLI usando a sinalização
`--annotation` ou `-a`.

## Anotações de ação
{: #action}

Anotações que descrevem ações incluem:

- `description`: uma descrição concisa da ação.
- `parameters`: uma matriz que descreve ações que são necessárias para executar a ação.
- `sampleInput`: um exemplo que mostra o esquema de entrada com valores típicos.
- `sampleOutput`: um exemplo que mostra o esquema de saída, geralmente para o `sampleInput`.



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

As anotações de ação da web a seguir devem ser configuradas explicitamente como `true`
para ativar a interatividade da API:

- `web-export`: quando aplicada a uma ação, a ação se torna uma
[ação da web](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions). A ação se torna acessível às chamadas REST sem
autenticação para que os usuários possam acessar as ações por meio de um navegador.
    * **Nota**: o proprietário da ação da web incorre no custo de executá-los no sistema. Em outras palavras, o proprietário da ação também tem o registro de ativações.
- `final`: quando aplicado a uma ação, qualquer parâmetro de ação que tiver sido
definido anteriormente se tornará imutável. Os parâmetros não podem ser substituídos por parâmetros fornecidos
durante a chamada.
- `raw-http`: quando aplicado a uma ação que tem a anotação `web-export`,
a consulta de solicitação e os parâmetros de corpo HTTP são passados para a ação como
propriedades reservadas.
- `web-custom-options`: permite que uma ação da web responda às solicitações
OPTIONS com cabeçalhos customizados. Caso contrário, uma  [ resposta CORS padrão ](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions#options-requests)  se aplicará.
- `require-whisk-auth`: a ação da web pode ser chamada somente por solicitações que
fornecem credenciais de autenticação apropriadas.
    * Quando configurado como um valor booleano, ele controla se o valor de Autenticação básica da
solicitação é autenticado. Um valor de `true` autentica as credenciais e um valor de `false` chama a ação sem nenhuma autenticação.
    * Quando configurado como um número inteiro ou uma sequência, esse valor deve corresponder ao valor
do cabeçalho `X-Require-Whisk-Auth` da solicitação.
    * **Nota**: o proprietário da ação da web incorre no custo de executá-los no sistema. Em outras palavras, o proprietário da ação também tem o registro de ativações.

## Anotações do pacote
{: #package}

As anotações que descrevem pacotes incluem:

- `description`: uma descrição concisa do pacote.
- `parameters`: uma matriz que descreve os parâmetros que estão com escopo definido para o pacote.

## Anotações de parâmetro
{: #parameter}

As anotações que descrevem parâmetros incluem:

- `name`: o nome do parâmetro.
- `description`: uma descrição concisa do parâmetro.
- `doclink`: um link para a documentação adicional para o parâmetro (útil para tokens OAuth).
- `required`: true para parâmetros necessários e false para os opcionais.
- `bindTime`: true se o parâmetro for especificado quando um pacote estiver ligado.
- `type`: o tipo do parâmetro, um de `password`, `array` (mas pode ser usado mais amplamente).

## Anotações de Ativação
{: #activation}

É possível documentar registros de ativação com as anotações a seguir:

- `path`: o nome do caminho completo da ação que gerou a ativação. Observe que se essa ativação foi o resultado de uma ação em uma ligação de pacote, o caminho se referirá ao pacote pai.
- `kind`: o tipo de ação executada e um dos tipos de tempo de execução do OpenWhisk de suporte.
- `limits`: os limites de tempo, memória e log aos quais essa ativação estava sujeita.

Para ativações relacionadas à sequência, o sistema gera as anotações a seguir:

- `topmost`: isso está presente somente para uma ação de sequência externa.
- `causedBy`: isso está presente somente para ações que estão contidas em uma sequência.

Para fornecer transparência de desempenho, as ativações também registram:

- `waitTime`: o tempo gasto aguardando no sistema OpenWhisk interno. Isso é aproximadamente o tempo gasto entre o controlador receber a solicitação de ativação e quando o invocador provisionou um contêiner para a ação. Esse valor está presente atualmente para ativações sem sequência. Para sequências, essas informações podem ser derivadas do registro de ativação de sequência `topmost`.
- `initTime`: o tempo gasto para inicializar a função. Se esse valor estiver presente, a ação requer inicialização e representa um cold start. Uma ativação warm ignora a inicialização e, neste caso, a anotação não é gerada.

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
