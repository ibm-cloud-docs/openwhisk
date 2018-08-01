---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Organizando ações em pacotes
{: #openwhisk_packages}

No {{site.data.keyword.openwhisk}}, é possível usar pacotes para empacotar um conjunto de ações relacionadas juntas e compartilhá-las com outras pessoas.
{: shortdesc}

Um pacote pode incluir *ações* e *feeds*.
- Uma ação é uma parte do código executada no {{site.data.keyword.openwhisk_short}}. Por exemplo, o pacote do {{site.data.keyword.cloudant}} inclui ações para ler e gravar registros em um banco de dados {{site.data.keyword.cloudant_short_notm}}.
- Um feed é usado para configurar uma origem de eventos externos para disparar eventos acionadores. Por exemplo, o pacote Alarme inclui um feed que pode disparar um acionador em uma frequência especificada.

Cada entidade do {{site.data.keyword.openwhisk_short}}, incluindo pacotes, pertence a um *namespace* e o nome completo de uma entidade é `/namespaceName[/packageName]/entityName`. Para obter mais informações, consulte as [diretrizes de nomenclatura](./openwhisk_reference.html#openwhisk_entities).

As seções a seguir descrevem como procurar pacotes e usar acionadores e feeds nos mesmos. Além
disso, se você estiver interessado em contribuir com seus próprios pacotes para o
catálogo, leia as seções sobre criação e compartilhamento de pacotes.

## Procurar pacotes
{: #browse-packages}

Vários pacotes são registrados com o {{site.data.keyword.openwhisk_short}}. É possível obter uma lista de pacotes em um namespace, listar as entidades em um pacote e obter uma descrição das entidades individuais em um pacote.

1. Obtenha uma lista de pacotes no namespace `/whisk.system`.
  ```
  ibmcloud fn package list /whisk.system
  ```
  {: pre}

  Saída da lista de pacotes:
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
  {: screen}

2. Obtenha uma lista de entidades no namespace `/whisk.system/cloudant`.
  ```
  ibmcloud fn package get --summary /whisk.system/cloudant
  ```
  {: pre}

  Exemplo de Saída:
  ```
  package /whisk.system/cloudant: {{site.data.keyword.cloudant_short_notm}} database service
     (params: {{site.data.keyword.Bluemix_notm}}ServiceName host username password dbname includeDoc overwrite)
   action /whisk.system/cloudant/read: Read document from database
   action /whisk.system/cloudant/write: Write document to database
   feed   /whisk.system/cloudant/changes: Database change feed
  ```
  {: screen}

  Essa saída mostra que o pacote do {{site.data.keyword.cloudant_short_notm}} fornece duas ações, `read` e `write`, e um feed de acionador chamado `changes`. O feed `changes` ocasiona o disparo de acionadores quando documentos são incluídos no banco de dados {{site.data.keyword.cloudant_short_notm}} especificado.

  O pacote do {{site.data.keyword.cloudant_short_notm}} também define os parâmetros `username`, `password`, `host` e `port`. Esses parâmetros devem ser especificados para que as ações e os feeds sejam significativos. Os parâmetros permitem que as ações operem em uma conta específica do {{site.data.keyword.cloudant_short_notm}}, por exemplo.

3. Obtenha uma descrição da ação `/whisk.system/cloudant/read`.
  ```
  ibmcloud fn action get --summary /whisk.system/cloudant/read
  ```
  {: pre}

  Exemplo de Saída:
  ```
  action /whisk.system/cloudant/read: ler documento do banco de dados
     (parâmetros: dbname includeDoc id)
  ```
  {: screen}

  Essa saída mostra que a ação `read` do {{site.data.keyword.cloudant_short_notm}} requer três parâmetros, incluindo o ID do banco de dados e do documento para recuperação.

## Chamar ações em um pacote
{: #openwhisk_package_invoke}

É possível chamar ações em um pacote, como com outras ações. As próximas poucas etapas mostram como chamar a ação `greeting` no pacote `/whisk.system/samples` com diferentes parâmetros.

1. Obtenha uma descrição da ação `/whisk.system/samples/greeting`.
  ```
  ibmcloud fn action get --summary /whisk.system/samples/greeting
  ```
  {: pre}

  Exemplo de Saída:
  ```
  action /whisk.system/samples/greeting: imprimir uma saudação amistosa
     (parâmetros: nome local)
  ```
  {: screen}

  Observe que a ação `greeting` usa dois parâmetros: `name` e `place`.

2. Chame a ação sem quaisquer parâmetros.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "payload": "Hello, stranger from somewhere!"
  }
  ```
  {: screen}

  A saída é uma mensagem genérica porque nenhum parâmetro foi especificado.

3. Chame a ação com parâmetros.
  ```
  ibmcloud fn action invoke --blocking --result /whisk.system/samples/greeting --param name Mork --param place Ork
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "payload": "Hello, Mork from Ork!"
  }
  ```
  {: screen}

  Observe que a saída usa os parâmetros `name` e `place` que foram passados para a ação.

## Criar e usar ligações de pacotes
{: #openwhisk_package_bind}

Embora seja possível usar as entidades em um pacote diretamente, você pode observar que está passando os mesmos parâmetros para a ação toda vez. É possível simplificar o processo ligando a um pacote e especificando parâmetros padrão, que são herdados pelas ações no pacote.

Por exemplo, no pacote `/whisk.system/cloudant`, é possível configurar valores padrão de `username`, `password` e `dbname` em uma ligação de pacote e esses valores serão passados automaticamente a qualquer ação no pacote.

No exemplo simples a seguir, você faz a ligação com o pacote `/whisk.system/samples`.

1. Faça a ligação com o pacote `/whisk.system/samples` e configure um valor de parâmetro `place` padrão.
  ```
  ibmcloud fn package bind /whisk.system/samples valhallaSamples --param place Valhalla
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: ligação valhallaSamples criada
  ```
  {: screen}

2. Obtenha uma descrição da ligação do pacote.
  ```
  ibmcloud fn package get --summary valhallaSamples
  ```
  {: pre}

  Exemplo de Saída:
  ```
  package /myNamespace/valhallaSamples
   action /myNamespace/valhallaSamples/greeting: Retorna uma saudação amistosa
   action /myNamespace/valhallaSamples/wordCount: Contar palavras em uma sequência
   action /myNamespace/valhallaSamples/helloWorld: Demonstra recursos de criação de log
   action /myNamespace/valhallaSamples/curl: Enrolar uma url de host
  ```
  {: screen}

  Observe que todas as ações no pacote `/whisk.system/samples` estão disponíveis na ligação do pacote `valhallaSamples`.

3. Chame uma ação na ligação do pacote.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "payload": "Hello, Odin from Valhalla!"
  }
  ```
  {: screen}

  Observe no resultado que a ação herda o parâmetro `place` que você configurou quando criou a ligação de pacote `valhallaSamples`.

4. Chame uma ação e sobrescreva o valor de parâmetro padrão.
  ```
  ibmcloud fn action invoke --blocking --result valhallaSamples/greeting --param name Odin --param place Asgard
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "payload": "Hello, Odin from Asgard!"
  }
  ```
  {: screen}

  Observe que o valor do parâmetro `place` especificado com a chamada da ação sobrescreve o valor padrão configurado na ligação do pacote `valhallaSamples`.

## Criar e usar feeds do acionador
{: #openwhisk_package_trigger}

Feeds oferecem uma maneira conveniente para configurar uma origem de eventos externos para disparar esses eventos para um acionador do {{site.data.keyword.openwhisk_short}}. Este exemplo mostra como usar um feed no pacote Alarmes para disparar um acionador a cada segundo e como usar uma regra para chamar uma ação a cada segundo.

1. Obtenha uma descrição do feed no pacote `/whisk.system/alarms`.
  ```
  ibmcloud fn package get --summary /whisk.system/alarms
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /whisk.system/alarms
   feed   /whisk.system/alarms/alarm
  ```
  {: screen}

  ```
  ibmcloud fn action get --summary /whisk.system/alarms/alarm
  ```
  {: pre}

  Exemplo de Saída:
  ```
  action /whisk.system/alarms/alarm: disparar acionador quando o alarme ocorrer
     (parâmetros: cron trigger_payload)
  ```
  {: screen}

  O feed `/whisk.system/alarms/alarm` usa dois parâmetros:
  - `cron`: uma especificação de crontab de quando disparar o acionador.
  - `trigger_payload`: o valor de parâmetro de payload para configurar em cada evento acionador.

2. Crie um acionador que dispare a cada 8 segundos.
  ```
  ibmcloud fn trigger create everyEightSeconds --feed /whisk.system/alarms/alarm -p cron "*/8 * * * * *" -p trigger_payload "{\"name\":\"Mork\", \"place\":\"Ork\"}"
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: feed acionador everyEightSeconds criado
  ```
  {: screen}

3. Crie um arquivo nomeado **hello.js** com o código de ação a seguir:
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

4. Certifique-se de que a ação existe.
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

5. Crie uma regra que chama a ação **hello** toda vez que o acionador `everyEightSeconds` é disparado.
  ```
  ibmcloud fn rule create myRule everyEightSeconds hello
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: regra myRule criada
  ```
  {: screen}

6. Verifique se a ação está sendo chamada pesquisando os logs de ativação.
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  É possível ver que as ativações são observadas a cada 8 segundos para o acionador, a regra e a ação. A ação recebe os parâmetros `{"name":"Mork", "place":"Ork"}` em cada chamada.

## Criar um pacote
{: #openwhisk_packages_create}

Um pacote é usado para organizar um conjunto de ações e feeds relacionados.
Também permite que os parâmetros sejam compartilhados entre todas as entidades no pacote.

Para criar um pacote customizado com uma ação simples nele, tente o exemplo a seguir:

1. Crie um pacote chamado **custom**.
  ```
  ibmcloud fn package create custom
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: pacote custom criado
  ```
  {: screen}

2. Obtenha um resumo do pacote.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /myNamespace/custom
  ```
  {: screen}

  Observe que o pacote está vazio.

3. Crie um arquivo chamado `identity.js` que contenha o código de ação a seguir. Essa ação retorna todos os parâmetros de entrada.
  ```javascript
  function main(args) { return args; }
  ```
  {: codeblock}

4. Crie uma ação chamada **identity** no pacote `custom`.
  ```
  ibmcloud fn action create custom/identity identity.js
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: ação custom/identity criada
  ```
  {: screen}

  A criação de uma ação em um pacote requer que o nome da ação tenha como prefixo um nome de pacote. Aninhamento de pacote não é permitido. Um pacote pode conter apenas ações e não pode conter outro pacote.

5. Obtenha um resumo do pacote novamente.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /myNamespace/custom
   ação /myNamespace/custom/identity
  ```
  {: screen}

  É possível ver a ação **custom/identity** em seu namespace agora.

6. Chame a ação no pacote.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {}
  ```
  {: screen}

É possível configurar parâmetros padrão para todas as entidades em um pacote configurando os parâmetros no nível do pacote que são herdados por todas as ações no pacote. Para ver como essa herança funciona, tente o exemplo a seguir:

1. Atualize o pacote **custom** com dois parâmetros: `city` e `country`.
  ```
  ibmcloud fn package update custom --param city Austin --param country USA
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: pacote custom atualizado
  ```
  {: screen}

2. Exiba os parâmetros no pacote **custom** e ação **identidy** e veja como a ação **identity** no pacote herda os parâmetros do pacote.
  ```
  ibmcloud fn package get custom parameters
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: got package custom, displaying field parameters

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
  {: screen}

  ```
  ibmcloud fn action get custom/identity parameters
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: got action custom/identity, displaying field parameters

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
  {: screen}

3. Chame a ação **identity** sem nenhum parâmetro para verificar se a ação realmente herda os parâmetros.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "city": "Austin",
      "country": "USA"
  }
  ```
  {: screen}

4. Chame a ação **identity** com alguns parâmetros. Parâmetros de chamada são mesclados com os parâmetros do pacote; os parâmetros de chamada substituem os parâmetros do pacote.
  ```
  ibmcloud fn action invoke --blocking --result custom/identity --param city Dallas --param state Texas
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "city": "Dallas",
      "country": "USA",
      "state": "Texas"
  }
  ```
  {: screen}

## Compartilhar um pacote
{: #openwhisk_packages_share}

Após as ações e os feeds que formam um pacote serem depurados e testados, o pacote pode ser compartilhado com todos os usuários do {{site.data.keyword.openwhisk_short}}. Compartilhar o pacote possibilita que os usuários liguem o pacote, chamem ações no pacote e criem regras e ações de sequência do {{site.data.keyword.openwhisk_short}}.

1. Compartilhe o pacote com todos os usuários:
  ```
  ibmcloud fn package update custom --shared yes
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: pacote custom atualizado
  ```
  {: screen}

2. Exiba a propriedade `publish` do pacote para verificar se agora é true.
  ```
  ibmcloud fn package get custom publish
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: got package custom, displaying field publish

  true
  ```
  {: screen}

Outros usuários agora podem usar seu pacote **custom**, incluindo ligação com o pacote ou chamando diretamente uma ação no mesmo. Outros usuários devem saber os nomes completos do pacote para ligá-lo ou chamar ações nele. As ações e os feeds dentro de um pacote compartilhado são _públicos_. Se
o pacote for privado, então, todo o seu conteúdo também será privado.

1. Obtenha uma descrição do pacote para mostrar os nomes completos do pacote e da ação.
  ```
  ibmcloud fn package get --summary custom
  ```
  {: pre}

  Exemplo de Saída:
  ```
  pacote /myNamespace/custom
   ação /myNamespace/custom/identity
  ```
  {: screen}

  No exemplo anterior, você está trabalhando com o namespace **myNamespace** e esse namespace aparece no nome completo.
