---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# CLI independente do {{site.data.keyword.openwhisk_short}}

**Em 9 de março de 2018, a CLI independente do {{site.data.keyword.openwhisk_short}} não estará mais disponível para download. Para continuar gerenciando entidades do {{site.data.keyword.openwhisk_short}} após essa data, use o [plug-in da CLI do {{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) para a CLI do {{site.data.keyword.Bluemix}}.**

A interface da linha de comandos **wsk** distribuída do {{site.data.keyword.openwhisk}} permite o gerenciamento de todas as entidades do {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

<!--
This service is deprecated: All instances of this service are deprecated. Existing instances can be used until 09 December 2016. For more information, see the [deprecation announcement blog](http://www.com){: new_window}.
{:deprecated}

IBM recommends that you use the new [{{site.data.keyword.openwhisk_short}} plug-in for the {{site.data.keyword.Bluemix_notm}} CLI](./bluemix_cli.html) to manage {{site.data.keyword.openwhisk_short}} entities. The following management tasks are easier if you use the plugin.
{: tip}
-->

* Configuração da chave de autenticação do {{site.data.keyword.openwhisk_short}}
  * Não é necessário acessar o console do {{site.data.keyword.openwhisk_short}} para obter a chave de autenticação
  * A geração de chave de autenticação automática ocorre ao alternar entre regiões, organizações e espaços
* Atualizando uma chave de autenticação do {{site.data.keyword.openwhisk_short}} expirada
* Atualizando a CLI para uma versão mais recente


As tarefas de gerenciamento a seguir requerem que você use o plug-in.

* Gerenciamento de API
  * Configuração do token de acesso do API GW
  * Atualizando um token de acesso do API GW expirado

## Configurando a CLI do {{site.data.keyword.openwhisk_short}} 
{: #openwhisk_cli_setup}

É possível usar a interface da linha de comandos (CLI) do {{site.data.keyword.openwhisk_short}} para configurar o namespace e a chave de autorização.
Acesse [Configurar CLI](https://console.bluemix.net/openwhisk/cli?loadWsk=true) e siga as instruções para instalá-la.

Primeiro, configure as duas propriedades a seguir:

1. **Host da API** (nome ou endereço IP) para a implementação do {{site.data.keyword.openwhisk_short}} que você deseja usar.
2. **Chave de autorização** (nome do usuário e senha) que concede acesso à API do {{site.data.keyword.openwhisk_short}}.

Estão disponíveis duas regiões do {{site.data.keyword.Bluemix_notm}} que requerem seu host de API e chave de autorização exclusivos.

* Sul dos Estados Unidos
  * Host da API: `openwhisk.ng.bluemix.net`

* 
  * Host da API: `openwhisk.eu-gb.bluemix.net`

Execute o comando a seguir para configurar o host de API para sua região do {{site.data.keyword.Bluemix_notm}} desejada:

Sul dos EUA:
```
wsk property set --apihost openwhisk.ng.bluemix.net
```
{: pre} 

Reino Unido:
```
wsk property set --apihost openwhisk.eu-gb.bluemix.net
```
{: pre}

Se você precisa alternar regiões, deve-se reconfigurar a CLI com o host de API e a chave de autorização porque a chave de autorização é específica por região.
{: tip}

Artefatos como Ações, Regras e pacotes são específicos da região. Então, se você usar o mesmo artefato em múltiplas regiões, deverá implementá-lo em cada região desejada.

Se você souber sua chave de autorização, será possível configurar a CLI para usá-la. 

Execute o comando a seguir para configurar a Chave de autorização:

```
wsk property set --auth <authorization_key>
```
{: pre}

**Dica:** a CLI do {{site.data.keyword.openwhisk_short}} armazena as propriedades configuradas em `~/.wskprops` por padrão. O local desse arquivo pode ser alterado configurando a variável de ambiente `WSK_CONFIG_FILE`. 

Para verificar a configuração da CLI, tente [criar e executar uma ação](./index.html#openwhisk_start_hello_world).

## Usando o CLI do {{site.data.keyword.openwhisk_short}}
{: #using_openwhisk_cli}

Quando o ambiente estiver configurado, será possível usar a CLI do {{site.data.keyword.openwhisk_short}} para executar as tarefas a seguir:

* Execute seus fragmentos de código ou ações no {{site.data.keyword.openwhisk_short}}. Veja [Criar e chamar ações](./openwhisk_actions.html).
* Use acionadores e regras para permitir que suas ações respondam a eventos. Veja [Criar acionadores e regras](./openwhisk_triggers_rules.html).
* Saiba como os pacotes empacotam ações e configuram origens de eventos externos. Consulte [Usando e criando pacotes](./openwhisk_packages.html).
* Explorar o catálogo de pacotes e aprimorar seus aplicativos com serviços externos, como uma [Origem de eventos do Cloudant](./openwhisk_cloudant.html). Veja [Pacotes do {{site.data.keyword.openwhisk_short}} pré-instalados](./openwhisk_catalog.html).

## Configure a CLI para usar um proxy HTTPS
{: #cli_https_proxy}

A CLI pode ser configurada para usar um proxy HTTPS. Para configurar um proxy HTTPS, uma variável de ambiente chamada `HTTPS_PROXY` deve ser criada. A variável deve ser configurada para o endereço do proxy HTTPS e sua porta usando o formato a seguir: `{PROXY IP}:{PROXY PORT}`.
