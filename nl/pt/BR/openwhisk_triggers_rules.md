---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Conceitos gerais
{: #openwhisk_triggers}

Acionadores e regras do {{site.data.keyword.openwhisk_short}} trazem recursos acionados por eventos para a plataforma. Eventos de origens de eventos externos e internos são canalizados por meio de um acionador e as regras permitem que suas ações reajam a esses eventos.
{: shortdesc}

## O que é um acionador?
{: #openwhisk_triggers_create}

Os acionadores são um canal denominado para uma classe de eventos. A seguir estão exemplos de acionadores:
- Um acionador de eventos de atualização de local.
- Um acionador de uploads de documentos para um website.
- Um acionador de e-mails recebidos.

Acionadores podem ser *disparados* (ativados) usando um dicionário de pares de valores de chaves. Às vezes, esse dicionário é referido como o *evento*. Como com ações, cada disparo de um acionador resulta em um **ID de ativação**.

Os acionadores podem ser explicitamente disparados por um usuário ou disparados em nome de um usuário por uma origem de eventos externos.
Um *Feed* é uma maneira conveniente de configurar uma origem de eventos externos para disparar eventos acionadores que podem ser consumidos pelo {{site.data.keyword.openwhisk_short}}. Consulte os feeds de exemplo a seguir:
- O feed de mudança de dados do {{site.data.keyword.cloudant}} que dispara um evento acionador toda vez que um documento em um banco de dados é incluído ou modificado.
- Um feed Git que dispara um evento acionador para cada confirmação em um repositório Git.

## Como as regras afetam os acionadores?
{: #openwhisk_rules_use}

Uma regra associa um acionador a uma ação para cada disparo do acionador que faz com que a ação correspondente seja chamada com o evento acionador como entrada.

Com o conjunto apropriado de regras, é possível que um único evento acionador chame várias ações ou que uma ação seja chamada como uma resposta a eventos de vários acionadores.

Por exemplo, considere um sistema com as ações a seguir:
- `classifyImage` - uma ação que detecta os objetos em uma imagem e os classifica.
- `thumbnailImage` - uma ação que cria uma versão miniatura de uma imagem.

Além disso, suponha que duas origens de eventos estejam disparando os acionadores a seguir:
- `newTweet` - um acionador que é disparado quando um novo tweet é postado.
- `imageUpload` - um acionador que é disparado quando uma imagem é transferida por upload para um website.

É possível configurar regras para que um único evento acionador chame várias ações e fazer vários acionadores chamarem a mesma ação:
- Regra `newTweet -> classifyImage`.
- Regra `imageUpload -> classifyImage`.
- Regra `imageUpload -> thumbnailImage`.

As três regras estabelecem o comportamento a seguir:
- As imagens em ambos os tweets são classificadas.
- As imagens transferidas por upload são classificadas
- Uma versão miniatura é gerada.

## Criar e disparar acionadores
{: #openwhisk_triggers_fire}

Os acionadores podem ser disparados quando determinados eventos ocorrem ou podem ser disparados manualmente.

Como um exemplo, crie um acionador para enviar atualizações de local do usuário e dispare o acionador manualmente.
1. Insira o comando a seguir para criar o acionador:
  ```
  ibmcloud wsk trigger create locationUpdate
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: acionador locationUpdate criado
  ```
  {: screen}

2. Verifique se você criou o acionador listando o conjunto de acionadores.
  ```
  ibmcloud wsk trigger list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  acionadores
  /someNamespace/locationUpdate                            privado
  ```
  {: screen}

  Agora um "canal" nomeado é criado para o qual os eventos podem ser disparados.

3. Em seguida, dispare um evento especificando o nome e os parâmetros do acionador:
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: locationUpdate acionado com id fa495d1223a2408b999c3e0ca73b2677
  ```
  {: screen}

Um acionador que é disparado sem uma regra que acompanha para fazer a correspondência não tem efeito visível.
Os acionadores não podem ser criados dentro de um pacote; eles devem ser criados diretamente sob um **Namespace**.

## Usando regras para associar acionadores com ações
{: #openwhisk_rules_assoc}

As regras são usadas para associar um acionador a uma ação. Toda vez que um evento acionador é disparado, a ação é chamada com os parâmetros de evento.

Como um exemplo, crie uma regra que chame a ação `hello` sempre que uma atualização de local for postada.
1. Crie um arquivo nomeado 'hello.js' com o código de ação a seguir:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Certifique-se de que o acionador e a ação existam:
  ```
  ibmcloud wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  ibmcloud wsk action update hello hello.js
  ```
  {: pre}

3. A próxima etapa é criar a regra. A regra é ativada na criação, o que significa que está imediatamente disponível para responder a ativações de seu acionador. Os três parâmetros são: _rule name_, _trigger name_ e _action name_.
  ```
  ibmcloud wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  A qualquer momento, é possível optar por desativar uma regra:
  ```
  ibmcloud wsk rule disable myRule
  ```
  {: pre}

4. Dispare o acionador **locationUpdate**. Cada vez que você dispara um evento, a ação **hello** é chamada com os parâmetros de evento.
  ```
  ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: locationUpdate acionado com id d5583d8e2d754b518a9fe6914e6ffb1e
  ```
  {: screen}

5. Verifique se a ação **hello** foi chamada, verificando a ativação mais recente.
  ```
  ibmcloud wsk activation list --limit 1 hello
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ativações
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  {: screen}

  Agora, consulte o ID de ativação listado na saída de comando anterior:
  ```
  ibmcloud wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```
  {: screen}

  Você vê que a ação **hello** recebeu a carga útil do evento e retornou a sequência esperada.

É possível criar várias regras que associem o mesmo acionador a diferentes ações.
Acionadores e regras não podem pertencer a um pacote. No entanto, a regra pode ser associada a uma ação
que pertença a um pacote, por exemplo:
  ```
  ibmcloud wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

Também é possível usar regras com sequências. Por exemplo, é possível criar uma sequência
de ações `recordLocationAndHello` que é ativada pela regra `anotherRule`.
  ```
  ibmcloud wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  ibmcloud wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
