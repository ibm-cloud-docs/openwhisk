---

copyright:
  years: 2016, 2018
lastupdated: "2018-01-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Criar e usar pacotes
{: #openwhisk_packages}

No {{site.data.keyword.openwhisk}}, é possível usar pacotes para empacotar um conjunto de ações relacionadas e compartilhá-las com outras pessoas.
{: shortdesc}

Um pacote pode incluir *Ações* e *Feeds*.
- Uma ação é uma parte de código que é executado no {{site.data.keyword.openwhisk_short}}. Por exemplo, o pacote Cloudant inclui ações para ler e gravar registros em um banco de dados Cloudant.
- Um feed é usado para configurar uma origem de eventos externos para disparar eventos acionadores. Por exemplo, o pacote Alarme inclui um feed que pode disparar um acionador em uma frequência especificada.

Cada entidade do {{site.data.keyword.openwhisk_short}}, incluindo pacotes, pertence a um *namespace* e o nome completo de uma entidade é `/namespaceName[/packageName]/entityName`. Para obter mais informações, consulte as [diretrizes de nomenclatura](./openwhisk_reference.html#openwhisk_entities).

As seções a seguir descrevem como procurar pacotes e usar os acionadores e feeds neles. Além
disso, se você estiver interessado em contribuir com seus próprios pacotes para o
catálogo, leia as seções sobre criação e compartilhamento de pacotes.

## Procurar pacotes
{: #browse-packages}

Vários pacotes são registrados com o {{site.data.keyword.openwhisk_short}}. É possível obter uma lista de pacotes em um namespace, listar as entidades em um pacote e obter uma descrição das entidades individuais em um pacote.

1. Obtenha uma lista de pacotes no namespace `/whisk.system`.

  ```
  wsk package list /whisk.system
  ```
  {: pre}
  ```
  packages
  /whisk.system/cloudant                                                 compartilhado
  /whisk.system/alarms                                                   compartilhado
  /whisk.system/watson                                                   compartilhado
  /whisk.system/websocket                                                compartilhado
  /whisk.system/weather                                                  compartilhado
  /whisk.system/system                                                   compartilhado
  /whisk.system/utils                                                    compartilhado
  /whisk.system/slack                                                    compartilhado
  /whisk.system/samples                                                  compartilhado
  /whisk.system/github                                                   compartilhado
  /whisk.system/pushnotifications                                        compartilhado
  ```

2. Obtenha uma lista de entidades no namespace `/whisk.system/cloudant`.

  ```
  wsk package get --summary /whisk.system/cloudant
  ```
  {: pre}
  ```
  package /whisk.system/cloudant: serviço de banco de dados do Cloudant
     (parâmetros: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: ler documento do banco de dados
   action /whisk.system/cloudant/write: gravar documento no banco de dados
   feed   /whisk.system/cloudant/changes: feed de mudança do banco de dados
  ```

  Essa saída mostra que o pacote Cloudant fornece duas ações, `read` e `write`, e um feed de acionador chamado `changes`. O feed `changes` faz os acionadores serem disparados quando documentos são incluídos no banco de dados Cloudant especificado.

  O pacote Cloudant também define os parâmetros `username`, `password`, `host` e `port`. Esses parâmetros devem ser especificados para que as ações e os feeds sejam significativos. Os parâmetros permitem que as ações operem em uma conta específica do Cloudant, por exemplo.

3. Obtenha uma descrição da ação `/whisk.system/cloudant/read`.
  ```
  wsk action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  ```
  action /whisk.system/cloudant/read: ler documento do banco de dados
     (parâmetros: dbname includeDoc id)
  ```

  Essa saída mostra que a ação `read` do Cloudant requer três parâmetros, incluindo o ID do banco de dados e do documento para recuperação.


## Chamar ações em um pacote
{: #openwhisk_package_invoke}

É possível chamar ações em um pacote, tal como com outras ações. As próximas poucas etapas mostram como chamar a ação `greeting` no pacote `/whisk.system/samples` com parâmetros diferentes.

1. Obtenha uma descrição da ação `/whisk.system/samples/greeting`.
  ```
  wsk action get --summary /whisk.system/samples/greeting
  ```
  {: pre}
  
  ```
  action /whisk.system/samples/greeting: imprimir uma saudação amistosa
     (parâmetros: nome local)
  ```

  Observe que a ação `greeting` usa dois parâmetros: `name` e `place`.

2. Chame a ação sem quaisquer parâmetros.
  ```
  wsk action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```

  A saída é uma mensagem genérica porque nenhum parâmetro foi especificado.

3. Chame a ação com parâmetros.
  ```
  wsk action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```

  Observe que o resultado usa os parâmetros `name` e `place` que foram passados para a ação.


## Criar e usar ligações de pacotes
{: #openwhisk_package_bind}

Embora seja possível usar as entidades em um pacote diretamente, é provável que você passe os mesmos parâmetros para a ação toda vez. É possível simplificar o processo ligando a um pacote e especificando parâmetros padrão, que são herdados pelas ações no pacote.

Por exemplo, no pacote `/whisk.system/cloudant`, você pode configurar os valores padrão `username`, `password` e `dbname` em uma ligação de pacote e esses valores serão passados automaticamente para quaisquer ações no pacote.

No exemplo simples a seguir, você faz a ligação com o pacote `/whisk.system/samples`.

1. Faça a ligação com o pacote `/whisk.system/samples` e configure um valor de parâmetro `place` padrão.

  ```
  wsk package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}
  ```
  ok: ligação valhallaSamples criada
  ```

2. Obtenha uma descrição da ligação do pacote.

  ```
  wsk package get --summary valhallaSamples
  ```
  {: pre}
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Retorna uma saudação amistosa
   action /myNamespace/valhallaSamples/wordCount: Contar palavras em uma sequência
   action /myNamespace/valhallaSamples/helloWorld: Demonstra recursos de criação de log
   action /myNamespace/valhallaSamples/curl: Enrolar uma url de host
  ```

  Observe que todas as ações no pacote `/whisk.system/samples` estão disponíveis na ligação de pacote `valhallaSamples`.

3. Chame uma ação na ligação de pacote.

  ```
  wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```

  Observe no resultado que a ação herda o parâmetro `place` que você configurou quando criou a ligação de pacote `valhallaSamples`.

4. Chame uma ação e sobrescreva o valor de parâmetro padrão.
  ```
  wsk action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```

  Observe que o valor de parâmetro `place` especificado com a chamada de ação sobrescreve o valor padrão configurado na ligação de pacote `valhallaSamples`.


## Criar e usar feeds de acionador
{: #openwhisk_package_trigger}

Os feeds oferecem uma maneira conveniente de configurar uma origem de eventos externos para disparar esses eventos para um acionador do {{site.data.keyword.openwhisk_short}}. Este exemplo mostra como usar um feed no pacote Alarmes para disparar um acionador a cada segundo e como usar uma regra para chamar uma ação a cada segundo.

1. Obtenha uma descrição do feed no pacote `/whisk.system/alarms`.
  ```
  wsk package get --summary /whisk.system/alarms
  ```
  {: pre}

  ```
  pacote /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```

  ```
  wsk action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  ```
  action /whisk.system/alarms/alarm: Fire Trigger when alarm occurs
     (params: cron trigger_payload)
  ```

  O feed `/whisk.system/alarms/alarm` usa dois parâmetros:
  - `cron`: uma especificação de crontab de quando disparar o acionador.
  - `trigger_payload`: o valor de parâmetro de carga útil para configurar em cada evento acionador.

2. Crie um acionador que dispare a cada 8 segundos.
  ```
  wsk trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  ```
  ok: feed acionador everyEightSeconds criado
  ```

3. Crie um arquivo 'hello.js' com o código de ação a seguir.
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Certifique-se de que a ação exista.
  ```
  wsk action update hello hello.js
  ```
  {: pre}

5. Crie uma regra que chame a ação `hello` sempre que o acionador `everyEightSeconds` for disparado.
  ```
  wsk rule create myRule everyEightSeconds hello
  ```
  {: pre}
  ```
  ok: regra myRule criada
  ```

6. Verifique se a ação está sendo chamada pesquisando os logs de ativação.

  ```
  wsk activation poll
  ```
  {: pre}

  É possível ver que as ativações são observadas a cada 8 segundos para o acionador, a regra e a ação. A ação recebe os parâmetros `{"name":"Mork", "place":"Ork"}` em cada chamada.


## Criar um pacote
{: #openwhisk_packages_create}

Um pacote é usado para organizar um conjunto de ações e feeds relacionados.
Também permite que os parâmetros sejam compartilhados entre todas as entidades no pacote.

Para criar um pacote customizado com uma ação simples nele, tente o exemplo a seguir:

1. Crie um pacote chamado "custom".
  ```
  wsk package create custom
  ```
  {: pre}

  ```
  ok: pacote custom criado
  ```

2. Obtenha um resumo do pacote.
  ```
  wsk package get --summary custom
  ```
  {: pre}

  ```
  pacote /myNamespace/custom
  ```

  Observe que o pacote está vazio.

3. Crie um arquivo chamado `identity.js` que contenha o código de ação a seguir. Essa ação retorna todos os parâmetros de entrada.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Crie uma ação `identity` no pacote `custom`.
  ```
  wsk action create custom/identity identity.js
  ```
  {: pre}
  
  ```
  ok: ação custom/identity criada
  ```

  Criar uma ação em um pacote requer que você prefixe o nome da ação com um nome de pacote. Aninhamento de pacote não é permitido. Um pacote pode conter somente ações e não pode conter outro pacote.

5. Obtenha um resumo do pacote novamente.
  ```
  wsk package get --summary custom
  ```
  {: pre}

  ```
  pacote /myNamespace/custom
   ação /myNamespace/custom/identity
  ```

  É possível ver a ação `custom/identity` em seu namespace agora.

6. Chame a ação no pacote.
  ```
  wsk action invoke --blocking --result custom/identity
  ```
  {: pre}

  ```json
  {}
  ```


É possível configurar parâmetros padrão para todas as entidades em um pacote configurando os parâmetros no nível do pacote que são herdados por todas as ações no pacote. Para ver como essa herança funciona, tente o exemplo a seguir:

1. Atualize o pacote `custom` com dois parâmetros: `city` e `country`.
  ```
  wsk package update custom --param city Austin --param country USA
  ```
  {: pre}

  ```
  ok: pacote custom atualizado
  ```

2. Exiba os parâmetros no pacote e ação e veja como a ação `identity` no pacote herda os parâmetros do pacote.
  ```
  wsk package get custom parameters
  ```
  {: pre}

  ```
  ok: got package custom, displaying field parameters
  ```

  ```json
  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```

  ```
  wsk action get custom/identity parameters
  ```
  {: pre}

  ```
  ok: got action custom/identity, , displaying field parameters
  ```

  ```json
  [
      {
          "key": "city",
          "value": "Austin"
      },
      {
          "key": "country",
          "value": "USA"
      }
  ]
  ```

3. Chame a ação identity sem quaisquer parâmetros para verificar se a ação realmente herda os parâmetros.

  ```
  wsk action invoke --blocking --result custom/identity
  ```
  {: pre}
  ```json
  {
      "city": "Austin",
      "country": "USA"
  }
  ```

4. Chame a ação de identidade com alguns parâmetros. Parâmetros de chamada são mesclados com os parâmetros do pacote; os parâmetros de chamada substituem os parâmetros do pacote.
  ```
  wsk action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  ```json
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```

## Compartilhar um pacote
{: #openwhisk_packages_share}

Após as ações e os feeds que formam um pacote serem depurados e testados, o pacote pode ser compartilhado com todos os usuários do {{site.data.keyword.openwhisk_short}}. Compartilhar o pacote possibilita que os usuários liguem o pacote, chame ações no pacote e crie regras e ações de sequência do {{site.data.keyword.openwhisk_short}}.

1. Compartilhe o pacote com todos os usuários:
  ```
  wsk package update custom --shared yes
  ```
  {: pre}

  ```
  ok: pacote custom atualizado
  ```

2. Exiba a propriedade `publish` do pacote para verificar se agora é true.
  ```
  wsk package get custom publish
  ```
  {: pre}

  ```
  ok: got package custom, displaying field publish
  ```

  ```json
  true
  ```


Outros usuários agora podem usar seu pacote `custom`, incluindo ligar ao pacote ou chamar diretamente uma ação nele. Outros usuários devem saber os nomes completos do pacote para ligá-lo ou chamar ações nele. Ações e feeds dentro de um pacote compartilhado são _public_. Se
o pacote for privado, então, todo o seu conteúdo também será privado.

1. Obtenha uma descrição do pacote para mostrar os nomes completos do pacote e ação.

  ```
  wsk package get --summary custom
  ```
  {: pre}
  ```
  pacote /myNamespace/custom
   ação /myNamespace/custom/identity
  ```

  No exemplo anterior, você está trabalhando com o namespace `myNamespace` e esse namespace aparece no nome completo.
