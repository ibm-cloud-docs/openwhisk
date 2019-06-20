---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-15"

keywords: actions, serverless, javascript, node, node.js

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# 测试无服务器应用程序
{: #test}

测试通过 CLI 创建的每个实体，以验证无服务器应用程序是否在正常运行，或者对当前可能发生问题的位置进行故障诊断。
{: shortdesc}


## 测试操作
{: #test-js}

可以通过运行 `invoke` 命令来测试操作。测试操作时可以使用或不使用参数。
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

Hello world 示例：
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

输出：
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### 测试存储在 JSON 文件中的参数
{: #test_json_file}

可以传递包含 JSON 格式参数的文件。
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

示例输出：
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
  ```
{: screen}


### 测试以 JSON 格式输入的参数
{: #test_json}

可以通过调用来传递 JSON 格式的参数。
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

示例输出：
```
{
    "payload": "Hello, Dorothy from Kansas"
  }
  ```
{: screen}


### 测试阻塞性操作
{: #test-block}

操作的调用可以是阻塞性或非阻塞性的。缺省情况下，调用是非阻塞性的。如果不是立即需要操作结果，请使用非阻塞性调用。
{: shortdesc}

阻塞性调用使用请求/响应样式，并等待激活结果可用。等待时间段为 60 秒或操作的[时间限制值](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)（两者取较短的时间）。

在云中，通过运行阻塞性调用来运行操作：

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


示例输出：
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b

{
    "result": {
        "payload": "Hello world"
    },
    "status": "success",
    "success": true
}
```
{: screen}

此命令输出以下信息：
* 调用结果（如果在预期的等待时间段内可用）
* 如果未使用 --result 选项，那么会在结果中显示激活标识。激活标识 (`44794bd6aab74415b4e42a308d880e5b`)，可用于检索调用的日志或结果。






## 测试触发器
{: #test_triggers}

触发器可以使用键/值对的字典来触发（或激活）。有时，此字典称为事件。触发器可以由用户显式触发，也可以由外部事件源代表用户触发。
与操作一样，每次触发与规则关联的触发器都会生成一个激活标识。
{: shortdesc}

1. 触发触发器。

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    未与规则关联的触发器在触发时没有任何可视效果。由于不存在与此触发器相关联的规则，因此传递的参数不会用作任何操作的输入。

    示例输出：

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. 通过检查最新的激活记录来验证是否调用了操作。
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    示例输出：
      ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. 从上一个命令输出中获取有关激活标识的更多信息。
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    示例输出：
      ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## 测试激活持续时间
{: #test_time}

通过获取激活日志来检查完成激活所用的时间。如果持续时间太长，或者需要调整缺省超时以允许函数运行更长时间，那么可以使用超时来更新操作。
{: shortdesc}

1. 获取激活标识。

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    示例输出：
      ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. 获取激活标识的日志。

    ```
      ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
      ```
    {: pre}

    `duration` 显示时间（以毫秒为单位）。完成此激活所用时间略长于 2 秒：

    ```
      ok: got activation b066ca51e68c4d3382df2d8033265db0
      {
          ...
        "activationId": "c2b36969fbe94562b36969fbe9856215",
          "start": 1532456307768,
          "end": 1532456309838,
          "duration": 2070,
          ...
    }
    ```
    {: screen}

3. 使用超时（以毫秒为单位）更新操作。

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    示例：
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## 测试内存使用情况
{: #test_memory}

如果应用程序打包成 Docker 映像，那么可以使用 Docker 命令来检查应用程序的内存使用情况。
{: shortdesc}

1. 在本地创建用于运行 Docker 映像的容器。

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. 获取容器的列表来取得容器标识。

    ```
    docker ps
    ```
    {: pre}

3. 检查正在运行的容器的统计信息。

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. 复查容器的内存使用情况值。如果值不符合系统限制，请对脚本进行一些调整。

5. 复查完信息后，可以停止正在运行的容器。

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. 除去容器。

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}






