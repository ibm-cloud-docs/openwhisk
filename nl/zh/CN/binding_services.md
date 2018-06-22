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

# 通过操作使用服务
{: #binding_services}

您可以利用 [{{site.data.keyword.openwhisk}} CLI 插件](./bluemix_cli.html)将服务绑定到操作。{{site.data.keyword.openwhisk_short}} 提供了 `service bind` 命令，使您的 {{site.data.keyword.Bluemix}} 服务凭证在运行时可用于云功能代码。`service bind` 命令不可与 Cloud Foundry 中提供的 `cf bind-service` 命令相混淆。service bind 命令只用于在现有操作上自动创建包含服务凭证的新参数。{{site.data.keyword.openwhisk_short}} `service bind` 命令更灵活，支持将任何 {{site.data.keyword.Bluemix_notm}} 服务绑定到 {{site.data.keyword.openwhisk_short}} 中定义的任何操作。唯一的警告是，您必须为要绑定的服务定义凭证。
{: shortdesc}

## 如何将服务绑定到操作
{: #cli_bind}

使用 [{{site.data.keyword.openwhisk_short}}](./bluemix_cli.html) CLI 插件提供的 `ibmcloud wsk service bind` 命令可将服务绑定到操作。可以在[限制](./binding_services.html#limitations)部分中找到更多信息。

`bind` 的使用语法：
```
ibmcloud wsk service bind SERVICE_NAME ACTION_NAME [--instance instance_name] [--keyname name]
```
{: pre}

`service bind` 命令需要服务类型以及要绑定到的操作名称。例如，如果要将 Watson Conversation 服务绑定到名为 `hello` 的操作，那么调用将类似于以下命令：
```
ibmcloud wsk service bind conversation hello
```
{: pre}

这将生成以下输出：
``` 
Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```

此命令在当前空间中搜索现有 Watson Conversation 服务，获取找到的第一个 Conversation 服务，然后检索属于此服务的所有凭证。使用属于此服务的第一组凭证时，会将这些凭证作为参数绑定到指定的 `hello` 操作。输出会显示操作绑定到具体哪个服务以及来自该服务的哪组凭证用于绑定。

要验证凭证是否已成功绑定，请发出以下命令：
```
ibmcloud wsk action get hello parameters
```
{: pre}

样本输出：
```
ok: got action Hello World
{
    "parameters": [
        {
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

在此，您可以看到此 Conversation 服务的凭证（以及其他服务类型的其他任何凭证）属于名为 `__bx_creds` 的参数，此参数现在可以在操作码中像其他任何绑定的参数一样使用。操作会选取第一个可用的 Conversation 服务，其中包含该服务中定义的第一组凭证。 

有关将参数传递到操作以及执行 `action update` 操作时对凭证有怎样影响的更多信息，请参阅以下文档：[创建和调用操作](openwhisk_actions.html#openwhisk_pass_params)。

`wsk service` 命令支持以下两个标志：

<dl>
    <dt>--instance</dt>
    <dd>要使用的类型的特定服务的名称。</dd>
    <dt>--keyname</dt>
    <dd>要使用的服务内特定凭证的名称。</dd>
</dl>

要了解如何使用这两个标志，请参阅以下示例。使用先前的 `ibmcloud wsk service bind` 命令时，假定实际有两个 Conversation 服务，而操作缺省值最终绑定了不正确的服务/凭证。您可以使用 `--instance` 和 `--keyname` 标志重新运行该命令，以确保将正确的服务绑定到正确的操作。首先，查看哪些服务可用，以及哪些凭证绑定到这些服务。如果要列出我们的服务，应该会看到类似下面的输出：

```
ibmcloud service list
name              service        plan   bound apps   last operation
Conversation-qp   conversation   free                create succeeded
Conversation-uc   conversation   free                create succeeded
Discovery-37      discovery      lite                create succeeded
```

在此输出中，我们看到 **Conversation-qp** 是列出的两个服务中的第一个服务，这是初始 `ibmcloud wsk service bind conversation hello` 命令最终绑定到的服务。您可能希望改为绑定到 **Conversation-uc** 服务。为了绝对有把握，您可以检查 **Conversation-uc** 包含哪些凭证，以确保使用正确的凭证集进行绑定。

```
ibmcloud service keys Conversation-uc
Invoking 'cf service-keys Conversation-uc'...

Getting keys for service instance Conversation-uc as [your id]...

name
Credentials-1
Credentials-2
```

您希望将此服务绑定到“Credentials-2”。要确保操作执行所需行为，请运行以下命令：
```
ibmcloud wsk service bind conversation hello --instance Conversation-uc --keyname Credentials-2
```
{: pre}

这将生成以下输出：
```
Service credentials 'Credentials-2' from service 'Conversation-uc' bound to action 'hello'.
```

在输出中，您可以看到正确的凭证集已绑定到操作。同样，要进行验证，可以查看以下 `ibmcloud wsk action get` 命令。
```
ibmcloud wsk action get hello parameters
```
{: pre}

这将生成以下结果：
```
ok: got action Hello World
{
    "parameters": [
        {
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

正常的调试标志受支持，并可打印调用中的响应头。

## 如何取消服务与操作的绑定
{: #cli_unbind}

使用 `ibmcloud wsk service unbind` 可取消服务与操作的绑定。`service unbind` 命令可除去 `service bind` 命令创建的现有绑定。

`unbind` 的使用语法：
```
ibmcloud wsk service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

## 限制
{: #limitations}

`service` 操作不支持任何定制标志，但支持常规调试和详细信息标志。该操作将查找 `__bx_creds` 绑定参数，并除去对所列出的服务类型的引用。如果该服务类型是唯一列出的服务类型，那么该操作将使 `__bx_creds` 参数的值无效。如果有多个服务绑定到该操作，那么 `__bx_creds` 参数将保持与仍绑定的任何服务一起使用。

每种类型只能有一个服务绑定到一个操作。不支持在单个操作中绑定同一类型的多个服务。
{: tip}

