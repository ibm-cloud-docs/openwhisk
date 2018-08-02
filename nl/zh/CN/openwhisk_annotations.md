---

copyright:
  years: 2016, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 注释
{: #openwhisk_annotations}

{{site.data.keyword.openwhisk}} 操作、触发器、规则和包（统称为“资产”）可以使用`注释`进行装饰。注释附加到资产，就像用 `key` 定义名称，用 `value` 定义值的参数一样。在命令行界面 (CLI) 中通过 `--annotation` 标志（或缩写 `-a`）可方便地对其进行设置。
{: shortdesc}

理由：向 {{site.data.keyword.openWhisk_short}} 添加了注释，以支持在不更改底层资产模式的情况下进行试验。在编写本文档之前，我们一直有意不定义允许使用哪些`注释`。但是，随着注释更加频繁地用于说明语义更改，请务必开始记录这些注释。

目前，注释的最普遍用法是记录操作和包。{{site.data.keyword.openwhisk_short}} 目录中有许多包带有注释，例如：对其操作提供的功能的描述、在包绑定时要使用的参数、调用时参数，或者参数是否为“秘密信息”（例如，密码）。注释是根据需要创造的，例如用于支持 UI 集成。

下面是针对 `echo` 操作的一组注释样本，此操作会返回未加修改的输入自变量（例如，`function main(args) { return args }`）。此操作可用于对输入参数进行日志记录（例如，作为某个序列或规则的一部分）。
```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

描述**包**的注释包括：

- `description`：对包的精简描述。
- `parameters`：一个数组，用于描述范围限定为该包的参数。

描述**操作**的注释包括：

- `description`：对操作的精简描述。
- `parameters`：一个数组，用于描述执行该操作所需的操作。
- `sampleInput`：一个示例，显示使用典型值的输入模式。
- `sampleOutput`：一个示例，显示输出模式，通常用于 `sampleInput`。

描述**参数**的注释包括：

- `name`：参数的名称
- `description`：对参数的精简描述。
- `doclink`：一个链接，指向参数的进一步文档（对于 OAuth 令牌非常有用）。
- `required`：对于必需参数为 True，对于可选参数为 false。
- `bindTime`：如果参数在绑定包时指定，那么为 True。
- `type`：参数的类型，为 `password` 或 `array`（但可以有更广的用法）。

_不会_对注释进行检查。所以，举例来说，虽然完全可以使用注释推断两个操作组合成一个序列是否合法，但系统目前还不会这样做。

## 特定于 Web 操作的注释
{: #annotations-specific-to-web-actions}

最近，核心 API 已使用新功能进行了扩展。为了支持包和操作参与这些功能，引入了在语义上有意义的新注释。这些注释必须显式设置为 `true` 才能生效。如果将值从 `true` 更改为 `false`，会将连接的资产排除在新 API 之外。这些注释在系统中并无其他意义。请参阅以下注释：

- `web-export`：仅应用于操作。如果出现此注释，会使其对应操作可供 REST 调用访问，而_无需_认证。这些操作称为 [_Web 操作_](openwhisk_webactions.html)，因为它们允许用户通过浏览器等工具来使用 OpenWhisk 操作。需要注意的是，Web 操作的_所有者_也需要在系统中运行这些操作。换言之，操作的_所有者_也拥有激活记录。
- `final`：仅应用于操作。此注释会使已经定义的所有操作参数都无法改变。带有此注释的操作参数一旦通过其封装包或操作定义来定义了参数值，即无法通过调用时参数进行覆盖。
- `raw-http`：仅在出现 `web-export` 注释时应用于操作。如果出现此注释，HTTP 请求查询和主体参数将作为保留的属性传递到操作。
- `web-custom-options`：设置此注释后，它将支持 Web 操作响应具有定制头的 OPTIONS 请求，否则将应用[缺省 CORS 响应](openwhisk_webactions.html#options-requests)。
- `require-whisk-auth`：此注释用于保护 Web 操作，以便只有提供相应认证凭证的请求才能调用该操作。设置为布尔值时，此注释可控制是否对请求的基本认证值（即 Whisk 认证密钥）进行认证。值为 `true` 将认证凭证，值为 `false` 将调用此操作，而不进行任何认证。设置为数字或字符串时，此值必须与请求的 `X-Require-Whisk-Auth` 头值相匹配。在这两种情况下，需要注意的是，Web 操作的_所有者_需要在系统中运行这些操作（即，操作的_所有者_也拥有激活记录）。

## 特定于激活的注释

系统可以使用以下注释来装饰激活记录：

- `path`：生成激活的操作的标准路径名。请注意，如果此激活是包绑定中操作的结果，那么路径会引用父包。
- `kind`：所执行操作的类型以及其中一种支持 OpenWhisk 运行时类型。
- `limits`：制约此激活的时间、内存和日志限制。

对于与序列相关的激活，系统将生成以下注释：

- `topmost`：仅对于最外层的序列操作，才会出现此注释。
- `causedBy`：仅对于序列中包含的操作，才会出现此注释。

最后，为了提供性能透明度，激活还会记录以下注释：

- `waitTime`：在内部 OpenWhisk 系统中等待的时间。这大约是在控制器收到激活请求与调用者为操作供应容器之间所用的时间。目前，此值仅对非序列相关的激活出现。对于序列，此信息可以从 `topmost` 序列激活记录来派生。
- `initTime`：初始化函数所用时间。如果出现此值，说明操作需要初始化并表示冷启动。热激活会跳过初始化，在这种情况下，不会生成注释。

下面显示了将在激活记录中显示的这些注释的示例。

```javascript
"annotations": [
  {
    "key": "path",
    "value": "guest/echo"
  },
  {
    "key": "waitTime",
    "value": 66
  },
  {
    "key": "kind",
    "value": "nodejs:6"
  },
  {
    "key": "initTime",
    "value": 50
  },
  {
    "key": "limits",
    "value": {
      "logs": 10,
      "memory": 256,
      "timeout": 60000
    }
  }
]
```
{: codeblock}
