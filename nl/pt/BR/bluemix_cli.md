---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

O {{site.data.keyword.openwhisk_short}} oferece um plug-in poderoso para a CLI do {{site.data.keyword.Bluemix_notm}} que permite o gerenciamento completo do sistema {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configurando a CLI do {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Faça download e instale a [CLI do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

## Configurando o Plug-in do {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Para instalar o plug-in do {{site.data.keyword.openwhisk_short}}, execute o comando a seguir:
```
ic plugin install cloud-functions
```
{: pre}


Para validar a instalação do plug-in do {{site.data.keyword.openwhisk_short}} com êxito, execute o comando a seguir:
```
ic plugin list cloud-functions
```
{: pre}


A saída exibe as informações da versão do {{site.data.keyword.openwhisk_short}} instalado:
```
Plugin Name          Version
Cloud-Functions      1.0.0
```

É possível fazer upgrade do plug-in do {{site.data.keyword.openwhisk_short}} executando o comando a seguir:
```
ic plugin update Cloud-Functions
```
{: pre}


Para obter mais informações sobre o comando de plug-in, use `ic plugin --help` ou verifique a documentação a seguir:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_plugin_list

## Autenticação da CLI do {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_auth}

O {{site.data.keyword.openwhisk_short}} está disponível em duas regiões do {{site.data.keyword.Bluemix_notm}}.

Ao efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}, é possível especificar o terminal de API do {{site.data.keyword.Bluemix_notm}} para a região de destino juntamente com a organização e o espaço.

Para efetuar login na região sul dos EUA:
```
ic login -a api.ng.bluemix.net
```
{: pre}


Para efetuar login na região do Reino Unido:
```
ic login -a api.eu-gb.bluemix.net
```
{: pre}


A sinalização `-a` especifica o terminal de API do {{site.data.keyword.Bluemix_notm}} a ser usado. Se o terminal de API for especificado, a opção `-a` não será necessária. É possível usar o comando `ic api` para configurar explicitamente o terminal de API do {{site.data.keyword.Bluemix_notm}}. Para exibir a configuração do terminal de API atual, use o comando `ic target`."

O comando `login` solicita a você informações como organização, espaço e senha, se não especificados. É possível especificar a organização e o espaço na linha de comandos para ignorar os prompts para eles.
```
ic login -o <MY_ORG> -s <MY_SPACE>
```
{: pre}


Também é possível usar uma chave API do {{site.data.keyword.Bluemix_notm}} para efetuar login. Este método é útil quando sua conta é configurada com um Login federado que requer que você efetue login com a sinalização `--sso`. O uso de uma chave API também é benéfico se você deseja configurar um Continue Integration (CI) e deseja configurar um pipeline não assistido.
https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key

Para criar uma nova chave API usando a CLI do {{site.data.keyword.Bluemix_notm}}, execute o comando a seguir:
```
ic iam api-key-create MyKey
```
{: pre}


Em seguida, use a o valor gerado pela chave API para efetuar login, como no exemplo a seguir:
```
ic login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
```
{: pre}


Para obter mais informações sobre o comando de login, use `ic login --help` ou revise a documentação a seguir:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login


## Usando o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_usage}

Verifique sua configuração. Execute uma chamada (síncrona) de bloqueio de eco, passando `hello` como um argumento, como no exemplo a seguir:
```
ic wsk action invoke whisk.system/utils/echo -p message hello --result
```
{: pre}


Veja a saída de exemplo a seguir:
```
{
    "message":"hello"
}
```

Depois de configurar seu ambiente, é possível usar a CLI do {{site.data.keyword.openwhisk_short}} para executar as tarefas a seguir:

* Execute seus fragmentos de código ou ações no {{site.data.keyword.openwhisk_short}}. Veja [Criando e chamando ações](./openwhisk_actions.html).
* Use acionadores e regras para permitir que suas ações respondam a eventos. Veja [Criando acionadores e regras](./openwhisk_triggers_rules.html).
* Saiba como os pacotes empacotam ações e configuram origens de eventos externos. Veja [Criar e usar pacotes](./openwhisk_packages.html).
* Explorar o catálogo de pacotes e aprimorar seus aplicativos com serviços externos, como uma [Origem de eventos do Cloudant](./openwhisk_cloudant.html). Veja [Usando serviços ativados pelo {{site.data.keyword.openwhisk_short}}](./openwhisk_catalog.html).

Para obter uma lista de comandos para o plug-in do {{site.data.keyword.openwhisk_short}}, execute `ic wsk` sem argumentos.

## Usando serviços de ações
{: #binding_services}

O {{site.data.keyword.openwhisk_short}} fornece o comando `service bind` para disponibilizar suas credenciais de serviço do {{site.data.keyword.Bluemix_notm}} para seu código no tempo de execução. O comando `service bind` pode ser alavancado para permitir que você ligue qualquer serviço do {{site.data.keyword.Bluemix_notm}} a qualquer ação definida no {{site.data.keyword.openwhisk_short}}.

Para obter etapas detalhadas sobre como usar serviços de ações, veja o tópico [Usando serviços de ações](./binding_services.html).

## Configurar a CLI do {{site.data.keyword.openwhisk_short}} para usar um proxy HTTPS
{: #cli_https_proxy}

A CLI do {{site.data.keyword.openwhisk_short}} pode ser configurada para usar um proxy HTTPS. Para configurar um proxy HTTPS, uma variável de ambiente chamada `HTTPS_PROXY` deve ser criada. A variável deve ser configurada para o endereço do proxy HTTPS e sua porta usando o formato a seguir:
`{PROXY IP}:{PROXY PORT}`.

## Trabalhando com regiões, organizações e espaços
{: #region_info}

É possível criar espaços para manipular suas implementações de pré-produção (preparação) e produção criando espaços para cada uma. A criação de espaços permite que o {{site.data.keyword.openwhisk_short}} tenha dois namespaces diferentes que estejam definidos para você.

É possível usar o `ic iam space-create` para criar mais espaços sob sua organização, como "staging" e "production". Veja o exemplo a seguir:
```
ic iam space-create "staging"
ic iam space-create "production"
```

Para obter mais informações sobre o comando `ic iam`, consulte a documentação a seguir: https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create

O {{site.data.keyword.openwhisk_short}} tem restrições sobre os nomes de namespace. Para obter mais informações sobre essas restrições, consulte a documentação a seguir: https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities

Se você já efetuou login, é possível executar o comando `ic target` na CLI do {{site.data.keyword.Bluemix_notm}} para alternar regiões, organização e espaços.

Use o comando a seguir para exibir todas as regiões do {{site.data.keyword.Bluemix_notm}}.

O {{site.data.keyword.openwhisk_short}} é suportado somente nas regiões `us-south` e `eu-gb`.
{: tip}

```
ic regions

Name       Geolocation          Customer   Deployment   Domain                   CF API Endpoint                  Type
us-south   US South             IBM        Production   ng.bluemix.net     https://api.ng.bluemix.net             public
eu-gb      United Kingdom       IBM        Production   eu-gb.bluemix.net  https://api.eu-gb.bluemix.net          public
```

Use o comando `ic target` para mudar regiões. Por exemplo, se você deseja alternar para a região do Reino Unido e o espaço `staging`:
```
ic target -r eu-gb -s staging
```
{: pre}


Se você precisar mudar espaços dentro da mesma região, como de preparação para produção, execute o comando a seguir:
```
ic target -s production
```
{: pre}


Para obter mais informações sobre o comando `target`, use `ic target --help` ou revise a documentação a seguir:
https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_target

## Migrando da CLI do OpenWhisk para o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Com a introdução do plug-in da CLI do {{site.data.keyword.openwhisk_short}}, a CLI do OpenWhisk não é mais necessária.

### Sintaxe do Comando
{: #command_syntax}

Todos os comandos `wsk`, exceto o comando `wsk bluemix login` que não é mais necessário, trabalham da mesma maneira usando o comando `ic wsk`. Todas as opções de comando e os argumentos são os mesmos.

### Autenticação e host de API
{: #api_authentication}

A CLI do OpenWhisk requer que você configure a chave API de autenticação e o host de API.
Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, não é necessário configurar explicitamente a chave API e o host de API. É necessário efetuar login com `ic login` e destinar sua região e namespace usando o comando `ic target`. Depois disso, todos os comandos subsequentes iniciam com `ic wsk`. Autenticar dessa maneira também evita acessar a página da web do {{site.data.keyword.Bluemix_notm}} para obter as informações específicas para configurar seu ambiente da CLI.

Se você precisar obter a chave API de autenticação para o {{site.data.keyword.openwhisk_short}} para ser usada em um cliente HTTP externo como cURL ou Postman, será possível recuperá-la com os comandos a seguir:

Para obter a chave API atual, execute o comando a seguir:
```
ic wsk property get --auth
```
{: pre}


Para obter o host de API atual, execute o comando a seguir:
```
ic wsk property get --apihost
```
{: pre}


A chave API é específica por região, organização e espaço destinado pelo plug-in da CLI do {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticação do API Gateway
{: #apigw_authentication}

A CLI do OpenWhisk requer atualmente que você execute o `wsk bluemix login` para poder configurar a autorização de API Gateway para gerenciamento de suas APIs usando o comando `wsk api`.

Com o plug-in da CLI do {{site.data.keyword.openwhisk}}, não há mais a necessidade de executar `wsk bluemix login`. Em vez disso, use o comando `ic login` para efetuar login no {{site.data.keyword.Bluemix_notm}} e o plug-in do {{site.data.keyword.openwhisk}} utiliza automaticamente o login atual e as informações de destino. Agora é possível gerenciar suas APIs usando o comando `ic wsk api`.

### Migrando scripts de implementação
{: #migrating_deploy_scripts}

Se você tiver scripts que usam a CLI do OpenWhisk com o binário `wsk`, todos os comandos funcionarão da mesma maneira usando o comando `ic wsk`. É possível modificar seus scripts para usar o plug-in da CLI do {{site.data.keyword.Bluemix_notm}} ou criar um alias ou wrapper para que as execuções atuais de `wsk` sejam traduzidas para `ic wsk`. Os comandos `ic login` e `ic target` na CLI do {{site.data.keyword.Bluemix_notm}} funcionam em modo não assistido. Com o modo não assistido, é possível configurar seu ambiente antes de executar comandos `ic wsk` para implementar e gerenciar suas entidades do {{site.data.keyword.openwhisk_short}}.


## Histórico de versões
{: #version_history}

Um registro histórico de versões que mostram destaques e correções de bug.

1.0.7 (02-02-2018)
* O `ic wsk api` agora aceita parâmetros de caminho como `/api/{id}`. Para obter informações, veja [API Gateway](./openwhisk_apigateway.html)
* Restaure o suporte do proxy.
* Remova `swift:3`.

1.0.6 (30-1-2018)
* Erro corrigido para o comando `ic wsk service bind` para ações dentro de um pacote.
