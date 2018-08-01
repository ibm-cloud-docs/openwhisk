---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Configurando o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_cli}

O {{site.data.keyword.openwhisk}} oferece um plug-in poderoso para a CLI do {{site.data.keyword.Bluemix_notm}} que permite o gerenciamento completo do sistema {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

## Configurando a CLI do {{site.data.keyword.Bluemix_notm}}
{: #bluemix_cli_setup}

Faça download e instale a CLI do {{site.data.keyword.Bluemix_notm}} e efetue login.
{: shortdesc}

1. Faça download e instale a [CLI do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

2. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. O {{site.data.keyword.openwhisk_short}} está disponível nas regiões Sul dos EUA e Reino Unido do {{site.data.keyword.Bluemix_notm}}. Se o terminal de API do {{site.data.keyword.Bluemix_notm}} não está especificado, especifique um com a sinalização `-a`.

    * Para efetuar login na região sul dos EUA:
      ```
      ibmcloud login -a api.ng.bluemix.net
      ```
      {: pre}

    * Para efetuar login na região do Reino Unido:
      ```
      ibmcloud login -a api.eu-gb.bluemix.net
      ```
      {: pre}

  É possível usar o comando `ibmcloud api` para configurar explicitamente o terminal de API do {{site.data.keyword.Bluemix_notm}}. Para exibir a configuração do terminal de API atual, use o comando `ibmcloud target`.
  {: tip}

3. O comando `ibmcloud login` solicita informações como organização, espaço e senha, se não especificadas.

  É possível especificar a organização e o espaço ao efetuar login para ignorar os prompts para eles. Use as sinalizações a seguir: `ibmcloud login -o <ORG> -s <SPACE>`.
  {: tip}

Também é possível usar uma chave API do {{site.data.keyword.Bluemix_notm}} para efetuar login. Esse método é útil quando sua conta é configurada com um login federado que requer que você efetue login com a sinalização `--sso`. [Usar uma chave API](https://console-regional.ng.bluemix.net/docs/cli/login_federated_id.html#using-an-api-key) também é benéfico se você deseja configurar a integração contínua (CI) e deseja configurar um pipeline não assistido.

1. Crie uma nova chave API.
    ```
    ibmcloud iam api-key-create MyKey
    ```
    {: pre}

2. Use o valor gerado da chave API para efetuar login.
    ```
    ibmcloud login -a api.ng.bluemix.net -o <MY_ORG> -s <MY_SPACE> --apikey <MY_KEY>
    ```
    {: pre}
</br>
Para obter mais informações sobre o comando `ibmcloud login`, use `ibmcloud login --help` ou revise o tópico [Comandos do IBM Cloud (bx)](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_login).

## Configurando o Plug-in do {{site.data.keyword.openwhisk_short}}
{: #cloudfunctions_plugin_setup}

Faça download e instale o plug-in do {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

1. Instale o plug-in do {{site.data.keyword.openwhisk_short}}.
    ```
    ibmcloud plugin install cloud-functions
    ```
    {: pre}

2. Verifique se o plug-in está instalado.
    ```
    ibmcloud plugin list cloud-functions
    ```
    {: pre}

    Saída:
    ```
    Plugin Name          Version
    Cloud-Functions      1.0.16
    ```
    {: screen}

3. Execute uma chamada (síncrona) de bloqueio de `echo`. Passe `hello` como um argumento.
    ```
    ibmcloud fn action invoke whisk.system/utils/echo -p message hello --result
    ```
    {: pre}

4. Verifique se a mensagem `hello` é retornada na saída.
    Exemplo de Saída:
    ```
    {
        "message":"hello"
    }
    ```
    {: screen}

5. É possível fazer upgrade do plug-in do {{site.data.keyword.openwhisk_short}} executando o comando a seguir:
    ```
    ibmcloud plugin update cloud-functions
    ```
    {: pre}

É possível usar a CLI do {{site.data.keyword.openwhisk_short}} para:

* Executar os fragmentos de código ou ações no {{site.data.keyword.openwhisk_short}}. Consulte [Criando e chamando ações](./openwhisk_actions.html).
* Usar acionadores e regras para ativar suas ações para responder a eventos. Consulte [Criando acionadores e regras](./openwhisk_triggers_rules.html).
* Aprender como pacotes empacotam ações e configuram origens de eventos externos. Veja [Criar e usar pacotes](./openwhisk_packages.html).
* Explore o catálogo de pacotes e aprimore seus aplicativos com serviços externos, como uma [origem de eventos do {{site.data.keyword.cloudant}}](./openwhisk_cloudant.html).

Para listar os comandos para o plug-in do {{site.data.keyword.openwhisk_short}}, execute `ibmcloud fn` sem argumentos.
{: tip}

## Usando serviços de ações
{: #binding_services}

O {{site.data.keyword.openwhisk_short}} fornece o comando `service bind` para disponibilizar suas credenciais de serviço do {{site.data.keyword.Bluemix_notm}} para seu código no tempo de execução. É possível usar o comando `service bind` para ligar qualquer serviço {{site.data.keyword.Bluemix_notm}} a qualquer ação que está definida no {{site.data.keyword.openwhisk_short}}.

Para obter etapas detalhadas sobre como usar serviços de ações, veja [Ligando serviços a ações](./binding_services.html).

## Configurando a CLI do {{site.data.keyword.openwhisk_short}} para usar um proxy HTTPS
{: #cli_https_proxy}

A CLI do {{site.data.keyword.openwhisk_short}} pode ser configurada para usar um proxy HTTPS. Para configurar um proxy HTTPS, uma variável de ambiente chamada `HTTPS_PROXY` deve ser criada. A variável deve ser configurada para o endereço do proxy HTTPS e sua porta usando o formato a seguir:
`{PROXY IP}:{PROXY PORT}`.

## Alternando para diferentes regiões, organizações e espaços
{: #region_info}

Se você já está com login efetuado, é possível executar o comando `ibmcloud target` na CLI do {{site.data.keyword.Bluemix_notm}} para alternar regiões, organização e espaços.

O {{site.data.keyword.openwhisk_short}} está disponível nas regiões Sul dos EUA e Reino Unido do {{site.data.keyword.Bluemix_notm}}. Para mudar regiões, use o comando `ibmcloud target`. Por exemplo, para alternar para a região do Reino Unido e para o espaço `staging` nessa região:
```
ibmcloud target -r eu-gb -s staging
```
{: pre}

É possível criar espaços para manipular suas implementações de pré-produção (preparação) e produção criando espaços para cada uma. A criação de espaços permite que o {{site.data.keyword.openwhisk_short}} tenha dois namespaces diferentes que estejam definidos para você. Execute [`ibmcloud iam space-create`](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_space_create) para criar mais espaços sob sua organização, como "staging" e "production":

```
ibmcloud iam space-create "staging"
ibmcloud iam space-create "production"
```
{: pre}

O {{site.data.keyword.openwhisk_short}} tem restrições sobre nomes de namespace. Para obter mais informações, consulte a documentação [Detalhes e limites do sistema](https://console.bluemix.net/docs/openwhisk/openwhisk_reference.html#openwhisk_entities).
{: tip}

## Migrando da CLI do OpenWhisk para o plug-in da CLI do {{site.data.keyword.openwhisk_short}}
{: #cli_migration}

Com a introdução do plug-in da CLI do {{site.data.keyword.openwhisk_short}}, a CLI independente do OpenWhisk não é mais necessária.

### Sintaxe do Comando
{: #command_syntax}

Todos os comandos `wsk`, exceto o comando `wsk bluemix login` que não é mais necessário, funcionam da mesma maneira usando o comando `ibmcloud fn`.  Todas as opções de comando e os argumentos são os mesmos.

### Autenticação e host de API
{: #api_authentication}

Na CLI do OpenWhisk, era necessário configurar a chave API de autenticação e o host de API.
Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, não é necessário configurar explicitamente a chave API e o host de API. Em vez disso, é possível efetuar login com o `ibmcloud login` e destinar sua região e namespace usando o comando `ibmcloud target`. Depois de efetuar login, todos os comandos iniciam com `ibmcloud fn`.

Se você precisar usar a chave API de autenticação para o {{site.data.keyword.openwhisk_short}} em um cliente HTTP externo como cURL ou Postman, será possível recuperá-la com os comandos a seguir:

Para obter a chave API atual:
```
ibmcloud fn property get --auth
```
{: pre}

Para obter o host de API atual:
```
ibmcloud fn property get --apihost
```
{: pre}

A chave API é específica por região, organização e espaço destinado pelo plug-in da CLI do {{site.data.keyword.openwhisk_short}}.
{: tip}

### Autenticação do API Gateway
{: #apigw_authentication}

Na CLI do OpenWhisk, era necessário executar o `wsk bluemix login` para poder configurar a autorização do API Gateway para gerenciamento de suas APIs usando o comando `wsk api`. Com o plug-in da CLI do {{site.data.keyword.openwhisk_short}}, você não precisa executar `wsk bluemix login`. Em vez disso, ao usar o comando `ibmcloud login` para efetuar login no {{site.data.keyword.Bluemix_notm}}, o plug-in do {{site.data.keyword.openwhisk}} utiliza automaticamente o login atual e as informações de destino. Agora é possível gerenciar suas APIs usando o comando `ibmcloud fn api`.

### Migrando scripts de implementação
{: #migrating_deploy_scripts}

Se você tem scripts que usam a CLI do OpenWhisk com o binário `wsk`, todos os comandos funcionam da mesma maneira usando o comando `ibmcloud fn`. É possível modificar seus scripts para usar o plug-in da CLI do {{site.data.keyword.Bluemix_notm}} ou criar um alias ou wrapper para que os comandos atuais usando `wsk` sejam convertidos para `ibmcloud fn`. Os comandos `ibmcloud login` e `ibmcloud target` na CLI do {{site.data.keyword.Bluemix_notm}} funcionam no modo não assistido. Com o modo não assistido, é possível configurar seu ambiente antes de executar comandos `ibmcloud fn` para implementar e gerenciar suas entidades do {{site.data.keyword.openwhisk_short}}.

## Histórico de versões
{: #version_history}

Um registro histórico de versões que mostram destaques e correções de bug.

1.0.18 (20-06-2018)
* Correção para desvincular instâncias de serviço fornecido pelo usuário.
* Melhorias de desempenho.

1.0.17 (12-06-2018)
* Incluir suporte para ligar (`ibmcloud wsk service bind`) e desvincular (`ibmcloud wsk service unbind`) instâncias de serviço fornecido pelo usuário que são criadas usando o comando `ibmcloud cf create-user-provided-service`.

1.0.16 (24-05-2018)
* Correções de bugs secundários e melhorias.

1.0.15 (21-05-2018)
* Correções de bugs secundários e melhorias.

1.0.14 (17-05-2018)
* Ativar o suporte para o caractere `&` em nomes de organização e espaço.

1.0.13 (07-05-2018)
* Correções menores de bug e melhorias de manipulação de erros.

1.0.12 (30-04-2018)
* Atualizações do {{site.data.keyword.Bluemix_notm}} SDK para manter a compatibilidade da CLI `bx`.

1.0.11 (23-04-2018)
* Correções de bugs secundários e melhorias.

1.0.10 (09-04-2018)
* Incluir nova opção `--web-secure` para os comandos `ibmcloud wsk action create|update` para proteger os terminais de ação da web.
* Corrigir o [defeito](https://github.com/apache/incubator-openwhisk-cli/issues/237) do parâmetro de caminho back-to-back.

1.0.9 (16-03-2018)
* Ativar o suporte para ligação de serviço no nível de pacote.

1.0.8 (22-02-2018)
* Ativar o suporte para ligação de serviço do IAM.

1.0.7 (02-02-2018)
* O `ibmcloud wsk api` agora aceita parâmetros de caminho como `/api/{id}`. Para obter informações, veja [API Gateway](./openwhisk_apigateway.html).
* Restaure o suporte do proxy.
* Remova `swift:3`.

1.0.6 (30-1-2018)
* Correção de bug para o comando `ibmcloud wsk service bind` para ações dentro de um pacote.
