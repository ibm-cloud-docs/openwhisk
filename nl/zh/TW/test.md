---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: actions, serverless, javascript, node, node.js, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}



# 測試無伺服器 APP
{: #test}

測試透過 CLI 建立的每個實體，以驗證無伺服器應用程式是否在正常運作，或者對當前可能發生問題的位置進行疑難排解。
{: shortdesc}


## 測試動作
{: #test-js}

可以透過執行 `invoke` 指令來測試動作。測試動作時可以使用或不使用參數。
{: shortdesc}

```bash
ibmcloud fn action invoke --result ACTION_NAME --param PARAMETER VALUE
```
{: pre}

**Hello world 範例**
```bash
ibmcloud fn action invoke --result myAction --param name stranger
```
{: pre}

**輸出**
```json
  {
      "greeting": "Hello stranger!"
  }
```
{: screen}



### 測試儲存在 JSON 檔案中的參數
{: #test_json_file}

可以傳遞包含 JSON 格式參數的檔案。
{: shortdesc}

```
ibmcloud fn action invoke --result ACTION_NAME --param-file JSON_FILE
```
{: pre}

**輸出範例**
```
{
    "payload": "Hello, Dorothy from Kansas"
    }
    ```
{: screen}


### 測試以 JSON 格式輸入的參數
{: #test_json}

可以透過呼叫來傳遞 JSON 格式的參數。
{: shortdesc}


```
ibmcloud fn action invoke --result ACTION_NAME -p person '{"PARAM_NAME": "PARAM_VALUE", "PARAM_NAME": "PARAM_VALUE"}'
```
{: pre}

**輸出範例**
```
{
    "payload": "Hello, Dorothy from Kansas"
    }
    ```
{: screen}


### 測試封鎖式動作
{: #test-block}

動作的呼叫可以是封鎖式或非封鎖式的。依預設，呼叫是非封鎖式的。如果不是立即需要動作結果，請使用非封鎖式呼叫。
{: shortdesc}

區塊處理呼叫會使用要求/回應樣式，並等待啟動結果可用。等待期間少於 60 秒，或是動作的[時間限制值](/docs/openwhisk?topic=cloud-functions-limits#limits_syslimits)。

執行區塊處理呼叫，在雲端執行動作。

```
ibmcloud fn action invoke --blocking ACTION_NAME
```
{: pre}


**輸出範例**
```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b{
    "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
{: screen}

此指令輸出下列資訊。
* 呼叫結果（如果在預期的等待期間內可用）
* 如果未使用 `--result` 選項，則會在結果中顯示啟動 ID。啟動 ID (`44794bd6aab74415b4e42a308d880e5b`)，可用於擷取呼叫的日誌或結果。


## 測試觸發程式
{: #test_triggers}

您可以使用鍵值組字典來發動或啟動觸發程式。這個字典有時稱為事件。使用者可以明確地發動觸發程式，或者由外部事件來源代表使用者發動觸發程式。與動作一樣，與規則相關聯的觸發程式的每一次發動都會導致啟動 ID。
{: shortdesc}

1. 發動觸發程式。

    ```
    ibmcloud fn trigger fire TRIGGER_NAME --param PARAM_NAME PARAM_VALUE --param PARAM_NAME PARAM_VALUE
    ```
    {: pre}

    與規則沒有關聯的觸發程式在發動時看不見效果。由於沒有與此觸發程式相關聯的規則，因此傳遞的參數不會用作任何動作的輸入。

    **輸出範例**

    ```
    ok: triggered TRIGGER_NAME with id fa495d1223a2408b999c3e0ca73b2677
    ```
    {: screen}

2. 透過檢查最新的啟動記錄來驗證是否呼叫了動作。
    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    **輸出範例**
    ```
    activations
    fa495d1223a2408b999c3e0ca73b2677             ACTION_NAME
    ```
    {: screen}

3. 從先前的指令輸出中，取得啟動 ID 的相關資訊。
    ```
    ibmcloud fn activation result ACTIVATION_ID
    ```
    {: pre}

    **輸出範例**
    ```
    {
       "payload": "Hello, Human from Earth"
    }
    ```
    {: screen}




## 測試啟動持續時間
{: #test_time}

透過取得啟動日誌來檢查完成啟動所用的時間。如果持續時間太長，或者需要調整預設逾時以容許函數執行更長時間，可以使用逾時來更新動作。
{: shortdesc}

1. 取得啟動 ID。

    ```
    ibmcloud fn activation list --limit 1 ACTION_NAME
    ```
    {: pre}

    輸出範例：
    ```
    activations
    b066ca51e68c4d3382df2d8033265db0             ACTION_NAME
    ```
    {: screen}

2. 取得啟動 ID 的日誌。

    ```
    ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
    ```
    {: pre}

    `duration` 顯示時間（以毫秒為單位）。完成啟動所用時間略長於 2 秒。

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

3. 使用逾時（以毫秒為單位）更新動作。

    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME --timeout VALUE
    ```
    {: pre}

    範例：
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10 --timeout 1000
    ```
    {: pre}


## 測試記憶體用量
{: #test_memory}

如果應用程式包裝成 Docker 映像檔，您可以使用 Docker 指令來檢查應用程式的記憶體用量。
{: shortdesc}

1. 在本端建立用於執行 Docker 映像檔的容器。

    ```
    docker run IMAGE_NAME
    ```
    {: pre}

2. 取得容器的清單來取得容器 ID。

    ```
    docker ps
    ```
    {: pre}

3. 檢查執行中的容器的統計資料。

    ```
    docker stats CONTAINER_ID
    ```
    {: pre}

4. 檢閱容器的記憶體用量值。如果值不符合系統限制，請調整 Script。

5. 檢閱完資訊後，可以停止執行中的容器。

    ```
    docker stop CONTAINER_ID
    ```
    {: pre}

6. 移除容器。

    ```
    docker rm CONTAINER_ID
    ```
    {: pre}








