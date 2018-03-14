---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Introdução ao {{site.data.keyword.openwhisk_short}}

O {{site.data.keyword.openwhisk}} é um serviço de cálculo distribuído, acionado por eventos, também referido como computação Sem servidor ou como Function as a Service (FaaS). O {{site.data.keyword.openwhisk_short}} executa a lógica de aplicativo em
resposta a eventos ou chamadas diretas de apps da web ou móveis sobre HTTP. Os eventos podem ser fornecidos por meio de serviços do {{site.data.keyword.Bluemix}} como o Cloudant e por meio de origens externas. Os desenvolvedores podem focar a composição da lógica de aplicativo e a criação de ações que são executadas sob demanda.
O benefício chave desse novo paradigma é que você não provisiona os servidores explicitamente. Eliminando, portanto, a preocupação com ajuste automático de escala, alta disponibilidade, atualizações, manutenção e custo por horas de tempo do processador quando o servidor está sendo executado, mas não entregando solicitações.
Seu código é executado quando há uma chamada HTTP, mudança de estado do banco de dados ou outro tipo de evento que aciona a execução do código.
Você é cobrado por milissegundo de tempo de execução (arredondado para os 100 ms mais próximos), não por hora de utilização de VM, independentemente se essa VM estava executando um trabalho útil ou não.
{: shortdesc}

Esse modelo de programação é uma correspondência perfeita para microsserviços, dispositivos móveis, IoT e muitos outros apps. Você obtém ajuste automático de escala e balanceamento de carga inerentes prontos para utilização, sem ter que configurar manualmente clusters, balanceadores de carga, plug-ins de http, etc. Caso você execute no {{site.data.keyword.openwhisk}}, também obterá um benefício de administração zero - o que significa que todo o hardware, a rede e o software é mantido pela IBM. Tudo o que você precisa fazer é fornecer o código que deseja executar e fornecê-lo ao {{site.data.keyword.openwhisk}}. O resto é “mágica”. Uma boa introdução para o modelo de programação Serverless está disponível no [blog de Martin Fowler](https://martinfowler.com/articles/serverless.html).

Também é possível obter o [Código-fonte do Apache OpenWhisk](https://github.com/openwhisk/openwhisk) e executar o sistema sozinho.

Para obter mais detalhes sobre como o {{site.data.keyword.openwhisk_short}} funciona, consulte [Sobre o {{site.data.keyword.openwhisk_short}}](./openwhisk_about.html).

É possível usar o Navegador ou a CLI para desenvolver os seus aplicativos {{site.data.keyword.openwhisk_short}}.
Ambos têm recursos semelhantes para desenvolver aplicativos; a CLI fornece mais controle sobre a sua implementação e as operações.

## Desenvolver em seu navegador
{: #openwhisk_start_editor}

Experimente o {{site.data.keyword.openwhisk_short}} em seu [Navegador](https://console.{DomainName}/openwhisk/actions) para criar ações, automatizar ações usando acionadores e explorar pacotes públicos. Visite a página [Saiba mais](https://console.{DomainName}/openwhisk/learn) para um tour rápido da Interface com o usuário do {{site.data.keyword.openwhisk_short}}.

## Desenvolver usando a CLI
{: #openwhisk_start_configure_cli}

É possível usar a interface da linha de comandos (CLI) do {{site.data.keyword.openwhisk_short}} para configurar o namespace e a chave de autorização.
Acesse [Configurar CLI](https://console.{DomainName}/openwhisk/cli) e siga as instruções para instalá-la.

## visão geral
{: #openwhisk_start_overview}
- [Como funciona o OpenWhisk](./openwhisk_about.html)
- [Casos de uso comuns para aplicativos Serverless](./openwhisk_use_cases.html)
- [Configurando e usando a CLI do OpenWhisk](./openwhisk_cli.html)
- [Usando o OpenWhisk por meio de um app iOS](./openwhisk_mobile_sdk.html)
- [Artigos, amostras e
tutoriais](https://github.com/openwhisk/openwhisk-external-resources)
- [FAQ do Apache OpenWhisk](http://openwhisk.org/faq)
- [Preços](https://console.ng.bluemix.net/openwhisk/learn/pricing)

## Gabarito de Programação
{: #openwhisk_start_programming}
- [Detalhes do Sistema](./openwhisk_reference.html)
- [Catálogo de serviços fornecidos pelo OpenWhisk](./openwhisk_catalog.html)
- [Ações
](./openwhisk_actions.html)
- [Acionadores e regras](./openwhisk_triggers_rules.html)
- [Feeds](./openwhisk_feeds.html)
- [Pacotes](./openwhisk_packages.html)
- [Anotações](./openwhisk_annotations.html)
- [Ações da web](./openwhisk_webactions.html)
- [Gateway de API](./openwhisk_apigateway.html)
- [Nomes de entidades](./openwhisk_reference.html#openwhisk_entities)
- [Semântica de ação](./openwhisk_reference.html#openwhisk_semantics)
- [  Limites
](./openwhisk_reference.html#openwhisk_syslimits)

## Exemplo Hello World do {{site.data.keyword.openwhisk_short}}
{: #openwhisk_start_hello_world}
Para iniciar o uso do {{site.data.keyword.openwhisk_short}}, tente o exemplo de código JavaScript a seguir.

```javascript
/**
 * Hello world como uma ação OpenWhisk.
 */
function main(params) {
    var name = params.name || 'World';
    return {payload:  'Hello, ' + name + '!'};
}
```
{: codeblock}

Para usar esse exemplo, siga estas etapas:

1. Salve o código em um arquivo. Por exemplo, *hello.js*.

2. Na linha de comandos da CLI do {{site.data.keyword.openwhisk_short}}, crie a ação inserindo este comando:
    ```
    wsk action create hello hello.js
    ```
    {: pre}

3. Em seguida, chame a ação inserindo os comandos a seguir.
    ```
    wsk action invoke hello --blocking --result
    ```
    {: pre}  

    Estas
saídas de comando:
    ```json
    {
        "payload": "Hello, World!"
    }
    ```
    
    ```
    wsk action invoke hello --blocking --result --param name Fred
    ```
    {: pre}  

    Estas
saídas de comando:
    ```json
    {
        "payload": "Hello, Fred!"
    }
    ```

Também é possível usar os recursos acionados por eventos no {{site.data.keyword.openwhisk_short}} para chamar essa ação em resposta a eventos. Siga o [exemplo de serviço de alarme](./openwhisk_packages.html#openwhisk_package_trigger) para configurar uma origem de eventos para chamar a Ação `hello` toda vez que um evento periódico é gerado.

Uma lista completa de
[Tutoriais
e Amostras do OpenWhisk pode ser localizada aqui](https://github.com/openwhisk/openwhisk-external-resources#sample-applications). Além das amostras, esse repositório contém links para artigos, apresentações, podcasts, vídeos e outros recursos relacionados ao {{site.data.keyword.openwhisk_short}}.

## Referência à API
{: #openwhisk_start_api notoc}
* [Documentação da API REST](./openwhisk_reference.html#openwhisk_ref_restapi)
* [REST API](https://console.{DomainName}/apidocs/98)

## Links relacionados
{: #general notoc}
* [Descobrir: {{site.data.keyword.openwhisk_short}}](http://www.ibm.com/cloud-computing/bluemix/openwhisk/)
* [{{site.data.keyword.openwhisk_short}} no IBM developerWorks](https://developer.ibm.com/openwhisk/)
* [Website do projeto Apache {{site.data.keyword.openwhisk_short}}](http://openwhisk.org)
