---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

---



{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}




# CLI do {{site.data.keyword.openwhisk_short}}
{: #functions-cli}

Execute esses comandos para gerenciar as entidades que compõem suas funções.
{: shortdec}

<br />

## Comandos de ação
{: #cli_action}



### `ibmcloud fn action create`
{: #cli_action_create}

Crie uma ação.

```
ibmcloud fn action create ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>O nome da ação. Para incluir a ação em um pacote, insira o nome no formato `PACKAGE_NAME`/`ACTION_NAME`. Este valor é obrigatório. </dd>

   <dt>`APP_FILE`</dt>
   <dd>O caminho para o arquivo ou o pacote de app a ser executado como uma ação. Essa opção é necessária.</dd>
   
   <dt>`--copy`</dt>
   <dd>Trate a ação como o nome de uma ação existente.</dd>

   <dt>`--docker` `DOCKER_HUB_USERNAME`/`IMAGE_NAME`</dt>
   <dd>O nome do usuário do Docker Hub e o nome da imagem do Docker no Docker Hub para executar a ação. Essa sinalização é necessária para criar ações por meio de imagens do Docker.</dd>

   <dt>`--kind` `LANGUAGE`</dt>
   <dd>O tempo de execução para seu app. Este sinalizador é opcional. Se nenhum `VALUE` for especificado, a versão padrão para o tempo de execução detectado será usada.
     Possíveis `VALUES` para a opção `--kind`.
     <table>
  <tr>
    <th>Idioma</th>
    <th>Identificador de tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (padrão), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (padrão)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (padrão)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (padrão)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (padrão)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (padrão)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (padrão)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (padrão)</td>
  </tr>
  <tr>
    <td>Outros idiomas são suportados usando ações do Docker.</td>
  </tr>
</table>
{: caption="Tabela 1. Tempos de execução suportados" caption-side="top"}
       </dd>

   <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
   <dd>O tamanho máximo de log em MB para a ação. O valor padrão é 10 MB.</dd>

   <dt>`--main` `ENTRY_METHOD_NAME`</dt>
   <dd>Se o método de entrada da ação não for `main`, especifique o nome customizado. Essa sinalização é necessária quando o método de entrada não é `main`. Para alguns tempos de execução, tais como Java, o nome deve ser o método completo.</dd>

   <dt>`--native`</dt>
   <dd>É possível usar o argumento `--native` como abreviação para `--docker openwhisk/dockerskeleton`. Ao usar esse argumento, é possível criar e implementar um executável que é executado dentro do SDK de ação padrão do Docker.
       <ol><li>Quando você cria uma imagem do Docker, um executável é criado dentro do contêiner em `/action/exec`. Copie o arquivo `/action/exec` em seu sistema de arquivos local e compacte-o em `exec.zip`.</li>
       <li>Crie uma ação do Docker que receba o executável como dados de inicialização. O argumento `--native` substitui o argumento `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

   <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
   <dd>Crie uma sequência de ações e inclua os nomes das ações relacionadas. Separe os `ACTION_NAMEs` por vírgulas.</dd>

   <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
   <dd>O `LIMIT` de tempo limite em milissegundos. O valor padrão é 60000 milissegundos. Quando o tempo limite for atingido, a ação será finalizada.</dd>

   <dt>`--web yes|true|raw|no|false`</dt>
   <dd>Trate a ação como uma ação da web, uma ação da web HTTP bruta ou como uma ação padrão. Especifique `yes` ou `true` para uma ação da web, `raw` para uma ação da web HTTP bruta ou `no` ou `false` para uma ação padrão. Para proteger sua ação da web, inclua empacotá-la a opção `--web-secure`.</dd>

   <dt>`--web-secure` `SECRET`</dt>
   <dd>Proteja a ação da web. O `VALUE` para `SECRET` pode ser `true`, `false` ou qualquer sequência. Essa opção pode ser usada apenas com a opção `--web`.</dd>
   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  **Saída**
  ```
  ok: created hello
  ```
  {: screen}


<br />

### `ibmcloud fn action delete`
{: #cli_action_delete}

É possível limpar seu namespace excluindo ações que você não deseja usar mais.

```
ibmcloud fn action delete ACTION_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  **Saída**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn action get`
{: #cli_action_get}

Obtenha metadados que descrevam uma ação específica.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as FILENAME] [--summary] [--url]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>O nome de uma ação. Este valor é obrigatório.</dd>

   <dt>`--save`</dt>
   <dd>É possível obter e salvar localmente o código que está associado a uma ação existente, exceto para sequências e ações do Docker. O `FILENAME` corresponde a um nome de ação existente no diretório atualmente em funcionamento e a extensão do arquivo corresponde ao tipo de ação. Por exemplo, para o código de ação que é um archive, uma extensão de .zip é usada. Este sinalizador é opcional.</dd>

  <dt>`--save-as` `FILENAME`</dt>
  <dd>Salve o código para ações em um arquivo nomeado customizado, fornecendo um caminho de arquivo, `FILENAME` e extensão. Este sinalizador é opcional.</dd>

  <dt>`--summary`</dt>
  <dd>Obtenha um resumo dos detalhes da ação. Os parâmetros com o prefixo "*" são ligados; os parâmetros com o prefixo "**" são ligados e finalizados. Este sinalizador é opcional.</dd>

  <dt>`--url`</dt>
  <dd>Obtenha a URL somente para a ação. Este sinalizador é opcional.</dd>
   </dl>

<br />**Exemplo**

```
ibmcloud fn action get hello
```
{: pre}

**Saída**
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
            "KEY": "exec",
            "VALUE": "nodejs:6"
        }
    ],
    "LIMIT s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />

### `ibmcloud fn action invoke`
{: #cli_action_invoke}

Execute uma ação para testá-la.

```
ibmcloud fn action invoke ACTION_NAME [--blocking] [--param KEY VALUE] [--param-file FILE] [--result]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>O nome da ação. Este valor é obrigatório. </dd>

   <dt>`--blocking, -b`</dt>
   <dd>As chamadas de bloqueio usam um estilo de solicitação e resposta para aguardar que o resultado de ativação esteja disponível. O período de espera é o menor valor entre 60 segundos ou [`LIMIT``VALUE` do tempo](/docs/openwhisk?topic=cloud-functions-limits) da ação. Este sinalizador é opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

   <dt>`--result, -r`</dt>
   <dd>O resultado do código do app é exibido como a saída do comando. Se essa opção não for especificada, o ID de ativação será exibido. A chamada é de bloqueio quando essa opção é especificada. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />

### `      ibmcloud fn action list
      `
{: #cli_action_list}

Liste todas as ações que você criou ou um número específico de ações.

```
ibmcloud fn action list ACTION_NAME [--limit NUMBER_OF_ACTIONS] [--name-sort] [--skip NUMBER_OF_ACTIONS]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`ACTION_NAME`</dt>
   <dd>O nome de um pacote de ações. Esse valor é opcional. Se não especificado, todas as ações serão listadas.</dd>

   <dt>`--limit` `NUMBER_OF_ACTIONS`, -l `NUMBER_OF_ACTIONS`</dt>
   <dd>Liste um número especificado de ações. O padrão é 30 ações.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Classifique a lista de ações retornadas por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>`--skip` `NUMBER_OF_ACTIONS`, -s `NUMBER_OF_ACTIONS`</dt>
   <dd>Exclua um número especificado das ações criadas mais recentemente por meio do resultado.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />

### `ibmcloud fn action update`
{: #cli_action_update}

Atualize uma ação ou o app dentro de uma ação.

Ao atualizar os parâmetros para um pacote, uma ação ou um acionador, deve-se especificar todos os parâmetros criados anteriormente. Caso contrário, os parâmetros criados anteriormente serão removidos. Para pacotes, todos os serviços que foram ligados ao pacote também são removidos, portanto, depois de atualizar outros parâmetros, deve-se [ligar serviços](/docs/openwhisk?topic=cloud-functions-services) ao seu pacote novamente.
{: important}

```
ibmcloud fn action update ACTION_NAME APP_FILE [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize LIMIT] [--main ENTRY_METHOD_NAME] [--native] [--param KEY VALUE] [--param-file FILE] [--sequence ACTION_NAME, ACTION_NAME] [--timeout LIMIT] [--web yes|true|raw|no|false] [--web-secure SECRET]
```
{: pre}

<br />**Opções de comandos**

  <dl>
  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>O nome da ação. Para incluir a ação em um pacote, insira o nome no formato `PACKAGE_NAME`/`ACTION_NAME`. Este valor é obrigatório. </dd>

  <dt>`APP_FILE`</dt>
  <dd>O caminho para o arquivo ou o pacote de app a ser executado como uma ação. Essa opção é necessária quando você deseja atualizar seu app dentro da ação.</dd>

  <dt>`--copy`</dt>
  <dd>Trate a ação como o nome de uma ação existente.</dd>

  <dt>`--docker DOCKER_HUB_USERNAME/IMAGE_NAME`</dt>
  <dd>O nome do usuário do Docker Hub e o nome da imagem do Docker no Docker Hub para executar a ação. Essa sinalização é necessária para criar ações por meio de imagens do Docker.</dd>

  <dt>`--kind LANGUAGE`</dt>
  <dd>O tempo de execução para seu app. Este sinalizador é opcional. Se nenhum VALUE for especificado, a versão padrão para o tempo de execução detectado será usada.
    Possíveis valores para a opção `--kind`.
    <table>
  <tr>
    <th>Idioma</th>
    <th>Identificador de tipo</th>
  </tr>
  <tr>
    <td>Node.js</td>
    <td> <code>nodejs:10</code> (padrão), <code>nodejs:8</code></td>
  </tr>
  <tr>
    <td>Python</td>
    <td><code>python:3.7</code>, <code>python:3.6</code>, <code>python:2</code> (padrão)</td>
  </tr>
  <tr>
    <td>Swift</td>
    <td><code>swift:4.2</code> (padrão)</td>
  </tr>
  <tr>
    <td>PHP</td>
    <td><code>php:7.3</code> (padrão)</td>
  </tr>
  <tr>
    <td>Go</td>
    <td><code>go:1.11</code> (padrão)</td>
  </tr>
  <tr>
    <td>Ruby</td>
    <td><code>ruby:2.5</code> (padrão)</td>
  </tr>
  <tr>
    <td>Java</td>
    <td><code>java (JDK 8)</code> (padrão)</td>
  </tr>
  <tr>
    <td>.NET Core</td>
    <td><code>dotnet:2.2</code> (padrão)</td>
  </tr>
  <tr>
    <td>Outros idiomas são suportados usando ações do Docker.</td>
  </tr>
</table>
{: caption="Tabela 1. Tempos de execução suportados" caption-side="top"}
      </dd>

  <dt>`--logsize` `LIMIT`, `-l` `LIMIT`</dt>
  <dd>O tamanho máximo de log em MB para a ação. O valor padrão é 10 MB.</dd>

  <dt>`--main ENTRY_METHOD_NAME`</dt>
  <dd>Se o método de entrada da ação não for `main`, especifique o nome customizado. Essa sinalização é necessária quando o método de entrada não é `main`. Para alguns tempos de execução, tais como Java, o nome deve ser o método completo.</dd>

  <dt>`--native`</dt>
  <dd>É possível usar o argumento `--native` como abreviação para `--docker openwhisk/dockerskeleton`. Ao usar o argumento, é possível criar e implementar um executável que é executado dentro do SDK de ação do Docker padrão.
      <ol><li>Quando você cria uma imagem do Docker, um executável é criado dentro do contêiner em `/action/exec`. Copie o arquivo `/action/exec` em seu sistema de arquivos local e compacte-o em `exec.zip`.</li>
      <li>Crie uma ação do Docker que receba o executável como dados de inicialização. O argumento `--native` substitui o argumento `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Um arquivo JSON que contém parâmetros `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

  <dt>`--sequence` `ACTION_NAME`, `ACTION_NAME`</dt>
  <dd>Crie uma sequência de ações, especificando o nome de ações relacionadas.</dd>

  <dt>`--timeout` `LIMIT`, `-t` `LIMIT`</dt>
  <dd>O limite de tempo limite em milissegundos. O valor padrão é 60000 milissegundos. Quando o tempo limite for atingido, a ação será finalizada.</dd>

  <dt>`--web yes|true|raw|no|false`</dt>
  <dd>Trate a ação como uma ação da web, uma ação da web HTTP bruta ou como uma ação padrão. Especifique `yes` ou `true` para uma ação da web, `raw` para uma ação da web HTTP bruta ou `no` ou `false` para uma ação padrão. Para proteger sua ação da web, inclua empacotá-la a opção `--web-secure`.</dd>

  <dt>`--web-secure` `SECRET`</dt>
  <dd>Proteja a ação da web. O `VALUE` para `SECRET` pode ser `true`, `false` ou qualquer sequência. Essa opção pode ser usada apenas com a opção `--web`.</dd>
  </dl>

<br />**Exemplo**
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Comandos de ativação
{: #cli_activation}


### `ibmcloud fn activation get`
{: #cli_activation_get}

Obtenha metadados que descrevam uma ativação específica.

```
ibmcloud fn activation get [ACTIVATION_ID] [FIELD_FILTER] [--last] [--summary]
```
{: pre}


<br />**Opções de comandos**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>O ID para uma ativação específica. Use `ibmcloud fn activation list` para recuperar uma lista de IDs disponíveis. Esse valor é necessário, a menos que a opção `--last` ou `-l` seja especificada.</dd>

  <dt>`FIELD_FILTER`</dt>
  <dd>Um campo nos metadados do qual exibir informações. Por exemplo, para exibir o campo de logs, execute `ibmcloud fn activation get ACTIVATION_ID logs`. Esse valor é opcional.</dd>

  <dt>`--last, -l`</dt>
  <dd>Exiba os metadados para a ativação mais recente. Este sinalizador é opcional.</dd>

  <dt>`--summary, -s`</dt>
  <dd>Exiba a resposta do resultado somente por meio dos detalhes de ativação. Este sinalizador é opcional.</dd>
  </dl>


<br />**Exemplo**
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation list
`
{: #cli_activation_list}

Liste todos os IDs de ativação para todas as ações em um pacote.

```
ibmcloud fn activation list [--full] [--limit NUMBER_OF_ACTIVATIONS] [--since UNIX_EPOCH_TIME] [--skip NUMBER_OF_ACTIVATIONS] [--upto UNIX_EPOCH_TIME]
```
{: pre}


<br />**Opções de comandos**

  <dl>
  <dt>`--full, -f`</dt>
  <dd>Exibir a descrição completa da ativação.</dd>

  <dt>`--limit` `NUMBER_OF_ACTIVATIONS`, `-l` `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Listar um número especificado de ativações. O padrão é 30 ativações e o máximo é 200 ativações.</dd>

  <dt>`--since` `UNIX_EPOCH_TIME`</dt>
  <dd>Listar ativações que foram criadas desde a data especificada. A duração é medida em milissegundos desde 01 de janeiro de 1970. Exemplo: `1560371263` é 12 de junho de 2019 08:27:43 UTC.</dd>

  <dt>`--skip` `NUMBER_OF_ACTIVATIONS`, -s `NUMBER_OF_ACTIVATIONS`</dt>
  <dd>Excluir um número especificado de ativações mais recentes do resultado.</dd>

  <dt>`--upto` `UNIX_EPOCH_TIME`</dt>
  <dd>Listar ativações que foram criadas antes da data especificada. A duração é medida em milissegundos desde 01 de janeiro de 1970. Exemplo: `1560371263` é 12 de junho de 2019 08:27:43 UTC.</dd>
  </dl>

<br />**Exemplo**
```
ibmcloud fn activation list
```
{: pre}

**Saída**
```
ativações
  44794bd6aab74415b4e42a308d880e5b         hello
  6bf1f670ee614a7eb5af3c9fde813043         hello
```
{: screen}


<br />

### `ibmcloud fn activation logs`
{: #cli_activation_logs}

Obtenha logs para uma ativação específica.

```
ibmcloud fn activation logs [ACTIVATION_ID] [--last] [--strip]
```
{: pre}

<br />**Opções de comandos**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>O ID para uma ativação específica. Use `ibmcloud fn activation list` para recuperar uma lista de IDs disponíveis. Esse valor é necessário, a menos que a opção `--last` ou `-l` seja especificada.</dd>

  <dt>`--last, -l`</dt>
  <dd>Exibir os logs para a ativação mais recente. Este sinalizador é opcional.</dd>

  <dt>`--strip, -r`</dt>
  <dd>Exibir apenas a mensagem de log; excluir as informações de registro de data e hora e de fluxo. Este sinalizador é opcional.</dd>
  </dl>

<br />**Exemplo**
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />

### `ibmcloud fn activation poll`
{: #cli_activation_poll}

Visualizar um fluxo, lista ativa de ativações para uma ação ou um namespace. É possível pressionar `CTRL + C` para sair da pesquisa.

```
ibmcloud fn activation poll [NAMESPACE] [ACTION_NAME] [--exit SECONDS] [--since-days DAYS] [-since-hours HOURS] [--since-minutes MINUTES] [--since-seconds SECONDS]
```
{: pre}

<br />**Opções de comandos**

  <dl>
  <dt>/`NAMESPACE`</dt>
  <dd>Um namespace iniciando com /. Ativações de pesquisa para um namespace, uma ação ou um espaço. Esse valor é opcional. Se um namespace ou uma ação não for especificada, o espaço será pesquisado.</dd>

  <dt>`ACTION_NAME`</dt>
  <dd>Ativações de pesquisa para um namespace, uma ação ou um espaço. Esse valor é opcional. Se um namespace ou uma ação não for especificada, o espaço será pesquisado.</dd>

  <dt>`--exit` `SECONDS`, `-e` `SECONDS`</dt>
  <dd>Pesquisar ativações um número especificado de segundos e, em seguida, sair. Este sinalizador é opcional.</dd>

  <dt>`--since-days` `DAYS`</dt>
  <dd>Iniciar pesquisa para ativações um número especificado de dias atrás. Este sinalizador é opcional.</dd>

  <dt>`--since-hours` `HOURS`</dt>
  <dd>Iniciar pesquisa para ativações um número especificado de horas atrás. Este sinalizador é opcional.</dd>

  <dt>`--since-minutes` `MINUTES`</dt>
  <dd>Iniciar pesquisa para ativações um número especificado de minutos atrás. Este sinalizador é opcional.</dd>

  <dt>`--since-seconds` `SECONDS`</dt>
  <dd>Iniciar pesquisa para ativações um número especificado de segundos atrás. Este sinalizador é opcional.</dd>
  </dl>

<br />**Exemplo**
```
ibmcloud fn activation poll
```
{: pre}


<br />

### `ibmcloud fn activation result`
{: #cli_activation_result}

Obtenha o resultado de uma ativação específica.

```
ibmcloud fn activation result [ACTIVATION_ID] [--last] [--strip]
```
{: pre}


<br />**Opções de comandos**

  <dl>
  <dt>`ACTIVATION_ID`</dt>
  <dd>O ID para uma ativação específica. Use `ibmcloud fn activation list` para recuperar uma lista de IDs disponíveis. Esse valor é necessário, a menos que a opção `--last` ou `-l` seja especificada.</dd>

  <dt>`--last, -l`</dt>
  <dd>Exibir o resultado para a ativação mais recente. Este sinalizador é opcional.</dd>

  </dl>

<br />**Exemplo**
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## Comandos de API
{: #cli_api}


### `ibmcloud fn api create`
{: #cli_api_create}

Crie uma API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB ACTION_NAME] [--apiname API_NAME] [--config-file FILE] [--response-type TYPE]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>O caminho base para a API.</dd>

   <dt>`API_NAME`</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>`API_VERB`</dt>
   <dd>O verbo para a API, tal como `get` ou `post`.</dd>

   <dt>`ACTION_NAME`</dt>
   <dd>O nome da ação.</dd>

   <dt>`--apiname API_NAME`, `-n API_NAME`</dt>
   <dd>O nome da API. Essa sinalização é ignorada quando um arquivo de configuração é especificado. O nome padrão é o `BASE_PATH`. Este sinalizador é opcional.</dd>

   <dt>`--config-file` `FILE`, `-c` `FILE`</dt>
   <dd>Um arquivo JSON que contém a configuração da API do Swagger. Quando esse sinalizador é usado, o sinalizador de nome da API é ignorado. Esse sinalizador é necessário.</dd>

   <dt>`--response-type TYPE`</dt>
   <dd>Configure o tipo de resposta da ação da web como `html`, `http`, `json`, `text` ou `svg`. O valor padrão é `json`. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  **Saída**
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />

### `ibmcloud fn api delete`
{: #cli_api_delete}

Exclua uma API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>O caminho base para a API.</dd>

   <dt>`API_NAME`</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>`API_PATH`</dt>
   <dd>O caminho para a API.</dd>

   <dt>`API_VERB`</dt>
   <dd>O verbo para a API, tal como `GET` ou `POST`.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Especifique o tipo de saída da API como `json` ou `yaml`. O valor padrão é `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Exiba os detalhes completos de configuração da API.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />

### `ibmcloud fn api get`
{: #cli_api_get}

Obtenha os metadados para uma API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>O caminho base para a API.</dd>

   <dt>`API_NAME`</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>`--format OUTPUT_TYPE`</dt>
   <dd>Especifique o tipo de saída da API como `json` ou `yaml`. O valor padrão é `json`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Exiba os detalhes completos de configuração da API.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />

### `  ibmcloud fn api list
  `
{: #cli_api_list}

Liste todas as APIs que você criou ou um número específico de APIs. Se nenhum nome ou caminho base for especificado, todas as APIs serão listadas.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`BASE_PATH`</dt>
   <dd>O caminho base para a API.</dd>

   <dt>`API_NAME`</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>`API_PATH`</dt>
   <dd>O caminho para a API.</dd>

   <dt>`API_VERB`</dt>
   <dd>O verbo para a API, tal como `GET` ou `POST`.</dd>

   <dt>`--full, -f`</dt>
   <dd>Exibir os detalhes completos da API. Este sinalizador é opcional. </dd>

   <dt>`--limit NUMBER_OF_APIS`, `-l NUMBER_OF_APIS`</dt>
   <dd>Listar um número especificado de APIs. O padrão é 30 APIs. Este sinalizador é opcional. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Classifique a lista de APIs retornadas por nome, caso contrário, a lista será classificada por data de criação. Este sinalizador é opcional. </dd>

   <dt>`--skip NUMBER_OF_APIS`, `-s NUMBER_OF_APIS`</dt>
   <dd>Excluir um número especificado das APIs criadas mais recentemente por meio do resultado. Este sinalizador é opcional. </dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn api list
  ```
  {: pre}


<br /><br />
## Comandos de implementação
{: #cli_deploy_cmds}


### `ibmcloud fn deploy`
{: #cli_deploy}

Use um arquivo manifest para implementar uma coleção de pacotes, ações, acionadores e regras.

```
ibmcloud fn deploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`--apihost HOST`</dt>
   <dd>O host da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>O arquivo de configuração. O padrão é `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>O caminho para o arquivo de implementação.</dd>

   <dt>`--manifest` `FILE`, `-m` `FILE`</dt>
   <dd>O caminho do arquivo de manifesto. Essa sinalização será necessária se o manifest.yaml não estiver no diretório atual.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>O nome ou o ID para um namespace.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

   <dt>`--preview` </dt>
   <dd>Exiba o plano de implementação antes de implementar.</dd>

   <dt>`--project PATH`</dt>
   <dd>O caminho para o projeto sem servidor. O padrão é <code>.</code> (diretório atual).</dd>

   <dt>`--strict`</dt>
   <dd>Permitir uma versão de tempo de execução definida pelo usuário.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Visualizar a saída detalhada.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />

### `ibmcloud fn undeploy`
{: #cli_undeploy}

Use um arquivo manifest para remover a implementação de uma coleção de pacotes, ações, acionadores e regras.

```
ibmcloud fn undeploy [--apihost HOST] [--auth KEY] [--config FILE] [--deployment FILE] [--manifest FILE] [--namespace NAMESPACE] [--param KEY VALUE] [--param-file FILE] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`--apihost HOST`</dt>
   <dd>O host da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--config` `FILE`</dt>
   <dd>O arquivo de configuração. O padrão é `$HOME/.wskprops`.</dd>

   <dt>`--deployment` `FILE`</dt>
   <dd>O caminho para o arquivo de implementação.</dd>

   <dt>`--manifest` `FILE`, -m `FILE`</dt>
   <dd>O caminho do arquivo de manifesto. Essa sinalização será necessária se o manifest.yaml não estiver no diretório atual.</dd>

   <dt>`--namespace` `NAMESPACE`, `-n` `NAMESPACE`</dt>
   <dd>O nome ou o ID para um namespace.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

   <dt>`--preview` </dt>
   <dd>Exibir o resultado do comando sem executar o comando.</dd>

   <dt>`--project PATH`</dt>
   <dd>O caminho para o projeto sem servidor. O padrão é `.` (diretório atual).</dd>

   <dt>`--strict`</dt>
   <dd>Permitir uma versão de tempo de execução definida pelo usuário.</dd>

   <dt>`--verbose, -v`</dt>
   <dd>Visualizar a saída detalhada.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## Comando List
{: #cli_list_cmd}


### `  ibmcloud fn list
  `
{: #cli_list}

Visualizar uma lista agrupada de pacotes, ações, acionadores e regras no namespace.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`--name-sort, -n`</dt>
   <dd>Classificar cada grupo de entidades retornadas por nome, caso contrário, cada grupo será classificado pela data de criação.</dd>
   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Comandos de namespace
{: #cli_namespace}


### `ibmcloud fn namespace create`
{: #cli_namespace_create}

Crie um namespace do IAM.

```
ibmcloud fn namespace create NAMESPACE [--description DESCRIPTION] 
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>O nome para um namespace. Não inclua hifens (-) no nome. Este valor é obrigatório.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Escreva sua própria descrição exclusiva para ajudar a identificar o namespace. Se a sua descrição tiver mais de uma palavra, inclua aspas (") em torno dela. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. Consulte Beth para obter informações sobre esse namespace."
  ```
  {: pre}


<br />

### `ibmcloud fn namespace delete`
{: #cli_namespace_delete}

Exclua um namespace do IAM.

```
ibmcloud fn namespace delete NAMESPACE
```
{: pre}


<br />**Exemplo**

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />

### `ibmcloud fn namespace get`
{: #cli_namespace_get}

Obtenha as entidades para ou as informações de metadados de um namespace do Cloud Foundry ou IAM.

```
ibmcloud fn namespace list NAMESPACE [--auth KEY] [--name-sort] [--properties] 
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>O nome ou o ID para um namespace. Este valor é obrigatório.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Classifique a lista de namespaces retornados por nome, caso contrário, a lista será classificada por data de criação. Este sinalizador é opcional. </dd>

   <dt>`--properties`</dt>
   <dd>Exibir as propriedades do namespace em vez das entidades contidas nele. Este sinalizador é opcional. </dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  **Saída**
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />

### `  ibmcloud fn namespace list
  `
{: #cli_namespace_list}

Listar os namespaces disponíveis do Cloud Foundry e do IAM.

```
ibmcloud fn namespace list [--auth KEY] [--cf] [--iam] [--limit NUMBER_OF_NAMESPACES] [--name-sort] [--skip NUMBER_OF_NAMESPACES] 
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--cf`</dt>
   <dd>Exibir apenas os namespaces do Cloud Foundry. Os namespaces do IAM não são exibidos. Este sinalizador é opcional.</dd>

   <dt>`--iam`</dt>
   <dd>Exibir apenas os namespaces do IAM. Os namespaces do Cloud Foundry não são exibidos. Este sinalizador é opcional.</dd>

   <dt>`--limit NUMBER_OF_``NAMESPACE``S`, `-l NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Listar um número especificado de namespaces. O padrão é 30 namespaces. Este sinalizador é opcional. </dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Classifique a lista de namespaces retornados por nome, caso contrário, a lista será classificada por data de criação. Este sinalizador é opcional. </dd>

   <dt>`--skip NUMBER_OF_NAMESPACES`, `-s NUMBER_OF_``NAMESPACE``S`</dt>
   <dd>Excluir um número especificado dos namespaces criados mais recentemente por meio do resultado. Este sinalizador é opcional. </dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />

### `ibmcloud fn namespace update`
{: #cli_namespace_update}

Mude o nome ou a descrição de um namespace do IAM.

```
ibmcloud fn namespace update NAMESPACE [NEW_NAMESPACE_NAME] [--description DESCRIPTION] 
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`NAMESPACE`</dt>
   <dd>O nome para um namespace. Não inclua hifens (-) no nome. Este valor é obrigatório.</dd>

   <dt>`NEW_``NAMESPACE``_NAME`</dt>
   <dd>O novo nome para um namespace. Não inclua hifens (-) no nome. Esse valor é opcional.</dd>

   <dt>`--description DESCRIPTION`, `-n DESCRIPTION`</dt>
   <dd>Escreva sua própria descrição exclusiva para ajudar a identificar o namespace. Se a sua descrição tiver mais de uma palavra, inclua aspas (") em torno dela. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Comandos package
{: #cli_pkg}


### `ibmcloud fn package bind`
{: #cli_pkg_bind}

Ligar parâmetros a um pacote. Todas as ações dentro do pacote herdam esses parâmetros, a menos que especificado de outra forma.

```
ibmcloud fn package bind PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opções de comandos**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>O nome do pacote. Este valor é obrigatório. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>
  </dl>

<br />**Exemplo**

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />

### `ibmcloud fn package create`
{: #cli_pkg_create}

Crie um pacote projetado para conter uma ou mais ações. Para incluir uma ação no pacote, inclua o nome do pacote com o nome da ação quando criar ou atualizar a ação.

```
ibmcloud fn package create PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opções de comandos**

  <dl>
  <dt>`PACKAGE_NAME`</dt>
  <dd>O nome do pacote. Este valor é obrigatório. </dd>

  <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
  <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

  <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
  <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

  <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
  <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

  <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
  <dd>Um arquivo JSON que contém o formato de parâmetro `KEYS``VALUE`. Este sinalizador é opcional.</dd>

  <dt>`--shared yes|no`</dt>
  <dd>Quando especificado sem um valor ou com um valor de yes, o pacote é compartilhado com outros usuários.</dd>
  </dl>

<br />**Exemplo**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Saída**
  ```
  ok: hellopkg criado
  ```
  {: screen}


<br />

### `ibmcloud fn package delete`
{: #cli_pkg_delete}

É possível limpar seu namespace excluindo pacotes que você não deseja usar mais.

```
ibmcloud fn package delete PACKAGE_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  **Saída**
  ```
  ok: deleted hello
  ```
  {: screen}


<br />

### `ibmcloud fn package get`
{: #cli_pkg_get}

Obtenha metadados que descrevam um pacote específico.

```
ibmcloud fn package get PACKAGE_NAME [--summary]
```
{: pre}

<br />**Opções de comandos**

  <dl>
   <dt>`PACKAGE_NAME`</dt>
   <dd>O nome de um pacote. Este valor é obrigatório.</dd>

   <dt>`--summary`</dt>
   <dd>Obtenha um resumo dos detalhes do pacote. Os parâmetros com o prefixo "*" são ligados. Este sinalizador é opcional.</dd>
   </dl>

<br />**Exemplo**

```
ibmcloud fn package get hello
```
{: pre}


<br />

### `      ibmcloud fn package list
      `
{: #cli_pkg_list}

Liste todos os pacotes que você criou ou um número específico de pacotes.

```
ibmcloud fn package list [NAMESPACE] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`NAMESPACE`</dt>
   <dd>Liste os pacotes em um namespace específico. Esse valor é opcional. Se não especificado, todos os pacotes serão listados.</dd>

   <dt>`--limit NUMBER_OF_PACKAGES`, `-l NUMBER_OF_PACKAGES`</dt>
   <dd>Listar um número especificado de pacotes. O padrão é 30 pacotes.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Classificar a lista de pacotes retornados por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>`--skip NUMBER_OF_PACKAGES`, `-s NUMBER_OF_PACKAGES`</dt>
   <dd>Excluir um número especificado dos pacotes criados mais recentemente do resultado.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Execute `ibmcloud fn package list /whisk.system` para visualizar uma lista de pacotes pré-instalados.
  {: tip}


<br />

### `Atualização do pacote ibmcloud fn`
{: #cli_pkg_refresh}

Atualize as ligações do pacote para todos os pacotes dentro de um namespace específico.

```
ibmcloud fn package refresh /NAMESPACE
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>/`NAMESPACE`</dt>
   <dd>Um namespace iniciando com /. Esse sinalizador é necessário. Execute `ibmcloud fn namespace list` para obter uma lista de namespaces para escolher.</dd>
   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />

### `ibmcloud fn package update`
{: #cli_pkg_update}

Atualize um pacote projetado para conter uma ou mais ações. Para incluir uma ação no pacote, inclua o nome do pacote com o nome da ação quando criar ou atualizar a ação.

Ao atualizar os parâmetros para um pacote, uma ação ou um acionador, deve-se especificar todos os parâmetros criados anteriormente. Caso contrário, os parâmetros criados anteriormente serão removidos. Para pacotes, todos os serviços que foram ligados ao pacote também são removidos, portanto, depois de atualizar outros parâmetros, deve-se [ligar serviços](/docs/openwhisk?topic=cloud-functions-services) ao seu pacote novamente.
{: important}

```
ibmcloud fn package update PACKAGE_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`PACKAGE_NAME`</dt>
   <dd>O nome do pacote. Este valor é obrigatório. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

   <dt>`--shared yes|no`</dt>
   <dd>Quando especificado sem um valor ou com um valor de `yes`, o pacote é compartilhado com outros usuários.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  **Saída**
  ```
  ok: hellopkg criado
  ```
  {: screen}




<br /><br />
## Comandos de propriedade
{: #cli_prop}

Configure propriedades globais para seu ambiente de CLI ou visualize propriedades sobre a CLI `wsk`, que é executada como parte da CLI `ibmcloud fn`.

### `ibmcloud fn property get`
{: #cli_prop_get}

Visualize os detalhes de metadados para uma propriedade por meio da CLI `wsk`.

```
ibmcloud fn property get [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`--all`</dt>
   <dd>Visualize todas as propriedades para a CLI `wsk`. Este sinalizador é opcional.</dd>

   <dt>`---apibuild`</dt>
   <dd>As informações de construção da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--apibuildno`</dt>
   <dd>O número da construção da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--apihost` `HOST`</dt>
   <dd>O host da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>A versão da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--auth` `KEY`, `-u` `KEY`</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>O certificado de cliente `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--cliversion`</dt>
   <dd>A versão da CLI `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>A `KEY` de cliente `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--namespace ` `NAMESPACE`</dt>
   <dd>Um namespace do IAM. Essa sinalização não pode ser configurada para namespaces do Cloud Foundry. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn property get -- auth
  ```
  {: pre}


<br />

### `ibmcloud fn property set`
{: #cli_prop_set}

Configure uma propriedade. É necessária pelo menos uma sinalização. Depois que uma propriedade é configurada, ela é retida em sua estação de trabalho em `<home_dir>/.bluemix/plugins/cloud-functions/config.json`. Para remover uma propriedade, execute [`ibmcloud fn property unset --<property>`](#cli_prop_set). 

```
ibmcloud fn property set [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>O host da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>A versão da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--auth` `KEY`, -u</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>O certificado de cliente `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>A `KEY` de cliente `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--namespace ` `NAMESPACE`</dt>
   <dd>Um namespace do IAM. Essa sinalização não pode ser configurada para namespaces do Cloud Foundry. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  **Saída**
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### `ibmcloud fn property unset`
{: #cli_prop_unset}

Desconfigure uma propriedade para a CLI `wsk`. É necessária pelo menos uma sinalização.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion VERSION] [--auth KEY] [--cert STRING] [--key STRING] [--namespace NAMESPACE]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`--apihost` `HOST`</dt>
   <dd>O host da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--apiversion` `VERSION`</dt>
   <dd>A versão da API `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--auth` `KEY`, `-u`</dt>
   <dd>A `KEY` de autorização `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--cert` `STRING`</dt>
   <dd>O certificado de cliente `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--key` `STRING`</dt>
   <dd>A `KEY` de cliente `wsk`. Este sinalizador é opcional.</dd>

   <dt>`--namespace ` `NAMESPACE`</dt>
   <dd>Um namespace do IAM. Essa sinalização não pode ser configurada para namespaces do Cloud Foundry. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn property unset --namespace
  ```
  {: pre}



<br /><br />
## Comandos de regra
{: #cli_rule}


### `ibmcloud fn rule create`
{: #cli_rule_create}

Crie uma regra para associar um acionador a uma ação. Antes de poder criar uma regra, crie um acionador e uma ação primeiro.

```
ibmcloud fn rule create RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  **Saída**
  ```
  ok: created myrule
  ```
  {: screen}


<br />

### `ibmcloud fn rule delete`
{: #cli_rule_delete}

Para limpar seu namespace, remova as regras que não são mais necessárias.

```
ibmcloud fn rule delete RULE_NAME [--disable]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>O nome de uma regra. Este valor é obrigatório.</dd>

  <dt>`--disable`</dt>
  <dd>Desative a regra antes da exclusão.</dd>
  </dl>


<br />**Exemplo**

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />

### `ibmcloud fn rule disable`
{: #cli_rule_disable}

Mude o status de uma regra para inativa e impeça-a de executar uma ação quando um acionador for disparado.

```
ibmcloud fn rule disable RULE_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule enable`
{: #cli_rule_enable}

Mude o status de uma regra de inativa para ativa. Quando ativa, uma ação é executada quando um acionador é disparado.

```
ibmcloud fn rule enable RULE_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### `ibmcloud fn rule get`
{: #cli_rule_get}

Obtenha metadados que descrevam uma regra específica.

```
ibmcloud fn rule get RULE_NAME [--summary]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>O nome de uma regra. Este valor é obrigatório.</dd>

  <dt>`--summary`</dt>
  <dd>Obtenha um resumo dos detalhes da regra.</dd>
  </dl>

<br />**Exemplo**

```
ibmcloud fn rule get myrule
```
{: pre}


<br />

### `  ibmcloud fn rule list
  `
{: #cli_rule_list}

Liste todas as regras que você criou ou um número específico de regras.

```
ibmcloud fn rule list RULE_NAME [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>O nome de uma regra. Esse valor é opcional. Se não especificado, todas as regras serão listadas.</dd>

   <dt>`--limit NUMBER_OF_RULES`, `-l NUMBER_OF_RULES`</dt>
   <dd>Listar um número especificado de regras. O padrão é 30 regras.</dd>

   <dt>`--name-sort`, `-n`</dt>
   <dd>Classificar a lista de regras retornadas por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>`--skip NUMBER_OF_RULES`, `-s NUMBER_OF_RULES`</dt>
   <dd>Excluir um número especificado das regras criadas mais recentemente do resultado.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />

### `ibmcloud fn rule status`
{: #cli_rule_status}

Veja se uma regra está ativa ou inativa. Execute os comandos `ibmcloud fn rule disable` ou `ibmcloud fn run enable` para mudar o status.

```
ibmcloud fn rule status RULE_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />

### `ibmcloud fn rule update`
{: #cli_rule_update}

Para mudar quais acionadores estão associados a quais regras, é possível atualizar uma regra.

```
ibmcloud fn rule update RULE_NAME TRIGGER_NAME ACTION_NAME
```
{: pre}

<br />**Exemplo**

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## Comando SDK
{: #cli_sdk}


### `ibmcloud fn sdk install`
{: #cli_sdk_install}

Instale um SDK.

```
ibmcloud fn sdk install COMPONENT [--limit NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`COMPONENT`</dt>
   <dd>O componente SDK, tal como `docker`, `iOS` e `bashauto`. Este valor é obrigatório.</dd>

   <dt>`--stdout, --s`</dt>
   <dd>Imprime os resultados do comando bash para `STDOUT`. Este sinalizador é opcional.</dd>


   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Comandos de serviço
{: #cli_service}


### `ibmcloud fn service bind`
{: #cli_service_bind}

Ligar credenciais de serviço a uma ação ou um pacote.

```
ibmcloud fn service bind SERVICE PACKAGE_or_ACTION_NAME [--instance SERVICE_INSTANCE] [--keyname SERVICE_KEY]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`SERVIÇO`</dt>
   <dd>O nome do serviço.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>O nome do pacote ou da ação à qual ligar as credenciais.</dd>

   <dt>`--instance SERVICE_INSTANCE`</dt>
   <dd>O nome da instância de serviço.</dd>

   <dt>`--keyname SERVICE_``KEY`</dt>
   <dd>O nome das credenciais de serviço `KEY` para ligar.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />

### `ibmcloud fn service unbind`
{: #cli_service_unbind}

Desvincule as credenciais de serviço de uma ação ou um pacote.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_ACTION_NAME
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`SERVIÇO`</dt>
   <dd>O nome do serviço.</dd>

   <dt>`PACKAGE_or_``ACTION_NAME`</dt>
   <dd>O nome do pacote ou ação da qual desvincular as credenciais.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Comandos do acionador
{: #cli_trigger}


### `ibmcloud fn trigger create`
{: #cli_trigger_create}

Crie um acionador.

```
ibmcloud fn trigger create TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--feed ACTION_NAME] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>O nome do acionador. Este valor é obrigatório. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--feed` `ACTION_NAME`, `-f` `ACTION_NAME`</dt>
   <dd>Configura o tipo de acionador como um feed. Este sinalizador é opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>


   </dl>

<br />**Exemplo**
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />

### `ibmcloud fn trigger delete`
{: #cli_trigger_delete}

Exclua um acionador.

```
ibmcloud fn trigger delete TRIGGER_NAME
```
{: pre}

<br />**Exemplo**

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />

### `ibmcloud fn trigger fire`
{: #cli_trigger_fire}

Teste um acionador disparando-o, em vez de esperar que ele seja acionado automaticamente.

```
ibmcloud fn trigger fire TRIGGER_NAME [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opções de comandos**

   <dl>

   <dt>`TRIGGER_NAME`</dt>
   <dd>O nome do acionador. Este valor é obrigatório. </dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Parâmetro `VALUES` no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />

### `ibmcloud fn trigger get`
{: #cli_trigger_get}

Obtenha metadados que descrevam um acionador específico.

```
ibmcloud fn trigger get TRIGGER_NAME [--summary]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>O nome de um acionador. Este valor é obrigatório.</dd>

  <dt>`--summary`</dt>
  <dd>Obtenha um resumo dos detalhes do acionador.</dd>
  </dl>

<br />**Exemplo**

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### `    ibmcloud fn trigger list
    `
{: #cli_trigger_list}

Listar todos os acionadores que você criou ou um número específico de acionadores.

```
ibmcloud fn trigger list TRIGGER_NAME [--limit NUMBER_OF_TRIGGERS] [--name-sort] [--skip NUMBER_OF_TRIGGERS]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`RULE_NAME`</dt>
   <dd>O nome de um acionador. Esse valor é opcional. Se não especificado, todos os acionadores serão listados.</dd>

   <dt>`--limit` `NUMBER_OF_TRIGGERS`, `-l` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Listar um número especificado de acionadores. O padrão é 30 acionadores.</dd>

   <dt>`--name-sort, -n`</dt>
   <dd>Classifique a lista de acionadores retornados por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>`--skip` `NUMBER_OF_TRIGGERS`, `-s` `NUMBER_OF_TRIGGERS`</dt>
   <dd>Exclua um número especificado dos acionadores criados mais recentemente do resultado.</dd>

   </dl>

<br />**Exemplo**

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### `ibmcloud fn trigger update`
{: #cli_trigger_update}

Atualize um acionador.

Ao atualizar os parâmetros para um pacote, uma ação ou um acionador, deve-se especificar todos os parâmetros criados anteriormente. Caso contrário, os parâmetros criados anteriormente serão removidos. Para pacotes, todos os serviços que foram ligados ao pacote também são removidos, portanto, depois de atualizar outros parâmetros, deve-se [ligar serviços](/docs/openwhisk?topic=cloud-functions-services) ao seu pacote novamente.
{: important}

```
ibmcloud fn trigger update TRIGGER_NAME [--annotation ANNOTATION_KEY ANNOTATION_VALUE] [--annotation-file FILE] [--param KEY VALUE] [--param-file FILE]
```
{: pre}

<br />**Opções de comandos**

   <dl>
   <dt>`TRIGGER_NAME`</dt>
   <dd>O nome do acionador. Este valor é obrigatório. </dd>

   <dt>`--annotation` `ANNOTATION_KEY` `ANNOTATION_VALUE`, `-a` `ANNOTATION_KEY` `ANNOTATION_VALUE`</dt>
   <dd>As anotações são especificadas em um formato `KEY` `VALUE`. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>`--annotation-file` `FILE`, `-A` `FILE`</dt>
   <dd>Um arquivo JSON que contém anotação em um formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param` `KEY` `VALUE`, `-p` `KEY` `VALUE`</dt>
   <dd>Valores de parâmetro no formato `KEY` `VALUE`. Este sinalizador é opcional.</dd>

   <dt>`--param-file` `FILE`, `-P` `FILE`</dt>
   <dd>Um arquivo JSON que contém o parâmetro `KEYS` e `VALUES`. Este sinalizador é opcional.</dd>
   </dl>

<br />**Exemplo**
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}






