---

copyright:
  years: 2018
lastupdated: "2018-06-21"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Ligando serviços a ações
{: #binding_services}

É possível usar o [Plug-in da CLI do {{site.data.keyword.openwhisk}}](./bluemix_cli.html) para ligar um serviço a uma ação ou um pacote. O comando `ibmcloud fn service bind` do {{site.data.keyword.openwhisk_short}} torna suas credenciais de serviço {{site.data.keyword.Bluemix_notm}} disponíveis para seu código do {{site.data.keyword.openwhisk_short}} no tempo de execução.
{: shortdesc}

Não confunda o comando `ibmcloud fn service bind` com o comando `cf bind-service` que está disponível no Cloud Foundry.
{: tip}

## Ligando um serviço a uma ação ou um pacote
{: #cli_bind}

Ligue qualquer serviço {{site.data.keyword.Bluemix_notm}} a qualquer ação que esteja definida no {{site.data.keyword.openwhisk_short}}. Ligar um serviço cria um novo parâmetro em sua ação existente que contém as credenciais da instância de serviço.

**Nota**: é possível ligar somente um serviço de cada tipo a uma ação ou um pacote. A ligação de múltiplos serviços do mesmo tipo não é suportada.

Antes de iniciar, [defina credenciais](/docs/apps/reqnsi.html#accser_external) para o serviço que você deseja ligar.

1. Obtenha o nome da instância de serviço que você deseja ligar a uma ação ou um pacote.
    ```
    Ibmcloud lista de serviços
    ```
    {: pre}

    Exemplo de Saída:
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Obtenha o nome das credenciais que estão definidas para a instância de serviço que você obteve na etapa anterior.
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Ligue o serviço a uma ação.
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes do comando <code>ibmcloud fn service bind</code></caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os componentes do comando <code>ibmcloud fn service bind</code></th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>O tipo de serviço que você está ligando.</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>O nome da ação ou do pacote ao qual você deseja ligar o serviço.</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>Opcional: especifique um nome da instância de serviço. Se você não especificar um nome da instância de serviço, a primeira instância para o serviço será selecionada.</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>Opcional: especifique um nome do conjunto de credenciais. Se você não especificar um nome do conjunto de credenciais, o primeiro conjunto de credenciais para a instância de serviço será selecionado.</td>
    </tr>
    </tbody></table>

    Por exemplo, para ligar um serviço de conversa do {{site.data.keyword.ibmwatson}} a uma ação nomeada `hello`:
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verifique se as credenciais foram ligadas com êxito. A ação à qual o serviço está ligado não suporta sinalizações customizadas, mas suporta as sinalizações detalhadas e de depuração.
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got action Hello World
{
        "parameters":[ {
                "key": "var1",
            "value": "val1"
            },
            {
                "key": "dog",
            "value": "cat"
            },
            {
                "key": "__bx_creds",
            "value": {
                    "conversation": {
                        "password": "[Service password]",
                    "url": "[Service url]",
                    "username": "[Service username]",
                    "instance": "Conversation-qp",
                    "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    Neste exemplo, as credenciais para o serviço de conversa, juntamente com quaisquer outras credenciais para outros tipos de serviço, pertencem a um parâmetro nomeado `__bx_creds`. A ação é para o parâmetro ligado `__bx_creds` e remove a referência ao tipo de serviço listado. Se esse tipo de serviço é o único listado, a ação anula o valor do parâmetro `__bx_creds`. Se mais de um serviço é ligado à ação, o parâmetro `__bx_creds` permanece com os serviços que ainda estão ligados.

Para obter mais informações sobre como passar parâmetros para uma ação ou um pacote e como as credenciais são afetadas durante as operações `update`, veja [Trabalhando com parâmetros](./parameters.html#pass-params-action).


## Desvinculando um serviço de uma ação ou um pacote
{: #cli_unbind}

Desvincule um serviço de uma ação ou um pacote. Desvincular um serviço remove as ligações existentes criadas pelo comando `service bind`.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
