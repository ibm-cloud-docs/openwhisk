---

copyright:
  years: 2017, [{CURRENT_<em>YEAR</em>}]
lastupdated: "2019-05-20"

keywords: managing actions, manage, activation, action logs, changing runtime, delete

subcollection: cloud-functions-cli-plugin

---



{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
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



### ibmcloud fn action create
{: #cli_action_create}

Crie uma ação.

```
ibmcloud fn action create <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>O nome da ação. Para incluir a ação em um pacote, insira o nome no formato <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em>. Este valor é obrigatório. </dd>

   <dt><em>APP_FILE</em></dt>
   <dd>O caminho para o arquivo ou o pacote de app a ser executado como uma ação. Essa opção é necessária.</dd>

   <dt>--concurrency <em>ACTIVATION_LIMIT</em>, -c <em>ACTIVATION_LIMIT</em></dt>
   <dd>O <em>LIMIT</em> máximo de ativação simultânea de intracontêiner para a ação. O valor padrão é uma ativação.</dd>

   <dt>--copy</dt>
   <dd>Trate a ação como o nome de uma ação existente.</dd>

   <dt>--docker <em>DOCKER_HUB_USERNAME</em>/<em>IMAGE_NAME</em></dt>
   <dd>O nome do usuário do Docker Hub e o nome da imagem do Docker no Docker Hub para executar a ação. Essa sinalização é necessária para criar ações por meio de imagens do Docker.</dd>

   <dt>--kind <em>LANGUAGE</em></dt>
   <dd>O tempo de execução para seu app. Este sinalizador é opcional. Se nenhum <em>VALUE</em> for especificado, a versão padrão para o tempo de execução detectado será usada.
     Possíveis <em>VALUES</em> para a opção --kind:
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

   <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
   <dd>O tamanho máximo de log em MB para a ação. O valor padrão é 10 MB.</dd>

   <dt>--main <em>ENTRY_METHOD_NAME</em></dt>
   <dd>Se o método de entrada da ação não for `main`, especifique o nome customizado. Essa sinalização é necessária quando o método de entrada não é `main`. Para alguns tempos de execução, como Java, o nome deve ser o método completo.</dd>

   <dt>--native</dt>
   <dd>É possível usar o argumento `--native` como abreviação para `--docker openwhisk/dockerskeleton`. Esse argumento permite criar e implementar um executável que é executado dentro do SDK de ação do Docker padrão.
       <ol><li>Quando você cria uma imagem do Docker, um executável binário é criado dentro do contêiner localizado em `/action/exec`. Copie o arquivo `/action/exec` para seu sistema de arquivos local e compacte-o com zip em `exec.zip`.</li>
       <li>Crie uma ação do Docker que receba o executável como dados de inicialização. O argumento `--native` substitui o argumento `--docker openwhisk/dockerskeleton`.</li></ol>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

   <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
   <dd>Crie uma sequência de ações e inclua os nomes de ações relacionadas separadas por vírgulas.</dd>

   <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
   <dd>O <em>LIMIT</em> de tempo limite em milissegundos. O valor padrão é 60000 milissegundos. Quando o tempo limite for atingido, a ação será finalizada.</dd>

   <dt>--web yes|true|raw|no|false</dt>
   <dd>Trate a ação como uma ação da web, uma ação da web HTTP bruta ou como uma ação padrão. Especifique <code>yes</code> ou <code>true</code> para uma ação da web, <code>raw</code> para uma ação da web HTTP bruta ou <code>no</code> ou <code>false</code> para uma ação padrão. Para proteger sua ação da web, inclua empacotá-la a opção `--web-secure`.</dd>

   <dt>--web-secure <em>SECRET</em></dt>
   <dd>Proteja a ação da web. O <em>VALUE</em> para <em>SECRET</em> pode ser <em>true</em>, <em>false</em> ou qualquer sequência. Essa opção pode ser usada apenas com a opção `--web`.</dd>
   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn action create hello folder/hello_world.js
  ```
  {: pre}

  Saída:
  ```
  ok: created hello
  ```
  {: screen}


<br />

### ibmcloud fn action delete
{: #cli_action_delete}

É possível limpar seu namespace excluindo ações que você não deseja usar mais.

```
ibmcloud fn action delete <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn action delete helloworld
  ```
  {: pre}

  Saída:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn action get
{: #cli_action_get}

Obtenha metadados que descrevam uma ação específica.

```
ibmcloud fn action get ACTION_NAME [--save] [--save-as <em>FILENAME</em>] [--summary] [--url]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>O nome de uma ação. Este valor é obrigatório.</dd>

   <dt>--save</dt>
   <dd>É possível obter e salvar localmente o código associado a uma ação existente, exceto para sequências e ações do Docker. O <em>FILENAME</em> corresponde a um nome de ação existente no diretório atualmente em funcionamento e a extensão do arquivo corresponde ao tipo de ação. Por exemplo, para o código de ação que é um arquivo zip, uma extensão de .zip é usada. Este sinalizador é opcional.</dd>

  <dt>--save-as <em>FILENAME</em></dt>
  <dd>Salve o código para ações em um arquivo nomeado customizado, fornecendo um caminho de arquivo, <em>FILENAME</em> e extensão. Este sinalizador é opcional.</dd>

  <dt>--summary</dt>
  <dd>Obtenha um resumo dos detalhes da ação. Os parâmetros com o prefixo "*" são ligados; os parâmetros com o prefixo "**" são ligados e finalizados. Este sinalizador é opcional.</dd>

  <dt>--url</dt>
  <dd>Obtenha a URL somente para a ação. Este sinalizador é opcional.</dd>
   </dl>

<br /><strong>Exemplo</strong>:

```
ibmcloud fn action get hello
```
{: pre}

Saída:
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
            "<em>KEY</em>": "exec",
            "<em>VALUE</em>": "nodejs:6"
        }
    ],
    "<em>LIMIT</em>s": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}




<br />
### ibmcloud fn action invoke
{: #cli_action_invoke}

Execute uma ação para testá-la.

```
ibmcloud fn action invoke <em>ACTION_NAME</em> [--blocking] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--result]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>O nome da ação. Este valor é obrigatório. </dd>

   <dt>--blocking, -b</dt>
   <dd>As chamadas de bloqueio usam um estilo de solicitação e resposta para aguardar que o resultado de ativação esteja disponível. O período de espera será o valor que for menor dentre 60 segundos ou o [ <em>LIMIT</em><em>VALUE</em>de tempo](/docs/openwhisk?topic=cloud-functions-limits) da ação. Este sinalizador é opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

   <dt>--result, -r</dt>
   <dd>O resultado do código do app é exibido como a saída do comando. Se essa opção não for especificada, o ID de ativação será exibido. A chamada é de bloqueio quando essa opção é especificada. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn action invoke hello --blocking
```
{: pre}


<br />
### ibmcloud fn action list
{: #cli_action_list}

Liste todas as ações que você criou ou um número específico de ações.

```
ibmcloud fn action list <em>ACTION_NAME</em> [--limit <em>NUMBER_OF_ACTIONS</em>] [--name-sort] [--skip <em>NUMBER_OF_ACTIONS</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>ACTION_NAME</em></dt>
   <dd>O nome de um pacote de ações. Esse valor é opcional. Se não especificado, todas as ações serão listadas.</dd>

   <dt>--limit <em>NUMBER_OF_ACTIONS</em>, -l <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>Liste um número especificado de ações. O padrão é 30 ações.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Classifique a lista de ações retornadas por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>--skip <em>NUMBER_OF_ACTIONS</em>, -s <em>NUMBER_OF_ACTIONS</em></dt>
   <dd>Exclua um número especificado das ações criadas mais recentemente por meio do resultado.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn action list
  ```
  {: pre}


<br />
### ibmcloud fn action update
{: #cli_action_update}

Atualize uma ação ou o app dentro de uma ação.

```
ibmcloud fn action update <em>ACTION_NAME</em> APP_<em>FILE</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--concurrency <em>ACTIVATION_LIMIT</em>] [--copy] [--docker DOCKER_HUB_USERNAME/IMAGE_NAME] [--kind LANGUAGE] [--logsize <em>LIMIT</em>] [--main ENTRY_METHOD_NAME] [--native] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em>] [--timeout <em>LIMIT</em>] [--web yes|true|raw|no|false] [--web-secure <em>SECRET</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

  <dl>
  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> VALE. Este sinalizador é opcional.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>O nome da ação. Para incluir a ação em um pacote, insira o nome no formato <em>PACKAGE_NAME</em>/<em>ACTION_NAME</em>. Este valor é obrigatório. </dd>

  <dt><em>APP_FILE</em></dt>
  <dd>O caminho para o arquivo ou o pacote de app a ser executado como uma ação. Essa opção é necessária quando você deseja atualizar seu app dentro da ação.</dd>

  <dt>--concurrency <em>ACTIVATION_LIMIT</em>, -c <em>ACTIVATION_LIMIT</em></dt>
  <dd>O limite máximo de ativação simultânea de intracontêiner para a ação. O valor padrão é uma ativação.</dd>

  <dt>--copy</dt>
  <dd>Trate a ação como o nome de uma ação existente.</dd>

  <dt>--docker DOCKER_HUB_USERNAME/IMAGE_NAME</dt>
  <dd>O nome do usuário do Docker Hub e o nome da imagem do Docker no Docker Hub para executar a ação. Essa sinalização é necessária para criar ações por meio de imagens do Docker.</dd>

  <dt>--kind LANGUAGE</dt>
  <dd>O tempo de execução para seu app. Este sinalizador é opcional. Se nenhum <em>VALUE</em> for especificado, a versão padrão para o tempo de execução detectado será usada.
    Possíveis valores para a opção --kind:
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

  <dt>--logsize <em>LIMIT</em>, -l <em>LIMIT</em></dt>
  <dd>O tamanho máximo de log em MB para a ação. O valor padrão é 10 MB.</dd>

  <dt>--main ENTRY_METHOD_NAME</dt>
  <dd>Se o método de entrada da ação não for `main`, especifique o nome customizado. Essa sinalização é necessária quando o método de entrada não é `main`. Para alguns tempos de execução, como Java, o nome deve ser o método completo.</dd>

  <dt>--native</dt>
  <dd>É possível usar o argumento `--native` como abreviação para `--docker openwhisk/dockerskeleton`. Esse argumento permite criar e implementar um executável que é executado dentro do SDK de ação do Docker padrão.
      <ol><li>Quando você cria uma imagem do Docker, um executável binário é criado dentro do contêiner localizado em `/action/exec`. Copie o arquivo `/action/exec` para seu sistema de arquivos local e compacte-o com zip em `exec.zip`.</li>
      <li>Crie uma ação do Docker que receba o executável como dados de inicialização. O argumento `--native` substitui o argumento `--docker openwhisk/dockerskeleton`.</li></ol>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

  <dt>--sequence <em>ACTION_NAME</em>, <em>ACTION_NAME</em></dt>
  <dd>Crie uma sequência de ações, especificando o nome de ações relacionadas.</dd>

  <dt>--timeout <em>LIMIT</em>, -t <em>LIMIT</em></dt>
  <dd>O limite de tempo limite em milissegundos. O valor padrão é 60000 milissegundos. Quando o tempo limite for atingido, a ação será finalizada.</dd>

  <dt>--web yes|true|raw|no|false</dt>
  <dd>Trate a ação como uma ação da web, uma ação da web HTTP bruta ou como uma ação padrão. Especifique <code>yes</code> ou <code>true</code> para uma ação da web, <code>raw</code> para uma ação da web HTTP bruta ou <code>no</code> ou <code>false</code> para uma ação padrão. Para proteger sua ação da web, inclua empacotá-la a opção `--web-secure`.</dd>

  <dt>--web-secure <em>SECRET</em></dt>
  <dd>Proteja a ação da web. O <em>VALUE</em> para <em>SECRET</em> pode ser <em>true</em>, <em>false</em> ou qualquer sequência. Essa opção pode ser usada apenas com a opção `--web`.</dd>
  </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn action update hello folder/hello_world.js
```
{: pre}




<br /><br />
## Comandos de ativação
{: #cli_activation}


### ibmcloud fn activation get
{: #cli_activation_get}

Obtenha metadados que descrevam uma ativação específica.

```
ibmcloud fn activation get [<em>ACTIVATION_ID</em>] [<em>FIELD_FILTER</em>] [--last] [--summary]
```
{: pre}


<br /><strong>Opções de comando</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>O ID para uma ativação específica. Use `ibmcloud fn activation list` para recuperar uma lista de IDs disponíveis. Esse valor é necessário, a menos que a opção `--last` ou `-l` seja especificada.</dd>

  <dt><em>FIELD_FILTER</em></dt>
  <dd>Um campo nos metadados do qual exibir informações. Por exemplo, para exibir o campo de logs, execute `ibmcloud fn activation get ACTIVATION_ID logs`. Esse valor é opcional.</dd>

  <dt>--last, -l</dt>
  <dd>Exiba os metadados para a ativação mais recente. Este sinalizador é opcional.</dd>

  <dt>--summary, -s</dt>
  <dd>Exiba a resposta do resultado somente por meio dos detalhes de ativação. Este sinalizador é opcional.</dd>
  </dl>


<br /><strong>Exemplo</strong>:
```
ibmcloud fn activation get 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation list
{: #cli_activation_list}

Liste todos os IDs de ativação para todas as ações em um pacote.

```
ibmcloud fn activation list [--full] [--limit <em>NUMBER_OF_ACTIVATIONS</em>] [--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>] [--skip <em>NUMBER_OF_ACTIVATIONS</em>] [--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em>]
```
{: pre}


<br /><strong>Opções de comando</strong>:

  <dl>
  <dt>--full, -f</dt>
  <dd>Exibir a descrição completa da ativação</dd>

  <dt>--limit <em>NUMBER_OF_ACTIVATIONS</em>, -l <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>Listar um número especificado de ativações. O padrão é 30 ativações e o máximo é 200 ativações.</dd>

  <dt>--since <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>Listar ativações que foram criadas desde a data especificada. Medido em milissegundos. Exemplo: Th, 01, Jan 1970</dd>

  <dt>--skip <em>NUMBER_OF_ACTIVATIONS</em>, -s <em>NUMBER_OF_ACTIVATIONS</em></dt>
  <dd>Excluir um número especificado de ativações mais recentes do resultado.</dd>

  <dt>--upto <em>DAY_OF_THE_WEEK</em>, <em>DAY</em>, <em>MONTH</em>, <em>YEAR</em></dt>
  <dd>Listar ativações que foram criadas antes da data especificada. Medido em milissegundos. Exemplo: Th, 01, Jan 1970</dd>
  </dl>

<br /><strong>Exemplo</strong>:
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


<br />
### ibmcloud fn activation logs
{: #cli_activation_logs}

Obtenha logs para uma ativação específica.

```
ibmcloud fn activation logs [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}

<br /><strong>Opções de comando</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>O ID para uma ativação específica. Use `ibmcloud fn activation list` para recuperar uma lista de IDs disponíveis. Esse valor é necessário, a menos que a opção `--last` ou `-l` seja especificada.</dd>

  <dt>--last, -l</dt>
  <dd>Exibir os logs para a ativação mais recente. Este sinalizador é opcional.</dd>

  <dt>--strip, -r</dt>
  <dd>Exibir apenas a mensagem de log; excluir as informações de registro de data e hora e de fluxo. Este sinalizador é opcional.</dd>
  </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn activation logs 8694a4501be6486a94a4501be6886a1e --summary
```
{: pre}


<br />
### ibmcloud fn activation poll
{: #cli_activation_poll}

Visualizar um fluxo, lista ativa de ativações para uma ação ou um namespace. É possível pressionar CTRL + C para sair da pesquisa.

```
ibmcloud fn activation poll [/<em>NAMESPACE</em>] [<em>ACTION_NAME</em>] [--exit <em>SECONDS</em>] [--since-days <em>DAYS</em>] [-since-hours <em>HOURS</em>] [--since-minutes <em>MINUTES</em>] [--since-seconds <em>SECONDS</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

  <dl>
  <dt>/<em>NAMESPACE</em></dt>
  <dd>Um namespace iniciando com /. Ativações de pesquisa para um namespace, uma ação ou um espaço. Esse valor é opcional. Se um namespace ou uma ação não for especificada, o espaço será pesquisado.</dd>

  <dt><em>ACTION_NAME</em></dt>
  <dd>Ativações de pesquisa para um namespace, uma ação ou um espaço. Esse valor é opcional. Se um namespace ou uma ação não for especificada, o espaço será pesquisado.</dd>

  <dt>--exit <em>SECONDS</em>, -e <em>SECONDS</em></dt>
  <dd>Pesquisar ativações um número especificado de segundos e, em seguida, sair. Este sinalizador é opcional.</dd>

  <dt>--since-days <em>DAYS</em></dt>
  <dd>Iniciar pesquisa para ativações um número especificado de dias atrás. Este sinalizador é opcional.</dd>

  <dt>--since-hours <em>HOURS</em></dt>
  <dd>Iniciar pesquisa para ativações um número especificado de horas atrás. Este sinalizador é opcional.</dd>

  <dt>--since-minutes <em>MINUTES</em></dt>
  <dd>Iniciar pesquisa para ativações um número especificado de minutos atrás. Este sinalizador é opcional.</dd>

  <dt>--since-seconds <em>SECONDS</em></dt>
  <dd>Iniciar pesquisa para ativações um número especificado de segundos atrás. Este sinalizador é opcional.</dd>
  </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn activation poll
```
{: pre}


<br />
### ibmcloud fn activation result
{: #cli_activation_result}

Obtenha o resultado de uma ativação específica.

```
ibmcloud fn activation result [<em>ACTIVATION_ID</em>] [--last] [--strip]
```
{: pre}


<br /><strong>Opções de comando</strong>:

  <dl>
  <dt><em>ACTIVATION_ID</em></dt>
  <dd>O ID para uma ativação específica. Use `ibmcloud fn activation list` para recuperar uma lista de IDs disponíveis. Esse valor é necessário, a menos que a opção `--last` ou `-l` seja especificada.</dd>

  <dt>--last, -l</dt>
  <dd>Exibir o resultado para a ativação mais recente. Este sinalizador é opcional.</dd>

  </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn activation result 8694a4501be6486a94a4501be6886a1e
```
{: pre}



<br /><br />

## Comandos de API
{: #cli_api}


### ibmcloud fn api create
{: #cli_api_create}

Crie uma API.

```
ibmcloud fn api create BASE_PATH API_PATH API_VERB <em>ACTION_NAME</em>] [--apiname API_NAME] [--config-file <em>FILE</em>] [--response-type TYPE]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>O caminho base para a API.</dd>

   <dt>API_NAME</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>API_VERB</dt>
   <dd>O verbo para a API, tal como `get` ou `post`.</dd>

   <dt><em>ACTION_NAME</em></dt>
   <dd>O nome da ação.</dd>

   <dt><em>--apiname API_NAME, -n API_NAME</em></dt>
   <dd>O nome da API. Essa sinalização é ignorada quando um arquivo de configuração é especificado. O nome padrão é BASE_PATH. Este sinalizador é opcional.</dd>

   <dt>--config-file <em>FILE</em>, -c <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém a configuração da API do Swagger. Quando esse sinalizador é usado, o sinalizador de nome da API é ignorado. Esse sinalizador é necessário.</dd>

   <dt>--response-type TYPE</dt>
   <dd>Configure o tipo de resposta da ação da web como `html`, `http`, `json`, `text` ou `svg`. O valor padrão é `json`. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn api create /hello /world get hello --response-type json
  ```
  {: pre}

  Saída:
  ```
  ok: created API /hello/world GET for action /_/hello
  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<GENERATED_API_ID>/hello/world

  ```
  {: screen}


<br />
### ibmcloud fn api delete
{: #cli_api_delete}

Exclua uma API.

```
ibmcloud fn api delete BASE_PATH API_NAME API_PATH API_VERB
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>O caminho base para a API.</dd>

   <dt>API_NAME</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>API_PATH</dt>
   <dd>O caminho para a API</dd>

   <dt>API_VERB</dt>
   <dd>O verbo para a API, tal como `get` ou `post`.</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>Especifique o tipo de saída da API como `json` ou `yaml`. O valor padrão é `json`.</dd>

   <dt>--full, -f</dt>
   <dd>Exiba os detalhes completos de configuração da API.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn api delete /hello /world get
  ```
  {: pre}



<br />
### ibmcloud fn api get
{: #cli_api_get}

Obtenha os metadados para uma API.

```
ibmcloud fn api get BASE_PATH API_NAME [--format OUTPUT_TYPE] [--full]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>O caminho base para a API.</dd>

   <dt>API_NAME</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>--format OUTPUT_TYPE</dt>
   <dd>Especifique o tipo de saída da API como `json` ou `yaml`. O valor padrão é `json`.</dd>

   <dt>--full, -f</dt>
   <dd>Exiba os detalhes completos de configuração da API.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn api get /hello /world
  ```
  {: pre}


<br />
### ibmcloud fn api list
{: #cli_api_list}

Liste todas as APIs que você criou ou um número específico de APIs. Se nenhum nome ou caminho base for especificado, todas as APIs serão listadas.

```
ibmcloud fn api list BASE_PATH API_NAME API_PATH API_VERB [--full] [--limit NUMBER_OF_APIS] [--name-sort] [--skip NUMBER_OF_APIS]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>BASE_PATH</dt>
   <dd>O caminho base para a API.</dd>

   <dt>API_NAME</dt>
   <dd>O nome da API. O nome da API pode ser o mesmo do caminho base.</dd>

   <dt>API_PATH</dt>
   <dd>O caminho para a API</dd>

   <dt>API_VERB</dt>
   <dd>O verbo para a API, tal como `get` ou `post`.</dd>

   <dt>--full, -f</dt>
   <dd>Exibir os detalhes completos da API. Este sinalizador é opcional. </dd>

   <dt>--limit NUMBER_OF_APIS, -l NUMBER_OF_APIS</dt>
   <dd>Listar um número especificado de APIs. O padrão é 30 APIs. Este sinalizador é opcional. </dd>

   <dt>--name-sort, -n</dt>
   <dd>Classifique a lista de APIs retornadas por nome, caso contrário, a lista será classificada por data de criação. Este sinalizador é opcional. </dd>

   <dt>--skip NUMBER_OF_APIS, -s NUMBER_OF_APIS</dt>
   <dd>Excluir um número especificado das APIs criadas mais recentemente por meio do resultado. Este sinalizador é opcional. </dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn api list
  ```
  {: pre}




<br /><br />
## Comandos de implementação
{: #cli_deploy_cmds}


### ibmcloud fn deploy
{: #cli_deploy}

Use um arquivo manifest para implementar uma coleção de pacotes, ações, acionadores e regras.

```
ibmcloud fn deploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>--apihost HOST</dt>
   <dd>O host da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>O arquivo de configuração. O padrão é <code>$HOME/.wskprops</code>.</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>O caminho para o arquivo de implementação.</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>O caminho do arquivo de manifesto. Essa sinalização será necessária se o manifest.yaml não estiver no diretório atual.</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>O nome ou o ID para um namespace.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

   <dt>--preview </dt>
   <dd>Exibir o plano de implementação antes de implementar.</dd>

   <dt>--project PATH</dt>
   <dd>O caminho para o projeto sem servidor. O padrão é <code>.</code> (diretório atual).</dd>

   <dt>--strict</dt>
   <dd>Permitir uma versão de tempo de execução definida pelo usuário.</dd>

   <dt>--verbose, -v</dt>
   <dd>Visualizar a saída detalhada.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn deploy --manifest folder/manifest.yaml
  ```
  {: pre}


<br />
### ibmcloud fn undeploy
{: #cli_undeploy}

Use um arquivo manifest para remover a implementação de uma coleção de pacotes, ações, acionadores e regras.

```
ibmcloud fn undeploy [--apihost HOST] [--auth <em>KEY</em>] [--config <em>FILE</em>] [--deployment <em>FILE</em>] [--manifest <em>FILE</em>] [--namespace <em>NAMESPACE<em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>] [--preview] [--project PATH] [--strict] [--verbose]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt>--apihost HOST</dt>
   <dd>O host da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--config <em>FILE</em></dt>
   <dd>O arquivo de configuração. O padrão é <code>$HOME/.wskprops</code>.</dd>

   <dt>--deployment <em>FILE</em></dt>
   <dd>O caminho para o arquivo de implementação.</dd>

   <dt>--manifest <em>FILE</em>, -m <em>FILE</em></dt>
   <dd>O caminho do arquivo de manifesto. Essa sinalização será necessária se o manifest.yaml não estiver no diretório atual.</dd>

   <dt>--namespace <em>NAMESPACE</em>, -n <em>NAMESPACE</em></dt>
   <dd>O nome ou o ID para um namespace.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

   <dt>--preview </dt>
   <dd>Exibir o plano de remoção de implementação antes da implementação.</dd>

   <dt>--project PATH</dt>
   <dd>O caminho para o projeto sem servidor. O padrão é <code>.</code> (diretório atual).</dd>

   <dt>--strict</dt>
   <dd>Permitir uma versão de tempo de execução definida pelo usuário.</dd>

   <dt>--verbose, -v</dt>
   <dd>Visualizar a saída detalhada.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn undeploy --manifest folder/manifest.yaml
  ```
  {: pre}



<br /><br />

## Comando List
{: #cli_list_cmd}


### ibmcloud fn list
{: #cli_list}

Visualizar uma lista agrupada de pacotes, ações, acionadores e regras no namespace.

```
ibmcloud fn list [--name-sort]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt>--name-sort, -n</dt>
   <dd>Classificar cada grupo de entidades retornadas por nome, caso contrário, cada grupo será classificado pela data de criação.</dd>
   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn list
  ```
  {: pre}




<br /><br />
## Comandos de namespace
{: #cli_namespace}


### ibmcloud fn namespace create
{: #cli_namespace_create}

Crie um namespace do IAM.

```
ibmcloud fn namespace create <em>NAMESPACE</em> [--description DESCRIPTION] 
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>O nome para um namespace. Não inclua hifens (-) no nome. Este valor é obrigatório.</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>Escreva sua própria descrição exclusiva para ajudar a identificar o namespace. Se a sua descrição tiver mais de uma palavra, inclua aspas (") em torno dela. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn namespace create HBCTeamProd --description "HBC Team Prod Environment. Consulte Beth para obter informações sobre esse namespace."
  ```
  {: pre}


<br />
### ibmcloud fn namespace delete
{: #cli_namespace_delete}

Exclua um namespace do IAM.

```
ibmcloud fn namespace delete <em>NAMESPACE</em>
```
{: pre}


<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn namespace delete mynamespace
  ```
  {: pre}



<br />
### ibmcloud fn namespace get
{: #cli_namespace_get}

Obtenha as entidades para ou as informações de metadados de um namespace do Cloud Foundry ou IAM.

```
ibmcloud fn namespace list <em>NAMESPACE</em> [--auth <em>KEY</em>] [--name-sort] [--properties] 
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>O nome ou o ID para um namespace. Este valor é obrigatório.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Classifique a lista de namespaces retornados por nome, caso contrário, a lista será classificada por data de criação. Este sinalizador é opcional. </dd>

   <dt>--properties</dt>
   <dd>Exibir as propriedades do namespace em vez das entidades contidas nele. Este sinalizador é opcional. </dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn namespace get user@domain.com_dev --properties
  ```
  {: pre}

  Saída:
  ```
  Name: user@domain.com_dev
  Description: This is a description of my namespace.
  Resource Plan Id: functions-base-plan
  Location: us-south
  ID: 58c2e718-8c60-48bc-96de-cf9373fe6709
  ```
  {: screen}



<br />
### ibmcloud fn namespace list
{: #cli_namespace_list}

Listar os namespaces disponíveis do Cloud Foundry e do IAM.

```
ibmcloud fn namespace list [--auth <em>KEY</em>] [--cf] [--iam] [--limit NUMBER_OF_<em>NAMESPACE</em>S] [--name-sort] [--skip NUMBER_OF_<em>NAMESPACE</em>S] 
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--cf</dt>
   <dd>Exibir apenas os namespaces do Cloud Foundry. Os namespaces do IAM não são exibidos. Este sinalizador é opcional.</dd>

   <dt>--iam</dt>
   <dd>Exibir apenas os namespaces do IAM. Os namespaces do Cloud Foundry não são exibidos. Este sinalizador é opcional.</dd>

   <dt>--limit NUMBER_OF_<em>NAMESPACE</em>S, -l NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>Listar um número especificado de namespaces. O padrão é 30 namespaces. Este sinalizador é opcional. </dd>

   <dt>--name-sort, -n</dt>
   <dd>Classifique a lista de namespaces retornados por nome, caso contrário, a lista será classificada por data de criação. Este sinalizador é opcional. </dd>

   <dt>--skip NUMBER_OF_<em>NAMESPACE</em>S, -s NUMBER_OF_<em>NAMESPACE</em>S</dt>
   <dd>Excluir um número especificado dos namespaces criados mais recentemente por meio do resultado. Este sinalizador é opcional. </dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn namespace list
  ```
  {: pre}


<br />
### ibmcloud fn namespace update
{: #cli_namespace_update}

Mude o nome ou a descrição de um namespace do IAM.

```
ibmcloud fn namespace update <em>NAMESPACE</em> [NEW_<em>NAMESPACE</em>_NAME] [--description DESCRIPTION] 
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt><em>NAMESPACE</em></dt>
   <dd>O nome para um namespace. Não inclua hifens (-) no nome. Este valor é obrigatório.</dd>

   <dt>NEW_<em>NAMESPACE</em>_NAME</dt>
   <dd>O novo nome para um namespace. Não inclua hifens (-) no nome. Esse valor é opcional.</dd>

   <dt>--description DESCRIPTION, -n DESCRIPTION</dt>
   <dd>Escreva sua própria descrição exclusiva para ajudar a identificar o namespace. Se a sua descrição tiver mais de uma palavra, inclua aspas (") em torno dela. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn namespace update HBCTeamProd HBCTeamStaging
  ```
  {: pre}




<br /><br />
## Comandos package
{: #cli_pkg}


### ibmcloud fn package bind
{: #cli_pkg_bind}

Ligar parâmetros a um pacote. Todas as ações dentro do pacote herdam esses parâmetros, a menos que especificado de outra forma.

```
ibmcloud fn package bind <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>O nome do pacote. Este valor é obrigatório. </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> VALE. Este sinalizador é opcional.</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>
  </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn package bind --param name Bob
  ```
  {: pre}



<br />
### ibmcloud fn package create
{: #cli_pkg_create}

Crie um pacote projetado para conter uma ou mais ações. Para incluir uma ação no pacote, inclua o nome do pacote com o nome da ação quando criar ou atualizar a ação.

```
ibmcloud fn package create <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

  <dl>
  <dt><em>PACKAGE_NAME</em></dt>
  <dd>O nome do pacote. Este valor é obrigatório. </dd>

  <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
  <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

  <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
  <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> VALE. Este sinalizador é opcional.</dd>

  <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
  <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

  <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
  <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e C. Esse sinalizador é opcional.</dd>

  <dt>--shared yes|no</dt>
  <dd>Quando especificado sem um valor ou com um valor de yes, o pacote é compartilhado com outros usuários.</dd>
  </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Saída:
  ```
  ok: hellopkg criado
  ```
  {: screen}


<br />
### ibmcloud fn package delete
{: #cli_pkg_delete}

É possível limpar seu namespace excluindo pacotes que você não deseja usar mais.

```
ibmcloud fn package delete <em>PACKAGE_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn package delete hello
  ```
  {: pre}

  Saída:
  ```
  ok: deleted hello
  ```
  {: screen}


<br />
### ibmcloud fn package get
{: #cli_pkg_get}

Obtenha metadados que descrevam um pacote específico.

```
ibmcloud fn package get <em>PACKAGE_NAME</em> [--summary]
```
{: pre}

<br /><strong>Opções de comando</strong>:

  <dl>
   <dt><em>PACKAGE_NAME</em></dt>
   <dd>O nome de um pacote. Este valor é obrigatório.</dd>

   <dt>--summary</dt>
   <dd>Obtenha um resumo dos detalhes do pacote. Os parâmetros com o prefixo "*" são ligados. Este sinalizador é opcional.</dd>
   </dl>

<br /><strong>Exemplo</strong>:

```
ibmcloud fn package get hello
```
{: pre}


<br />
### ibmcloud fn package list
{: #cli_pkg_list}

Liste todos os pacotes que você criou ou um número específico de pacotes.

```
ibmcloud fn package list [<em>NAMESPACE</em>] [--limit NUMBER_OF_PACKAGES] [--name-sort] [--skip NUMBER_OF_PACKAGES]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>NAMESPACE</em></dt>
   <dd>Liste os pacotes em um namespace específico. Esse valor é opcional. Se não especificado, todos os pacotes serão listados.</dd>

   <dt>--limit NUMBER_OF_PACKAGES, -l NUMBER_OF_PACKAGES</dt>
   <dd>Listar um número especificado de pacotes. O padrão é 30 pacotes.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Classificar a lista de pacotes retornados por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>--skip NUMBER_OF_PACKAGES, -s NUMBER_OF_PACKAGES</dt>
   <dd>Excluir um número especificado dos pacotes criados mais recentemente do resultado.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn package list
  ```
  {: pre}

  Execute `ibmcloud fn package list /whisk.system` para visualizar uma lista de pacotes pré-instalados.
  {: tip}


<br />
### Atualização do pacote ibmcloud fn
{: #cli_pkg_refresh}

Atualize as ligações do pacote para todos os pacotes dentro de um namespace específico.

```
ibmcloud fn package refresh /<em>NAMESPACE</em>
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>/<em>NAMESPACE</em></dt>
   <dd>Um namespace iniciando com /. Esse sinalizador é necessário. Execute <code>ibmcloud fn namespace list</code> para obter uma lista de namespaces para escolher.</dd>
   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn package refresh /user@domain.com_dev
  ```
  {: pre}


<br />
### ibmcloud fn package update
{: #cli_pkg_update}

Atualize um pacote projetado para conter uma ou mais ações. Para incluir uma ação no pacote, inclua o nome do pacote com o nome da ação quando criar ou atualizar a ação.

```
ibmcloud fn package update <em>PACKAGE_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt><em>PACKAGE_NAME</em></dt>
   <dd>O nome do pacote. Este valor é obrigatório. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> VALE. Este sinalizador é opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

   <dt>--shared yes|no</dt>
   <dd>Quando especificado sem um valor ou com um valor de <code>yes</code>, o pacote é compartilhado com outros usuários.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn package create hellopkg
  ```
  {: pre}

  Saída:
  ```
  ok: hellopkg criado
  ```
  {: screen}




<br /><br />
## Comandos de propriedade
{: #cli_prop}

Configure propriedades globais para seu ambiente de CLI ou visualize propriedades sobre a CLI <code>wsk</code>, que é executada como parte da CLI `ibmcloud fn`.

### ibmcloud fn property get
{: #cli_prop_get}

Visualize os detalhes de metadados para uma propriedade por meio da CLI <code>wsk</code>.

```
ibmcloud fn property get [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt>--all</dt>
   <dd>Visualize todas as propriedades para a CLI <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>---apibuild</dt>
   <dd>As informações de construção da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--apibuildno</dt>
   <dd>O número da construção da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--apihost <em>HOST</em></dt>
   <dd>O host da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>A versão da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--auth <em>KEY</em>, -u <em>KEY</em></dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>O certificado de cliente <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--cliversion</dt>
   <dd>A versão da CLI <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>A <em>KEY</em> de cliente <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>Um namespace do IAM. Essa sinalização não pode ser configurada para namespaces do Cloud Foundry. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn property get --cliversion
  ```
  {: pre}


<br />
### ibmcloud fn property set
{: #cli_prop_set}

Configure uma propriedade. É necessária pelo menos uma sinalização.

```
ibmcloud fn property set [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd>O host da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>A versão da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>O certificado de cliente <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>A <em>KEY</em> de cliente <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>Um namespace do IAM. Essa sinalização não pode ser configurada para namespaces do Cloud Foundry. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn property set --namespace myNamespace
  ```
  {: pre}

  Saída:
  ```
  ok: whisk namespace set to myNamespace
  ```
  {: screen}

<br />

### ibmcloud fn property unset
{: #cli_prop_unset}

Desconfigure uma propriedade para a CLI <code>wsk</code>. É necessária pelo menos uma sinalização.

```
ibmcloud fn property unset [--apihost HOST] [--apiversion <em>VERSION</em>] [--auth <em>KEY</em>] [--cert <em>STRING</em>] [--key <em>STRING</em>] [--namespace <em>NAMESPACE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt>--apihost <em>HOST</em></dt>
   <dd>O host da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--apiversion <em>VERSION</em></dt>
   <dd>A versão da API <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--auth <em>KEY</em>, -u</dt>
   <dd>A <em>KEY</em> de autorização <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--cert <em>STRING</em></dt>
   <dd>O certificado de cliente <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--key <em>STRING</em></dt>
   <dd>A <em>KEY</em> de cliente <code>wsk</code>. Este sinalizador é opcional.</dd>

   <dt>--namespace <em>NAMESPACE</em></dt>
   <dd>Um namespace do IAM. Essa sinalização não pode ser configurada para namespaces do Cloud Foundry. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn property unset --key my<em>KEY</em>
  ```
  {: pre}



<br /><br />
## Comandos de regra
{: #cli_rule}


### ibmcloud fn rule create
{: #cli_rule_create}

Crie uma regra para associar um acionador a uma ação. Antes de poder criar uma regra, crie um acionador e uma ação primeiro.

```
ibmcloud fn rule create <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn rule create myrule mytrigger myaction
  ```
  {: pre}

  Saída:
  ```
  ok: created myrule
  ```
  {: screen}


<br />
### ibmcloud fn rule delete
{: #cli_rule_delete}

Para limpar seu namespace, remova as regras que não são mais necessárias.

```
ibmcloud fn rule delete <em>RULE_NAME</em> [--disable]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>O nome de uma regra. Este valor é obrigatório.</dd>

  <dt>--disable</dt>
  <dd>Desative a regra antes de excluí-la.</dd>
  </dl>


<br /><strong>Exemplo</strong>:

```
ibmcloud fn rule delete myrule
```
{: pre}


<br />
### ibmcloud fn rule disable
{: #cli_rule_disable}

Mude o status de uma regra para inativa e impeça-a de executar uma ação quando um acionador for disparado.

```
ibmcloud fn rule disable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn rule disable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule enable
{: #cli_rule_enable}

Mude o status de uma regra de inativa para ativa. Quando ativa, uma ação é executada quando um acionador é disparado.

```
ibmcloud fn rule enable <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn rule enable myrule
  ```
  {: pre}

<br />

### ibmcloud fn rule get
{: #cli_rule_get}

Obtenha metadados que descrevam uma regra específica.

```
ibmcloud fn rule get <em>RULE_NAME</em> [--summary]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>O nome de uma regra. Este valor é obrigatório.</dd>

  <dt>--summary</dt>
  <dd>Obtenha um resumo dos detalhes da regra.</dd>
  </dl>

<br /><strong>Exemplo</strong>:

```
ibmcloud fn rule get myrule
```
{: pre}


<br />
### ibmcloud fn rule list
{: #cli_rule_list}

Liste todas as regras que você criou ou um número específico de regras.

```
ibmcloud fn rule list <em>RULE_NAME</em> [--limit NUMBER_OF_RULES] [--name-sort] [--skip NUMBER_OF_RULES]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>O nome de uma regra. Esse valor é opcional. Se não especificado, todas as regras serão listadas.</dd>

   <dt>--limit NUMBER_OF_RULES, -l NUMBER_OF_RULES</dt>
   <dd>Listar um número especificado de regras. O padrão é 30 regras.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Classificar a lista de regras retornadas por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>--skip NUMBER_OF_RULES, -s NUMBER_OF_RULES</dt>
   <dd>Excluir um número especificado das regras criadas mais recentemente do resultado.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn rule list
  ```
  {: pre}


<br />
### ibmcloud fn rule status
{: #cli_rule_status}

Veja se uma regra está ativa ou inativa. Execute os comandos `ibmcloud fn rule disable` ou `ibmcloud fn run enable` para mudar o status.

```
ibmcloud fn rule status <em>RULE_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn rule status myrule
  ```
  {: pre}


<br />
### ibmcloud fn rule update
{: #cli_rule_update}

Para mudar quais acionadores estão associados a quais regras, é possível atualizar uma regra.

```
ibmcloud fn rule update <em>RULE_NAME</em> <em>TRIGGER_NAME</em> <em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn rule update myrule mytrigger myaction
  ```
  {: pre}



<br /><br />

## Comando SDK
{: #cli_sdk}


### ibmcloud fn sdk install
{: #cli_sdk_install}

Instale um SDK.

```
ibmcloud fn sdk install <em>COMPONENT</em> [--limit <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>COMPONENT</em></dt>
   <dd>O componente SDK, tal como `docker`, `iOS` e `bashauto`. Este valor é obrigatório.</dd>

   <dt>--stdout, --s</dt>
   <dd>Imprime os resultados do comando bash para stdout. Este sinalizador é opcional.</dd>


   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}



<br /><br />

## Comandos de serviço
{: #cli_service}


### ibmcloud fn service bind
{: #cli_service_bind}

Ligar credenciais de serviço a uma ação ou um pacote.

```
ibmcloud fn service bind SERVICE PACKAGE_or_<em>ACTION_NAME</em> [--instance SERVICE_INSTANCE] [--keyname SERVICE_<em>KEY</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>SERVIÇO</dt>
   <dd>O nome do serviço.</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>O nome do pacote ou da ação à qual ligar as credenciais.</dd>

   <dt>--instance SERVICE_INSTANCE</dt>
   <dd>O nome da instância de serviço.</dd>

   <dt>--keyname SERVICE_<em>KEY</em></dt>
   <dd>O nome das credenciais de serviço <em>KEY</em> para ligar.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn service bind cloudant hello --instance CLOUDANT_SERVICE
  ```
  {: pre}


<br />
### ibmcloud fn service unbind
{: #cli_service_unbind}

Desvincule as credenciais de serviço de uma ação ou um pacote.

```
ibmcloud fn service unbind SERVICE PACKAGE_or_<em>ACTION_NAME</em>
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt>SERVIÇO</dt>
   <dd>O nome do serviço.</dd>

   <dt>PACKAGE_or_<em>ACTION_NAME</em></dt>
   <dd>O nome do pacote ou da ação à qual ligar as credenciais.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn service unbind cloudant hello
  ```
  {: pre}



<br /><br />

## Comandos do acionador
{: #cli_trigger}


### ibmcloud fn trigger create
{: #cli_trigger_create}

Crie um acionador.

```
ibmcloud fn trigger create <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--feed <em>ACTION_NAME</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>O nome do acionador. Este valor é obrigatório. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> VALE. Este sinalizador é opcional.</dd>

   <dt>--feed <em>ACTION_NAME</em>, -f <em>ACTION_NAME</em></dt>
   <dd>Configura o tipo de acionador como um feed. Este sinalizador é opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>


   </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn trigger create mytrigger --param name Bob
```
{: pre}


<br />
### ibmcloud fn trigger delete
{: #cli_trigger_delete}

Exclua um acionador.

```
ibmcloud fn trigger delete <em>TRIGGER_NAME</em>
```
{: pre}

<br /><strong>Exemplo</strong>:

```
ibmcloud fn trigger delete mytrigger
```
{: pre}


<br />
### ibmcloud fn trigger fire
{: #cli_trigger_fire}

Teste um acionador disparando-o, em vez de esperar que ele seja acionado automaticamente.

```
ibmcloud fn trigger fire <em>TRIGGER_NAME</em> [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>

   <dt><em>TRIGGER_NAME</em></dt>
   <dd>O nome do acionador. Este valor é obrigatório. </dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Parâmetro <em>VALUES</em> no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn trigger fire --param name Bob
  ```
  {: pre}


<br />
### ibmcloud fn trigger get
{: #cli_trigger_get}

Obtenha metadados que descrevam um acionador específico.

```
ibmcloud fn trigger get <em>TRIGGER_NAME</em> [--summary]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>O nome de um acionador. Este valor é obrigatório.</dd>

  <dt>--summary</dt>
  <dd>Obtenha um resumo dos detalhes do acionador.</dd>
  </dl>

<br /><strong>Exemplo</strong>:

```
ibmcloud fn trigger get mytrigger
```
{: pre}

<br />

### ibmcloud fn trigger list
{: #cli_trigger_list}

Listar todos os acionadores que você criou ou um número específico de acionadores.

```
ibmcloud fn trigger list <em>TRIGGER_NAME</em> [--limit <em>NUMBER_OF_TRIGGERS</em>] [--name-sort] [--skip <em>NUMBER_OF_TRIGGERS</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>RULE_NAME</em></dt>
   <dd>O nome de um acionador. Esse valor é opcional. Se não especificado, todos os acionadores serão listados.</dd>

   <dt>--limit <em>NUMBER_OF_TRIGGERS</em>, -l <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>Listar um número especificado de acionadores. O padrão é 30 acionadores.</dd>

   <dt>--name-sort, -n</dt>
   <dd>Classifique a lista de acionadores retornados por nome, caso contrário, a lista será classificada por data de criação.</dd>

   <dt>--skip <em>NUMBER_OF_TRIGGERS</em>, -s <em>NUMBER_OF_TRIGGERS</em></dt>
   <dd>Exclua um número especificado dos acionadores criados mais recentemente do resultado.</dd>

   </dl>

<br /><strong>Exemplo</strong>:

  ```
  ibmcloud fn trigger list
  ```
  {: pre}


<br />

### ibmcloud fn trigger update
{: #cli_trigger_update}

Atualize um acionador.

```
ibmcloud fn trigger update <em>TRIGGER_NAME</em> [--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>] [--annotation-file <em>FILE</em>] [--param <em>KEY</em> <em>VALUE</em>] [--param-file <em>FILE</em>]
```
{: pre}

<br /><strong>Opções de comando</strong>:

   <dl>
   <dt><em>TRIGGER_NAME</em></dt>
   <dd>O nome do acionador. Este valor é obrigatório. </dd>

   <dt>--annotation <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em>, -a <em>ANNOTATION_KEY</em> <em>ANNOTATION_VALUE</em></dt>
   <dd>As anotações são especificadas em um formato <em>KEY</em> <em>VALUE</em>. Para incluir mais de uma anotação, especifique essa opção para cada anotação. Este sinalizador é opcional.</dd>

   <dt>--annotation-file <em>FILE</em>, -A <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém anotação em um formato <em>KEY</em> VALE. Este sinalizador é opcional.</dd>

   <dt>--param <em>KEY</em> <em>VALUE</em>, -p <em>KEY</em> <em>VALUE</em></dt>
   <dd>Valores de parâmetro no formato <em>KEY</em> <em>VALUE</em>. Este sinalizador é opcional.</dd>

   <dt>--param-file <em>FILE</em>, -P <em>FILE</em></dt>
   <dd>Um arquivo JSON que contém o parâmetro <em>KEYS</em> e <em>VALUES</em>. Este sinalizador é opcional.</dd>
   </dl>

<br /><strong>Exemplo</strong>:
```
ibmcloud fn trigger update mytrigger --param name Jim
```
{: pre}



