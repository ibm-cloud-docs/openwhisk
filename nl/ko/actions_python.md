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


# Python 액션 작성
{: #creating-python-actions}

다음 절에서는 단일 Python 액션의 작성 및 호출과 해당 액션에 매개변수 추가에 대해 안내합니다.

## Python 액션 작성 및 호출
{: #openwhisk_actions_python_invoke}

액션은 단순히 최상위 레벨의 Python 함수입니다. Python 액션을 작성하려면 다음을 수행하십시오.

1. `hello.py`라고 하는 파일에 다음 코드를 저장하십시오.
```
python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
      print(greeting)
    return {"greeting": greeting}
  ```
{: codeblock}
    
Python 액션에서는 항상 사전을 이용하고 사전을 생성합니다. 액션에 대한 진입 메소드는 기본적으로 `main`이지만, `--main` 플래그를 사용하여 `wsk` CLI에서 액션을 작성하도록 지정될 수 있습니다.
{: note}

2. `helloPython` 액션을 작성하십시오. 소스 파일 확장자를 사용하여 액션의 유형이 판별됩니다. `.py` 소스 파일의 경우, 액션은 Python 2 런타임을 사용하여 실행됩니다.

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    출력 예:

    ```
ok: created action helloPython
    ```
    {: screen}

3. 액션을 호출하십시오.

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    출력 예:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    Python 3.x와 함께 실행되는 액션도 작성할 수 있습니다. 
    * Python 3.6의 경우, `--kind python:3.6` 매개변수(이전 이름은 `python-jessie:3`)를 사용하십시오.
    * Python 3.7의 경우, `--kind python:3.7` 매개변수를 사용하십시오.
    
    두 런타임에는 모두 {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} 및 {{site.data.keyword.ibmwatson_notm}} 같은 IBM Cloud 서비스용 추가 패키지가 포함되어 있습니다.
    
이러한 Python 3 런타임에 포함된 패키지에 대한 자세한 정보는 Python [런타임 참조](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments)를 참조하십시오.

## zip 파일로 Python 액션 패키징
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

Python 액션 및 종속 모듈을 Zip 파일에 패키징할 수 있습니다. 예를 들어, `helper.py`라고 하는 헬퍼 모듈로 액션을 작성할 수 있습니다. 

1. 소스 파일이 포함된 아카이브를 작성하십시오. **참고**: 시작점이 포함된 소스 파일의 이름은 `__main__.py`여야 합니다.

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. 액션을 작성하십시오.

    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

## zip 파일에 가상 환경의 Python 액션 패키징
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

가상 환경 `virtualenv`를 사용하여 Python 종속 항목을 패키징할 수 있습니다. 가상 환경은 예를 들어 [`pip` ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://packaging.python.org/installing/)를 사용하여 설치될 수 있는 추가 패키지를 링크할 수 있도록 허용합니다.

종속 항목을 설치하고 가상 환경에서 이를 패키징하고 호환 가능한 OpenWhisk 액션을 작성할 수 있습니다. 

1. 설치할 `pip` 모듈과 버전이 포함된 [requirements.txt ![외부 링크 아이콘](../icons/launch-glyph.svg "외부 링크 아이콘")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) 파일을 작성하십시오.

2. 종속 항목을 설치하고 가상 환경을 작성하십시오. 가상 환경 디렉토리의 이름은 `virtualenv`여야 합니다. OpenWhisk 런타임 컨테이너와의 호환성을 보장하려면 가상 환경 내의 패키지 설치에서 해당 유형에 대응되는 이미지를 사용해야 합니다.

    * `python:3.7` 유형에는 Docker 이미지 `ibmfunctions/action-python-v3.7`을 사용하십시오.
    * `python:3.6` 유형에는 Docker 이미지 `ibmfunctions/action-python-v3.6`을 사용하십시오.
    * `python:2` 유형에는 Docker 이미지 `openwhisk/python2action`을 사용하십시오.

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. `virtualenv` 디렉토리 및 추가 Python 파일을 패키징하십시오. 시작점이 포함된 소스 파일의 이름은 `__main__.py`여야 합니다.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. `helloPython` 액션을 작성하십시오.

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

선택된 런타임 환경의 일부가 아닌 모듈만 `requirements.txt`에 추가하십시오. 그러면 `virtualenv`를 최소 크기로 유지하는 데 도움이 됩니다.
{: tip}


