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


# 将 {{site.data.keyword.cloud_notm}} 服务绑定到 {{site.data.keyword.openwhisk_short}} 实体
{: #services}

您可以在 {{site.data.keyword.openwhisk_short}} 应用程序中包含 IBM Cloud 服务中的功能。
{: shortdesc}

**如何向应用程序添加 IBM Cloud 服务？**

1. 可以将 REST API 调用硬编码到应用程序中。此选项可能是与 IBM Cloud 服务进行通信的最快方法。
2. 可以使用预安装或可安装的包来合并功能。您可以在应用程序代码内使用存储在包中的操作和订阅源。此选项可能会略为精简代码，如果应用程序已接近系统限制，那么此选项可能很有用。


**如何设置必须由应用程序访问的参数？**

这些参数可能包含使应用程序可复用于不同数据的值，也可能包含服务需要的值（例如，凭证）。 
1. 可以将参数硬编码到应用程序中。此选项可能并不是存储保密信息（如凭证）的最安全方式。
2. 可以通过将参数绑定到操作或包，从而将这些参数绑定到应用程序。


## 将服务绑定到操作或包
{: #services_bind}

将任何 {{site.data.keyword.cloud_notm}} 服务绑定到任何操作。绑定服务时，会在现有操作上创建包含服务实例凭证的新参数。

不能将同一服务的多个实例绑定到一个操作或包。只能绑定服务的一个实例。
{: note}

开始之前，为要绑定到操作的服务[创建操作](/docs/openwhisk?topic=cloud-functions-actions)并[定义凭证](/docs/resources?topic=resources-externalapp#externalapp)。

1. 获取要绑定到操作或包的服务和服务实例的名称。在示例输出中，`composer` 是服务，`Composer-qp` 是服务实例名称。
    ```
    ibmcloud service list
    ```
    {: pre}

    **示例输出**
    ```
    name              service        plan   bound apps   last operation
    Composer-qp   composer   free                create succeeded
    Composer-uc   composer   free                create succeeded
    Discovery-37      discovery      lite                create succeeded
    ```
    {: screen}

2. 获取为服务实例所定义的凭证的名称。
    ```
    ibmcloud service keys SERVICE_NAME
    ```
    {: pre}

    **示例**
    ```
    ibmcloud service keys Composer-qp
    ```
    {: pre}

    **示例输出**
    ```
    Invoking 'cf service-keys Composer-qp'...

    Getting keys for service instance Composer-qp as <your ID>...

    name
Credentials-1
Credentials-2
```
    {: screen}

3. 将服务绑定到操作。
    `ibmcloud fn service bind` 命令可使您的 {{site.data.keyword.cloud_notm}} 服务凭证在运行时可用于 {{site.data.keyword.openwhisk_short}} 代码。
    以下命令参数可用于 `ibmcloud fn service bind` 命令。

    <table>
    <thead>
        <tr>
        <th>参数</th>
        <th>描述</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><code>SERVICE</code></td>
        <td>要绑定的服务名称。</td>
        </tr>
        <tr>
        <td><code>ACTION_NAME</code></td>
        <td>要将服务绑定到的操作或包的名称。</td>
        </tr>
        <tr>
        <td><code>--instance INSTANCE_NAME</code></td>
        <td>（可选）指定服务实例名称。如果未指定服务实例名称，那么将选择服务的第一个实例。</td>
        </tr>
        <tr>
        <td><code>--keyname CREDENTIALS_NAME</code></td>
        <td>（可选）指定凭证名称。如果未指定凭证名称，那么将选择服务实例的第一个凭证。</td>
        </tr>
    </tbody>
    </table>

    **示例语法**
    ```
    ibmcloud fn service bind SERVICE ACTION_NAME [--instance INSTANCE_NAME][--keyname CREDENTIALS_NAME]
    ```
    {: pre}

    例如，要将 {{site.data.keyword.ibmwatson}} Composer 服务绑定到名为 `hello` 的操作，请运行以下命令。
    ```
    ibmcloud fn service bind composer hello --instance Composer-qp --keyname Credentials-1
    ```
    {: pre}

    **输出**
    ```
    Service credentials 'Credentials-1' from service 'Composer-qp' bound to action 'hello'.
    ```
    {: screen}

4. 验证凭证是否已成功绑定。服务绑定到的操作不支持任何定制标志，但支持调试和详细信息标志。
    

    ```
    ibmcloud fn action get hello parameters
    ```
    {: pre}

    **示例输出**
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

    在此示例中，Composer 服务的凭证（以及其他服务类型的其他任何凭证）属于名为 `__bx_creds` 的参数。该操作将查找 `__bx_creds` 绑定参数，并除去对所列出的服务类型的引用。如果该服务类型是唯一列出的服务类型，那么该操作将使 `__bx_creds` 参数的值无效。如果有多个服务绑定到该操作，那么 `__bx_creds` 参数将保持与仍绑定的任何服务一起使用。

有关将参数传递到操作或包的更多信息，请参阅[将参数绑定到操作](/docs/openwhisk?topic=cloud-functions-actions#actions_params)。

## 取消服务与操作的绑定
{: #services_unbind}

取消服务与操作或包的绑定会除去现有服务绑定。

```
ibmcloud fn service unbind SERVICE_NAME ACTION_NAME
```
{: pre}

