---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

keywords: functions, cognitive,

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


# {{site.data.keyword.visualrecognitionshort}}
{: #pkg_visual_recognition}

O serviço instalável do {{site.data.keyword.visualrecognitionfull}} usa algoritmos de deep learning para identificar cenas, objetos e rostos em imagens que você faz upload para o serviço. É possível criar e treinar um classificador customizado para identificar sujeitos de acordo com suas necessidades.
{:shortdesc}

O pacote do  {{site.data.keyword.visualrecognitionshort}}  contém as entidades a seguir. Para obter mais informações, consulte a referência da API do {{site.data.keyword.visualrecognitionshort}} clicando no nome da entidade.

| Entity | Tipo | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`visual-recognition-v3`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html){: external} | Pacote | `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`  | Trabalhe com o serviço  {{site.data.keyword.visualrecognitionshort}} . |
| [`classify`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#classify){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `accept_language`, `url`, `threshold`, `owners`, `classifier_ids`, `images_file_content_type`  | Classificar imagens. |
| [`detect-faces`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#detect-faces){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `images_file`, `url`, `images_file_content_type`  | Detectar faces em imagens. |
| [`create-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#create-classifier){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`,   `name`, `classname_positive_examples`, `negative_examples` | Crie um classificador. |
| [`delete-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-classifier){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`  | Excluir um classificador. |
| [`get-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-classifier){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`  | Recuperar detalhes do classificador. |
| [`list-classifiers`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#list-classifiers){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `verbose`  | Recupere uma lista de classificadores. |
| [`update-classifier`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#update-classifier){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`, `classname_positive_examples`, `negative_examples`  | Atualizar um classificador. |
| [`get-core-ml-model`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#get-core-ml-model){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `classifier_id`  | Recuperar um modelo ML de núcleo de um classificador. |
| [`delete-user-data`](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#delete-user-data){: external} | Ação |  `username`, `password`, `iam_access_token`, `iam_apikey`, `iam_url`, `headers`, `headers[X-Watson-Learning-Opt-Out]`, `url`, `customer_id`  | Exclua os dados rotulados. |

## Criando uma instância de serviço do  {{site.data.keyword.visualrecognitionshort}}
{: #service_instance_recognition}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.visualrecognitionshort}}.
{: shortdesc}

1. [Crie uma instância de serviço do {{site.data.keyword.visualrecognitionshort}}](https://cloud.ibm.com/catalog/services/watson_vision_combined){: external}.
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.visualrecognitionshort}}
{: #install_recognition}

Depois de ter uma instância de serviço do {{site.data.keyword.visualrecognitionshort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.visualrecognitionshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #visualrecognition_cli}

**Antes de iniciar** [Instale o plug-in do {{site.data.keyword.openwhisk_short}} para a CLI do {{site.data.keyword.cloud_notm}}](/docs/openwhisk?topic=cloud-functions-cli_install).

Para instalar o pacote do {{site.data.keyword.visualrecognitionshort}}, execute os comandos a seguir.

1. Clone o repo do pacote do  {{site.data.keyword.visualrecognitionshort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    ibmcloud fn deploy -m openwhisk-sdk/packages/visual-recognition-v3/manifest.yaml
    ```
    {: pre}

3. Verifique se o pacote foi incluído em sua lista de pacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    **Saída**
    ```
    packages
    /myOrg_mySpace/visual-recognition-v3                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.visualrecognitionshort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind watson-vision-combinados visual-Reconhecer-v3
    ```
    {: pre}

    **Saída de exemplo**
    ```
    Credentials 'Credentials-1' from 'watson-vision-combined' service instance 'Watson Visual Recognition' bound to 'visual-recognition-v3'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.visualrecognitionshort}}.
    ```
    ibmcloud fn package get visual-UNK-v3 parameters
    ```
    {: pre}

    **Saída de exemplo**
    ```
    ok: got package visual-recognition-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "watson-vision-combinados": {
            "apikey": "AA1Aa111AAA1aaAA1AAAAaaAaaa1AAAA1AaAA1",
            "credentials": "Auto-generated service credentials",
            "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:ibmcloud:public:language-translator:us-south:a/a11a1a11aa111a111aa1aa1111a1a111:1111a11-aaa1-11a1-111a-1a1111a11111::",
            "iam_apikey_name": "auto-generated-apikey-aa1a1a1-aa1a-11a1-a1aa-aa11aa1a11a1",
            "iam_role_crn": "crn:v1:ibmcloud:public:iam::::serviceRole:Manager",
            "iam_serviceid_crn": "crn:v1:ibmcloud:public:iam-identity::a/a11a1a11aa111a111aa1aa1111a1a111::serviceid:ServiceId-11a1111-11a1-1111-1111-aaa11a11a11a",
            "instance": "Watson Visual Recognition-g2",
            "url": "https://gateway.watsonplatform.net/visual-recognition/api"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #visualrecognition_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar](https://cloud.ibm.com/openwhisk/create){: external}.

2. Usando as listas **Organização do Cloud Foundry** e **Espaço do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes **Watson**.

5. Clique no pacote **Reconhecimento visual**.

5. Clique em  ** Instalar **.

6. Depois que o pacote estiver instalado, você será redirecionado para a página de ações e poderá procurar seu novo pacote, que é denominado **visual-recognition-v3**.

7. Para usar as ações no Pacote **visual-recognition-v3**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 4 e 5 nas [instruções da CLI](#visualrecognition_cli).
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU.
   
  Deve-se concluir as etapas a seguir para cada ação que você deseja usar.
  {: note}

    1. Clique em uma ação por meio do Pacote **visual-recognition-v3** que você deseja usar. A página de detalhes para essa ação é aberta.
    2. Na navegação à esquerda, clique na seção **Parâmetros**.
    3. Insira um novo **parâmetro**. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.visualrecognitionshort}}
{: #usage_recognition}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke visual-recognition-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas as ações requerem um parâmetro de versão no formato AAAA-MM-DD. Quando a API mudar de uma maneira inversa de forma incompatível, uma nova data da versão será liberada. Consulte mais detalhes na
[Referência da API](https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/curl.html?curl#versioning){: external}.

As funções desse pacote usam a versão atual do Visual Recognition, `2018-03-19`. Experimente a ação  ` list-classificfiers ` .
```
ibmcloud fn action invoke visual-recognition-v3/list-classifiers -b -p version 2018-03-19
```
{: pre}


