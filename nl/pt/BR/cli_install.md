---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: functions cli, serverless, bluemix cli, install, functions plug-in

subcollection: cloud-functions

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

# Instalando a CLI e o plug-in
{: #cli_install}

O {{site.data.keyword.openwhisk}} oferece um plug-in poderoso para a CLI do {{site.data.keyword.Bluemix_notm}} que permite o gerenciamento completo do sistema {{site.data.keyword.openwhisk_short}}. É possível usar o plug-in da CLI do {{site.data.keyword.openwhisk_short}} para gerenciar os seus fragmentos de código em ações, criar acionadores e regras para ativar as suas ações para responder a eventos e empacotar ações em pacotes.
{:shortdesc}


## Configurando a CLI do {{site.data.keyword.Bluemix_notm}}
{: #cli_setup}

Faça download e instale a CLI do {{site.data.keyword.Bluemix_notm}} e efetue login.
{: shortdesc}

1. Faça download e instale o [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Para especificar uma região do IBM Cloud, [inclua o terminal de API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

  É possível especificar a organização e o espaço ao efetuar login para ignorar os prompts para eles usando os sinalizadores a seguir: `ibmcloud login -o <ORG> -s <SPACE>`
  {: tip}

3. Se você não especificou uma organização e um espaço, conclua os prompts que seguem o comando de login.


## Configurando o Plug-in do {{site.data.keyword.openwhisk_short}}
{: #cli_plugin_setup}

Para trabalhar com o {{site.data.keyword.openwhisk_short}}, faça download e instale o plug-in da CLI.
{: shortdesc}

É possível usar o plug-in para:

* Executar os fragmentos de código ou ações no {{site.data.keyword.openwhisk_short}}. Consulte [Criando e chamando ações](/docs/openwhisk?topic=cloud-functions-actions).
* Usar acionadores e regras para ativar suas ações para responder a eventos. Consulte [Criando acionadores e regras](/docs/openwhisk?topic=cloud-functions-triggers).
* Aprender como pacotes empacotam ações e configuram origens de eventos externos. Veja [Criar e usar pacotes](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Explore o catálogo de pacotes e aprimore seus aplicativos com serviços externos, como uma [origem de eventos do {{site.data.keyword.cloudant}}](/docs/openwhisk?topic=cloud-functions-pkg_cloudant).

Para ver tudo o que você pode fazer com o plug-in do {{site.data.keyword.openwhisk_short}}, execute `ibmcloud fn` sem argumentos.
{: tip}

1. Instale o plug-in do {{site.data.keyword.openwhisk_short}}.

  ```
  ibmcloud plugin install cloud-functions
  ```
  {: pre}

2. Verifique se o plug-in está instalado.

  ```
  ibmcloud plugin list
  ```
  {: pre}

  Saída:
  ```
  Plugin Name          Version
  cloud-functions/wsk/functions/fn      1.0.30
  ```
  {: screen}

Já possui o plug-in, mas precisa ser atualizado? Execute `ibmcloud plugin update cloud-functions`.
{:tip}



## Configurando a CLI do {{site.data.keyword.openwhisk_short}} para usar um proxy HTTPS
{: #cli_proxy}

A CLI do {{site.data.keyword.openwhisk_short}} pode ser configurada para usar um proxy HTTPS. Para configurar um proxy HTTPS, uma variável de ambiente chamada `HTTPS_PROXY` deve ser criada. A variável deve ser configurada para o endereço do proxy HTTPS e sua porta usando o formato a seguir: `{PROXY IP}:{PROXY PORT}`.



## Alternando para diferentes regiões, organizações e espaços <
{: #cli_regions}

Se você já está com login efetuado, é possível executar o comando `ibmcloud target` na CLI do {{site.data.keyword.Bluemix_notm}} para alternar regiões, organização e espaços.

Para criar e gerenciar entidades, você deve destinar um espaço de nomes. O namespace padrão, que pode ser denotado por um sublinhado (`_`) em algumas situações, corresponde ao namespace baseado em Cloud Foundry que é destinado atualmente.

É possível criar espaços baseados no IAM para manipular suas implementações de pré-produção (preparação) e produção criando espaços para cada uma. A criação de espaços permite que o {{site.data.keyword.openwhisk_short}} tenha dois namespaces diferentes que estejam definidos para você. Execute [`ibmcloud iam space-create`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_account#ibmcloud_account_space_create) para criar mais espaços sob sua organização, como "staging" e "production":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

O {{site.data.keyword.openwhisk_short}} tem restrições sobre nomes de namespace. Para obter mais informações, consulte a documentação [Detalhes e limites do sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}

**Aviso**: mudar o nome da organização ou espaço cria um novo namespace com base no nome mudado. As entidades no namespace antigo não estão visíveis no novo namespace e podem ser excluídas.


## Migrando da CLI do OpenWhisk para o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cli_migrate}

Agora é possível usar o plug-in da CLI do {{site.data.keyword.openwhisk_short}} para interagir com as entidades do {{site.data.keyword.openwhisk_short}}. Embora seja possível continuar a usar a CLI do OpenWhisk independente, ela não terá os recursos mais recentes que são suportados pelo {{site.data.keyword.openwhisk_short}}, como namespaces baseados em IAM e `service bind`.
{: shortdesc}

### Sintaxe do Comando
{: #cli_syntax}

Todos os comandos `wsk`, exceto o comando `wsk bluemix login`, que não é mais necessário, funcionam da mesma maneira usando o comando `ibmcloud fn`. Todas as opções de comando e argumentos para comandos no plug-in da CLI do Cloud Functions são os mesmos que os comandos para a CLI independente do OpenWhisk. Para obter mais informações, consulte o [projeto de CLI do Apache OpenWhisk ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/apache/incubator-openwhisk-cli).

### Autenticação e host de API
{: #cli_api_auth}

Na CLI do OpenWhisk, era necessário configurar a chave API de autenticação e o host de API. Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, não é necessário configurar explicitamente a chave API e o host de API. Em vez disso, é possível efetuar login com `ibmcloud login`. É possível ter como destino um namespace ativado para o IAM executando `ibmcloud fn property set --namespace NAME` ou um namespace baseado no Cloud Foundry executando `ibmcloud target -o ORG -s SPACE`. Após efetuar login, todos os comandos iniciam com `ibmcloud fn`.


Se você precisar usar a chave API de autenticação para o {{site.data.keyword.openwhisk_short}} em um cliente HTTP externo como cURL ou Postman, será possível recuperá-la com os comandos a seguir:

Para obter a chave API atual:
```
ibmcloud fn property get -- auth
```
{: pre}

Para obter o host de API atual:
```
ibmcloud fn property get -- apihost
```
{: pre}

A chave API é específica por região, organização e espaço destinado pelo plug-in da CLI do {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticação do API Gateway
{: #cli_apigw_authentication}

Na CLI do OpenWhisk, era necessário executar o `wsk bluemix login` para poder configurar a autorização do API Gateway para gerenciamento de suas APIs usando o comando `wsk api`. Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, você não precisa executar `wsk bluemix login`. Em vez disso, ao usar o comando `ibmcloud login` para efetuar login no {{site.data.keyword.Bluemix_notm}}, o plug-in do {{site.data.keyword.openwhisk}} utiliza automaticamente o login atual e as informações de destino. Agora é possível gerenciar suas APIs usando o comando `ibmcloud fn api`.

### Migrando scripts de implementação
{: #cli_migrating_deploy_scripts}

Se você tiver scripts que usam a CLI do OpenWhisk com o binário `wsk`, todos os comandos funcionarão da mesma maneira usando o comando `ibmcloud fn`. É possível modificar seus scripts para usar o plug-in da CLI do {{site.data.keyword.Bluemix_notm}} ou criar um alias ou um wrapper para que os comandos atuais que estão usando `wsk` sejam convertidos em `ibmcloud fn`. Os comandos `ibmcloud login` e `ibmcloud target` na CLI do {{site.data.keyword.Bluemix_notm}} funcionam no modo não assistido. Com o modo não assistido, é possível configurar seu ambiente antes de executar os comandos `ibmcloud fn` para implementar e gerenciar suas entidades do {{site.data.keyword.openwhisk_short}}.




## Histórico da versão da CLI
{: #cli_versions}

Um registro histórico de versões que mostram destaques e correções de bug.

v1.0.30 (03/04/2019)
* Melhorou a manipulação de `service bind` de serviços IAM e de organização baseados em espaço.
* Incluída uma correção para manipular o terminal da API https://cloud.ibm.com.

v1.0.29 (06/02/2019)
* Incluídos os comandos `deploy` e `undeploy` para implementar ou remover a implementação de uma coleção de entidades do Functions por meio de um arquivo manifest. Para obter mais informações, consulte a documentação [Implementação](/docs/openwhisk?topic=cloud-functions-deploy#deploy).

v1.0.28 (21/01/2019)
* Incluída uma mensagem de erro quando `update|delete|get namespace name` existir várias vezes.

v1.0.27 (11/12/2018)
* Incluídas correções de `namespace get`.
* Incluída uma correção para `--save-as` quando uma ação é uma ação de caixa preta.
* Incluído sinalizador `--concurrency` para comandos de criação de ação e atualização de ação.

v1.0.26 (30/11/2018)
* Ativado `fn property get --auth` para retornar corretamente a chave de autenticação em um novo ambiente.

v1.0.25 (23/11/2018)
* Exibição do resultado da mensagem de erro melhorada.
* Incluída uma correção `fn namespace get`para exibir corretamente as propriedades do namespace.

1.0.23 (15/10/2018)
* Incluído suporte para reconhecimento de código de ação ruby (.rb).

1.0.22 (20/08/2018)
* Incluído o suporte de região leste.

1.0.21 (2018-08-01)
* Os aliases `fn` e `functions` agora podem ser usados para os comandos do {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>`  e  ` ibmcloud fn <command>`. Também é possível usar o `ibmcloud wsk <command>`.

1.0.19 (2018-07-02)
* Correções de bugs secundários e melhorias.

1.0.18 (20-06-2018)
* Incluída uma correção para desvincular instâncias de serviço fornecidas pelo usuário.
* Melhorias de desempenho.

1.0.17 (12-06-2018)
* Incluído o suporte para ligação (`ibmcloud wsk service bind`) e desvinculação (`ibmcloud wsk service unbind`) de instâncias de serviço fornecidas pelo usuário que são criadas usando o comando `ibmcloud cf create-user-provided-service`.

1.0.16 (24-05-2018)
* Correções de bugs secundários e melhorias.

1.0.15 (21-05-2018)
* Correções de bugs secundários e melhorias.

1.0.14 (17-05-2018)
* Suporte ativado para o caractere `&` em nomes de organização e espaço.

1.0.13 (07-05-2018)
* Correções menores de bug e melhorias de manipulação de erros.

1.0.12 (30-04-2018)
* Atualizações do {{site.data.keyword.Bluemix_notm}} SDK para manter a compatibilidade da CLI `bx`.

1.0.11 (23-04-2018)
* Correções de bugs secundários e melhorias.

1.0.10 (09-04-2018)
* Incluída nova opção `--web-secure` nos comandos `ibmcloud wsk action create|update` para proteger os terminais de ação da web.
* Corrigido o [defeito](https://github.com/apache/incubator-openwhisk-cli/issues/237) do parâmetro de caminho back-to-back.

1.0.9 (16-03-2018)
* Suporte ativado para a ligação de serviço no nível de pacote.

1.0.8 (22-02-2018)
* Suporte ativado para a ligação de serviço do IAM.

1.0.7 (02-02-2018)
* Atualizado `ibmcloud wsk api` para aceitar parâmetros de caminho, como `/api/{id}`. Para obter informações, veja [API Gateway](/docs/openwhisk?topic=cloud-functions-apigateway).
* Suporte de proxy restaurado.
* Removido `swift:3`.

1.0.6 (30-1-2018)
* Corrigido um erro com o comando `ibmcloud wsk service bind` para ações dentro de um pacote.

