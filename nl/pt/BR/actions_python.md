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


# Criando ações Python
{: #creating-python-actions}

As seções a seguir orientam você na criação e na chamada de uma ação Python única e na inclusão
de parâmetros nessa ação.

## Criando e chamando uma ação do Python
{: #openwhisk_actions_python_invoke}

Uma ação é simplesmente uma função Python de nível superior. Para criar uma ação Python:

1. Salve o código a seguir em um arquivo chamado `hello.py`.
```
  python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
      print(greeting)
    return {"greeting": greeting}
  ```
{: codeblock}
    
As ações Python sempre consomem e produzem um dicionário. O método de entrada para a ação é `main` por padrão, mas pode ser especificado para criar a ação com a CLI `wsk` usando o sinalizador `--main`.
{: note}

2. Crie uma ação  ` helloPython ` . O tipo de ação é determinado pelo uso da extensão do arquivo de origem. Para arquivos de origem `.py`, a ação é executada usando um tempo de execução do Python 2.

    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Exemplo de Saída:

    ```
    ok: created action helloPython
    ```
    {: screen}

3. Invoque a ação.

    ```
    ibmcloud fn action invoke --result helloPython --param name World
    ```
    {: pre}

    Exemplo de Saída:

    ```
      {
          "greeting": "Hello World!"
      }
    ```
    {: screen}
    
    Também é possível criar uma ação que é executada com o Python 3.x. 
    * Para o Python 3.6, use o parâmetro `--kind python:3.6` (anteriormente denominado `python-jessie:3`).
    * Para o Python 3.7, use o parâmetro `--kind python:3.7`
    
    Os dois tempos de execução contêm pacotes adicionais para serviços do IBM Cloud como {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} e {{site.data.keyword.ibmwatson_notm}}.
    
Para obter mais informações sobre os pacotes que estão incluídos nesses tempos de execução do Python 3, consulte a [referência de tempo de execução](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) do Python.

## Empacotando ações do Python em arquivos zip
{: #actions_python_zip}
{: #openwhisk_actions_python_zip}

É possível empacotar uma ação do Python e os módulos dependentes em um arquivo zip. Por exemplo, é possível criar uma ação com um módulo auxiliar chamado `helper.py`.

1. Crie um archive contendo os seus arquivos de origem. **Nota:** o arquivo de
origem que contém o ponto de entrada deve ser denominado `__main__.py`.

    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Crie a ação.

    ```bash
    ibmcloud fn action create helloPython -- kind python: 3 helloPython.zip
    ```
    {: pre}

## Empacotando ações do Python com um ambiente virtual em arquivos zip
{: #actions_python_virtualenv}
{: #openwhisk_actions_python_virtualenv}

É possível empacotar dependências do Python usando um ambiente virtual, `virtualenv`. O
ambiente virtual permite que você vincule pacotes adicionais que podem ser instalados usando
o `pip` do [
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://packaging.python.org/installing/),
por exemplo.

É possível instalar dependências, empacotá-las em um ambiente virtual e criar uma ação OpenWhisk compatível.

1. Crie um arquivo [requirements.txt
![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://pip.pypa.io/en/latest/user_guide/#requirements-files)
que contenha os módulos e as versões do `pip` a serem instalados.

2. Instale as dependências e crie um ambiente virtual. O diretório de ambiente virtual deve ser
denominado `virtualenv`. Para assegurar a compatibilidade com o contêiner de tempo de
execução do OpenWhisk, as instalações de pacote dentro de um ambiente virtual devem usar a imagem que
corresponda ao tipo.

    * Para o tipo `python:3.7`, use a imagem do Docker `ibmfunctions/action-python-v3.7`.
    * Para o tipo `python:3.6`, use a imagem do Docker `ibmfunctions/action-python-v3.6`.
    * Para o tipo `python:2`, use a imagem do Docker `openwhisk/python2action`.

   ```
   docker pull ibmfunctions/action-python-v3.7
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Empacote o diretório `virtualenv` e quaisquer arquivos Python adicionais. O arquivo
de origem que contém o ponto de entrada deve ser denominado `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Crie a ação  ` helloPython `.

    ```
    ibmcloud fn action create helloPython2 --kind python:3.7 helloPython.zip
    ```
    {: pre}

Inclua somente os módulos que não fazem parte do ambiente de tempo de execução selecionado no
`requirements.txt`. Isso ajuda a manter o `virtualenv` em um tamanho
mínimo.
{: tip}


