---

copyright:
  years: 2018
lastupdated: "2018-02-14"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# Usando serviços de ações
{: #binding_services}

É possível alavancar o [plug-in da CLI do {{site.data.keyword.openwhisk}}](./bluemix_cli.html) para ligar um serviço a uma Ação. O {{site.data.keyword.openwhisk_short}} fornece o comando `service bind` para disponibilizar suas credenciais de serviço do {{site.data.keyword.Bluemix}} para seu código do Cloud Functions no tempo de execução. O comando `service bind` não deve ser confundido com o comando `cf bind-service` que está disponível no Cloud Foundry. É simplesmente uma maneira automatizada para criar um novo parâmetro na Ação existente que contém credenciais de serviço. O comando `service bind` do {{site.data.keyword.openwhisk_short}} é mais flexível e permite ligar qualquer serviço do {{site.data.keyword.Bluemix_notm}} a qualquer Ação definida no {{site.data.keyword.openwhisk_short}}. A única ressalva é que deve-se ter credenciais definidas para o serviço que você deseja ligar.
{: shortdesc}

## Como ligar um serviço a uma Ação
{: #cli_bind}

Ligue um serviço a uma Ação usando o comando `ic wsk service bind` que é fornecido pelo plug-in da CLI do [{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html). Informações adicionais podem ser localizadas na seção [Limitações](./binding_services.html#limitations).

Sintaxe de uso com `bind`:
```
ic wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

O comando `service bind` requer um tipo de serviço e um nome da Ação ao qual ligar. Por exemplo, se você desejasse ligar um serviço de conversa do Watson a uma Ação nomeada `hello`, sua chamada seria semelhante ao comando a seguir:
```
ic wsk service bind conversation hello
```
{: pre}

O que produz a saída a seguir:
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

Esse comando procura o seu espaço atual para serviços de conversa do Watson existentes, toma o primeiro serviço de conversa que ele localiza e, então, recupera todas as credenciais que pertencem a esse serviço. Usando o primeiro conjunto de credenciais que pertencem a esse serviço, ele liga essas credenciais como um parâmetro à Ação `hello` especificada. A saída mostra exatamente a qual serviço a Ação é ligada e qual conjunto de credenciais desse serviço foi usado para a ligação.

Para verificar se as credenciais foram ligadas com êxito, emita o comando a seguir:
```
ic wsk action get hello parameters
```
{: pre}

Saída
de mostra:
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

Daqui, é possível ver que as credenciais para esse serviço de conversa (juntamente com quaisquer outras credenciais para outros tipos de serviço) pertencem a um parâmetro denominado `__bx_creds`, que agora pode ser usado de dentro do código de ação como qualquer outro parâmetro ligado. A ação escolhe o primeiro serviço de conversa disponível que inclui o primeiro conjunto de credenciais definido nesse serviço. 

Para obter informações adicionais sobre como passar parâmetros para uma ação e como as credenciais são afetadas ao executar uma operação `action update`, veja o documento a seguir [Criar e chamar Ações](openwhisk_actions.html#openwhisk_pass_params).

O comando `wsk service` suporta as duas sinalizações a seguir:

<dl>
    <dt>--instance</dt>
    <dd>O nome do serviço específico do tipo que você deseja usar.</dd>
    <dt>--keyname</dt>
    <dd>O nome das credenciais específicas dentro do serviço que você deseja usar.</dd>
</dl>

Para entender como usar essas sinalizações, veja o exemplo a seguir. Usando o comando `ic wsk service bind`, suponha que existiam dois serviços de conversa e o padrão de ação acabou ligando o serviço/credenciais incorretas. Você poderia executar novamente o comando com as sinalizações `--instance` e `--keyname` para assegurar que você ligue o serviço correto à ação correta. Primeiro, veja quais serviços estão disponíveis e quais credenciais estão ligadas a eles. Se fôssemos listar nossos serviços, veríamos uma saída como a seguinte:

```
ic service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

Nessa saída, vemos que **Conversation-qp** é o primeiro de dois serviços listados e é aquele ao qual o comando `ic wsk service bind conversation hello` inicial acabou se ligando. Talvez você deseje se ligar ao serviço **Conversation-uc**. Para ter certeza absoluta, é possível verificar quais credenciais o **Conversation-uc** contém, para assegurar que você ligue usando o conjunto correto de credenciais.

```
ic service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

Você deseja ligar-se ao "Credentials-2" por meio desse serviço. Para certificar-se de que a ação execute o comportamento desejado, execute o comando a seguir:
```
ic wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

O que produz a saída a seguir:
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

Na saída, é possível ver se o conjunto correto de credenciais está ligado à ação. Novamente, para verificar, é possível ver o comando `ic wsk action get` a seguir.
```
ic wsk action get hello parameters
```
{: pre}

O que produz os resultados a seguir:
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
                    "instance": "Conversation-uc",
                    "credentials": "Credentials-2"
                }
            }
        }
    ],
}
```

As sinalizações de depuração normal são suportadas, além de impressão de cabeçalhos de resposta de chamadas.

## Como desvincular um serviço de uma ação
{: #cli_unbind}

Desvincule um serviço de uma ação usando o `ic wsk service unbind`. O comando `service unbind` remove as ligações existentes criadas pelo comando `service bind`.

Sintaxe de uso com `unbind`:
```
ic wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## Limitações
{: #limitations}

A ação `service` não suporta nenhuma sinalização customizada, mas suporta a depuração usual e as sinalizações detalhadas. A ação procura o parâmetro ligado `__bx_creds` e remove a referência ao tipo de serviço listado. Se esse tipo de serviço é o único listado, a ação anula o valor do parâmetro `__bx_creds`. Se mais de um serviço é ligado à ação, o parâmetro `__bx_creds` permanece com os serviços que ainda estão ligados.

É possível ligar somente um serviço de cada tipo a uma ação. A ligação de múltiplos serviços do mesmo tipo dentro de uma única ação não é suportada.
{: tip}

