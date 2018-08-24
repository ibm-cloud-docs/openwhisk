---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Criando e chamando ações
{: #openwhisk_actions}

Ações são fragmentos de código stateless executados na plataforma {{site.data.keyword.openwhisk}}. 
Por exemplo, uma ação pode ser usada para detectar as faces em uma imagem, responder a uma mudança no banco de
dados, agregar um conjunto de chamadas API ou postar um tweet.
{:shortdesc}

As ações podem ser chamadas explicitamente ou executar em resposta a um evento. Em qualquer um dos casos, cada execução de uma ação resulta em um registro de ativação identificado por um ID de ativação exclusivo. 
A entrada para uma ação e o resultado de uma ação são um dicionário de pares chave-valor, em que a chave é uma
sequência e o valor é um valor JSON válido. As ações também podem ser compostas por chamadas a outras ações ou uma sequência definida de ações.

Uma ação pode ser gravada como uma função JavaScript, Swift, Python, PHP, como um método Java ou como
qualquer executável binário compatível, como programas Go e executáveis customizados empacotados como
contêineres do Docker. Aprenda a criar, chamar e depurar ações em seu ambiente de desenvolvimento
preferencial.

## Criando ações do JavaScript
{: #creating-and-invoking-javascript-actions}

As seções a seguir o orientam pelo trabalho com ações em JavaScript. Inicie a criação e a chamada de uma
ação simples. Em seguida, inclua parâmetros na ação e chame essa ação com parâmetros. Em seguida, configure
parâmetros padrão e chame-os. Finalmente, crie ações assíncronas.

### Criando e chamando uma ação simples JavaScript
{: #openwhisk_single_action_js}

Revise as etapas e os exemplos a seguir para criar sua primeira ação JavaScript.

1. Salve o código a seguir em um arquivo denominado `hello.js`.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  O arquivo JavaScript pode conter funções adicionais. No entanto, por convenção, uma função chamada `main` deve existir para fornecer o ponto de entrada para a ação.

2. Crie uma ação chamada `hello` usando a função JavaScript.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ok: ação hello criada
  ```
  {: screen}

  A CLI supõe automaticamente o tipo da ação usando a extensão do arquivo de origem. Para arquivos de origem `.js`, a ação é executada usando um tempo de execução do Node.js 6. Também é possível criar uma ação que é executada com o Node.js 8 especificando explicitamente o parâmetro `--kind nodejs:8`. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_javascript_environments) do Node.js 6 vs 8.

3. Verifique se a ação `hello` está em sua lista de ações.
  ```
  ibmcloud fn action list
  ```
  {: pre}

  Exemplo de Saída:
  ```
  ações
  hello       privada
  ```
  {: screen}

4. Execute a ação na nuvem ao executar uma chamada de bloqueio. As chamadas de bloqueio usam um estilo
de solicitação/resposta e aguardam a disponibilidade do resultado de ativação. O período de espera é o menor de 60 segundos ou o [valor limite de tempo](./openwhisk_reference.html#openwhisk_syslimits) da ação.
    ```
    ibmcloud fn action invoke -- blocking hello
    ```
    {: pre}

    O comando expõe as informações a seguir:
        * O ID de ativação (`44794bd6aab74415b4e42a308d880e5b`), que pode ser usado
para recuperar os logs ou o resultado da chamada
        * O resultado da chamada se ele estiver disponível dentro do período de espera estimado
    ```
    ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

    {
        "result": {
            "payload": "Hello world"
        },
      "status": "success",
      "success": true
  }
    ```
    {: screen}
    Para obter dicas sobre ativações de monitoramento, consulte a
[Saída de ação de monitoramento](openwhisk_managing.html#monitor-action-output).
    {: tip}

5. Se você não precisa do resultado da ação imediatamente, é possível omitir a sinalização
`--blocking` para executar uma chamada sem bloqueio.
    1. Execute uma chamada sem bloqueio.
        ```
        ibmcloud fn action invoke hello
        ```
        {: pre}

        Exemplo de Saída:
        ```
        ok: hello chamada com id 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: screen}

    2. Use o ID de ativação para obter o resultado da ação.
        ```
        ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
        ```
        {: pre}

        O resultado da ação é retornado:
        ```
        {
            "payload": "Hello world"
  }
        ```
        {: screen}

6. Caso você se esqueça de registrar o ID de ativação, é possível obter uma lista de ativações ordenadas da mais para a menos recente.
    ```
    ibmcloud fn activation list
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

1. Salve o código a seguir em um arquivo denominado `asyncAction.js`.
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

    * A função  ` main `  retorna uma Promessa. O Promise indica que a ativação ainda
não foi concluída, mas espera-se que seja no futuro.
    * A função JavaScript `setTimeout()` aguarda por 2 segundos antes de chamar a
função de retorno de chamada do Promise, que representa o código assíncrono.
    * O retorno de chamada do Promise aceita os argumentos `resolve` e
`reject`, em que ambos são funções.
      * A chamada para `resolve()` cumpre a Promessa e indica que a ativação é concluída normalmente.
      * Uma chamada para `reject()` pode ser usada para rejeitar a Promessa e sinalizar que a ativação é concluída de forma anormal.

2. Crie uma ação chamada `asyncAction`.
    ```
    ibmcloud fn action create asyncAction asyncAction.js
    ```
    {: pre}

3. Invoque a ação.
    ```
    ibmcloud fn action invoke -- result asyncAction
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "done": true
  }
    ```
    {: screen}

3. Verifique quanto tempo a ativação levou para ser concluída obtendo o log de ativação.

  1. Obtenha o ID de ativação.
      ```
      ibmcloud fn activation list --limit 1 asyncAction
      ```
      {: pre}

      Exemplo de Saída:
      ```
      ativações
  b066ca51e68c4d3382df2d8033265db0             asyncAction
      ```
      {: screen}

  2. Obtenha o log para o ID de ativação.
      ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
      {: pre}

      A `duration` mostra que essa ativação demorou um pouco mais de 2 segundos
para ser concluída:
      ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
          "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
      }
      ```
      {: screen}

### Usando ações para chamar uma API externa
{: #openwhisk_apicall_action}

Os exemplos fornecidos até agora são funções JavaScript autocontidas. Também é possível criar uma ação que chama uma API externa.
{: shortdesc}

O exemplo a seguir chama o serviço NASA Astronomy Picture of the Day (APOD) que fornece uma imagem exclusiva do nosso universo todos os dias.

1. Salve o código a seguir em um arquivo denominado `apod.js`.
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

      if (result.media_type == "video") {
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

    Uma chamada é feita para a API APOD da NASA e os campos são extraídos do resultado JSON. Para
obter mais informações sobre quais pacotes do Node.js podem ser usados em suas ações, consulte
[Detalhes e limites do sistema](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

2. Crie uma ação chamada  ` apod `.
    ```
    ibmcloud fn action create apod apod.js
    ```
    {: pre}

3. Chame a ação `apod`.
    ```
    ibmcloud fn action invoke -- result apod
    ```
    {: pre}

    O objeto de exemplo a seguir é retornado:
    ```
    {
      "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? ...",
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

Como uma alternativa para gravar todo o seu código de ação em um único arquivo de origem JavaScript, é possível gravar uma ação como um pacote `npm`.

Por exemplo, considere um diretório com os arquivos a seguir:

1. Salve o código a seguir em um arquivo denominado `package.json`.
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

2. Salve o código a seguir em um arquivo denominado `index.js`.
    ```javascript function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    exports.main = myAction;
    ```
    {: codeblock}
    * A ação é exposta por meio de `exports.main`.
    * O manipulador de ações pode ter qualquer nome, desde que ele esteja em conformidade com a
assinatura convencional de aceitação e de retorno de um objeto (ou com um `Promise` de um
objeto).
    * Deve-se nomear este arquivo **index.js** ou especificar o nome do
arquivo que você preferir como a propriedade `main` em **package.json**.

3. Instale todas as dependências localmente.
    ```
    npm install
    ```
    {: pre}
    **Nota:** embora a maioria dos pacotes `npm` instala origens
do JavaScript no `npm install`, alguns também instalam e compilam artefatos binários. O
upload de archive suporta atualmente apenas dependências do JavaScript. Chamadas de ação poderão falhar se o
archive incluir dependências binárias.

4. Crie um archive `.zip` contendo todos os arquivos, incluindo todas as dependências.
    ```
    zip -r action.zip *
    ```
    {: pre}

    Usar a ação do Windows Explorer para criar o arquivo zip resulta em uma estrutura incorreta. As ações de zip do {{site.data.keyword.openwhisk_short}} devem ter `package.json` na raiz do zip, enquanto o Windows Explorer o coloca em uma pasta aninhada. A opção mais segura é usar o comando `zip` da linha de comandos.
    {: tip}

5. Crie a ação. Ao criar uma ação de um archive `.zip`, deve-se
fornecer explicitamente um valor para a sinalização `--kind` usando
`nodejs:6` ou `nodejs:8`.
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
    ```
    {: pre}

6. Invoque a ação.
    ```
    ibmcloud fn action invoke --result packageAction --param lines "[\"and now\", \"for something completely\", \"different\" ]"
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

### Empacotando uma ação como um único pacote configurável
{: #openwhisk_js_webpack_action}

Se o empacotamento da ação como um zip incluir muitos arquivos desnecessários ou se você precisar de uma
implementação mais rápida, será possível gravar o código mínimo em um único arquivo `.js`
que inclua dependências.
{: shortdesc}

É possível empacotar uma ação usando um bundler de módulo JavaScript, como
[webpack
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://webpack.js.org/concepts/). Quando o webpack processa seu código, ele constrói recursivamente um gráfico de
dependência que inclui cada módulo de que a sua ação precisa.

1. Salve o código a seguir em um arquivo denominado `package.json`. O
`webpack` é incluído como uma dependência de desenvolvimento.
    ```json
     {
      "name": "my-action",
  "main": "dist/bundle.js",
  "scripts": {
        "build": "webpack --config webpack.config.js",
        "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

2. Salve o código de configuração do webpack a seguir em um arquivo denominado `webpack.config.js`.
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

3. Salve o código a seguir em um arquivo denominado `index.js`. A variável
`global.main` é configurada para a função principal da ação.
    ```javascript function myAction(args) {
        const leftPad = require("left-pad")
    const lines = args.lines || [];
    return { padded: lines.map(l => leftPad(l, 30, ".")) }
    }

    global.main = myAction;
    ```
    {: codeblock}

4. Instale todas as dependências localmente.
    ```
    npm install
    ```
    {: pre}

5. Construa o pacote configurável do webpack.
    ```
    npm run build
    ```
    {: pre}

    O arquivo `dist/bundle.js` é criado e implementado como o código de origem da
ação.

6. Crie a ação usando o script `npm` ou a CLI.
    * Usando o script  ` npm ` :
        ```
        npm run deploy
        ```
        {: pre}

    * Usando a CLI:
        ```
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **Nota:** o arquivo de pacote configurável que é construído pelo
`webpack` suporta apenas dependências do JavaScript. As chamadas de ação poderão falhar se o
pacote configurável depender de dependências binárias, já que isso não está incluído com o arquivo
`bundle.js`.

## Criando ações Python
{: #creating-python-actions}

As seções a seguir orientam você na criação e na chamada de uma ação Python única e na inclusão
de parâmetros nessa ação.

### Criando e chamando uma ação do Python
{: #openwhisk_actions_python_invoke}

Uma ação é simplesmente uma função Python de nível superior. Para criar uma ação Python:

1. Salve o código a seguir em um arquivo chamado `hello.py`.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
    return {"greeting": greeting}
    ```
    {: codeblock}

  * As ações Python sempre consomem e produzem um dicionário.
  * O método de entrada para a ação é `main` por padrão, mas pode ser especificado
explicitamente para criar a ação com a CLI `wsk` usando a sinalização `--main`.

2. Crie uma ação  ` helloPython ` .
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: created action helloPython
    ```
    {: screen}

    A CLI supõe automaticamente o tipo da ação usando a extensão do arquivo de origem. Para arquivos de origem `.py`, a ação é executada usando um tempo de execução do Python 2. Também é possível criar uma ação executada com o Python 3 especificando explicitamente o parâmetro `--kind python:3`. 
Também é possível usar o tempo de execução do Python 3 com o tipo `python-jessie:3`, que
contém pacotes adicionais para o IBM Cloud Services como {{site.data.keyword.cloudant_short_notm}},
{{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} e
{{site.data.keyword.ibmwatson_notm}}. Para obter mais informações sobre pacotes incluídos nesse tempo de execução do Python 3, veja a [referência](./openwhisk_reference.html#openwhisk_ref_python_environments) do tempo de execução do Python.

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result helloPython --param name World
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

É possível empacotar uma ação do Python e os módulos dependentes em um arquivo zip. Por exemplo, para
criar uma ação com um módulo auxiliar chamado `helper.py`:

1. Crie um archive contendo os seus arquivos de origem. **Nota:** o arquivo de
origem que contém o ponto de entrada deve ser denominado `__main__.py`.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Crie a ação.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### Empacotando ações do Python com um ambiente virtual em arquivos zip
{: #openwhisk_actions_python_virtualenv}

É possível empacotar dependências do Python usando um ambiente virtual, `virtualenv`. O
ambiente virtual permite que você vincule pacotes adicionais que podem ser instalados usando
o `pip` do [
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://packaging.python.org/installing/),
por exemplo.

Para instalar dependências, compactá-las em um ambiente virtual e criar uma ação do OpenWhisk compatível:

1. Crie um arquivo [requirements.txt
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files)
que contenha os módulos e as versões do `pip` a serem instalados.

2. Instale as dependências e crie um ambiente virtual. O diretório de ambiente virtual deve ser
denominado `virtualenv`. Para assegurar a compatibilidade com o contêiner de tempo de
execução do OpenWhisk, as instalações de pacote dentro de um ambiente virtual devem usar a imagem que
corresponda ao tipo.
    - Para o tipo `python:2`, use a imagem do docker `openwhisk/python2action`.
    - Para o tipo `python:3`, use a imagem do docker `openwhisk/python3action`.
    - Para o tipo `python-jessie:3`, use a imagem do docker `ibmfunctions/action-python-v3`.
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. Empacote o diretório `virtualenv` e quaisquer arquivos Python adicionais. O arquivo
de origem que contém o ponto de entrada deve ser denominado `__main__.py`.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Crie a ação  ` helloPython `.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Inclua somente os módulos que não fazem parte do ambiente de tempo de execução selecionado no
`requirements.txt`. Isso ajuda a manter o `virtualenv` em um tamanho
mínimo.
{: tip}

## Criando ações PHP
{: #creating-php-actions}

As seções a seguir orientam você na criação e na chamada de uma ação de PHP única e na inclusão
de parâmetros nessa ação.

### Criando e chamando uma ação PHP
{: #openwhisk_actions_php_invoke}

Uma ação é simplesmente uma função PHP de nível superior. Para criar uma ação de PHP:

1. Salve o código a seguir em um arquivo chamado `hello.php`.
    ```
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

    * As ações PHP consomem uma matriz associativa e retornam uma matriz associativa.
    * O método de entrada para a ação é `main` por padrão, mas pode ser especificado
explicitamente ao criar a ação com a CLI `ibmcloud fn` usando a sinalização
`--main`.

2. Crie uma ação chamada  ` helloPHP `.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem. Para arquivos de origem `.php`, a ação é executada usando um tempo de execução do PHP 7.1. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_php) do PHP.

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result helloPHP --param name World
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

É possível empacotar uma ação PHP e outros arquivos ou pacotes dependentes em um arquivo zip. Por
exemplo, para empacotar uma ação com um segundo arquivo chamado `helper.php`:

1. Crie um archive contendo os seus arquivos de origem. **Nota:** o arquivo de
origem que contém o ponto de entrada deve ser denominado `index.php`.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Crie a ação.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Criando ações do Swift
{: #creating-swift-actions}

As seções a seguir orientam você na criação e na chamada de uma ação do Swift única e no empacotamento de
uma ação em um arquivo zip.

**Nota:** as ações do Swift são executadas em um ambiente Linux. O Swift no Linux
ainda está em desenvolvimento e o {{site.data.keyword.openwhisk_short}} usa a liberação mais recente
disponível. Talvez essas liberações não estejam estáveis. A versão do Swift que é usada com o
{{site.data.keyword.openwhisk_short}} pode estar inconsistente com as versões do Swift de liberações
estáveis do Xcode no MacOS.

Para obter mais informações sobre o tempo de execução do Swift, consulte a
[referência](./openwhisk_reference.html#swift-actions) do Swift.
{: tip}

### Criando e chamando uma ação
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

Uma ação é simplesmente uma função Swift de nível superior. Para criar uma ação do Swift 3:

1. Salve o código a seguir em um arquivo chamado `hello.swift`.
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

2. Crie uma ação chamada  ` helloSwift `.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem. Para arquivos de origem `.php`, a ação é executada usando um tempo de execução do PHP 7.1. Para obter mais informações, veja a [referência](./openwhisk_reference.html#openwhisk_ref_php) do PHP.

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Exemplo de Saída:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

Além da assinatura da função principal, o Swift 4 fornece mais duas assinaturas que aproveitam o tipo
[Codable
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/documentation/swift/codable). É possível aprender mais sobre os tipos de dados
que são codificáveis e decodificáveis para compatibilidade com representações externas, como JSON,
[aqui
![Ícone de link externo](../icons/launch-glyph.svg "Ícone
de link externo")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

1. Salve o código a seguir em um arquivo chamado `hello.swift`.
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
    Este exemplo usa um parâmetro de entrada como `Codable Input` com o campo
`name` e retorna uma `Codable output` com um campo
`greetings`.

2. Crie uma ação chamada  ` helloSwift `.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result helloSwift --param name World
    ```
    {: pre}

    Exemplo de Saída:
    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}

### Empacotando uma ação como um executável do Swift
{: #packaging-an-action-as-a-swift-executable}

Ao criar uma ação do Swift do {{site.data.keyword.openwhisk_short}} com um arquivo de
origem do Swift, o arquivo deverá ser compilado em um binário antes de a ação ser executada. Esse atraso é conhecido como o atraso de cold start. 
Depois que o binário é criado, as chamadas subsequentes para a ação são muito mais rápidas até que o contêiner
que retém sua ação seja limpo. Para evitar o atraso de cold start, é possível compilar o arquivo Swift em um
binário e, em seguida, fazer upload do binário para o {{site.data.keyword.openwhisk_short}} em um
arquivo zip.

É possível usar um script para automatizar o empacotamento da ação.

**Pré-requisito**: o script usado nas etapas a seguir supõe que você tenha um
diretório chamado `actions`, com cada diretório de nível superior representando uma ação.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. Salve o código a seguir em um arquivo de script denominado `compile.sh`.
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

2. Para incluir dependências, crie o arquivo `Package.swift`. O exemplo a seguir
inclui `example-package-deckofplayingcards` como uma dependência. Como
`CCurl`, `Kitura-net` e `SwiftyJSON` são fornecidos na ação do Swift padrão,
é necessário incluí-los em seu próprio `Package.swift` somente para as ações do Swift 3.
    * Sintaxe de exemplo do Swift 3:
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

    * Sintaxe de exemplo do Swift 4:
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

3. Para criar um `hello.zip` no `build`, construa a ação.
    * Swift 3:
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. Faça upload do zip para o {{site.data.keyword.openwhisk_short}} com o nome da ação ` helloSwiftly`.
    * Swift 3:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4:
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. Invoque a ação.
    ```
    ibmcloud fn action invoke helloSwiftly -- blocking
    ```
    {: pre}

    O tempo que levou para a ação ser executada está na propriedade `duration1.

6. É possível comparar a duração da chamada de ação pré-compilada com a duração da chamada de um comando com uma etapa de compilação. Chame a ação na seção anterior:
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Manipulação de erros no Swift 4
{: #error-handling-swift4}

Usando o manipulador de conclusão Codable, é possível passar um erro para indicar uma falha em sua ação. [A manipulação de erros no Swift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) lembra a manipulação de exceções em outros idiomas, com o uso das palavras-chave `try`, `catch` e `throw`.

O snippet a seguir mostra um exemplo de manipulação de um erro:
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

As seções a seguir orientam você na criação e na chamada de uma ação Java única e na inclusão de parâmetros nessa ação.

Para compilar, testar e criar um archive de arquivos Java, deve-se ter o [JDK 8 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://openjdk.java.net/install) instalado localmente.

### Criando e chamando uma ação Java
{: #openwhisk_actions_java_invoke}

Uma ação Java é um programa Java com um método chamado `main`. O `main` deve ter a assinatura exata a seguir:
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Para criar uma ação Java:

1. Salve o código a seguir em um arquivo denominado `Hello.java`.
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

2. Compile `Hello.java` em um arquivo JAR denominado `hello.jar`. **Nota:** o [google-gson ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/gson) deve existir em seu CLASSPATH Java.
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Crie uma ação.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * Deve-se especificar o nome da classe principal usando `--main`. Uma classe principal elegível é aquela que implementa um método `main` estático. Se a classe não estiver no pacote padrão, use o nome completo de classe Java, por exemplo, `--main com.example.MyMain`.
  * É possível customizar o nome do método de sua ação Java. Isso é feito especificando o nome completo do método de sua ação, por exemplo, `--main com.example.MyMain#methodName`.
  * A CLI infere automaticamente o tipo da ação na extensão do arquivo de origem.

4. Invoque a ação.
    ```
    ibmcloud fn action invoke --result helloJava --param name World
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
{: shortdesc}

O seu código é compilado em um binário executável e integrado em uma imagem do Docker. O programa binário interage com o sistema aceitando entrada de `stdin` e respondendo por meio de `stdout`.   É possível localizar mais informações sobre como criar ações Docker na seção [Referências](./openwhisk_reference.html#openwhisk_ref_docker).

Pré-requisito: deve-se ter uma conta do Docker Hub. Configure um ID do Docker e uma conta grátis no [Docker Hub![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com).

Para configurar um binário customizado e usar a imagem do Docker transferida por upload como uma ação:

1. Faça download e instale o esqueleto do Docker. A estrutura básica é um modelo do contêiner do Docker no qual é possível injetar seu código na forma de binários customizados.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configure seu binário customizado na estrutura básica da caixa preta. A estrutura básica inclui um programa C que pode ser usado. Parte do arquivo `example.c` é compilada como parte do processo de construção da imagem do Docker, de modo que o C não precisa ser compilado em sua máquina.
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Exemplo de Saída:
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Opcional: inclua código e dependências adicionais na imagem do Docker modificando o `Dockerfile` para construir seu executável. Observe os requisitos e limites a seguir:
  * O binário deve estar localizado dentro do contêiner em `/action/exec`.
  * O executável recebe um único argumento a partir da linha de comandos. Esse argumento é uma serialização de sequência do objeto JSON que representa os argumentos para a ação.
  * O programa pode efetuar log em `stdout` ou em `stderr`.
  * Por convenção, a última linha de saída deve ser um objeto da JSON em sequência que represente o resultado da ação.

4. Construa a imagem do Docker e faça upload da mesma usando um script fornecido.
    1. Efetue login no Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Acesse o diretório  ` dockerSkeleton ` .
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Execute o script.
        ```
        ./buildAndPush.sh < username> /blackboxdemo
        ```
        {: pre}

5. Use seu contêiner do Docker para criar uma ação.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Invoque a ação.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Exemplo de Saída:
    ```
    {
        "args": {
            "payload": "Rey"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```
    {: screen}

7. Para atualizar a ação do Docker, faça upload da imagem mais recente para o Docker Hub. Isso permitirá que o sistema puxe a sua nova imagem do Docker na próxima vez em que ele executar o código para a sua ação.
    ```
    ./buildAndPush.sh < username> /blackboxdemo
    ```
    {: pre}

8. Se houver um contêiner quente que usa uma versão anterior de sua imagem do Docker, quaisquer novas chamadas continuarão a usar essa imagem. Atualize a ação para que novas chamadas iniciem o uso da nova imagem.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Opcional: é possível usar o argumento `--native` como uma abreviação para `--docker openwhisk/dockerskeleton`. Esse argumento facilita a criação e a implementação de executáveis que são executados dentro do SDK de ação do Docker padrão.
    1. As etapas acima criam um binário executável dentro do contêiner localizado em `/action/exec`. Copie o arquivo `/action/exec` para seu sistema de arquivos local e compacte-o em `exec.zip`.
    2. Crie uma ação do Docker que receba o executável como dados de inicialização. O argumento `--native` substitui o argumento `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Criando ações do Go
{: #creating-go-actions}

O argumento `--native` permite que você compacte qualquer executável Go como uma ação.

Observe os requisitos e limites a seguir.
  * O executável Go recebe um único argumento da linha de comandos. O argumento é uma serialização de sequência do objeto JSON que representa os argumentos para a ação.
  * O programa pode efetuar log em `stdout` ou em `stderr`.
  * Por convenção, a última linha de saída deve ser um objeto da JSON em sequência que represente o resultado da ação.

Para criar uma ação Ir:

1. Salve o código a seguir em um arquivo denominado `sample.go`.
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

2. Compilação cruzada  ` sample.go `  para  {{site.data.keyword.openwhisk_short}}. O executável deve ser chamado `exec`.
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. Invoque a ação.
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## Criando sequências de ação
{: #openwhisk_create_action_sequence}

É possível criar uma ação que encadeia uma sequência de ações juntas. O resultado de uma ação é passado
como um argumento para a próxima ação.
{: shortdesc}

Várias ações do utilitário são fornecidas no pacote `/whisk.system/utils` que podem ser usadas para criar sua primeira sequência.

1. Liste as ações no pacote `/whisk.system/utils`.
    ```
    ibmcloud fn package get -- summary /whisk.system/utils
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

2. Usando as ações `split` e `sort`, crie uma sequência de ações para que o resultado de `split` seja passado como um argumento para `sort`. Essa sequência de ações converte algumas linhas de texto a uma matriz e classifica as linhas.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Invoque a ação.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Na saída, as linhas de divisão são classificadas em ordem alfabética.
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

** Nota **:
* Os parâmetros que são passados entre as ações na sequência são explícitos, exceto para os parâmetros padrão. Portanto, os parâmetros passados para a sequência de ações estão disponíveis somente para a primeira ação na sequência. O resultado da primeira ação na sequência se torna o objeto JSON de entrada para a segunda ação na sequência e assim por diante. Esse objeto não inclui nenhum dos parâmetros que são originalmente passados para a sequência, a menos que a primeira ação os inclua explicitamente em seu resultado. Os parâmetros de entrada para uma ação são mesclados com os parâmetros padrão da ação, com o antigo tendo precedência e substituindo qualquer parâmetro padrão correspondente. Para obter mais informações sobre como chamar sequências de ações com múltiplos parâmetros nomeados, veja [Configurando parâmetros padrão em uma ação](./parameters.html#default-params-action).
* Uma sequência não tem um tempo limite geral separado dos tempos limite de cada ação dentro da sequência. Como uma sequência é um pipeline de operações, uma falha em uma ação quebra o pipeline. Se uma ação atingir o tempo limite, a sequência inteira será encerrada com essa falha.

## Gerenciando ações grandes
{: #large-app-support}

O tamanho máximo do código para uma ação é 48 MB. Os aplicativos que contêm muitos módulos de terceiros, bibliotecas nativas ou ferramentas externas podem atingir esse limite. Se você criar uma ação de pacote .zip ou .jar que seja maior que 48 MB, deverá estender a imagem de tempo de execução com dependências e, em seguida, usar um único arquivo de origem ou archive menor do que 48 MB.

Por exemplo, ao construir um tempo de execução do Docker customizado que inclui bibliotecas compartilhadas necessárias, as dependências não precisam estar presentes no archive. Os arquivos de origem privados podem ser empacotados no archive e injetados no tempo de execução.

### Reduzindo o tamanho das ações
{: #large-app-reduce}

Para reduzir o tamanho do código de um app Python:

1. Coloque a biblioteca `opencv-python` no `opencv`.
2. Instale o binário opencv na imagem do S.O.
3. Aumente a imagem com mais bibliotecas Python ao executar `pip install requirements.txt`.
4. Use  ` action.py `  com a nova imagem.

Para reduzir o tamanho do código de um app Node.js:

1. Instale o  ` opencv `.
   ```
   npm install opencv
   ```
   {: pre}

2. Instale o  ` package.json `.
   ```
   npm install package.json
   ```
   {: pre}

3. Use  ` action.js `  com a nova imagem.
