---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-20"

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


# 建立動作
{: #actions}

建立動作，這是傳回 JSON 物件的最上層函數。可以將動作合併到一個套件中，以簡化對動作的管理。
{: shortdesc}

開始之前：
若要建立動作，原始碼必須符合特定需求。例如，如果要透過包含在多個檔案中的程式碼建立動作，請在建立動作之前，先將程式碼包裝成單一檔案。如需每個運行環境的需求詳細資料，請參閱[為無伺服器準備應用程式程式碼](/docs/openwhisk?topic=cloud-functions-prep)。


## 透過 CLI 建立動作
{: #actions_cli}

1. 建立動作。
    
  ```
  ibmcloud fn action create ACTION_NAME APP_FILE --kind RUNTIME
  ```
  {: pre}

  範例：
  ```
  ibmcloud fn action create hello hello.js --kind nodejs:10
  ```
  {: pre}

  輸出範例：

  ```
  ok: created action hello
  ```
  {: screen}

  提示：
  - 為了節省成本，您可以設定限制。
      - 若要為記憶體用量設定限制，請在 create 指令中包含 `--memory VALUE`，其中值以 MB 為單位。
      - 若要設定逾時，請在 create 指令中包含 `--timeout VALUE`，其中值以毫秒為單位。
  - 如果已將程式碼包裝成 Docker 映像檔，請在 create 指令中包含 `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG`，而不是包含應用程式的本端路徑和 --kind 旗標。儘可能不要使用 `latest` 標籤，以便妥善管理您的映像檔。使用 `latest` 標籤時,會使用具有該標籤的映像檔，這可能不一定是最近建立的映像檔。  
      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
  


2. 驗證動作是否在動作清單中。

  ```
  ibmcloud fn action list
  ```
  {: pre}

  輸出範例：

  ```
  actions
  hello       private
  ```
  {: screen}


## 在運作時更新應用程式或運行環境
{: #actions_update}

每當需要更新應用程式中的程式碼或移轉到更高版本的運行環境，都可以執行 update 指令。例如，由於 Node.js V8 處於維護模式，因此您可能希望將運行環境切換為 Node.js 10。

移轉到新的運行環境版本時，可能需要變更應用程式中的程式碼以符合新的運行環境版本。在大部分情況下，運行環境版本會相容。
{: tip}

1. 本端更新應用程式。

2. 如果已將應用程式包裝成 Docker 映像檔，請將最新映像檔上傳到 Docker Hub。這可讓系統在下次執行您動作的程式碼時取回新的 Docker 映像檔。如果有使用前版 Docker 映像檔的執行中容器，任何新呼叫都將繼續使用該映像檔。必須執行 update 指令，這樣新呼叫才能開始使用新映像檔。

3. 更新動作並包含應用程式或 Docker 映像檔的本端路徑。
    ```
    ibmcloud fn action update ACTION_NAME APP_FILE --kind RUNTIME
    ```
    {: pre}

    範例：
    ```
    ibmcloud fn action update hello hello.js --kind nodejs:10
    ```
    {: pre}

    輸出範例：

    ```
    ok: updated action hello
    ```
    {: screen}

    如果已將程式碼包裝成 Docker 映像檔，請在 create 指令中包含 `--docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG`，而不是包含本端應用程式的路徑和 --kind 旗標。儘可能不要使用 `latest` 標籤，以便妥善管理您的映像檔。使用 `latest` 標籤時,會使用具有該標籤的映像檔，這可能不一定是最近建立的映像檔。
      ```
      ibmcloud fn action create hello --docker <DOCKER_HUB_USERNAME>/<DOCKER_HUB_IMAGE>:TAG
      ```
      {: pre}
    {: tip}
      


## 將參數連結到動作
{: #actions_params}

可以將參數連結到動作來設定預設參數。連結的參數會充當動作的預設參數，除非在呼叫時提供了參數。
{: shortdesc}

開始之前，請[建立動作](#actions_cli)。

若要連結參數，請執行下列動作：

1. 更新動作並將預設參數連結到該動作。

    ```
    ibmcloud fn action update ACTION_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    範例：
    ```
    ibmcloud fn action update MyApp --param name World
    ```
    {: pre}

    輸出範例：
    ```
    ok: updated action MyApp
    ```
    {: screen}

    如果您修改非服務認證參數，則執行含新參數的 `action update` 指令，會移除目前已存在但未指定在 `action update` 指令中的所有參數。例如，如果您執行 `action update -p key1 new-value -p key2 new-value`，但省略任何其他已設定的參數，則在更新動作之後，這些參數就不再存在。此外，還會移除已連結至動作的任何服務。如果已連結服務，您必須再次[將服務連結至動作](/docs/openwhisk?topic=cloud-functions-services)。
    {: tip}

3. 驗證參數是否已連結至動作。
    ```
    ibmcloud fn action get MyApp parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got action MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

選用：若要清除先前連結的參數，請在不包含任何參數的情況下更新動作。
```
ibmcloud fn action update ACTION_NAME APP_FILE
```
{: pre}


## 將動作鏈結在一起作為動作序列
{: #actions_seq}

您可以建立一個動作，將一連串的動作鏈結在一起。將某個動作的結果當作下一個動作的引數來傳遞。
{: shortdesc}

```
ibmcloud fn action create SEQUENCE_NAME --sequence ACTION_1,ACTION_2
```
{: pre}

序列中動作之間所傳遞的參數十分明確，但預設參數除外。因此，傳遞給動作序列的參數僅適用於序列中的第一個動作。序列中第一個動作的結果會變成序列中第二個動作的輸入 JSON 物件，以此類推。此物件不包括一開始傳遞給序列的任何參數，除非第一個動作將它們包括在結果中。動作的輸入參數會與動作的預設參數合併，而前者的優先順序較高，並且會置換任何相符的預設參數。

序列不會具有與序列中每個動作之逾時分開的整體逾時。因為序列是作業管線，所以一個動作失敗將會中斷管線。如果有一個動作逾時，則整個序列會因為該失敗而結束。

接下來，建立規則或呼叫動作時，請使用序列的名稱。




## 包裝動作
{: #actions_pkgs}

在 {{site.data.keyword.openwhisk}} 中，可以使用套件將一組相關動作和資訊來源組合在一起，然後與其他人共用。套件還容許在套件中的所有實體之間共用參數。
{: shortdesc}

套件可以包含*動作* 及*資訊來源*。
- 動作是在 {{site.data.keyword.openwhisk_short}} 上執行的程式碼片段。例如，{{site.data.keyword.cloudant}} 套件包括在 {{site.data.keyword.cloudant_short_notm}} 資料庫中讀取及寫入記錄的動作。
- 資訊來源是用來配置外部事件來源，以發動觸發程式事件。例如，「警示」套件包含可依指定的頻率發動觸發程式的資訊來源。



1. 建立套件。
  ```
  ibmcloud fn package create PACKAGE_NAME
  ```
  {: pre}

2. 取得套件的摘要。請注意，套件是空的。
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  輸出範例：
  ```
  package /myNamespace/custom
  ```
  {: screen}

4. 建立動作並將其包含在套件中。在套件中建立動作，需要您在動作名稱前面加上套件名稱。不容許套件巢狀。套件只能包含動作，而且不能包含另一個套件。

  ```
  ibmcloud fn package create PACKAGE_NAME/ACTION_NAME APP_FILE
  ```
  {: pre}

5. 取得套件的摘要。
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  輸出範例：
  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}




## 將參數連結至套件
{: #actions_pkgs_params}

您可以設定套件中所有動作所繼承的套件層次參數，來設定套件中所有實體的預設參數。

連結參數作為套件中動作的預設參數，除非：

- 動作本身具有預設參數
- 動作的參數是在呼叫時間提供

開始之前，請建立至少包含一個動作的套件。

1. 更新套件並將預設參數連結到該套件。

    ```
    ibmcloud fn package update PACKAGE_NAME --param PARAMETER_NAME PARAMETER_VALUE
    ```
    {: pre}

    範例：
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

    輸出範例：
    ```
    ok: updated package MyApp
    ```
    {: screen}

    如果修改了非服務認證參數，則執行具有新參數的 `package update` 指令將移除現行存在但未在 `package update` 指令中指定的任何參數。例如，如果您執行 `package update -p key1 new-value -p key2 new-value`，但省略任何其他已設定的參數，則在更新套件之後，這些參數就不再存在。還會移除已連結至套件的任何服務，因此在更新其他參數後，必須再次[將服務連結至套件](/docs/openwhisk?topic=cloud-functions-services)。
  {: tip}

3. 驗證參數是否已連結至套件。
    ```
    ibmcloud fn package get MyApp parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got package MyApp, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}

4. 驗證參數是否已由套件繼承。
    ```
    ibmcloud fn package get MyApp/MyAction parameters
    ```
    {: pre}

    輸出範例：
    ```
    ok: got package MyApp/MyAction, displaying field parameters

    [
        {
            "key": "name",
            "value": "World"
        }
    ]
    ```
    {: screen}



## 共用動作套件
{: #actions_pkgs_share}

在除錯及測試包含套件的動作及資訊來源之後，即可與所有 {{site.data.keyword.openwhisk_short}} 使用者共用套件。透過共用套件，使用者可以連結套件，呼叫套件中的動作，以及編寫 {{site.data.keyword.openwhisk_short}} 規則和序列動作。共用套件內的動作及資訊來源為_公用_。如果套件為專用，則其所有內容也會是專用。
{: shortdesc}

1. 與所有使用者共用套件：
  ```
  ibmcloud fn package update PACKAGE_NAME --shared yes
  ```
  {: pre}

2. 顯示套件的 `publish` 內容，以驗證它現在為 true。
  ```
  ibmcloud fn package get PACKAGE_NAME publish
  ```
  {: pre}

  輸出範例：
  ```
  ok: got package PACKAGE_NAME, displaying field publish

  true
  ```
  {: screen}

3. 取得套件的說明，以向他人提供套件的完整名稱，以便可以連結套件或呼叫套件中的動作。完整名稱包含名稱空間，在此範例中為 `myNamespace` 名稱空間。
  ```
  ibmcloud fn package get --summary PACKAGE_NAME
  ```
  {: pre}

  輸出範例：
  ```
  package /NAMESPACE/PACKAGE_NAME
   action /NAMESPACE/PACKAGE_NAME/ACTION_NAME
  ```
  {: screen}



## 動作的環境變數
{: #actions_envvars}

動作環境包含多個特定於執行中動作的環境變數。這些內容可供系統中所有受支援運行環境進行存取。這些內容容許動作以程式設計方式透過 REST API 來使用資產，或者設定在動作即將耗盡其分配的時間預算時發出的內部警示。
{: shortdesc}

| 內容 |說明|
| -------- | ----------- |
| `__OW_API_HOST` |執行此動作的部署的 API 主機。|
| `__OW_API_KEY` | 呼叫動作之主題的 API 金鑰。此金鑰可能是受限 API 金鑰，除非明確要求，否則此金鑰不會存在。請參閱[註釋](/docs/openwhisk?topic=cloud-functions-annotations)。|
| `__OW_NAMESPACE` | 啟動的名稱空間。此名稱空間可能與動作的名稱空間不同。|
| `__OW_ACTION_NAME` | 執行中動作的完整名稱。|
| `__OW_ACTIVATION_ID` | 此執行中動作實例的啟動 ID。|
| `__OW_DEADLINE` | 此動作將耗用其整個有效天數配額的大致時間（以新紀元毫秒為單位）。|

### 在應用程式中包含動作環境變數
{: #actions_envvars_app}

若要檢視某個動作的值，請在應用程式程式碼中包含這些值的顯示，並將其輸出到結果中。

針對 Python 的範例：
```python
def main(dict):
  import os
  __OW_ACTION_NAME = os.environ.get('__OW_ACTION_NAME')
  result = {'__OW_ACTION_NAME':__OW_ACTION_NAME}
  return result

```
{: codeblock}

更新並啟動動作中的程式碼後，結果會包含該動作的完整名稱。
```bash
"response": {
    "status": "success",
        "statusCode": 0,
        "success": true,
        "result": {
            "__OW_ACTION_NAME": "/NAMESPACE/PACKAGE_NAME/ACTION_NAME"
            }

```
