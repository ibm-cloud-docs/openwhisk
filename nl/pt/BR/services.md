---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"

keywords: services, serverless, functions

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


# Ligando serviços do {{site.data.keyword.cloud_notm}} a entidades do {{site.data.keyword.openwhisk_short}}
{: #services}

É possível usar a funcionalidade incorporada dos serviços do IBM Cloud em seu app {{site.data.keyword.openwhisk_short}}.
{: shortdesc}

**Como incluir serviços do IBM Cloud em meu app?**

1. É possível codificar permanentemente as chamadas API de REST em seu app. Essa opção pode ser a maneira mais rápida de se comunicar com um serviço IBM Cloud.
2. É possível usar um pacote pré-instalado ou instalável para incorporar a funcionalidade. É possível usar as ações e feeds que são armazenados nos pacotes dentro do código do app. Essa opção pode limitar o seu código um pouco, o que pode ser útil se seu app está próximo dos limites do sistema.


**Como configurar parâmetros que devem ser acessados por meu app?**

Esses parâmetros podem incluir valores que tornam seu app reutilizável com dados diferentes ou eles podem incluir valores que são requeridos pelo serviço, como credenciais. 
1. É possível codificar permanentemente parâmetros de código em seu app. Essa opção pode não ser a maneira mais segura de armazenar informações confidenciais como credenciais.
2. É possível ligar os parâmetros a seu app ligando-os a uma ação ou a um pacote.


## Ligando um serviço a uma ação ou um pacote
{: #services_bind}

Ligue qualquer serviço {{site.data.keyword.cloud_notm}} a qualquer ação. Quando um serviço é ligado, um novo parâmetro é criado em sua ação existente que contém as credenciais da instância de serviço.

Não é possível ligar múltiplas instâncias do mesmo serviço a uma ação ou a um pacote. É possível ligar somente uma instância de um serviço.
{: note}

Antes de iniciar, [crie uma ação](/docs/openwhisk?topic=cloud-functions-actions) e [defina credenciais](/docs/resources?topic=resources-externalapp#externalapp) para o serviço que você deseja ligar à ação.

1. Obtenha o nome do serviço e a instância de serviço que você deseja ligar a uma ação ou a um pacote. Na saída de exemplo, `composer` está o serviço e `Composer-qp` é o nome da instância de serviço.
    ```
    Ibmcloud lista de serviços
    ```
    {: pre}

    **Saída de exemplo**
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. Obtenha o nome das credenciais que são definidas para uma instância de serviço.
    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    **Exemplo**
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Invoking 'cf service-keys Composer-qp'...

    Obtendo chaves para a instância de serviço Composer-qp como <seu ID>...

    name
Credentials-1
Credentials-2
    ```
    {: screen}

3. Ligue o serviço a uma ação. O comando `ibmcloud fn service bind` torna suas credenciais de serviço {{site.data.keyword.cloud_notm}} disponíveis para seu código do {{site.data.keyword.openwhisk_short}} no tempo de execução. Os parâmetros de comando a seguir estão disponíveis para o comando `ibmcloud fn service bind`.

    <table>
    <thead>
        <tr>
        <th>Parâmetro</th>
        <th>Descrição</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><code>SERVICE</code></td>
        <td>O nome do serviço que você está ligando.</td>
        </tr>
        <tr>
        <td><code>ACTION_NAME</code></td>
        <td>O nome da ação ou do pacote ao qual você deseja ligar o serviço.</td>
        </tr>
        <tr>
        <td><code>--instance INSTANCE_NAME</code></td>
        <td>(Opcional) Especifique um nome de instância de serviço. Se você não especificar um nome da instância de serviço, a primeira instância para o serviço será selecionada.</td>
        </tr>
        <tr>
        <td><code>--keyname CREDENTIALS_NAME</code></td>
        <td>(Opcional) Especifique o nome das credenciais. Se você não especificar o nome das credenciais, as primeiras credenciais para a instância de serviço serão selecionadas.</td>
        </tr>
    </tbody>
    </table>

    **Sintaxe de exemplo**
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME][--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    Por exemplo, para ligar um serviço do {{site.data.keyword.ibmwatson}} Composer a uma ação chamada `hello`, execute o comando a seguir.
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    **Saída**
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. Verifique se as credenciais foram ligadas com êxito. A ação à qual o serviço está ligado não suporta nenhum sinalizador customizado, mas suporta os sinalizadores de depuração e detalhado.

    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    **Saída de exemplo**
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
                    "composer": {
                        "password": "[Service password]",
                        "url": "[Service url]",
                        "username": "[Service username]",
                        "instance": "Composer-qp",
                        "credentials": "Credentials-1"
                    },
                }
            }
        ],
    }
    ```
    {: screen}

    Nesse exemplo, as credenciais para o serviço Composer, juntamente com quaisquer outras credenciais para outros tipos de serviço, pertencem a um parâmetro denominado `__bx_creds`. A ação é para o parâmetro ligado `__bx_creds` e remove a referência ao tipo de serviço listado. Se esse tipo de serviço é o único listado, a ação anula o valor do parâmetro `__bx_creds`. Se mais de um serviço é ligado à ação, o parâmetro `__bx_creds` permanece com os serviços que ainda estão ligados.

Para obter mais informações sobre como passar parâmetros para uma ação ou um pacote, consulte [Ligando parâmetros a ações](/docs/openwhisk?topic=cloud-functions-actions#actions_params).

## Desvinculando serviços de ações
{: #services_unbind}

A desvinculação de um serviço de uma ação ou de um pacote remove as ligações de serviços existentes.

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

