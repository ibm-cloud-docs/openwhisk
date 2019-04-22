---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-27"

keywords: actions, serverless, python

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


# 建立 Python 動作
{: #creating-python-actions}

下列各節會引導您建立及呼叫單一 Python 動作，以及將參數新增至該動作。

## 建立及呼叫 Python 動作
{: #openwhisk_actions_python_invoke}

動作只是最上層 Python 函數。若要建立 Python 動作，請執行下列動作：

1. 將下列程式碼儲存至稱為 `hello.py` 的檔案中。
```
python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
    return {"greeting": greeting}
```
{: codeblock}
    
Python 動作一律會使用某個字典，並產生一個字典。動作的進入方法依預設是 `main`，但可以使用 `--main` 旗標，以 `wsk` CLI 指定來建立動作。
{: note}

2. 建立 `helloPython` 動作。
    動作的類型是使用來源副檔名來決定。對於 `.py` 原始檔，動作會使用 Python 2 執行時期來執行。

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    輸出範例：

    ```
ok: created action helloPython
    ```
    {: screen}

3. 呼叫動作。
    

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    輸出範例：

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    您也可以建立使用 Python 3.x 執行的動作。 
    * 對於 Python 3.6，使用參數 `--kind python:3.6`（先前命名為 `python-jessie:3`）。
    * 對於 Python 3.7，使用參數 `--kind python:3.7`
    
    兩個運行環境都包含 IBM Cloud Services 的額外套件，例如 {{site.data.keyword.cloudant_short_notm}}、{{site.data.keyword.Db2_on_Cloud_long_notm}}、{{site.data.keyword.cos_full_notm}} 及 {{site.data.keyword.ibmwatson_notm}}。
    
如需這些 Python 3 運行環境中所含套件的相關資訊，請參閱 Python [運行環境參照](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)。

## 將 Python 動作包裝在 zip 檔案中
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

您可以將 Python 動作及相依模組包裝在 zip 檔案中。例如，您可以建立 helper 模組稱為 `helper.py` 的動作。

1. 建立包含原始檔的保存檔。**附註**：包含進入點的原始檔必須名為 `__main__.py`。
    

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. 建立動作。
    

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## 使用 zip 檔案中的虛擬環境來包裝 Python 動作
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

您可以使用虛擬環境 `virtualenv` 來包裝 Python 相依關係。虛擬環境容許您鏈結其他套件，這些套件可以使用例如 [`pip` ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://packaging.python.org/installing/) 來安裝。

您可以安裝相依關係，將它們包裝在虛擬環境中，然後建立相容的 OpenWhisk 動作。

1. 建立 [requirements.txt ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 檔，其中包含要安裝的 `pip` 模組和版本。

2. 安裝相依關係並建立虛擬環境。虛擬環境目錄必須命名為 `virtualenv`。若要確保與 OpenWhisk 執行時期容器的相容性，虛擬環境內的套件安裝必須使用對應於該類型的映像檔。

    * 對於類型 `python:3.7`，請使用 Docker 映像檔 `ibmfunctions/action-python-v3.7`。
    * 對於類型 `python:3.6`，請使用 Docker 映像檔 `ibmfunctions/action-python-v3.6`。
    * 對於類型 `python:2`，請使用 Docker 映像檔 `openwhisk/python2action`。

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. 包裝 `virtualenv` 目錄及任何其他 Python 檔案。包含進入點的原始檔必須名為 `__main__.py`。
    

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. 建立動作 `helloPython`。
    

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

只將不屬於所選取運行環境的模組新增至 `requirements.txt`。這有助於將 `virtualenv` 保持為最小。
{: tip}


