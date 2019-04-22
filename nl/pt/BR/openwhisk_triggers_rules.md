---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: trigger rules, triggers, actions, channel events

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Respondendo aos eventos com acionadores e regras
{: #openwhisk_triggers}

Acionadores e regras do {{site.data.keyword.openwhisk}} trazem recursos acionados por eventos para a plataforma. Eventos de origens de eventos externos e internos são canalizados por meio de um acionador e as regras permitem que suas ações reajam a esses eventos.
{: shortdesc}

## Conceitos gerais
{: #definitions}

### disparos
{: #openwhisk_triggers_create}

Os acionadores são um canal denominado para uma classe de eventos.
{: shortdesc}

Um acionador é uma declaração de que você deseja reagir a um determinado tipo de evento, seja de um usuário ou por uma origem de eventos. A seguir estão exemplos de acionadores.
- Um acionador de eventos de atualização de local
- Um acionador de uploads de documento para um website
- Um acionador de e-mails recebidos

Os acionadores podem ser disparados ou ativados usando um dicionário de pares chave-valor. Às vezes, esse dicionário é referido como o evento. Os acionadores podem ser explicitamente disparados por um usuário ou disparados em nome de um usuário por uma origem de eventos externos. Como com as ações, cada disparo de um acionador que está associado a uma regra resulta em um ID de ativação. Um acionador que não estiver associado com uma regra não terá efeito visível quando ele for disparado.

Um feed é uma maneira conveniente de configurar uma origem de eventos externos para disparar eventos
acionadores que podem ser consumidos pelo {{site.data.keyword.openwhisk_short}}. A seguir estão exemplos de feeds.
- Um feed de mudança de dados do {{site.data.keyword.cloudant}} que dispara um evento
acionador toda vez que um documento é incluído ou modificado em um banco de dados
- Um feed do Git que dispara um evento acionador para cada confirmação em um repositório Git

### Regras
{: #openwhisk_rules_use}

Uma regra associa um acionador a uma ação.
{: shortdesc}

Toda vez que o acionador for disparado, a regra usará o evento acionador como entrada e chamará a ação associada. Com o conjunto apropriado de regras, é possível que um único evento acionador chame várias ações ou que uma ação seja chamada como uma resposta a eventos de vários acionadores.

Por exemplo, considere um sistema com as ações a seguir.
- `classifyImage` - uma ação que detecta os objetos em uma imagem e os classifica.
- `thumbnailImage` - uma ação que cria uma versão miniatura de uma imagem.

Além disso, suponha que duas origens de eventos estejam disparando os acionadores a seguir.
- `newTweet` - um acionador que é disparado quando um novo tweet é postado.
- `imageUpload` - um acionador que é disparado quando uma imagem é transferida por upload para um website.

É possível configurar regras para que um único evento acionador chame múltiplas ações e que múltiplos
acionadores chamem a mesma ação.
- Regra ` newTweet-> classifyImage `
- Regra ` imageUpload-> classifyImage `
- Regra ` imageUpload-> thumbnailImage `

As três regras estabelecem o comportamento a seguir.
- As imagens em ambos os tweets são classificadas.
- As imagens transferidas por upload são classificadas
- Uma versão miniatura é gerada.

## Criando acionadores para eventos do canal
{: #openwhisk_triggers_fire}

As etapas a seguir mostram como criar um acionador de exemplo para enviar atualizações de local do
usuário e como disparar manualmente o acionador.

1. Crie o acionador. Os acionadores devem ser criados diretamente dentro de um namespace e não podem
ser criados dentro de pacotes.
    ```
    ibmcloud fn trigger create locationUpdate
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: acionador locationUpdate criado
    ```
    {: screen}

2. Verifique se o acionador foi criado.
    ```
    ibmcloud fn trigger list
    ```
    {: pre}

    Exemplo de Saída:
    ```
    acionadores
  /someNamespace/locationUpdate                            privado
    ```
    {: screen}
    O acionador serve como um canal nomeado no qual os eventos podem ser disparados.

3. Disparar um evento acionador.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Como não há nenhuma regra associada a esse acionador, os parâmetros passados não são usados como
entrada por nenhuma ação. Exemplo de Saída:
    ```
    ok: locationUpdate acionado com id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

Na próxima seção, é possível associar o acionador a uma ação criando uma regra.

## Usando regras para associar acionadores com ações
{: #openwhisk_rules_assoc}

As regras são usadas para associar um acionador a uma ação. Cada vez que um evento acionador é
disparado, a ação é chamada com os parâmetros do evento acionador.

Após criar o acionador do
[`locationUpdate`](#openwhisk_triggers_fire), as etapas a seguir mostrarão
como criar uma regra de exemplo que chame a ação `hello` sempre que uma atualização de
local for postada.

1. Crie um arquivo nomeado 'hello.js' com o código de ação a seguir:
    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: pre}

2. Crie a ação `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Crie a regra `myRule` para associar o acionador `locationUpdate`
à ação `hello`. As regras devem ser criadas diretamente dentro de um namespace e não podem
ser criadas dentro de pacotes.
    ```
    ibmcloud fn rule create myRule locationUpdate hello
    ```
    {: pre}

4. Dispare o acionador `locationUpdate`. Toda vez que um evento acionador ocorre, a ação `hello` é chamada com os parâmetros de evento.
    ```
    ibmcloud fn trigger fire locationUpdate --param name Human --param place "Earth"
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: locationUpdate acionado com id d5583d8e2d754b518a9fe6914e6ffb1e
    ```
    {: screen}

5. Verifique se a ação `hello` foi chamada consultando o registro de ativação mais
recente.
    ```
    ibmcloud fn activation list --limit 1 hello
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ativações
  9c98a083b924426d8b26b5f41c5ebc0d             hello
    ```
    {: screen}

6. Obtenha mais informações sobre o ID de ativação na saída de comando anterior.
    ```
    ibmcloud fn result result 9c98a083b924426d8b26b5f41c5ebc0d
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}
    Você vê que a ação `hello` recebeu a carga útil do evento e retornou a sequência esperada.

7. Para desativar a regra, é possível executar o comando a seguir.
    ```
    ibmcloud fn rule disable myRule
    ```
    {: pre}

Também é possível usar regras para associar acionadores às sequências. Por exemplo, é possível criar uma
sequência de ações chamada `recordLocationAndHello` que é ativada pela regra
`anotherRule`:
```
ibmcloud fn action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
```
{: pre}

```
ibmcloud fn rule create anotherRule locationUpdate recordLocationAndHello
```
{: pre}
