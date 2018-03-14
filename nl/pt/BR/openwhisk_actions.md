---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Criar e chamar ações
{: #openwhisk_actions}

Ações são fragmentos de código stateless executados na plataforma {{site.data.keyword.openwhisk}}. Por exemplo, uma ação pode ser usada para detectar as faces em uma imagem, responder a uma mudança do banco de dados, agregar um conjunto de chamadas API ou postar um Tweet. Uma ação pode ser gravada como uma função JavaScript, Swift, Python, PHP, método Java ou qualquer executável compatível com binário, incluindo programas Go e executáveis customizados empacotados como contêineres do Docker.
{:shortdesc}

As ações podem ser chamadas explicitamente ou executar em resposta a um evento. Em qualquer caso, cada execução de uma ação resulta em um registro de ativação que é identificado por um ID de ativação exclusivo. A entrada para uma ação e o resultado de uma ação são um dicionário de pares de valores de chaves, em que a chave é uma sequência e o valor é um valor JSON válido. As ações também podem ser compostas por chamadas a outras ações ou uma sequência definida de ações.

Saiba como criar, chamar e depurar ações em seu ambiente de desenvolvimento preferencial:
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Ir
](#creating-go-actions)
* [Executáveis arbitrários](#creating-actions-arbitrary)

Além disso, saiba mais sobre:
* [Observando a ação de saída](#watching-action-output)
* [Suporte a aplicativos grandes](#large-app-support)
* [Listando ações](#listing-actions)
* [Excluindo Ações](#deleting-actions)
* [Acessando metadados de ação dentro do corpo de ação](#accessing-action-metadata-within-the-action-body)


## Criar e chamar ações do JavaScript
{: #creating-and-invoking-javascript-actions}

As seções a seguir conduzem você pelo trabalho com ações em JavaScript. Você começa com a criação e a chamada de uma ação simples. Em seguida, move para a inclusão de parâmetros em uma ação e a chamada dessa ação com os parâmetros. Em seguida, você configura os parâmetros padrão e os chama. Depois, cria ações assíncronas e finalmente trabalha com sequências de ações.


### Criar e chamar uma ação simples do JavaScript
{: #openwhisk_single_action_js}

Revise as etapas e os exemplos a seguir para criar sua primeira ação JavaScript.

1. Crie um arquivo JavaScript com o conteúdo a seguir. Para esse exemplo, o nome do arquivo é 'hello.js'.

  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  O arquivo JavaScript pode conter funções adicionais. No entanto, por convenção, uma função chamada `main` deve existir para fornecer o ponto de entrada para a ação.

2. Crie uma ação a partir da função JavaScript a seguir. Para este exemplo, a ação é chamada 'hello'.

  ```
  wsk action create hello hello.js
  ```
  {: pre}

  ```
  ok: ação hello criada
  ```
  A CLI supõe automaticamente o tipo da ação usando a extensão do arquivo de origem. Para arquivos de origem `.js`, a ação é executada usando um tempo de execução do Node.js 6. Também é possível criar uma ação que é executada com o Node.js 8 especificando explicitamente o parâmetro `--kind nodejs:8`. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_javascript_environments) do Node.js 6 vs 8.
  
3. Liste as ações que você criou:

  ```
  wsk action list
  ```
  {: pre}

  ```
  ações
  hello       privada
  ```

  É possível ver a ação `hello` que você criou.

4. Após você criar a sua ação, será possível executá-la na nuvem no OpenWhisk com o comando 'invoke'. É possível chamar ações com uma chamada de *bloco* (ou seja, estilo de solicitação/resposta) ou uma chamada *sem bloqueio* especificando uma sinalização no comando. Uma solicitação de chamada de bloqueio _espera_ o resultado de ativação ficar disponível. O período de espera é o menor de 60 segundos ou o [valor limite de tempo](./openwhisk_reference.html#openwhisk_syslimits) da ação. O resultado da ativação será retornado se ele estiver disponível dentro do período de espera. Caso contrário, a ativação continua o processamento no sistema e um ID de ativação é retornado para que se possa verificar o resultado posteriormente, como com solicitações sem bloqueio (veja [aqui](#watching-action-output) para obter dicas sobre monitoramento de ativações).

  Este exemplo usa o parâmetro de bloqueio, `--blocking`:

  ```
  wsk action invoke --blocking hello
  ```
  {: pre}

  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```

  ```json
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```

  A saída de comando inclui duas informações importantes:
  * O ID de ativação (`44794bd6aab74415b4e42a308d880e5b`)
  * O resultado da chamada se ele estiver disponível dentro do período de espera estimado

  O resultado nesse caso é a sequência `Hello world` retornada pela função JavaScript. O ID de ativação pode ser usado para recuperar os logs ou o resultado da chamada em um momento futuro.  

5. Se você não precisar do resultado da ação imediatamente, será possível omitir a sinalização `--blocking` para fazer uma chamada sem bloqueio. É possível obter o resultado posteriormente usando o ID da ativação. Veja o exemplo a seguir:

  ```
  wsk action invoke hello
  ```
  {: pre}

  ```
  ok: hello chamada com id 6bf1f670ee614a7eb5af3c9fde813043
  ```

  ```
  wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  ```json
  {
      "payload": "Hello world"
  }
  ```

6. Se esquecer de registrar o ID da ativação, será possível obter uma lista de ativações ordenadas da mais recente até a mais antiga. Execute o comando a seguir para obter uma lista de suas ativações:

  ```
  wsk activation list
  ```
  {: pre}

  ```
  ativações
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```

### Passar parâmetros para uma ação
{: #openwhisk_pass_params}

Os parâmetros podem ser passados para a ação quando for chamada.

1. Use parâmetros na ação. Por exemplo, atualize o arquivo 'hello.js' com o conteúdo a seguir:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Os parâmetros de entrada são passados como um parâmetro de objeto JSON para a função `main`. Observe como os parâmetros `name` e `place` são recuperados a partir do objeto `params` neste exemplo.

2. Atualize e chame a ação `hello`, enquanto passa os valores de parâmetro `name` e `place`. Veja o exemplo a seguir:

  ```
  wsk action update hello hello.js
  ```
  {: pre}

  Se você precisar modificar seus parâmetros de credenciais sem serviço, esteja ciente de que executar um comando `action update` com novos parâmetros remove quaisquer parâmetros que existem atualmente, mas não estão especificados no comando `action update`. Por exemplo, se houver dois parâmetros além do `__bx_creds`, com chaves nomeadas key1 e key2. Se você executar um comando `action update` com `-p key1 new-value -p key2 new-value`, mas omitir o parâmetro `__bx_creds`, o parâmetro `__bx_creds` não existirá mais após o `action update` ser concluído com êxito. Deve-se, então, religar as credenciais de serviço. Essa é uma limitação conhecida sem uma solução alternativa.
  {: tip}  

3.  Os parâmetros podem ser fornecidos explicitamente na linha de comandos ou fornecendo um arquivo que contém os parâmetros desejados.

  Para passar os parâmetros diretamente pela linha de comandos, forneça um par de chave/valor para a sinalização `--param`:
  ```
  wsk action invoke --result hello --param name Bernie --param place Vermont
  ```
  {: pre}

  Para usar um arquivo que contém conteúdo de parâmetro, crie um arquivo que contém os parâmetros no formato JSON. O nome do arquivo deve então ser passado para a sinalização `param-file`:

  Veja o arquivo de parâmetro de exemplo a seguir chamado `parameters.json`:
  ```json
  {
      "name": "Bernie",
      "place": "Vermont"
  }
  ```

  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Observe o uso da opção `--result`: isso significa uma chamada de bloqueio em que a CLI aguarda a ativação ser concluída e, em seguida, exibe somente o resultado. Por conveniência, essa opção pode ser usada sem `--blocking` que é inferido automaticamente.

  Além disso, se os valores de parâmetro que estiverem especificados na linha de comandos forem JSON válido, eles serão analisados e enviados para a sua ação como um objeto estruturado. Por exemplo, atualize a ação hello para o seguinte:

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  Agora a ação espera que um único parâmetro `person` tenha os campos `name` e `place`. Em seguida, chame a ação com um único parâmetro `person` parâmetro que é um JSON válido, como no exemplo a seguir:

  ```
  wsk action invoke --result hello -p person '{"name": "Bernie", "place": "Vermont"}'
  ```
  {: pre}

  O resultado será o mesmo porque a CLI analisará automaticamente o valor de parâmetro `person` para o objeto estruturado que a ação agora espera:
  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

### Configurando parâmetros padrão
{: #openwhisk_binding_actions}

Ações podem ser chamadas com vários parâmetros denominados. Lembre-se de que a ação `hello` do exemplo anterior espera dois parâmetros: *name* de uma pessoa e *place* de onde ela é.

Em vez de passar todos os parâmetros para uma ação toda vez, é possível fazer a ligação de determinados parâmetros. O exemplo a seguir liga o parâmetro *place* para que a ação use como padrão o local "Vermont":

1. Atualize a ação usando a opção `--param` para ligar os valores de parâmetros ou passando um arquivo que contenha os parâmetros para `--param-file`

  Para especificar os parâmetros padrão explicitamente na linha de comandos, forneça um par de chave/valor para a sinalização `param`:

  ```
  wsk action update hello --param place Vermont
  ```
  {: pre}

  Passar os parâmetros de um arquivo requer a criação de um arquivo que contém o conteúdo desejado no formato JSON. O nome do arquivo deve então ser passado para a sinalização `-param-file`:

  Veja o arquivo de parâmetro de exemplo a seguir chamado `parameters.json`:
  ```json
  {
      "place": "Vermont"
  }
  ```
  {: codeblock}

  ```
  wsk action update hello --param-file parameters.json
  ```
  {: pre}

2. Chame a ação, passando somente o parâmetro `name` desta vez.

  ```
  wsk action invoke --result hello --param name Bernie
  ```
  {: pre}

  ```json
  {
      "payload": "Hello, Bernie from Vermont"
  }
  ```

  Observe que você não precisou especificar o parâmetro place quando chamou a ação. Os parâmetros ligados ainda podem ser substituídos especificando o valor de parâmetro no momento da chamada.

3. Chame a ação, passando os valores `name` e `place`. O último sobrescreve o valor que está ligado à ação.

  Usando a sinalização `--param`:

  ```
  wsk action invoke --result hello --param name Bernie --param place "Washington, DC"
  ```
  {: pre}

  Usando a sinalização `--param-file`:

  Arquivo parameters.json:
  ```json
  {
    "name": "Bernie",
    "place": "Vermont"
  }
  ```
  {: codeblock}
  ```
  wsk action invoke --result hello --param-file parameters.json
  ```
  {: pre}

  ```json
  {  
      "payload": "Hello, Bernie from Washington, DC"
  }
  ```

### Obter uma URL de ação

Uma ação pode ser chamada pela interface REST por meio de uma solicitação de HTTPS. Para obter uma URL de ação, execute o comando a seguir:

```
wsk action get actionName --url
```
{: pre}

```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```

A autenticação deve ser fornecida ao chamar uma ação por meio de uma solicitação de HTTPS. Para obter mais informações sobre
as chamadas de ação usando a interface REST, veja [Usando APIs de REST com o OpenWhisk](./openwhisk_rest_api.html#openwhisk_rest_api).
{: tip}

### Salvar código de ação

O código associado a uma ação existente é buscado e salvo localmente. O salvamento é executado em todas as ações, exceto sequências e ações do docker. Ao salvar o código de ação em um arquivo, o código é salvo no diretório atualmente em funcionamento e o caminho de arquivo salvo é exibido.

1. Salve o código de ação em um nome de arquivo que corresponda a um nome de ação existente. Uma extensão de arquivo que corresponde ao tipo de ação é usada ou uma extensão do tipo `.zip` será usada para o código de ação que é um arquivo zip.
  ```
  wsk action get actionName --save
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```

2. Em vez de permitir que a CLI determine o nome do arquivo e a extensão do código salvo, um nome de arquivo customizado e uma extensão podem ser fornecidos usando a sinalização `--save-as`.
  ```
  wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```

### Criar ações assíncronas
{: #openwhisk_asynchrony_js}

As funções JavaScript que são executadas de forma assíncrona podem retornar o resultado da ativação após o retorno da função `main`, retornando uma Promessa em sua ação.

1. Salve o conteúdo a seguir em um arquivo chamado `asyncAction.js`.

  ```javascript
  function main(args) {
       return new Promise(function(resolve, reject) {
         setTimeout(function() {
           resolve({ done: true });
         }, 2000);
      })
   }
  ```
  {: codeblock}

  Observe que a função `main` retorna uma Promessa, que indica que a ativação não foi concluída ainda, mas espera-se no futuro.

  A função JavaScript `setTimeout()` nesse caso aguarda 2 segundos antes de chamar a função de retorno de chamada, que representa o código assíncrono e vai dentro da função de retorno de chamada da Promessa.

  O retorno de chamada da Promessa aceita dois argumentos, resolver e rejeitar, que são ambos funções.  A chamada para `resolve()` cumpre a Promessa e indica que a ativação é concluída normalmente.

  Uma chamada para `reject()` pode ser usada para rejeitar a Promessa e sinalizar que a ativação é concluída de forma anormal.

2. Execute os comandos a seguir para criar a ação e chamá-la:

  ```
  wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  ```
  Chamar a ação wsk -- asyncAction resultado
  ```
  {: pre}

  ```json
  {
      "done": true
  }
  ```

  Observe que você executou uma chamada de bloqueio de uma ação assíncrona.

3. Busque o log de ativação para ver quanto tempo a ativação levou para concluir:

  ```
  wsk activation list --limit 1 asyncAction
  ```
  {: pre}
  
  ```
  ativações
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```

  ```
  wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}
 
  ```json
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```

  Comparando os registros de data e hora `start` e `end` no registro de ativação, é possível ver que essa ativação levou um pouco mais de 2 segundos para ser concluída.

### Use ações para chamar uma API externa
{: #openwhisk_apicall_action}

Os exemplos até agora são funções JavaScript autocontidas. Também é possível criar uma ação que chama uma API externa.

Este exemplo chama um serviço Yahoo Weather para obter as condições atuais em um local específico.

1. Salvar o conteúdo a seguir em um arquivo chamado `weather.js`.

  ```javascript
  var request = require('request');

  function main(params) {
      var location = params.location || 'Vermont';
      var url = 'https://query.yahooapis.com/v1/public/yql?q=select item.condition from weather.forecast where woeid in (select woeid from geo.places(1) where text="' + location + '")&format=json';

      return new Promise(function(resolve, reject) {
          request.get(url, function(error, response, body) {
              if (error) {
                  reject(error);
              }
              else {
                  var condition = JSON.parse(body).query.results.channel.item.condition;
                  var text = condition.text;
                  var temperature = condition.temp;
                  var output = 'It is ' + temperature + ' degrees in ' + location + ' and ' + text;
                  resolve({msg: output});
              }
          });
      });
  }
  ```
  {: codeblock}

 A ação no exemplo usa a biblioteca JavaScript `request` para fazer uma solicitação de HTTP para a API do Yahoo Weather e extrai campos do resultado JSON. As [Referências](./openwhisk_reference.html#openwhisk_ref_javascript_environments) detalham os pacotes do Node.js que podem ser usados em suas ações.

  Este exemplo também mostra a necessidade de ações assíncronas. A ação retorna uma Promessa para indicar que o resultado dessa ação não está disponível ainda quando a função é retornada. Em vez disso, o resultado está disponível no retorno de chamada `request` após a chamada HTTP ser concluída e é passado como um argumento para a função `resolve()`.

2. Execute os comandos a seguir para criar a ação e chamá-la:

  ```
  wsk action create weather weather.js
  ```
  {: pre}

  ```
  wsk action invoke --result weather --param location "Brooklyn, NY"
  ```
  {: pre}

  ```json
  {
      "msg": "It is 28 degrees in Brooklyn, NY and Cloudy"
  }
  ```

### Empacotar uma ação como um módulo Node.js
{: #openwhisk_js_packaged_action}

Como uma alternativa para gravar todo o seu código de ação em um único arquivo de origem JavaScript, é possível gravar uma ação como um pacote `npm`. Considere como um exemplo um diretório com os seguintes arquivos:

Primeiro, `package.json`:

```json
{
  "name": "my-action",
  "main": "index.js",
  "dependencies" : {
    "left-pad" : "1.1.3"
  }
}
```
{: codeblock}

Então, `index.js`:

```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}

exports.main = myAction;
```
{: codeblock}

A ação é exposta por meio de `exports.main`. O manipulador de ações em si pode ter qualquer nome, contanto que se adeque à assinatura usual de aceitação e retorno de um objeto (ou um `Promise` de um objeto). Por convenção do Node.js, deve-se nomear esse arquivo como `index.js` ou especificar o nome do arquivo que você preferir como a propriedade `main` no package.json.

Para criar uma ação do OpenWhisk a partir deste pacote:

1. Instale primeiro todas as dependências localmente

  ```
  npm install
  ```
  {: pre}

2. Crie um archive `.zip` contendo todos os arquivos (incluindo todas as dependências):

  ```
  zip -r action.zip *
  ```
  {: pre}

  Usar a ação do Windows Explorer para criar o arquivo zip resulta em uma estrutura incorreta. As ações zip do OpenWhisk devem ter o `package.json` na raiz do zip, enquanto o Windows Explorer o coloca dentro de uma pasta aninhada. A opção mais segura é usar o comando `zip` da linha de comandos.
  {: tip}

3. Crie a ação:

  ```
  wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Ao criar uma ação de um archive `.zip` com a ferramenta CLI, deve-se fornecer explicitamente um valor para a sinalização `--kind` usando `nodejs:6` ou `nodejs:8`.

4. É possível chamar a ação como qualquer outra:

  ```
  wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}
  
  ```json
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```

Finalmente, observe que enquanto a maioria dos pacotes `npm` instala fontes JavaScript em `npm install`, alguns também instalam e compilam os artefatos binários. O upload do archive atualmente não suporta dependências binárias, mas apenas as dependências JavaScript. As chamadas de ação poderão falhar se o archive incluir dependências binárias.

### Empacotar uma ação como um único pacote configurável
{: #openwhisk_js_webpack_action}

É conveniente incluir somente o código mínimo em um único arquivo `.js` que inclua dependências. Essa abordagem permite implementações mais rápidas e em algumas circunstâncias nas quais o empacotamento da ação como um zip pode ser muito grande porque inclui arquivos desnecessários.

É possível usar um bundler do módulo JavaScript como [webpack](https://webpack.js.org/concepts/). Quando o webpack processa seu código, ele constrói recursivamente um gráfico de dependência que inclui cada módulo que a ação precisa.

Aqui está um exemplo rápido usando webpack:

Tomando o `package.json` do exemplo anterior, inclua `webpack` como uma dependência de desenvolvimento e inclua alguns comandos de script npm.
```json
{
  "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "deploy": "bx wsk action update my-action dist/bundle.js --kind nodejs:8"
  },
  "dependencies": {
    "left-pad" : "1.1.3"
  },
  "devDependencies": {
    "webpack": "^3.8.1"
  }
}
```
{: codeblock}

Crie o arquivo de configuração do webpack `webpack.config.js`.
```javascript
var path = require('path');
module.exports = {
  entry: './index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  target: 'node'
};
```
{: codeblock}

Configure a variável `global.main` para a função principal da ação.
Do exemplo anterior:
```javascript
function myAction(args) {
    const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
}
global.main = myAction;
```
{: codeblock}

Se seu nome da função for `main`, use esta sintaxe:
```javascript
global.main = main;
```
{: codeblock}


Para construir e implementar uma Ação do OpenWhisk usando `npm` e `webpack`:

1. Primeiro, instale as dependências localmente:

  ```
  npm install
  ```
  {: pre}

2. Construa o pacote configurável do webpack:

  ```
  npm run build
  ```
  {: pre}

  O arquivo `dist/bundle.js` é criado e usado para ser implementado como o código-fonte da ação.

3. Crie a ação usando o script `npm` ou a CLI.
  Usando o script `npm`:
  ```
  npm run deploy
  ```
  {: pre}
  Usando a CLI:
  ```
  bx wsk action update my-action dist/bundle.js
  ```
  {: pre}


Finalmente, o arquivo de pacote configurável que é construído pelo `webpack` não suporta dependências binárias, mas as dependências de JavaScript. Portanto, as chamadas de ação falharão se o pacote configurável depender das dependências binárias, porque isso não é incluído com o arquivo `bundle.js`.

## Criar sequências de ações
{: #openwhisk_create_action_sequence}

É possível criar uma ação que encadeia uma sequência de ações.

Várias ações do utilitário são fornecidas em um pacote que é chamado `/whisk.system/utils` que pode ser usado para criar a primeira sequência. É possível aprender mais sobre pacotes na seção [Pacotes](./openwhisk_packages.html).

1. Exiba as ações no pacote `/whisk.system/utils`.

  ```
  wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  ```
  package /whisk.system/utils: construindo blocos que formatam e montam dados
   action /whisk.system/utils/head: extrair prefixo de uma matriz
   action /whisk.system/utils/split: dividir uma sequência em uma matriz
   action /whisk.system/utils/sort: classifica uma matriz
   action /whisk.system/utils/echo: retorna a entrada
   action /whisk.system/utils/date: data e hora atual
   action /whisk.system/utils/cat: concatena a entrada em uma sequência
  ```

  Você está usando as ações `split` e `sort` nesse exemplo.

2. Crie uma sequência de ações de modo que o resultado de uma ação seja passado como um argumento para a próxima ação.

  ```
  wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Essa sequência de ações converte algumas linhas de texto a uma matriz e classifica as linhas.

3. Chame a ação:

  ```
  wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  ```json
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```

  No resultado, você vê que as linhas estão classificadas.

**Nota**: os parâmetros passados entre ações na sequência são explícitos, exceto parâmetros padrão.
Portanto, os parâmetros passados para a sequência de ações estão disponíveis somente para a primeira ação na sequência.
O resultado da primeira ação na sequência torna-se o objeto JSON de entrada para a segunda ação na sequência (e assim por diante).
Esse objeto não inclui nenhum dos parâmetros que são originalmente passados para a sequência, a menos que a primeira ação os inclua explicitamente em seu resultado.
Os parâmetros de entrada para uma ação são mesclados com os parâmetros padrão da ação, com o antigo tendo precedência e substituindo qualquer parâmetro padrão correspondente.
Para obter mais informações sobre como chamar sequências de ações com múltiplos parâmetros nomeados, consulte [Configurando parâmetros padrão](./openwhisk_actions.html#openwhisk_binding_actions).

## Criar ações do Python
{: #creating-python-actions}

O processo de criação de ações do Python é semelhante ao de ações do JavaScript. As seções a seguir orientam você na criação e chamada de uma única ação Python e na inclusão de parâmetros nessa ação.

### Criar e chamar uma ação do Python
{: #openwhisk_actions_python_invoke}

Uma ação é simplesmente uma função Python de nível superior. Por exemplo, crie um arquivo chamado
`hello.py` com o código de origem a seguir:

```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

As ações do Python sempre consomem um dicionário e produzem um dicionário. O método de entrada para a ação é `main` por padrão, mas pode ser especificado explicitamente para criar a ação com a CLI `wsk` usando `--main`, como com qualquer outro tipo de ação.

É possível criar uma ação OpenWhisk chamada `helloPython` a partir
dessa função da seguinte forma:
```
wsk action create helloPython hello.py
```
{: pre}

A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem. Para arquivos de origem `.py`, a ação é executada usando um tempo de execução do Python 2. Também é possível criar uma ação executada com o Python 3 especificando explicitamente o parâmetro `--kind python:3`.
Além disso, há um tempo de execução do Python 3 com o tipo `python-jessie:3` que contém pacotes adicionais para o IBM Cloud Services, como o IBM Cloudant, IBM DB2, IBM COS e IBM Watson.
Para obter mais informações sobre pacotes incluídos nesse tempo de execução do Python 3, veja a [referência](./openwhisk_reference.html#openwhisk_ref_python_environments) do tempo de execução do Python.

A chamada de ação para ações do Python é a mesma que para ações do JavaScript:
```
wsk action invoke --result helloPython --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Empacotar ações do Python em arquivos zip
{: #openwhisk_actions_python_zip}

É possível empacotar uma ação do Python e os módulos dependentes em um arquivo zip.
O nome do arquivo de origem que contém o ponto de entrada (por exemplo, `main`) deve ser `__main__.py`.
Por exemplo, para criar uma ação com um módulo auxiliar chamado `helper.py`, crie primeiramente um archive contendo seus arquivos de origem:

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}

Em seguida, crie a ação:

```bash
wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Embora essas etapas sejam mostradas para o Python 3 (com tipo `python:3`), é possível fazer o mesmo com tipos alternativos do Python `python:2` ou `python-jessie:3`.


### Empacotar ações do Python com um ambiente virtual em arquivos zip
{: #openwhisk_actions_python_virtualenv}

Outra maneira de empacotar dependências do Python é usando um ambiente virtual (`virtualenv`) que permite vincular pacotes adicionais que podem ser instalados por meio de [`pip`](https://packaging.python.org/installing/) por exemplo.


Tal como com o suporte de arquivo zip básico, o nome do arquivo de origem que contém o ponto de entrada principal deve ser `__main__.py`. Para esclarecer, o conteúdo de `__main__.py` é a função principal, então para este exemplo é possível renomear `hello.py` para `__main__.py` na seção anterior. Além disso, o diretório virtualenv deve ser denominado `virtualenv`. Veja o cenário de exemplo a seguir para instalar dependências, empacotando-as em um virtualenv e criando uma ação do OpenWhisk compatível.

Para assegurar a compatibilidade com o contêiner de tempo de execução do OpenWhisk, as instalações de pacotes dentro de um virtualenv devem ser feitas no ambiente de destino usando a imagem correspondente para o tipo.
- Para o tipo `python:2`, use a imagem do docker `openwhisk/python2action`.
- Para o tipo `python:3`, use a imagem do docker `openwhisk/python3action`.
- Para o tipo `python-jessie:3`, use a imagem do docker `ibmfunctions/action-python-v3`.

1. Dado um arquivo [requirements.txt ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) que contém os módulos e versões `pip` a serem instalados, execute o seguinte para instalar as dependências e crie um virtualenv usando uma imagem do Docker compatível:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
    ```
    {: pre}

2. Arquive o diretório virtualenv e quaisquer arquivos adicionais do Python:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Crie a ação:
    ```
    wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}


## Criar ações do PHP
{: #creating-php-actions}

O processo de criação de ações do PHP é semelhante ao de ações do JavaScript. As seções a seguir fornecem orientação durante a criação e chamada de uma única ação PHP e na inclusão de parâmetros nessa ação.

### Criar e chamar uma ação do PHP
{: #openwhisk_actions_php_invoke}

Uma ação é simplesmente uma função PHP de nível superior. Por exemplo, crie um arquivo chamado `hello.php` com o código-fonte a seguir:

```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
```

As ações do PHP sempre consomem uma matriz associativa e retornam uma matriz associativa. O método de entrada para a ação é `main` por padrão, mas pode ser especificado explicitamente ao criar a ação com a CLI `wsk` usando `--main`, como com qualquer outro tipo de ação.

É possível criar uma ação OpenWhisk chamada `helloPHP` por meio dessa função, como a seguir:

```
wsk action create helloPHP hello.php
```
{: pre}

A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem. Para arquivos de origem `.php`, a ação é executada usando um tempo de execução do PHP 7.1. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_php) do PHP.

A chamada de ação para ações do PHP é a mesma que para ações do JavaScript:

```
wsk action invoke --result helloPHP --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

### Empacotar ações do PHP em arquivos zip
{: #openwhisk_actions_php_zip}

É possível empacotar uma ação PHP junto a outros arquivos e pacotes dependentes em um arquivo zip.
O nome do arquivo de origem que contém o ponto de entrada (por exemplo, `main`) deve ser `index.php`.
Por exemplo, para criar uma ação que inclua um segundo arquivo chamado `helper.php`, primeiro crie um archive que contenha seus arquivos de origem:

```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Em seguida, crie a ação:

```bash
wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Criar ações do Swift
{: #creating-swift-actions}

O processo de criação de ações do Swift é semelhante ao de ações do JavaScript. As seções a seguir o guiam pela criação e chamada de uma única ação swift e a inclusão de parâmetros nessa ação.

Também é possível usar o [Ambiente de simulação do Swift ](https://swiftlang.ng.bluemix.net) para testar seu código Swift sem precisar instalar o Xcode em sua máquina.

### Criar e chamar uma ação

Uma ação é simplesmente uma função Swift de nível superior. Por exemplo, crie um arquivo chamado
`hello.swift` com o conteúdo a seguir:

```swift
func main(args: [String:Any]) -> [String:Any] {
    if let name = args["name"] as? String {
        return [ "greeting" : "Hello \(name)!" ]
    } else {
        return [ "greeting" : "Hello stranger!" ]
    }
}
```
{: codeblock}

As ações do Swift sempre consomem um dicionário e produzem um dicionário.

É possível criar uma ação do {{site.data.keyword.openwhisk_short}} chamada `helloSwift` a partir desta função da seguinte
forma:

```
wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}
 

Sempre especifique `swift:3.1.1`, pois versões anteriores do Swift não são suportadas.
{: tip}

A chamada de ação para ações do Swift é a mesma que para ações do JavaScript:

```
wsk action invoke --result helloSwift --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

**Atenção:** as ações do Swift que são executadas em um ambiente Linux ainda estão em desenvolvimento e o {{site.data.keyword.openwhisk_short}} geralmente usa a liberação mais recente disponível, que não é necessariamente estável. Além disso, a versão do Swift usada com o {{site.data.keyword.openwhisk_short}} pode estar inconsistente com versões do Swift de liberações estáveis do XCode no MacOS.

### Empacotar uma ação como um executável do Swift
{: #openwhisk_actions_swift_zip}

Ao criar uma ação do Swift do OpenWhisk com um arquivo de origem do Swift, ele precisa ser compilado em um binário antes de a ação ser executada. Uma vez feito isso, as chamadas subsequentes para a ação são muito mais rápidas até que o contêiner que retém sua ação seja limpo. Esse atraso é conhecido como o atraso de cold start.

Para evitar o atraso de cold start, é possível compilar seu arquivo Swift em um
binário e, em seguida, fazer upload dele para o OpenWhisk em um arquivo zip. Como você precisa do andaime do OpenWhisk, a maneira mais fácil de criar o binário é construí-lo no mesmo ambiente em que é executado. Veja as etapas a seguir:

- Execute um contêiner de ações interativas do Swift usando o comando a seguir:
  ```
  docker run --rm -it -v "$(pwd):/owexec" openwhisk/action-swift-v3.1.1 bash
  ```
  {: pre}
  
- Copie o código-fonte e prepare para construí-lo.
  ```
  cp /owexec/hello.swift /swift3Action/spm-build/main.swift 
  ```
  {: pre}

  ```
  cat /swift3Action/epilogue.swift >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

  ```
  echo '_run_main(mainFunction:main)' >> /swift3Action/spm-build/main.swift
  ```
  {: pre}

- (Opcional) Crie o arquivo `Package.swift` para incluir dependências.
   ```
   swift import PackageDescription
   
   let package = Package(
     name: "Action",
         dependencies: [
             .Package(url: "https://github.com/apple/example-package-deckofplayingcards.git", majorVersion: 3),
             .Package(url: "https://github.com/IBM-Swift/CCurl.git", "0.2.3"),
             .Package(url: "https://github.com/IBM-Swift/Kitura-net.git", "1.7.10"),
             .Package(url: "https://github.com/IBM-Swift/SwiftyJSON.git", "15.0.1"),
             .Package(url: "https://github.com/watson-developer-cloud/swift-sdk.git", "0.16.0")
         ]
   )
   ```
   {: pre}

  Esse exemplo inclui as dependências `swift-watson-sdk` e `example-package-deckofplayingcards`.
  Observe que `CCurl`, `Kitura-net` e `SwiftyJSON` são fornecidos na ação padrão do Swift para que seja possível incluí-los em seu próprio `Package.swift`.

- Copie Package.swift para o diretório spm-build
  ```
  cp /owexec/Package.swift /swift3Action/spm-build/Package.swift
  ```
  {: pre}

- Mude para o diretório spm-build
  ```
  cd /swift3Action/spm-build
  ```
  {: pre}

- Compile sua ação do Swift.
  ```
  swift build -c release
  ```
  {: pre}

- Crie o archive zip.
  ```
  zip /owexec/hello.zip .build/release/Action
  ```
  {: pre}

- Saia do contêiner do Docker.
  ```
  exit
  ```
  {: pre}

É possível ver que hello.zip é criado no mesmo diretório que hello.swift. 

- Faça upload dele no OpenWhisk com o nome de ação helloSwifty:
  ```
  wsk action update helloSwiftly hello.zip --kind swift:3.1.1
  ```
  {: pre}

- Para verificar o quanto ele é mais rápido, execute 
  ```
  wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

O tempo que levou para a ação ser executada está na propriedade "duration" e é comparado com o tempo que leva para ela ser executada com uma etapa de compilação na ação hello.

## Criar ações do Java
{: #creating-java-actions}

O processo de criação de ações do Java é semelhante ao de ações do JavaScript e Swift. As seções a seguir orientam você na criação e chamada de uma única ação Java e na inclusão de parâmetros nessa ação.

Para compilar, testar e arquivar os arquivos Java, deve-se ter um [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) instalado localmente.

### Criar e chamar uma ação
{: #openwhisk_actions_java_invoke}

Uma ação Java é um programa Java com um método chamado `main` que tem a assinatura exata conforme a seguir:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Por exemplo, crie um arquivo Java chamado `Hello.java` com o conteúdo a seguir:

```java
import com.google.gson.JsonObject;
public class Hello {
    public static JsonObject main(JsonObject args) {
        String name = "stranger";
        if (args.has("name"))
            name = args.getAsJsonPrimitive("name").getAsString();
        JsonObject response = new JsonObject();
        response.addProperty("greeting", "Hello " + name + "!");
        return response;
    }
}
```
{: codeblock}

Em seguida, compile `Hello.java` em um arquivo JAR `hello.jar` conforme a seguir:
```
javac Hello.java
```
{: pre}

```
jar cvf hello.jar Hello.class
```
{: pre}

O [google-gson](https://github.com/google/gson) deve existir em seu CLASSPATH Java para compilar o arquivo Java.
{: tip}

É possível criar uma ação OpenWhisk chamada `helloJava` a partir desse arquivo JAR conforme
a seguir:

```
wsk action create helloJava hello.jar --main Hello
```
{: pre}

Ao usar a linha de comandos e um arquivo de origem `.jar`, não é necessário
especificar que você está criando uma ação Java;
a ferramenta determina isso a partir da extensão do arquivo.

É necessário especificar o nome da classe principal usando `--main`. Uma classe principal elegível é aquela que implementa um método `main` estático. Se a classe não estiver no pacote padrão, use o nome completo de classe Java, por exemplo, `--main com.example.MyMain`.

Se necessário, também é possível customizar o nome do método da ação do Java. Isso é feito especificando o nome completo do método Java de sua ação, por exemplo, `--main com.example.MyMain#methodName`.

A chamada de ação para ações do Java é a mesma que para ações do Swift e JavaScript:

```
wsk action invoke --result helloJava --param name World
```
{: pre}

```json
  {
      "greeting": "Hello World!"
  }
```

## Criar ações do Docker
{: #creating-docker-actions}

Com ações do Docker do {{site.data.keyword.openwhisk_short}}, é possível gravar suas ações em qualquer linguagem.

O seu código é compilado em um binário executável e integrado em uma imagem do Docker. O programa binário interage com o sistema aceitando entrada de `stdin` e respondendo por meio de `stdout`.

Como um pré-requisito, deve-se ter uma conta do Docker Hub.  Para configurar um ID e uma conta do Docker grátis, acesse o [Docker Hub](https://hub.docker.com).

Para as instruções a seguir, suponha que o ID do usuário do Docker seja `janesmith` e a senha seja `janes_password`.  Supondo que a CLI esteja configurada, três etapas restam para configurar um binário customizado para uso pelo {{site.data.keyword.openwhisk_short}}. Depois disso, a imagem do Docker transferida por upload poderá ser usada como uma ação.

1. Faça download da estrutura básica do Docker. É possível fazer download e instalá-la usando a CLI conforme a seguir:

  ```
  wsk sdk install docker
  ```
  {: pre}

  A estrutura básica do Docker agora está instalada no diretório atual.
  
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```

  A estrutura básica é um modelo do contêiner do Docker no qual é possível injetar seu código na forma de binários customizados.

2. Configure seu binário customizado na estrutura básica da caixa preta. A estrutura básica já inclui um programa C que pode ser usado.

  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

  É possível modificar esse arquivo conforme necessário ou incluir código e dependências adicionais na imagem do Docker.
  Nesse último, é possível ajustar o `Dockerfile` conforme necessário para construir o executável.
  O binário deve estar localizado dentro do contêiner em `/action/exec`.

  O executável recebe um único argumento a partir da linha de comandos. É uma serialização de sequência do objeto JSON que representa os argumentos para a ação. O programa pode efetuar log em `stdout` ou em `stderr`.
  Por convenção, a última linha de saída _deve_ ser um objeto JSON em sequência que representa o resultado da ação.

3. Construa a imagem do Docker e faça upload da mesma usando um script fornecido. Deve-se primeiro executar `docker login` para autenticação e, em seguida, executar o script com um nome de imagem escolhido.

  ```
  docker login -u janesmith -p janes_password
  ```
  {: pre}

  ```
  cd dockerSkeleton
  ```
  {: pre}

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  Observe que parte do arquivo `example.c` é compilada como parte do processo de construção de imagem do Docker, então você não precisa de C compilado em sua máquina.
  Na verdade, a menos que você esteja compilando o binário em uma máquina host compatível, ele não pode ser executado dentro do contêiner porque os formatos não correspondem.

  Seu contêiner do Docker agora pode ser usado como uma ação do OpenWhisk.


  ```
  wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Observe o uso de `--docker` para criar uma ação. Todas as imagens do Docker são presumidas como hospedadas no Docker Hub.
  A ação pode ser chamada como qualquer outra ação do {{site.data.keyword.openwhisk_short}}. 

  ```
  wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  ```json
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```

  Para atualizar a ação do Docker, execute `buildAndPush.sh` para fazer upload da imagem mais recente do Docker Hub. Isso permitirá que o sistema puxe a sua nova imagem do Docker na próxima vez em que ele executar o código para a sua ação. Se não houver nenhum contêiner quente, as novas chamadas usarão a nova imagem do Docker. No entanto, se houver um contêiner quente que use uma versão anterior de sua imagem do Docker, qualquer chamada nova continuará usando essa imagem, a menos que você execute `wsk action update`. Para novas chamadas, isso indica ao sistema para executar um pull do docker para obter sua nova imagem do Docker.

  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  ```
  wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  É possível localizar mais informações sobre como criar ações do Docker na seção [Referências](./openwhisk_reference.html#openwhisk_ref_docker).

  A versão anterior da CLI suportava `--docker` sem um parâmetro e o nome da imagem era um argumento posicional. Para permitir que as ações do Docker aceitem dados de inicialização por meio de um arquivo (zip), normalize a experiência do usuário para ações do Docker para que um argumento posicional, se presente, seja um arquivo (por exemplo, um arquivo zip). O nome da imagem deve ser especificado depois da opção `--docker`. Graças ao feedback do usuário, o argumento `--native` foi incluído como abreviação para `--docker openwhisk/dockerskeleton`, de maneira que os executáveis que são executados dentro do SDK de ação padrão do Docker são mais convenientes para criação e implementação.
  
  Por exemplo, este tutorial cria um executável binário dentro do contêiner localizado em `/action/exec`. Se você copiar esse arquivo para seu sistema de arquivos local e compactá-lo com zip `exec.zip`, será possível usar os comandos a seguir para criar uma ação do docker que receba o executável como dados de inicialização. 

  ```
  wsk action create example exec.zip --native
  ```
  {: pre}

  Que é equivalente ao comando a seguir. 
  ```
  wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Criando ações usando executáveis arbitrários
{: #creating-actions-arbitrary}

Usando o `--native`, é possível ver que qualquer executável pode ser executado como uma ação do OpenWhisk. Isso inclui scripts de `bash` ou binários compilados cruzados. Para esse último, a restrição é que o binário deve ser compatível com a imagem `openwhisk/dockerskeleton`.

## Criando ações do Go
{: #creating-go-actions}

A opção `--native` permite o empacotamento de qualquer executável como uma ação. Isso funciona para Go como um exemplo.
Assim como com as ações Docker, o executável do Go recebe um único argumento na linha de comandos.
É uma serialização de sequência do objeto JSON que representa os argumentos para a ação.
O programa pode efetuar log em `stdout` ou em `stderr`.
Por convenção, a última linha de saída _deve_ ser um objeto JSON em sequência que representa o resultado da ação.

Aqui está uma ação do Go de exemplo.
```go
package main

import "encoding/json"
import "fmt"
import "os"

func main() {
    //program receives one argument: the JSON object as a string
    arg := os.Args[1]
   
    // unmarshal the string to a JSON object
    var obj map[string]interface{}
    json.Unmarshal([]byte(arg), &obj)

    // can optionally log to stdout (or stderr)
    fmt.Println("hello Go action")

    name, ok := obj["name"].(string)
    if !ok { name = "Stranger" }

    // last line of stdout is the result JSON object as a string
    msg := map[string]string{"msg": ("Hello, " + name + "!")}
    res, _ := json.Marshal(msg)
    fmt.Println(string(res))
}
```

Salve o código acima em um arquivo `sample.go` e compile-o de forma cruzada para o OpenWhisk. O executável deve ser chamado `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
wsk action create helloGo --native exec.zip
```

A ação pode ser executada como qualquer outra ação.
```bash
wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Os logs são recuperados de uma maneira semelhante também.
```bash
wsk activation logs --last --strip
my first Go action.
```

## Monitorar a saída de ação
{: #watching-action-output}

As ações do {{site.data.keyword.openwhisk_short}} podem ser chamadas por outros usuários em resposta a vários eventos ou como parte de uma sequência de ações. Nesses casos, pode ser útil monitorar as chamadas.

É possível usar a CLI do {{site.data.keyword.openwhisk_short}} para ver a saída de ações à medida que elas são chamadas.

1. Emita o comando a seguir a partir de um shell:
  ```
  wsk activation poll
  ```
  {: pre}

  Esse comando inicia um loop de pesquisa que verifica continuamente logs de ativações.

2. Alterne para outra janela e chame uma ação:

  ```
  wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  ```
  ok: /whisk.system/samples/helloWorld chamada com id 7331f9b9e2044d85afd219b12c0f1491
  ```

3. Observe o log de ativação na janela de pesquisa:

  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```

  Da mesma forma, sempre que executa o utilitário de pesquisa, você vê em tempo real os logs de quaisquer ações que são executadas em seu nome no OpenWhisk.

## Suporte a aplicativos grandes
{: #large-app-support}

O tamanho máximo do código para uma ação é 48 MB. Os aplicativos que contêm muitos módulos de terceiros, bibliotecas nativas ou ferramentas externas podem encontrar esse limite.

Caso você crie uma ação de pacote (zip ou jar) que seja maior que 48 MB, a solução será estender a imagem de tempo de execução com dependências e, em seguida, usar um único arquivo de origem ou archive menor que 48 MB.

Por exemplo, construindo um tempo de execução do Docker customizado, que inclui as bibliotecas compartilhadas necessárias, essas dependências não precisam estar presentes no archive. Os arquivos de origem privados podem ser empacotados no archive e injetados no tempo de execução.

Outro benefício em reduzir os tamanhos de archive é que os tempos de implementação também são melhorados.

### Exemplo Python

No exemplo Python a seguir, opencv pode incluir a biblioteca `opencv-python` e, em seguida, instalar o binário opencv na imagem do OS. É possível então usar o `requirements.txt` e executar `pip install requirements.txt` para aumentar a imagem com mais bibliotecas Python. Em seguida, é possível usar `action.py` com a nova imagem.

### Exemplo do Node.js

No exemplo do Node.js a seguir, é possível instalar pacotes extras para a imagem do OS:

Instale o opencv usando `npm`:
```
npm install opencv
```
{: pre}

Da mesma forma, se você tiver um `package.json`, instale isso usando `npm`:
```
npm install package.json
```
{: pre}

Em seguida, continue a usar `action.js` com a nova imagem.

## Listar ações
{: #listing-actions}

É possível listar todas as ações criadas usando o comando a seguir:

```
wsk action list
```
{: pre}

Conforme você grava mais ações, essa lista fica mais longa e pode ser útil agrupar ações relacionadas em [pacotes](./openwhisk_packages.html). Para filtrar sua lista de ações apenas àquelas de um pacote específico, é possível usar a sintaxe de comando a seguir: 

```
wsk action list [PACKAGE NAME]
```
{: pre}

## Excluir ações
{: #deleting-actions}

É possível limpar excluindo ações que você não deseja usar.

1. Execute o comando a seguir para excluir uma ação:
  ```
  wsk action delete hello
  ```
  {: pre}

  ```
  ok: deleted hello
  ```

2. Verifique se a ação não aparece mais na lista de ações.
  ```
  wsk action list
  ```
  {: pre}

  ```
  actions
  ```

## Acessar metadados de ação dentro do corpo de ação
{: #accessing-action-metadata-within-the-action-body}

O ambiente de ação contém várias propriedades que são específicas da ação em execução.
Elas permitem que a ação trabalhe programaticamente com ativos do OpenWhisk por meio da API de REST
ou configure um alarme interno quando a ação estiver prestes a esgotar seu orçamento de tempo atribuído.
As propriedades são acessíveis por meio do ambiente do sistema para todos os tempos de execução suportados:
ações do Node.js, Python, Swift, Java e Docker ao usar a estrutura básica do Docker do OpenWhisk.

* `__OW_API_HOST` o host da API para a implementação do OpenWhisk que executa esta ação
* `__OW_API_KEY` a chave API para o assunto que chama a ação, essa chave pode ser uma chave API restrita
* `__OW_NAMESPACE` o namespace para a _ativação_ (este não pode ser o mesmo namespace que para a ação)
* `__OW_ACTION_NAME` o nome completo da ação em execução
* `__OW_ACTIVATION_ID` o ID de ativação para esta instância de ação em execução
* `__OW_DEADLINE` o tempo aproximado quando essa ação terá consumido toda sua cota de duração (medido em milissegundos de época)
