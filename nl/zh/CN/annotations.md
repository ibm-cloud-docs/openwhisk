---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: annotations, annotate, package, parameters, actions, functions

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


# 注释
{: #annotations}

{{site.data.keyword.openwhisk}} 操作、触发器、规则和包（统称为“实体”）可以包含注释。
{: shortdesc}

注释可附加到参数等实体。注释由 `key`（用于定义名称）和 `value`（用于定义值）组成。注释最常用于记录操作和包。{{site.data.keyword.openwhisk_short}} 目录中的包带有注释。这些注释包括对其操作提供的功能的描述、在包绑定时要使用的参数、调用时参数，或者参数是否为秘密信息。注释是根据需要创造的，例如用于支持 UI 集成。

可以在 CLI 中使用 `--annotation` 或 `-a` 标志来记录实体。

## 操作注释
{: #annotations_action}

|注释|描述|
| --- | --- |
|`description`|操作的描述。|
|`parameters`|一个数组，用于描述执行该操作所需的操作。|
|`sampleInput`|一个示例，显示使用典型值的输入模式。|
|`sampleOutput`|一个示例，显示输出模式，通常用于 `sampleInput`。|



以下代码是针对 `echo` 操作的一组注释示例，此操作会返回未加修改的输入自变量。此操作可用于对输入参数进行日志记录（例如，作为某个序列或规则的一部分）。

```
ibmcloud fn action create echo echo.js \
    -a description 'An action which returns its input. Useful for logging input to enable debug/replay.' \
    -a parameters  '[{ "required":false, "description": "Any JSON entity" }]' \
    -a sampleInput  '{ "msg": "Five fuzzy felines"}' \
    -a sampleOutput '{ "msg": "Five fuzzy felines"}'
```
{: pre}

## Web 操作注释
{: #annotations-specific-to-web-actions}

以下 Web 操作注释必须显式设置为 `true` 才能启用 API 交互性。

|注释|描述|
| --- | --- | 
|`web-export`|应用于某个操作时，该操作会变为 [Web 操作](/docs/openwhisk?topic=cloud-functions-actions_web)。该操作无需认证即可供 REST 调用访问，以便用户可以通过浏览器访问操作。Web 操作的所有者会产生在系统中运行这些操作的开销。换言之，操作的所有者也拥有激活记录。|
|`final`|应用于某个操作时，在调用期间提供的参数无法覆盖先前定义的任何操作参数。|
|`raw-http`|应用于具有 `web-export` 注释的操作时，HTTP 请求查询和主体参数将作为保留的属性传递到该操作。|
|`web-custom-options`|支持 Web 操作响应具有定制头的 OPTIONS 请求。否则，将应用[缺省 CORS 响应](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_options)。|
|`require-whisk-auth`|Web 操作只能由提供相应认证凭证的请求调用。设置为布尔值时，此注释可控制是否对请求的基本认证值进行认证。值为 `true` 将认证凭证，值为 `false` 将调用此操作，而不进行任何认证。设置为整数或字符串时，此值必须与请求的 `X-Require-Whisk-Auth` 头值相匹配。|

## 包注释
{: #annotations_package}

|注释|描述|
| --- | --- |
|`description`|包的描述。|
|`parameters`|一个数组，用于描述作用域限定为该包的参数。|

## 参数注释
{: #annotations_parameter}

|注释|描述|
| --- | --- |
|`name`|参数的名称。|
|`description`|对参数的精简描述。|
|`doclink`|一个链接，指向参数的进一步文档（对于 OAuth 令牌非常有用）。|
|`required`|对于必需参数为 true，对于可选参数为 false。|
|`bindTime`|如果参数在绑定包时指定，那么为 true。|
|`type`|参数的类型，为 `password` 或 `array`（但可以有更广的用法）。|

## 激活注释
{: #annotations_activation}

可以使用以下注释来记录激活记录：

|注释|描述|
| --- | --- |
|`path`|生成激活的操作的标准路径名。如果此激活是包绑定中操作的结果，那么路径会引用父包。|
|`kind`|所执行操作的类型，以及其中一种受支持的 {{site.data.keyword.openwhisk_short}} 运行时类型。|
|`limits`|制约此激活的时间、内存和日志限制。|

对于与序列相关的激活，系统将生成以下注释：

|注释|描述|
| --- | --- |
|`topmost`|仅对于最外层的序列操作，才会出现此注释。|
|`causedBy`|仅对于序列中包含的操作，才会出现此注释。|
|`waitTime`|在内部 {{site.data.keyword.openwhisk_short}} 系统中等待的时间。这大约是在收到激活请求与调用者为操作供应容器之间所用的时间。此值仅对非序列相关的激活出现。对于序列，此信息可以从 `topmost` 序列激活记录来派生。|
|`initTime`|初始化函数所用时间。如果出现此值，说明操作需要初始化并表示冷启动。热激活会跳过初始化，在这种情况下，不会生成注释。|

以下示例显示了可能会在激活记录中显示的这些注释。

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




