---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Criando e chamando ações
{: #openwhisk_actions}

Ações são fragmentos de código stateless executados na plataforma {{site.data.keyword.openwhisk}}. Por exemplo, uma ação pode ser usada para detectar as faces em uma imagem, responder a uma mudança do banco de dados, agregar um conjunto de chamadas API ou postar um Tweet. Uma ação pode ser gravada como uma função JavaScript, Swift, Python, PHP, método Java ou qualquer executável compatível com binário, incluindo programas Go e executáveis customizados empacotados como contêineres do Docker.
{:shortdesc}

As ações podem ser chamadas explicitamente ou executar em resposta a um evento. Em qualquer um dos casos, cada execução de uma ação resulta em um registro de ativação identificado por um ID de ativação exclusivo. A entrada para uma ação e o resultado de uma ação são um dicionário de pares de valores de chaves, em que a chave é uma sequência e o valor é um valor JSON válido. As ações também podem ser compostas por chamadas a outras ações ou uma sequência definida de ações.

Aprenda como criar, chamar e depurar ações em seu ambiente de desenvolvimento preferencial:
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
* [Monitorando a saída de ação](#monitor-action-output)
* [Obtendo ações](#getting-actions)
* [Listando Ações](#listing-actions)
* [Excluindo Ações](#deleting-actions)
* [Suporte a aplicativos grandes](#large-app-support)
* [Acessando metadados de ação dentro do corpo de ação](#accessing-action-metadata-within-the-action-body)

## Criando e chamando ações JavaScript
{: #creating-and-invoking-javascript-actions}

As seções a seguir o orientam pelo trabalho com ações em JavaScript. Você começa com a criação e a chamada de uma ação simples. Em seguida, move para a inclusão de parâmetros em uma ação e a chamada dessa ação com os parâmetros. Em seguida, você configura os parâmetros padrão e os chama. Em seguida, você cria ações assíncronas e finalmente trabalha com sequências de ações.

### Criando e chamando uma ação simples JavaScript
{: #openwhisk_single_action_js}

Revise as etapas e os exemplos a seguir para criar sua primeira ação JavaScript.

1. Crie um arquivo JavaScript com o conteúdo a seguir. Para este exemplo, nomeie o arquivo **hello.js**.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  O arquivo JavaScript pode conter funções adicionais. No entanto, por convenção, uma função chamada **main** deve existir para fornecer o ponto de entrada para a ação.

2. Crie uma ação a partir da função JavaScript a seguir. Para este exemplo, a ação é chamada **hello**.
  ```
  ibmcloud wsk action create hello hello.js
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: ação hello criada
  ```
  {: screen}

  A CLI supõe automaticamente o tipo da ação usando a extensão do arquivo de origem. Para arquivos de origem `.js`, a ação é executada usando um tempo de execução do Node.js 6. Também é possível criar uma ação que é executada com o Node.js 8 especificando explicitamente o parâmetro `--kind nodejs:8`. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_javascript_environments) do Node.js 6 vs 8.

3. Liste as ações que você criou:
  ```
  ibmcloud wsk action list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ações
  hello       privada
  ```
  {: screen}

  É possível ver a ação **hello** que você criou.

4. Depois de criar sua ação, é possível executá-la na nuvem com o comando **invoke**. É possível chamar ações com uma chamada de *bloqueio* (ou seja, estilo de solicitação/resposta) ou uma chamada *não bloqueio* especificando uma sinalização no comando. Uma solicitação de chamada de bloqueio _espera_ o resultado de ativação ficar disponível. O período de espera é o menor de 60 segundos ou o [valor limite de tempo](./openwhisk_reference.html#openwhisk_syslimits) da ação. O resultado da ativação será retornado se ele estiver disponível dentro do período de espera. Caso contrário, a ativação continua o processamento no sistema e um ID de ativação é retornado para que se possa verificar o resultado posteriormente, como com solicitações sem bloqueio (veja [aqui](#monitor-action-output) para obter dicas sobre monitoramento de ativações).

  Este exemplo usa o parâmetro de bloqueio, `--blocking`:
  ```
  ibmcloud wsk action invoke --blocking hello
  ```
  {: pre}

  A saída de comando inclui duas informações importantes:
  * O ID de ativação (`44794bd6aab74415b4e42a308d880e5b`)
  * O resultado da chamada se ele estiver disponível dentro do período de espera estimado

  **A saída exibe o ID de ativação:**
  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```
  {: screen}

  **Resultado da chamada:**
  ```
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

  O resultado nesse caso é a sequência `Hello world` retornada pela função JavaScript. O ID de ativação pode ser usado para recuperar os logs ou o resultado da chamada em um momento futuro.

5. Se você não precisar do resultado da ação imediatamente, será possível omitir a sinalização `--blocking` para fazer uma chamada sem bloqueio. É possível obter o resultado posteriormente usando o ID da ativação.

  Verifique os seguintes exemplos:
  ```
  ibmcloud wsk action invoke hello
  ```
  {: pre}

  **Saída de comando:**
  ```
  ok: hello chamada com id 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: screen}

  Agora que você sabe o ID de ativação, é possível especificá-lo para obter o resultado da ação:
  ```
  ibmcloud wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  **Resultado da ação:**
  ```
  {
      "payload": "Hello world"
  }
  ```
  {: screen}

6. Se esquecer de registrar o ID da ativação, será possível obter uma lista de ativações ordenadas da mais recente até a mais antiga. Execute o comando a seguir para obter uma lista de suas ativações:

  **Listar ativações:**
  ```
  ibmcloud wsk activation list
  ```
  {: pre}

  Saída:
  ```
  ativações
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
  ```
  {: screen}

### Criando ações assíncronas
{: #openwhisk_asynchrony_js}

As funções JavaScript que são executadas de forma assíncrona podem retornar o resultado da ativação após o retorno da função `main`, retornando uma Promessa em sua ação.

1. Salve o conteúdo a seguir em um arquivo chamado **asyncAction.js**.
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

2. Execute os comandos a seguir para criar a ação e chamá-la.

  Crie uma ação chamada **asyncAction**:
  ```
  ibmcloud wsk action create asyncAction asyncAction.js
  ```
  {: pre}

  Chame a ação:
  ```
  ibmcloud wsk action invoke --result asyncAction
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "done": true
  }
  ```
  {: screen}

  Observe que você executou uma chamada de bloqueio de uma ação assíncrona.

3. Busque o log de ativação para ver quanto tempo a ativação levou para ser concluída.

  Para fazer isso, liste primeiramente a ação para obter o ID de ativação:
  ```
  ibmcloud wsk activation list --limit 1 asyncAction
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ativações
  b066ca51e68c4d3382df2d8033265db0             asyncAction
  ```
  {: screen}

  Agora, obtenha as informações do log de ativação usando o ID de ativação:
  ```
  ibmcloud wsk activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}

  ```
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```
  {: screen}

  Comparando os registros de data e hora `start` e `end` no registro de ativação, é possível ver que essa ativação levou um pouco mais de 2 segundos para ser concluída.

### Usando ações para chamar uma API externa
{: #openwhisk_apicall_action}

Os exemplos fornecidos até agora são funções JavaScript autocontidas. Também é possível criar uma ação que chama uma API externa.

O exemplo a seguir chama o serviço NASA Astronomy Picture of the Day (APOD) que fornece uma imagem exclusiva do nosso universo todos os dias.

1. Salve o conteúdo a seguir em um arquivo nomeado **apod.js**.
  ```javascript
  var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

  $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
      $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
      $("#copyright").text("Image Credits: " + "Public Domain");
    }

    if(result.media_type == "video") {
      $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
      $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
    $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
  });
  ```
  {: codeblock}

  Uma chamada é feita para a API do NASA APOD e extrai campos do resultado JSON. O tópico [Referências](./openwhisk_reference.html#openwhisk_ref_javascript_environments) detalha os pacotes do Node.js que você pode usar em suas ações.

2. Execute os comandos a seguir para criar a ação e chamá-la.

  Crie a ação nomeada **apod**:
  ```
  ibmcloud wsk action create apod apod.js
  ```
  {: pre}

  Chame a ação **apod**:
  ```
  ibmcloud wsk action invoke --result apod
  ```
  {: pre}

  **Objeto de retorno:**
  ```
  {
    "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? No, although it was during a blue moon that this single-shot image was taken. During a full moon that happened to be the second of the month -- the situation that defines a blue moon -- the photographer created the juxtaposition in late January by quickly moving around to find just the right spot to get the background Moon superposed behind the arc of a foreground tree. Unfortunately, in this case, there seemed no other way than getting bogged down in mud and resting the camera on a barbed-wire fence. The arc in the oak tree was previously created by hungry cows in Knight's Ferry, California, USA. Quirky Moon-tree juxtapositions like this can be created during any full moon though, given enough planning and time. Another opportunity will arise this weekend, coincidently during another blue moon. Then, the second blue moon in 2018 will occur, meaning that for the second month this year, two full moons will appear during a single month (moon-th). Double blue-moon years are relatively rare, with the last occurring in 1999, and the next in 2037.",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
  ```
  {: screen}

### Empacotamento de uma ação como um módulo Node.js
{: #openwhisk_js_packaged_action}

Como uma alternativa para gravar todo o seu código de ação em um único arquivo de origem JavaScript, é possível gravar uma ação como um pacote `npm`. Considere, como um exemplo, um diretório com os arquivos a seguir:

**package.json:**
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

**index.js:**
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

Para criar uma ação do {{site.data.keyword.openwhisk_short}} por meio desse pacote:

1. Instale todas as dependências localmente:
  ```
  npm install
  ```
  {: pre}

2. Crie um archive `.zip` contendo todos os arquivos (incluindo todas as dependências):
  ```
  zip -r action.zip *
  ```
  {: pre}

  Usar a ação do Windows Explorer para criar o arquivo zip resulta em uma estrutura incorreta. As ações de zip do {{site.data.keyword.openwhisk_short}} devem ter `package.json` na raiz do zip, enquanto o Windows Explorer o coloca em uma pasta aninhada. A opção mais segura é usar o comando `zip` da linha de comandos.
  {: tip}

3. Crie a ação:
  ```
  ibmcloud wsk action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Ao criar uma ação de um archive `.zip` com a ferramenta CLI, deve-se fornecer explicitamente um valor para a sinalização `--kind` usando `nodejs:6` ou `nodejs:8`.

4. É possível chamar a ação como qualquer outra:
  ```
  ibmcloud wsk action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "padded": [
          ".......................and now",
          "......for something completely",
          ".....................different"
      ]
  }
  ```
  {: screen}

Finalmente, observe que enquanto a maioria dos pacotes `npm` instala fontes JavaScript em `npm install`, alguns também instalam e compilam os artefatos binários. O upload do archive atualmente não suporta dependências binárias, mas apenas as dependências JavaScript. As chamadas de ação poderão falhar se o archive incluir dependências binárias.

### Empacotando uma ação como um único pacote configurável
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
    "deploy": "ibmcloud wsk action update my-action dist/bundle.js --kind nodejs:8"
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

Se seu nome da função for `main`, use a sintaxe a seguir no lugar:
```javascript
global.main = main;
```
{: codeblock}

Para construir e implementar uma ação do OpenWhisk usando `npm` e `webpack`:

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

  O arquivo `dist/bundle.js` é criado e é usado para implementar como o código-fonte da ação.

3. Crie a ação usando o script `npm` ou a CLI.

  Usando o script `npm`:
  ```
  npm run deploy
  ```
  {: pre}

  Usando a CLI:
  ```
  ibmcloud wsk action update my-action dist/bundle.js
  ```
  {: pre}

Finalmente, o arquivo de pacote configurável que é construído pelo `webpack` não suporta dependências binárias, mas as dependências de JavaScript. Então, as chamadas de ação falharão se o pacote configurável depender das dependências binárias, porque isso não é incluído com o arquivo `bundle.js`.

## Criando sequências de ação
{: #openwhisk_create_action_sequence}

É possível criar uma ação que encadeia uma sequência de ações juntas.

Várias ações do utilitário são fornecidas em um pacote que é chamado `/whisk.system/utils` que você pode usar para criar sua primeira sequência. É possível aprender mais sobre pacotes na seção [pacotes](./openwhisk_packages.html).

1. Exiba as ações no pacote `/whisk.system/utils`.
  ```
  ibmcloud wsk package get --summary /whisk.system/utils
  ```
  {: pre}

  Exemplo de Saída:
  ```
  package /whisk.system/utils: construindo blocos que formatam e montam dados
   action /whisk.system/utils/head: extrair prefixo de uma matriz
   action /whisk.system/utils/split: dividir uma sequência em uma matriz
   action /whisk.system/utils/sort: classifica uma matriz
   action /whisk.system/utils/echo: retorna a entrada
   action /whisk.system/utils/date: data e hora atual
   action /whisk.system/utils/cat: concatena a entrada em uma sequência
  ```
  {: screen}

  Você estará usando as ações `split` e `sort` neste exemplo.

2. Crie uma sequência de ações de modo que o resultado de uma ação seja passado como um argumento para a próxima ação.
  ```
  ibmcloud wsk action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Essa sequência de ações converte algumas linhas de texto a uma matriz e classifica as linhas.

3. Chame a ação:
  ```
  ibmcloud wsk action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  Exemplo de Saída:
  ```
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```
  {: screen}

  No resultado, você vê que as linhas estão classificadas.

**Nota**: os parâmetros que são passados entre as ações na sequência são explícitos, exceto parâmetros padrão.
Portanto, os parâmetros passados para a sequência de ações estão disponíveis somente para a primeira ação na sequência. O resultado da primeira ação na sequência torna-se o objeto JSON de entrada para a segunda ação na sequência (e assim por diante). Esse objeto não inclui nenhum dos parâmetros que são originalmente passados para a sequência, a menos que a primeira ação os inclua explicitamente em seu resultado. Os parâmetros de entrada para uma ação são mesclados com os parâmetros padrão da ação, com o antigo tendo precedência e substituindo qualquer parâmetro padrão correspondente. Para obter mais informações sobre como chamar sequências de ações com múltiplos parâmetros nomeados, veja [Configurando parâmetros padrão em uma ação](./parameters.html#default-params-action).

## Criando ações Python
{: #creating-python-actions}

O processo de criação de ações Python é semelhante ao de ações JavaScript. As seções a seguir orientam você na criação e chamada de uma única ação Python e na inclusão de parâmetros nessa ação.

### Criando e chamando uma ação do Python
{: #openwhisk_actions_python_invoke}

Uma ação é simplesmente uma função Python de nível superior. Por exemplo, crie um arquivo chamado
**hello.py** com o código de origem a seguir:
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
    return {"greeting": greeting}
```
{: codeblock}

As ações Python sempre consomem e produzem um dicionário. O método de entrada para a ação é `main` por padrão, mas pode ser especificado explicitamente para criar a ação com a CLI `wsk` usando `--main`, como com qualquer outro tipo de ação.

É possível criar a ação do {{site.data.keyword.openwhisk_short}} chamada **helloPython** por meio dessa função, conforme a seguir:
```
ibmcloud wsk action create helloPython hello.py
```
{: pre}

A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem. Para arquivos de origem `.py`, a ação é executada usando um tempo de execução do Python 2. Também é possível criar uma ação executada com o Python 3 especificando explicitamente o parâmetro `--kind python:3`. Além disso, há um tempo de execução do Python 3 com o tipo `python-jessie:3` que contém pacotes adicionais para o IBM Cloud Services, como o IBM Cloudant, IBM DB2, IBM COS e IBM Watson.
Para obter mais informações sobre pacotes incluídos nesse tempo de execução do Python 3, veja a [referência](./openwhisk_reference.html#openwhisk_ref_python_environments) do tempo de execução do Python.

A chamada da ação é a mesma para ações do Python que é para ações do JavaScript:
```
ibmcloud wsk action invoke --result helloPython --param name World
```
{: pre}

Exemplo de Saída:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### Empacotando ações do Python em arquivos zip
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
ibmcloud wsk action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Embora essas etapas sejam mostradas para o Python 3 (com tipo `python:3`), é possível fazer o mesmo com tipos alternativos do Python `python:2` ou `python-jessie:3`.

### Empacotando ações do Python com um ambiente virtual em arquivos zip
{: #openwhisk_actions_python_virtualenv}

Outra maneira de empacotar dependências do Python é usando um ambiente virtual (`virtualenv`) que permite vincular pacotes adicionais que podem ser instalados usando [`pip`](https://packaging.python.org/installing/) por exemplo.

Tal como com o suporte de arquivo zip básico, o nome do arquivo de origem que contém o ponto de entrada principal deve ser `__main__.py`. Para esclarecer, o conteúdo de `__main__.py` é a função principal, então para este exemplo é possível renomear `hello.py` para `__main__.py` na seção anterior. Além disso, o diretório virtualenv deve ser denominado `virtualenv`. Veja o cenário de exemplo a seguir para instalar dependências, empacotando-as em um virtualenv e criando uma ação do OpenWhisk compatível.

Para assegurar a compatibilidade com o contêiner de tempo de execução do OpenWhisk, as instalações de pacotes dentro de um virtualenv devem ser feitas no ambiente de destino usando a imagem correspondente para o tipo.
- Para o tipo `python:2`, use a imagem do docker `openwhisk/python2action`.
- Para o tipo `python:3`, use a imagem do docker `openwhisk/python3action`.
- Para o tipo `python-jessie:3`, use a imagem do docker `ibmfunctions/action-python-v3`.

1. Dado um arquivo [requirements.txt ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) que contém os módulos e versões `pip` a serem instalados, execute o seguinte para instalar as dependências e crie um virtualenv usando uma imagem do Docker compatível:
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

2. Arquive o diretório virtualenv e quaisquer arquivos adicionais do Python:
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Crie a ação **helloPython**:
    ```
    ibmcloud wsk action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Somente inclua os módulos no `requirements.txt` que não são parte do ambiente de tempo de execução selecionado. Isso ajuda a manter o virtualenv em um tamanho mínimo.
{: tip}

## Criando ações PHP
{: #creating-php-actions}

O processo de criação de ações PHP é semelhante àquele de ações JavaScript. As seções a seguir fornecem orientação durante a criação e chamada de uma única ação PHP e na inclusão de parâmetros nessa ação.

### Criando e chamando uma ação PHP
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
{: codeblock}

As ações PHP consomem uma matriz associativa e retornam uma matriz associativa. O método de entrada para a ação é `main` por padrão, mas pode ser especificado explicitamente ao criar a ação com a CLI `ibmcloud wsk` usando `--main`, como com qualquer outro tipo de ação.

É possível criar uma ação do {{site.data.keyword.openwhisk_short}} chamada **helloPHP** por meio dessa função, conforme a seguir:
```
ibmcloud wsk action create helloPHP hello.php
```
{: pre}

A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem. Para arquivos de origem `.php`, a ação é executada usando um tempo de execução do PHP 7.1. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_php) do PHP.

A chamada da ação é a mesma para ações PHP que para as ações JavaScript:
```
ibmcloud wsk action invoke --result helloPHP --param name World
```
{: pre}

Exemplo de Saída:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### Empacotando ações PHP em arquivos zip
{: #openwhisk_actions_php_zip}

É possível empacotar uma ação PHP junto a outros arquivos e pacotes dependentes em um arquivo zip.
O nome do arquivo de origem que contém o ponto de entrada (por exemplo, `main`) deve ser `index.php`.

Por exemplo, para criar uma ação que inclua um segundo arquivo chamado `helper.php`, primeiro crie um archive que contenha seus arquivos de origem:
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Em seguida, crie a ação **helloPHP**:
```bash
ibmcloud wsk action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Criando ações do Swift
{: #creating-swift-actions}

O processo de criação de ações Swift é semelhante ao de ações JavaScript. As seções a seguir orientam você na criação e chamada de uma única ação do Swift e no empacotamento de uma ação em um arquivo zip.

Também é possível usar o [Online Swift Playground](http://online.swiftplayground.run) on-line para testar seu código Swift sem precisar instalar o Xcode em sua máquina.

**Atenção:** as ações Swift são executadas em um ambiente Linux. O Swift no Linux ainda está em
desenvolvimento e o OpenWhisk geralmente usa a liberação mais recente disponível, que não é necessariamente estável. Além disso, a versão do Swift que é usada com o OpenWhisk pode estar inconsistente com versões do Swift de liberações estáveis do Xcode no MacOS.

### Criando e chamando uma ação

#### Swift 3
Uma ação é simplesmente uma função Swift de nível superior. Por exemplo, crie um arquivo chamado **hello.swift** com o conteúdo a seguir:

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

Neste exemplo, a ação do Swift consome um dicionário e produz um dicionário.

É possível criar uma ação do OpenWhisk chamada **helloSwift** por meio dessa função,
conforme a seguir:
```
ibmcloud wsk action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}

#### Swift 4

Novo no Swift 4, além da assinatura da função principal acima, há mais duas assinaturas prontas para utilização que aproveitam o tipo [Codable](https://developer.apple.com/documentation/swift/codable). É possível aprender mais sobre tipos de dados que são codificáveis e decodificáveis para compatibilidade com representações externas, como JSON, [aqui](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

O exemplo a seguir toma um parâmetro de entrada como **Entrada Codable** com o campo `name` e retorna uma **Saída Codable** com um campo `greetings`.
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

Neste exemplo, a ação do Swift consome um Codable e produz um tipo Codable.
Se você não precisar manipular nenhuma entrada, será possível usar a assinatura da função que não toma nenhuma entrada, somente a saída Codable.
```swift
struct Output: Codable {
    let greeting: String
}
func main(completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello OpenWhisk!")
    completion(result, nil)
}
```
{: codeblock}

É possível criar uma ação do OpenWhisk chamada `helloSwift` por meio dessa função, conforme
a seguir:
```
ibmcloud wsk action create helloSwift hello.swift --kind swift:4.1
```
{: pre}

Veja a [referência](./openwhisk_reference.html#swift-actions) do Swift para obter mais informações sobre o tempo de execução do Swift.

A chamada da ação é a mesma para as ações Swift que das ações JavaScript:
```
ibmcloud wsk action invoke --result helloSwift --param name World
```
{: pre}

Exemplo de Saída:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

Descubra mais sobre os parâmetros no tópico [Trabalhando com parâmetros](./parameters.html).

### Empacotando uma ação como um executável do Swift
{: #packaging-an-action-as-a-swift-executable}

Ao criar uma ação do Swift do OpenWhisk com um arquivo de origem do Swift, ele precisa ser compilado em um binário antes de a ação ser executada. Depois de isso ser feito, as chamadas subsequentes para a ação serão muito mais rápidas até que o contêiner que está mantendo sua ação seja limpo. Esse atraso é conhecido como o atraso de cold start.

Para evitar o atraso de cold start, é possível compilar seu arquivo Swift em um
binário e, em seguida, fazer upload dele para o OpenWhisk em um arquivo zip. Uma vez que o andaime do OpenWhisk será necessária, a maneira mais fácil de criar o binário será construí-lo no mesmo ambiente em que será executado.

### Usando um script para construir ação empacotada por Swift

É possível usar um script para automatizar o empacotamento da ação. Crie um arquivo de script nomeado `compile.sh` usando o código de amostra a seguir.
```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
    exit 2
fi
OUTPUT_DIR="build"
if [ ${2} == "swift:3.1.1" ]; then
  BASE_PATH="/swift3Action"
  DEST_SOURCE="$BASE_PATH/spm-build"
  RUNTIME="openwhisk/action-swift-v3.1.1"
elif [ ${2} == "swift:4.1" ]; then
  RUNTIME="ibmfunctions/action-swift-v4.1"
  BASE_PATH="/swift4Action"
  DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
else
  echo "Error: Kind $2 not recognize"
  exit 3
fi
DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

BUILD_FLAGS=""
if [ -n "$3" ] ; then
    BUILD_FLAGS=${3}
fi

echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

echo 'Setting up build...'
cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

# action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

O script supõe que você tenha um diretório chamado `actions`, com cada diretório de nível superior representando uma ação.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

- Crie o arquivo `Package.swift` para incluir dependências. **Nota:** a sintaxe é diferente de ferramentas Swift 3 para Swift 4.

  Sintaxe de exemplo do Swift 3:
  ```swift
  import PackageDescription

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
  {: codeblock}

  Sintaxe de exemplo do Swift 4:
  ```swift
  // swift-tools-version:4.0
  import PackageDescription

  let package = Package(
      name: "Action",
      products: [
        .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
        .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
      ],
      targets: [
        .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
      ]
  )
  ```
  {: codeblock}

  Como é possível ver, este exemplo inclui `example-package-deckofplayingcards` como uma dependência. Observe que `CCurl`, `Kitura-net` e `SwiftyJSON` são fornecidos na ação padrão do Swift, então é necessário incluí-los em seu próprio `Package.swift` somente para ações do Swift 3.

- Construa a ação executando o comando a seguir para uma ação do Swift 3:
  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Para compilar para Swift 4, use `swift:4.1` em vez de `swift:3.1.1`:
  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Isso criou `hello.zip` no `build`.

- Faça upload dele no OpenWhisk com o nome da ação **helloSwifty**. Para Swift 3, use o tipo `swift:3.1.1`
  ```
  ibmcloud wsk action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Para Swift 4, use o tipo `swift:3.1.1`:
  ```
  ibmcloud wsk action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

- Para verificar o quanto ele é mais rápido, execute o comando a seguir:
  ```
  ibmcloud wsk action invoke helloSwiftly --blocking
  ```
  {: pre}

  O tempo que levou para a ação ser executada está na propriedade "duration" e é comparado com o tempo que leva para execução com uma etapa de compilação na ação **hello**.

### Manipulação de erros no Swift 4

Com o novo manipulador de conclusão Codable, é possível passar um Erro para indicar uma falha em sua ação.
[Manipulação de erros no Swift](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) é semelhante à manipulação de exceção em outras linguagens, com o uso das palavras-chave `try, catch` e `throw`.

O fragmento a seguir mostra um exemplo de manipulação de erro:
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}

## Criando ações Java
{: #creating-java-actions}

O processo de criação de ações Java é semelhante ao de ações JavaScript e Swift. As seções a seguir orientam você na criação e chamada de uma única ação Java e na inclusão de parâmetros nessa ação.

Para compilar, testar e arquivar os arquivos Java, deve-se ter um [JDK 8](http://openjdk.java.net/install) instalado localmente.

### Criando e chamando uma ação Java
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

É possível criar uma ação do {{site.data.keyword.openwhisk_short}} chamada **helloJava** por meio desse arquivo JAR,
conforme a seguir:
```
ibmcloud wsk action create helloJava hello.jar --main Hello
```
{: pre}

Ao usar a linha de comandos e um arquivo de origem `.jar`, não é necessário
especificar que você está criando uma ação Java;
a ferramenta determina isso a partir da extensão do arquivo.

É necessário especificar o nome da classe principal usando `--main`. Uma classe principal elegível é aquela que implementa um método `main` estático. Se a classe não estiver no pacote padrão, use o nome completo de classe Java, por exemplo, `--main com.example.MyMain`.

Se necessário, também é possível customizar o nome do método da ação do Java. Isso é feito especificando o nome completo do método Java de sua ação, por exemplo, `--main com.example.MyMain#methodName`.

A chamada de ação para as ações Java é a mesma que para as ações Swift e JavaScript:
```
ibmcloud wsk action invoke --result helloJava --param name World
```
{: pre}

Exemplo de Saída:
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

## Criando ações Docker
{: #creating-docker-actions}

Com as ações Docker do {{site.data.keyword.openwhisk_short}}, é possível escrever suas ações em qualquer linguagem.

O seu código é compilado em um binário executável e integrado em uma imagem do Docker. O programa binário interage com o sistema aceitando entrada de `stdin` e respondendo por meio de `stdout`.

Como um pré-requisito, deve-se ter uma conta do Docker Hub.  Para configurar um ID e uma conta do Docker grátis, acesse o [Docker Hub](https://hub.docker.com).

Para as instruções a seguir, suponha que o ID do usuário do Docker seja `janesmith` e a senha seja `janes_password`.  Supondo que a CLI esteja configurada, três etapas restam para configurar um binário customizado para uso pelo {{site.data.keyword.openwhisk_short}}. Depois disso, a imagem do Docker transferida por upload poderá ser usada como uma ação.

1. Faça download da estrutura básica do Docker. É possível fazer download e instalá-la usando a CLI conforme a seguir:
  ```
  ibmcloud wsk sdk install docker
  ```
  {: pre}

  A estrutura básica do Docker agora está instalada no diretório atual.
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  Exemplo de Saída:
  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```
  {: screen}

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

  Observe que parte do arquivo `example.c` é compilada como parte do processo de construção de imagem do Docker, então você não precisa de C compilado em sua máquina. Na verdade, a menos que você esteja compilando o binário em uma máquina host compatível, ele não pode ser executado dentro do contêiner porque os formatos não correspondem.

  Seu contêiner do Docker agora pode ser usado como uma ação do {{site.data.keyword.openwhisk_short}}:
  ```
  ibmcloud wsk action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Observe o uso de `--docker` para criar uma ação. Todas as imagens do Docker são presumidas como hospedadas no Docker Hub. A ação pode ser chamada como qualquer outra ação do {{site.data.keyword.openwhisk_short}}.
  ```
  ibmcloud wsk action invoke --result example --param payload Rey
  ```
  {: pre}

  **Saída de chamada:**
  ```
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
  {: screen}

  Para atualizar a ação do Docker, execute `buildAndPush.sh` para fazer upload da imagem mais recente do Docker Hub. Isso permitirá que o sistema puxe a sua nova imagem do Docker na próxima vez em que ele executar o código para a sua ação. Se não houver nenhum contêiner quente, as novas chamadas usarão a nova imagem do Docker. No entanto, se houver um contêiner quente que use uma versão anterior de sua imagem do Docker, quaisquer novas chamadas continuarão usando essa imagem, a menos que você execute `ibmcloud wsk action update`. Para novas chamadas, isso indica ao sistema para executar um pull do docker para obter sua nova imagem do Docker.

  **Fazer upload da imagem mais recente do Docker Hub:**
  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  **Atualizar a ação para que novas chamadas comecem a usar a nova imagem:***
  ```
  ibmcloud wsk action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  É possível localizar mais informações sobre como criar ações Docker na seção [Referências](./openwhisk_reference.html#openwhisk_ref_docker).

  A versão anterior da CLI suportava `--docker` sem um parâmetro e o nome da imagem era um argumento posicional. Para permitir que as ações do Docker aceitem dados de inicialização usando um arquivo (zip), normalize a experiência do usuário para ações do Docker para que um argumento posicional, se presente, seja um arquivo (por exemplo, um arquivo zip). O nome da imagem deve ser especificado depois da opção `--docker`. Graças ao feedback do usuário, o argumento `--native` foi incluído como abreviação para `--docker openwhisk/dockerskeleton`, de maneira que os executáveis que são executados dentro do SDK de ação padrão do Docker são mais convenientes para criação e implementação.

  Por exemplo, este tutorial cria um executável binário dentro do contêiner localizado em `/action/exec`. Se você copiar esse arquivo para seu sistema de arquivos local e compactá-lo em `exec.zip`, será possível usar os comandos a seguir para criar uma ação do Docker que receba o executável como dados de inicialização.

  **Criar ação do arquivo zip:**
  ```
  ibmcloud wsk action create example exec.zip --native
  ```
  {: pre}

  Que é equivalente ao comando a seguir.
  ```
  ibmcloud wsk action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Criando ações do Go
{: #creating-go-actions}

A opção `--native` permite o empacotamento de qualquer executável como uma ação. Isso funciona para Go como um exemplo. Assim como com as ações Docker, o executável do Go recebe um único argumento na linha de comandos. É uma serialização de sequência do objeto JSON que representa os argumentos para a ação. O programa pode efetuar log em `stdout` ou em `stderr`. Por convenção, a última linha de saída _deve_ ser um objeto JSON em sequência que representa o resultado da ação.

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
{: codeblock}

Salve o código acima em um arquivo `sample.go` e compile-o de forma cruzada para o OpenWhisk. O executável deve ser chamado `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
ibmcloud wsk action create helloGo --native exec.zip
```
{: codeblock}

A ação pode ser executada como qualquer outra ação.
```bash
ibmcloud wsk action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Os logs são recuperados de uma maneira semelhante também.
```bash
ibmcloud wsk activation logs --last --strip
my first Go action.
```

## Criando ações usando executáveis arbitrários
{: #creating-actions-arbitrary}

Usando o `--native`, é possível ver que _qualquer_ executável pode ser executado como uma ação do OpenWhisk. Isso inclui scripts de `bash` ou binários compilados cruzados. Para esse último, a restrição é que o binário deve ser compatível com a imagem `openwhisk/dockerskeleton`.

## Monitorando a saída de ação
{: #monitor-action-output}

As ações do {{site.data.keyword.openwhisk_short}} podem ser chamadas por outros usuários em resposta a vários eventos ou como parte de uma sequência de ações. Nesses casos, pode ser útil monitorar as chamadas.

É possível usar a CLI do {{site.data.keyword.openwhisk_short}} para observar a saída de ações à medida que são chamadas.

1. Emita o comando a seguir a partir de um shell:
  ```
  ibmcloud wsk activation poll
  ```
  {: pre}

  Esse comando inicia um loop de pesquisa que verifica continuamente logs de ativações.

2. Alterne para outra janela e chame uma ação:
  ```
  ibmcloud wsk action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: /whisk.system/samples/helloWorld chamada com id 7331f9b9e2044d85afd219b12c0f1491
  ```
  {: screen}

3. Observe o log de ativação na janela de pesquisa:
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
  {: screen}

  Da mesma forma, sempre que executa o utilitário de pesquisa, você vê em tempo real os logs de quaisquer ações que são executadas em seu nome no OpenWhisk.

## Obtendo ações
{: #getting-actions}

Os metadados que descrevem as ações existentes podem ser recuperados usando o comando `ibmcloud wsk action` get.

**Comando :**
```
ibmcloud wsk action get hello
```

***Resultado:**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

### Obtendo uma URL de ação
{: #get-action-url}

Uma ação pode ser chamada usando a interface REST por meio de uma solicitação de HTTPS. Para obter uma URL de ação, execute o comando a seguir:
```
ibmcloud wsk action get actionName --url
```
{: pre}

Uma URL com o formato a seguir é retornada para ações padrão:
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Para [ações da web](./openwhisk_webactions.html), uma URL é retornada no formato a seguir:
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Nota:** para ações padrão, a autenticação deve ser fornecida quando chamada por meio de uma solicitação de HTTPS. Para obter mais informações sobre as chamadas de ação usando a interface REST, veja a [Referência da API de REST](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

### Salvando o código de ação
{: #save-action}

O código associado a uma ação existente pode ser recuperado e salvo localmente. O salvamento pode ser executado em todas as ações, exceto sequências e ações do Docker.

1. Salve o código de ação em um nome de arquivo que corresponda a um nome de ação existente no diretório atualmente em funcionamento. Uma extensão de arquivo que corresponde ao tipo de ação é usada ou uma extensão .zip será usada para o código de ação que é um arquivo zip.
  ```
  ibmcloud wsk action get actionName --save
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. Em vez de permitir que a CLI determine o destino do código a ser salvo, um caminho de arquivo customizado, o nome do arquivo e a extensão podem ser fornecidos usando a sinalização `--save-as`.
  ```
  ibmcloud wsk action get actionName --save-as codeFile.js
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## Listando Ações
{: #listing-actions}

É possível listar todas as ações criadas usando o comando a seguir:
```
ibmcloud wsk action list
```
{: pre}

Conforme você gravar mais ações, essa lista ficará mais longa e poderá ser útil para agrupar ações relacionadas nos [pacotes](./openwhisk_packages.html). Para filtrar sua lista de ações apenas àquelas de um pacote específico, é possível usar a sintaxe de comando a seguir:
```
ibmcloud wsk action list [PACKAGE NAME]
```
{: pre}

## Excluindo ações
{: #deleting-actions}

É possível limpar excluindo ações que você não deseja usar.

1. Execute o comando a seguir para excluir uma ação:
  ```
  ibmcloud wsk action delete hello
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: deleted hello
  ```
  {: screen}

2. Verifique se a ação não aparece mais na lista de ações.
  ```
  ibmcloud wsk action list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  actions
  ```
  {: screen}

## Suporte a aplicativos grandes
{: #large-app-support}

O tamanho máximo do código para uma ação é 48 MB. Os aplicativos que contêm muitos módulos de terceiros, bibliotecas nativas ou ferramentas externas podem encontrar esse limite.

Se você criar uma ação de pacote (zip ou jar) que seja maior que 48 MB, a solução será estender a imagem de tempo de execução com dependências e, em seguida, usar um único arquivo de origem ou archive menor que 48 MB.

Por exemplo, construindo um tempo de execução do Docker customizado, que inclui as bibliotecas compartilhadas necessárias, essas dependências não precisam estar presentes no archive. Os arquivos de origem privados podem ser empacotados no archive e injetados no tempo de execução.

Outro benefício em reduzir os tamanhos de archive é que os tempos de implementação também são melhorados. Dois exemplos de execução são fornecidos nas seções a seguir para demonstrar como os tamanhos de aplicativos podem ser reduzidos alavancando essa técnica.

### Exemplo Python

Para um aplicativo Python, consulte as etapas a seguir para reduzir seu tamanho de código.

1. Coloque a biblioteca `opencv-python` em opencv.
2. Em seguida, instale o binário opencv na imagem do S.O.
3. É possível então usar o `requirements.txt` e executar `pip install requirements.txt` para aumentar a imagem com mais bibliotecas Python.
4. Em seguida, é possível usar `action.py` com a nova imagem.

### Exemplo do Node.js

Nos esforços para reduzir o tamanho de um aplicativo Node.js, veja as etapas a seguir para instalar pacotes extras na imagem do S.O.:

1. Instale o opencv usando `npm`:
   ```
   npm install opencv
   ```
   {: pre}

2. Da mesma forma, se você tiver um `package.json`, instale isso usando `npm`:
   ```
   npm install package.json
   ```
   {: pre}

3. Em seguida, continue a usar `action.js` com a nova imagem.

## Acessando metadados de ação dentro do corpo de ação
{: #accessing-action-metadata-within-the-action-body}

O ambiente de ação contém várias propriedades que são específicas da ação em execução. Essas propriedades permitem que a ação trabalhe programaticamente com ativos do OpenWhisk por meio da API de REST ou configurar um alarme interno quando a ação estiver prestes a esgotar seu orçamento de tempo atribuído. As propriedades são acessíveis no ambiente do sistema para todos os tempos de execução suportados: ações do Node.js, Python, Swift, Java e Docker ao usar a estrutura básica do OpenWhisk Docker.

* `__OW_API_HOST` o host da API para a implementação do OpenWhisk que executa esta ação
* `__OW_API_KEY` a chave API para o assunto que chama a ação, essa chave pode ser uma chave API restrita
* `__OW_NAMESPACE` o namespace para a _ativação_ (este não pode ser o mesmo namespace que para a ação)
* `__OW_ACTION_NAME` o nome completo da ação em execução
* `__OW_ACTIVATION_ID` o ID de ativação para esta instância de ação em execução
* `__OW_DEADLINE` o tempo aproximado quando essa ação terá consumido toda sua cota de duração (medido em milissegundos de época)
