---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Criar acionadores e regras
{: #openwhisk_triggers}

Os acionadores e regras do {{site.data.keyword.openwhisk_short}} trazem recursos acionados por eventos para a plataforma. Os eventos de origens de eventos externos e internos são canalizados por meio de um acionador e as regras permitem que suas ações reajam a esses eventos.
{: shortdesc}

## Criar Acionadores
{: #openwhisk_triggers_create}

Os acionadores são um canal denominado para uma classe de eventos. A seguir estão exemplos de acionadores:
- Um acionador de eventos de atualização de local.
- Um acionador de uploads de documentos para um website.
- Um acionador de e-mails recebidos.

Acionadores podem ser *disparados* (ativados) usando um dicionário de pares de valores de chaves. Às vezes, esse dicionário é referido como o *evento*. Tal como com as ações, cada disparo de um acionador resulta em um ID de ativação.

Os acionadores podem ser explicitamente disparados por um usuário ou disparados em nome de um usuário por uma origem de eventos externos.
Um *Feed* é uma maneira conveniente para configurar uma origem de eventos externos para disparar eventos acionadores que podem ser consumidos pelo {{site.data.keyword.openwhisk_short}}. Consulte os feeds de exemplo a seguir:
- Feed de mudança de dados do Cloudant que dispara um evento acionador toda vez que um documento em um banco de dados é incluído ou modificado.
- Um feed Git que dispara um evento acionador para cada confirmação em um repositório Git.

## Usando Regras
{: #openwhisk_rules_use}

Uma regra associa um acionador com uma ação, com cada disparo do acionador que faz a ação correspondente ser chamada com o evento acionador como entrada.

Com o conjunto apropriado de regras, é possível que um único evento acionador
chame múltiplas ações ou uma Ação seja chamada como uma resposta a eventos
de múltiplos acionadores.

Por exemplo, considere um sistema com as ações a seguir:
- `classifyImage` - uma ação que detecta os objetos em uma imagem e os classifica.
- `thumbnailImage` - uma ação que cria uma versão miniatura de uma imagem.

Além disso, suponha que duas origens de eventos estejam disparando os acionadores a seguir:
- `newTweet` - um acionador que é disparado quando um novo tweet é postado.
- `imageUpload` - um acionador que é disparado quando uma imagem é transferida por upload para um website.

É possível configurar regras para que um único evento acionador chame múltiplas ações e permitir que múltiplos acionadores chamem a mesma ação:
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

Como um exemplo, crie um Acionador para enviar atualizações de local do usuário e dispare o acionador manualmente.
1. Insira o comando a seguir para criar o acionador:
  ```
  wsk trigger create locationUpdate
  ```
  {: pre}

  ```
  ok: acionador locationUpdate criado
  ```

2. Verifique se você criou o acionador listando o conjunto de acionadores.
  ```
  wsk trigger list
  ```
  {: pre}

  ```
  acionadores
  /someNamespace/locationUpdate                            privado
  ```

  Agora um "canal" nomeado é criado para o qual os eventos podem ser disparados.

3. Em seguida, dispare um evento acionador especificando o nome do acionador e parâmetros:
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: locationUpdate acionado com id fa495d1223a2408b999c3e0ca73b2677
  ```

Um acionador que é disparado sem uma regra associada para correspondência não tem efeito visível.
Os acionadores não podem ser criados dentro de um pacote; eles devem ser criados diretamente sob um namespace.

## Associar acionadores e ações usando regras
{: #openwhisk_rules_assoc}

As regras são usadas para associar um acionador com uma ação. Cada vez que um evento acionador é disparado, a ação é chamada com os parâmetros de evento.

Como um exemplo, crie uma regra que chame a ação `hello` sempre que uma atualização de local é postada.
1. Crie um arquivo 'hello.js' com o código de ação, tal como:
  ```javascript
  function main(params) {
     return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Certifique-se de que o acionador e a ação existam.
  ```
  wsk trigger update locationUpdate
  ```
  {: pre}

  ```
  wsk action update hello hello.js
  ```
  {: pre}

3. A próxima etapa é criar a regra. A regra é ativada no momento da criação, o que significa que está imediatamente disponível para responder a ativações de seu acionador. Os três parâmetros são: o nome da regra, o acionador e a ação.
  ```
  wsk rule create myRule locationUpdate hello
  ```
  {: pre}

  A qualquer momento, é possível escolher desativar uma regra.
  ```
  wsk rule disable myRule
  ```
  {: pre}

4. Dispare o acionador `locationUpdate`. Cada vez que você dispara um evento, a ação `hello` é chamada com os parâmetros de evento.
  ```
  wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
  ```
  {: pre}

  ```
  ok: locationUpdate acionado com id d5583d8e2d754b518a9fe6914e6ffb1e
  ```

5. Verifique se a ação foi chamada, verificando a ativação mais recente.
  ```
  wsk activation list --limit 1 hello
  ```
  {: pre}

  ```
  ativações
  9c98a083b924426d8b26b5f41c5ebc0d             hello
  ```
  ```
  wsk activation result 9c98a083b924426d8b26b5f41c5ebc0d
  ```
  {: pre}

  ```json
  {
     "payload": "Hello, Donald from Washington, D.C."
  }
  ```

  Você vê que a ação `hello` recebeu a carga útil do evento e retornou a sequência esperada.

É possível criar múltiplas regras que associam o mesmo acionador com ações diferentes.
Os acionadores e regras não podem pertencer a um pacote. No entanto, a regra pode ser associada a uma ação que
pertence a um pacote, por exemplo:
  ```
  wsk rule create recordLocation locationUpdate /whisk.system/utils/echo
  ```
  {: pre}

Também é possível usar regras com sequências. Por exemplo, alguém pode criar uma sequência
de ações `recordLocationAndHello` que é ativada pela regra `anotherRule`.
  ```
  wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
  ```
  {: pre}

  ```
  wsk rule create anotherRule locationUpdate recordLocationAndHello
  ```
  {: pre}
