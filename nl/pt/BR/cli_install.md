---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-18"

keywords: functions cli, serverless, cli, install, functions plug-in

subcollection: cloud-functions

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


# Instalando a CLI e o plug-in
{: #cli_install}

O {{site.data.keyword.openwhisk}} oferece um plug-in poderoso para a CLI do {{site.data.keyword.cloud_notm}} que permite o gerenciamento completo do sistema {{site.data.keyword.openwhisk_short}}. É possível usar o plug-in da CLI do {{site.data.keyword.openwhisk_short}} para gerenciar os seus fragmentos de código em ações, criar acionadores e regras para ativar as suas ações para responder a eventos e empacotar ações em pacotes.
{:shortdesc}


## Configurando a CLI do {{site.data.keyword.cloud_notm}}
{: #cli_setup}

**Antes de iniciar**

Deve-se criar uma [conta do {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.

Faça download e instale a CLI do {{site.data.keyword.cloud_notm}} e efetue login.
{: shortdesc}

1. Faça download e instale o [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

2. Efetue login na CLI do {{site.data.keyword.cloud_notm}}. Para especificar uma região do {{site.data.keyword.cloud_notm}}, [inclua o terminal de API](/docs/openwhisk?topic=cloud-functions-cloudfunctions_regions).

  ```
  ibmcloud login
  ```
  {: pre}

3. Siga os prompts para selecionar sua conta do {{site.data.keyword.cloud_notm}}.

4. Obtenha uma lista de grupos de recursos. 

```
ibmcloud resource groups
```
{: pre}

**Saída de exemplo**

```
Recuperando todos os grupos de recursos na conta <account_name> como email@ibm.com...
OK
Nome      ID                                 Grupo padrão   Estado
padrão   a8a12accd63b437bbd6d58fb8b462ca7   true            ACTIVE
teste      a8a12accd63b437bbd6d58fb8b462ca7   false           ACTIVE
```
{: screen}

5. Opcional: tenha como destino um grupo de recursos diferente do padrão executando o comando a seguir.
```
ibmcloud target -g <resource_group>
```
{: pre}


**Saída de exemplo**

```
Targeted resource group <resource_group>
```
{: screen}

## Configurando o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cli_plugin_setup}

Para trabalhar com o {{site.data.keyword.openwhisk_short}}, faça download e instale o plug-in da CLI.
{: shortdesc}

É possível usar o plug-in da CLI do {{site.data.keyword.openwhisk_short}} para executar as tarefas a seguir.

* Executar os fragmentos de código ou ações no {{site.data.keyword.openwhisk_short}}. Consulte [Criando e chamando ações](/docs/openwhisk?topic=cloud-functions-actions).
* Crie acionadores e regras para ativar suas ações para responder a eventos. Consulte [Criando acionadores e regras](/docs/openwhisk?topic=cloud-functions-triggers).
* Ações do pacote configurável e configurar origens de eventos externos. Veja [Criar e usar pacotes](/docs/openwhisk?topic=cloud-functions-pkg_ov).
* Explore o catálogo de pacotes e aprimore seus aplicativos com serviços externos. Consulte [Incluindo o {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-services).

Conclua as etapas a seguir para instalar o plug-in da CLI do {{site.data.keyword.openwhisk_short}}

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

  **Saída**
  ```
  Nome de plug-in          Versão
  cloud-functions/wsk/functions/fn      1.0.32
  ```
  {: screen}

3. Depois de efetuar login, todos os comandos do {{site.data.keyword.openwhisk_short}} iniciam com `ibmcloud fn`. Para ver tudo o que você pode fazer com o plug-in do {{site.data.keyword.openwhisk_short}}, execute `ibmcloud fn` sem argumentos.
  ```
  ibmcloud fn
  ```
  {: pre}




## Direcionando os namespaces do {{site.data.keyword.openwhisk_short}}
{: #cli_regions}
Por padrão, o {{site.data.keyword.openwhisk_short}} usa [namespaces ativados pelo IAM](/docs/iam?topic=iam-iamoverview){: external}. Não é mais possível criar namespaces baseados no Cloud Foundry.
{: important}

### Criar ou ter como destino um namespace.
Para obter uma lista de seus namespaces do {{site.data.keyword.openwhisk_short}}, execute `ibmcloud fn namespace list`.

#### Crie um namespace ativado pelo IAM.
  ```
  ibmcloud fn namespace create <namespace_name> [--description <"description">]
  ```
  {: pre}

**Resposta**
  ```
  ok: namespace <namespace_name> criado
  ```
  {: screen}


#### Tenha como destino um namespace ativado pelo IAM. 
  ```
  ibmcloud fn property set --namespace <namespace_name>
  ``` 
  {: pre}


**Resposta**
  ```
  ok: namespace whisk configurado para <namespace_name>
  ```
  {: screen}
  
#### Tenha como destino um namespace baseado no Cloud Foundry. 
  
É possível usar os sinalizadores `-o` e `-s` para ter como destino um `org` e um `space` específicos ou é possível seguir os prompts.

* Tenha como destino um namespace do Cloud Foundry incluindo os nomes `org` e `space` no comando `target`.

```
ibmcloud target --cf  -o <org> -s <space>
```
{: pre}

* Tenha o Cloud Foundry como destino e siga os prompts para selecionar uma `org` e um `space`.

```
ibmcloud target --cf
```
{: pre}


**Resposta**
  ```
  Targeted Cloud Foundry (https://api.ng.bluemix.net)

  Targeted org <org_name>

  Targeted space <space_name>

  API endpoint:      https://cloud.ibm.com
  Region:            us-south
  User:              <email>
  Account:           (<account_id>) <-> <account>
  Resource group:    default
  CF API endpoint:   https://api.ng.bluemix.net (API version: 2.128.0)
  Org:               <org_name>
  Space:             <space_name>
  ```
  {: screen} 





#### Opcional: criando namespaces para implementações de temporariedade e produção.

É possível criar namespaces ativados pelo IAM para manipular suas implementações de pré-produção (temporariedade) e produção do {{site.data.keyword.openwhisk_short}} criando namespaces para cada uma. Execute [`ibmcloud fn namespace create`](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_namespace_create) para criar mais namespaces sob sua organização, como "staging" e "production":

Crie um namespace temporário.
```
ibmcloud fn namespace create staging
```
{: pre}

Crie um namespace de produção.
```
ibmcloud fn namespace create production
```
{: pre}

O {{site.data.keyword.openwhisk_short}} tem restrições sobre nomes de namespace. Para obter mais informações, consulte a documentação [Detalhes e limites do sistema](/docs/openwhisk?topic=cloud-functions-limits#limits_entities).
{: tip}


## Configurando a CLI do {{site.data.keyword.openwhisk_short}} para usar um proxy HTTPS
{: #cli_proxy}

A CLI do {{site.data.keyword.openwhisk_short}} pode ser configurada para usar um proxy HTTPS. Para configurar um proxy HTTPS, uma variável de ambiente chamada `HTTPS_PROXY` deve ser criada. A variável deve ser configurada para o endereço do proxy HTTPS e sua porta usando o formato a seguir: `{PROXY IP}:{PROXY PORT}`.

A mudança do nome da `org` ou do `space` cria um novo namespace com base no nome mudado. As entidades no namespace antigo não estão visíveis no novo namespace e podem ser excluídas.
{: important}


## Migrando da CLI do OpenWhisk para o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cli_migrate}

Agora é possível usar o plug-in da CLI do {{site.data.keyword.openwhisk_short}} para interagir com as entidades do {{site.data.keyword.openwhisk_short}}. Embora seja possível continuar a usar a CLI do OpenWhisk independente, ela não tem os recursos mais recentes que são suportados pelo {{site.data.keyword.openwhisk_short}}, como namespaces baseados no IAM e `service bind`.
{: shortdesc}


### Sintaxe do Comando
{: #cli_syntax}

Todas as opções de comando e argumentos para comandos no plug-in da CLI do Cloud Functions são os mesmos que as opções para a [CLI independente do OpenWhisk](https://github.com/apache/incubator-openwhisk-cli){: external}. Mas, observe as diferenças a seguir.

* O plug-in do {{site.data.keyword.openwhisk}} utiliza automaticamente suas informações de login e de destino atuais.
* Os comandos `wsk` agora são executados como `ibmcloud fn`.
* O comando `wsk ibmcloud login` não é mais necessário. É possível se conectar usando o `ibmcloud login`.
* É possível gerenciar suas APIs usando o `ibmcloud fn api`.

Para obter mais informações, consulte a [Referência da CLI do {{site.data.keyword.openwhisk_short}}](/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli).

### Autenticação e host de API
{: #cli_api_auth}

Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, não é necessário configurar explicitamente a chave API e o host de API. Em vez disso, é possível efetuar login com `ibmcloud login`. É possível ter como destino um namespace ativado pelo IAM executando `ibmcloud fn property set --namespace <namespace_name>` ou um namespace baseado no Cloud Foundry executando `ibmcloud target --cf`. Após efetuar login, todos os comandos iniciam com `ibmcloud fn`.


Se for necessário usar a chave de API de autenticação para o {{site.data.keyword.openwhisk_short}} em um cliente HTTP externo, como cURL ou Postman, será possível recuperá-lo com os comandos a seguir.

Obtenha a chave de API atual executando o comando a seguir.
```
ibmcloud fn property get -- auth
```
{: pre}

Obtenha o host da API atual executando o comando a seguir.
```
ibmcloud fn property get -- apihost
```
{: pre}

A chave API é específica por região, organização e espaço destinado pelo plug-in da CLI do {{site.data.keyword.openwhisk_short}}.
{: tip}


### Autenticação do API Gateway
{: #cli_apigw_authentication}

A CLI do OpenWhisk exigiu que você executasse o `wsk ibmcloud login` para poder configurar a autorização do Gateway de API para gerenciamento de suas APIs usando o comando `wsk api`. Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, você não precisa executar o `wsk ibmcloud login`. Em vez disso, ao usar o comando `ibmcloud login` para efetuar login no {{site.data.keyword.cloud_notm}}, o plug-in do {{site.data.keyword.openwhisk}} utiliza automaticamente o login atual e as informações de destino. Agora é possível gerenciar suas APIs usando o comando `ibmcloud fn api`.


### Migrando scripts de implementação
{: #cli_migrating_deploy_scripts}

Se você tiver scripts que usem a CLI do OpenWhisk com os comandos `wsk`, todos os comandos funcionarão da mesma maneira usando o comando `ibmcloud fn`. É possível modificar seus scripts para usar o plug-in da CLI do {{site.data.keyword.cloud_notm}} ou criar um alias ou um wrapper para que os comandos atuais que estão usando `wsk` sejam convertidos em `ibmcloud fn`. Os comandos `ibmcloud login` e `ibmcloud target` na CLI do {{site.data.keyword.cloud_notm}} funcionam no modo não assistido. Com o modo não assistido, é possível configurar seu ambiente antes de executar os comandos `ibmcloud fn` para implementar e gerenciar suas entidades do {{site.data.keyword.openwhisk_short}}.

## Histórico da versão da CLI
{: #cli_versions}

Um registro histórico de versões que mostram destaques e correções de bug.

v1.0.30 (03 de abril de 2019)
* Melhorou a manipulação de `service bind` de serviços IAM e de organização baseados em espaço.
* Incluída uma correção para manipular o terminal da API https://cloud.ibm.com.

v1.0.29 (06 de fevereiro de 2019)
* Incluídos os comandos `deploy` e `undeploy` para implementar ou remover a implementação de uma coleção de entidades do Functions por meio de um arquivo manifest. Para obter mais informações, consulte a documentação [Implementação](/docs/openwhisk?topic=cloud-functions-deploy#deploy).

v1.0.28 (21 de janeiro de 2019)
* Incluída uma mensagem de erro quando `update|delete|get namespace name` existir várias vezes.

v1.0.27 (11 de dezembro de 2018)
* Incluídas correções de `namespace get`.
* Incluída uma correção para `--save-as` quando uma ação é uma ação de caixa preta.

v1.0.26 (30 de novembro de 2018)
* Ativado `fn property get --auth` para retornar corretamente a chave de autenticação em um novo ambiente.

v1.0.25 (23 de novembro de 2018)
* Exibição do resultado da mensagem de erro melhorada.
* Incluída uma correção `fn namespace get`para exibir corretamente as propriedades do namespace.

1.0.23 (15 de outubro de 2018)
* Incluído o suporte para reconhecimento de código de ação ruby (`.rb`).

1.0.22 (20 de agosto de 2018)
* Incluído o suporte de região leste.

1.0.21 (01 de agosto de 2018)
* Os aliases `fn` e `functions` agora podem ser usados para os comandos do {{site.data.keyword.openwhisk_short}}: `ibmcloud fn <command>`  e  ` ibmcloud fn <command>`. Também é possível usar o `ibmcloud wsk <command>`.

1.0.19 (02 de julho de 2018)
* Correções de bugs secundários e melhorias.

1.0.18 (20 de junho de 2018)
* Incluída uma correção para desvincular instâncias de serviço fornecidas pelo usuário.
* Melhorias de desempenho.

1.0.17 (12 de junho de 2018)
* Incluído o suporte para ligação (`ibmcloud wsk service bind`) e desvinculação (`ibmcloud wsk service unbind`) de instâncias de serviço fornecidas pelo usuário que são criadas usando o comando `ibmcloud cf create-user-provided-service`.

1.0.16 (24 de maio de 2018)
* Correções de bugs secundários e melhorias.

1.0.15 (21 de maio de 2018)
* Correções de bugs secundários e melhorias.

1.0.14 (17 de maio de 2018)
* Suporte ativado para o caractere `&` em nomes de organização e espaço.

1.0.13 (07 de maio de 2018)
* Correções menores de bug e melhorias de manipulação de erros.

1.0.12 (30 de abril de 2018)
* Atualizações do {{site.data.keyword.cloud_notm}} SDK para manter a compatibilidade da CLI `bx`.

1.0.11 (23 de abril de 2018)
* Correções de bugs secundários e melhorias.

1.0.10 (09 de abril de 2018)
* Incluída nova opção `--web-secure` nos comandos `ibmcloud wsk action create|update` para proteger os terminais de ação da web.
* Corrigido o [defeito](https://github.com/apache/incubator-openwhisk-cli/issues/237){: external} do parâmetro de caminho back-to-back.

1.0.9 (16 de março de 2018)
* Suporte ativado para a ligação de serviço no nível de pacote.

1.0.8 (22 de fevereiro de 2018)
* Suporte ativado para a ligação de serviço do IAM.

1.0.7 (02 de fevereiro de 2018)
* Atualizado `ibmcloud wsk api` para aceitar parâmetros de caminho, como `/api/{id}`. Para obter mais informações, consulte [Gateway de API](/docs/openwhisk?topic=cloud-functions-apigateway).
* Suporte de proxy restaurado.
* Removido `swift:3`.

1.0.6 (30 de janeiro de 2018)
* Corrigido um erro com o comando `ibmcloud wsk service bind` para ações dentro de um pacote.



