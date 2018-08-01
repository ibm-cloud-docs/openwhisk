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

# 将服务绑定到操作
{: #binding_services}

您可以使用 [{{site.data.keyword.openwhisk}} CLI 插件](./bluemix_cli.html)将服务绑定到操作或包。{{site.data.keyword.openwhisk_short}} `ibmcloud fn service bind` 命令可使您的 {{site.data.keyword.Bluemix_notm}} 服务凭证在运行时可用于 {{site.data.keyword.openwhisk_short}} 代码。
{: shortdesc}

不要将 `ibmcloud fn service bind` 命令与 Cloud Foundry 中提供的 `cf bind-service` 命令相混淆。
{: tip}

## 将服务绑定到操作或包
{: #cli_bind}

将任何 {{site.data.keyword.Bluemix_notm}} 服务绑定到 {{site.data.keyword.openwhisk_short}} 中定义的任何操作。绑定服务会在现有操作上创建包含服务实例凭证的新参数。

**注**：每种类型只能有一个服务绑定到一个操作或包。不支持绑定同一类型的多个服务。

开始之前，请为要绑定的服务[定义凭证](/docs/apps/reqnsi.html#accser_external)。

1. 获取要绑定到操作或包的服务实例的名称。
    ```
    ibmcloud service list
    ```
    {: pre}

    示例输出：
    ```
    name              service        plan   bound apps   last operation
    Conversation-qp   conversation   free                create succeeded
    Conversation-uc   conversation   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 获取为您在上一步中获取的服务实例所定义的凭证的名称。
    ```
    ibmcloud service keys Conversation-qp
    ```
    {: pre}

    示例输出：
    ```
    Invoking 'cf service-keys Conversation-qp'...

    Getting keys for service instance Conversation-qp as <your ID>...

    name
Credentials-1
Credentials-2
```
    {: screen}

3. 将服务绑定到操作。
    ```
    ibmcloud fn service bind SERVICE_TYPE ACTION_NAME [--instance instance_name] [--keyname credentials_name]
    ```
    {: pre}

    <table>
    <caption>了解 <code>ibmcloud fn service bind</code> 命令的组成部分</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="“构想”图标"/> 了解 <code>ibmcloud fn service bind</code> 命令的组成部分</th>
    </thead>
    <tbody>
    <tr>
    <td><code>SERVICE_TYPE</code></td>
    <td>要绑定的服务的类型。</td>
    </tr>
    <tr>
    <td><code>ACTION_NAME</code></td>
    <td>要将服务绑定到的操作或包的名称。</td>
    </tr>
    <tr>
    <td>--instance <code>instance_name</code></td>
    <td>可选：指定服务实例名称。如果未指定服务实例名称，那么将选择服务的第一个实例。</td>
    </tr>
    <tr>
    <td>--keyname <code>credentials_name</code></td>
    <td>可选：指定凭证集名称。如果未指定凭证集名称，那么将选择服务实例的第一个凭证集。</td>
    </tr>
    </tbody></table>

    例如，要将 {{site.data.keyword.ibmwatson}} Conversation 服务绑定到名为 `hello` 的操作：
    ```
    ibmcloud fn service bind conversation hello --instance Conversation-qp --keyname Credentials-1

    Service credentials 'Credentials-1' from service 'Conversation-qp' bound to action 'hello'.
```
    {: screen}

4. 验证凭证是否已成功绑定。服务绑定到的操作不支持任何定制标志，但支持调试和详细信息标志。
    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    示例输出：
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
    {: screen}

    在此示例中，Conversation 服务的凭证（以及其他服务类型的其他任何凭证）属于名为 `__bx_creds` 的参数。该操作将查找 `__bx_creds` 绑定参数，并除去对所列出的服务类型的引用。如果该服务类型是唯一列出的服务类型，那么该操作将使 `__bx_creds` 参数的值无效。如果有多个服务绑定到该操作，那么 `__bx_creds` 参数将保持与仍绑定的任何服务一起使用。

有关将参数传递到操作或包以及在 `update` 操作期间凭证如何受到影响的更多信息，请参阅[使用参数](./parameters.html#pass-params-action)。


## 取消服务与操作或包的绑定
{: #cli_unbind}

取消服务与操作或包的绑定。取消绑定服务可除去 `service bind` 命令创建的现有绑定。

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}
