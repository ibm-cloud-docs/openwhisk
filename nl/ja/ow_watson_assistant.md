---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} パッケージ 

{{site.data.keyword.conversationfull}} サービスは、機械学習、自然言語理解、統合ダイアログの各ツールを組み合わせて、アプリケーションとユーザーの間の対話の流れを作成します。
{: shortdesc}

{{site.data.keyword.conversationshort}} パッケージには、以下のエンティティーが含まれています。エンティティー名をクリックすると、{{site.data.keyword.conversationshort}} API 参照で追加の詳細を確認できます。

| エンティティー | タイプ | パラメーター | 説明 |
| --- | --- | --- | --- |
| [`assistant-v1`](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html) | パッケージ | username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url | {{site.data.keyword.conversationshort}} サービスを操作します。 |
| [message](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、input、alternate_intents、context、entities、intents、output、nodes_visited_details | ユーザー入力に対する応答を取得します。 |
| [create-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-workspace) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、name、description、language、intents、entities、dialog_nodes、counterexamples、metadata、learning_opt_out  | ワークスペースを作成します。 |
| [delete-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-workspace) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id  | ワークスペースを削除します。 |
| [get-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-workspace) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、export、include_audit  | ワークスペースに関する情報を取得します。 |
| [list-workspaces](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-workspaces) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、page_limit、include_count、sort、cursor、include_audit  | ワークスペースをリストします。 |
| [update-workspace](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-workspace) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、name、description、language、intents、entities、dialog_nodes、counterexamples、metadata、learning_opt_out、append  | ワークスペースを更新します。 |
| [create-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-intent) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、description、examples  | インテントを作成します。 |
| [delete-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-intent) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent  | インテントを削除します。 |
| [get-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-intent) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、export、include_audit  | インテントに関する情報を取得します。 |
| [list-intents](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-intents) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、export、page_limit、include_count、sort、cursor、include_audit  | インテントをリストします。 |
| [update-intent](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-intent) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、new_intent、new_description、new_examples  | インテントを更新します。 |
| [create-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-example) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、text  | ユーザー入力の例を作成します。 |
| [delete-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-example) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、text  | ユーザー入力の例を削除します。 |
| [get-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-example) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、text、include_audit  | ユーザー入力の例に関する情報を取得します。 |
| [list-examples](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-examples) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、page_limit、include_count、sort、cursor、include_audit  | ユーザー入力の例をリストします。 |
| [update-example](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-example) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、intent、text、new_text  | ユーザー入力の例を更新します。 |
| [create-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-counterexample) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、text  | 反例を作成します。|
| [delete-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-counterexample) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、text  | 反例を削除します。|
| [get-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-counterexample) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、text、include_audit  | 反例に関する情報を取得します。 |
| [list-counterexamples](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-counterexamples) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、page_limit、include_count、sort、cursor、include_audit  | 反例をリストします。 |
| [update-counterexample](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-counterexample) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、text、new_text  | 反例を更新します。|
| [create-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-entity) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、description、metadata、values、fuzzy_match  | エンティティーを作成します。 |
| [delete-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-entity) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity  | エンティティーを削除します。 |
| [get-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-entity) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、export、include_audit  | エンティティーに関する情報を取得します。 |
| [list-entities](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-entities) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、export、page_limit、include_count、sort、cursor、include_audit  | エンティティーをリストします。 |
| [update-entity](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-entity) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、new_entity、new_description、new_metadata、new_fuzzy_match、new_values  | エンティティーを更新します。 |
| [create-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-value) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、metadata、synonyms、patterns、value_type  | エンティティー値を追加します。 |
| [delete-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-value) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value  | エンティティー値を削除します。 |
| [get-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-value) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、export、include_audit  | エンティティー値を取得します。 |
| [list-values](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-values) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、export、page_limit、include_count、sort、cursor、include_audit  | エンティティー値をリストします。 |
| [update-value](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-value) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、new_value、new_metadata、new_type、new_synonyms、new_patterns  | エンティティー値を更新します。 |
| [create-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-synonym) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、synonym  | エンティティー値シノニムを追加します。|
| [delete-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-synonym) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、synonym  | エンティティー値シノニムを削除します。|
| [get-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-synonym) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、synonym、include_audit  | エンティティー値シノニムを取得します。|
| [list-synonyms](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-synonyms) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、page_limit、include_count、sort、cursor、include_audit  | エンティティー値シノニムをリストします。 |
| [update-synonym](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-synonym) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、entity、value、synonym、new_synonym  | エンティティー値シノニムを更新します。|
| [create-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#create-dialog-node) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、dialog_node、description、conditions、parent、previous_sibling、output、context、metadata、next_step、actions、title、node_type、event_name、variable、digress_in、digress_out、digress_out_slots  | ダイアログ・ノードを作成します。|
| [delete-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-dialog-node) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、dialog_node  | ダイアログ・ノードを削除します。|
| [get-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#get-dialog-node) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、dialog_node、include_audit  | ダイアログ・ノードを取得します。|
| [list-dialog-nodes](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-dialog-nodes) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、page_limit、include_count、sort、cursor、include_audit  | ダイアログ・ノードをリストします。 |
| [update-dialog-node](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#update-dialog-node) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、dialog_node、new_dialog_node、new_description、new_conditions、new_parent、new_previous_sibling、new_output、new_context、new_metadata、new_next_step、new_title、new_type、new_event_name、new_variable、new_actions、new_digress_in、new_digress_out、new_digress_out_slots  | ダイアログ・ノードを更新します。|
| [list-all-logs](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-all-logs) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、filter、sort、page_limit、cursor  | すべてのワークスペースのログ・イベントをリストします。 |
| [list-logs](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#list-logs) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、workspace_id、sort、filter、page_limit、cursor  | あるワークスペースのログ・イベントをリストします。 |
| [delete-user-data](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data) | アクション |  username、password、iam_access_token、iam_apikey、iam_url、headers、headers[X-Watson-Learning-Opt-Out]、url、customer_id  | ラベル付きデータを削除します。|

## {{site.data.keyword.conversationshort}} サービス・インスタンスの作成
{: #service_instance}

パッケージをインストールする前に、{{site.data.keyword.conversationshort}} サービス・インスタンスおよびサービス資格情報を作成する必要があります。

1. [{{site.data.keyword.conversationshort}} サービス・インスタンスを作成 ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/catalog/services/conversation) します。
2. サービス・インスタンスの作成時に、自動生成されたサービス資格情報も作成されます。

## {{site.data.keyword.conversationshort}} パッケージのインストール
{: #install}

{{site.data.keyword.conversationshort}} サービス・インスタンスの作成後に、{{site.data.keyword.openwhisk}} CLI を使用して、{{site.data.keyword.conversationshort}} パッケージを名前空間にインストールします。
{: shortdesc}

### {{site.data.keyword.openwhisk_short}} CLI からのインストール
{: #conversation_cli}

始める前に:
  1. [{{site.data.keyword.Bluemix_notm}} CLI 用の {{site.data.keyword.openwhisk_short}} プラグインをインストールします](bluemix_cli.html#cloudfunctions_cli)。
  2. [`wskdeploy` コマンド ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) をインストールして、ダウンロードしたバイナリーを PATH に追加します。

{{site.data.keyword.conversationshort}} パッケージをインストールするには、次のようにします。

1. {{site.data.keyword.conversationshort}} パッケージ・リポジトリーを複製します。
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. パッケージをデプロイします。
    ```
    wskdeploy -m openwhisk-sdk/packages/assistant-v1/manifest.yaml
    ```
    {: pre}

3. パッケージがパッケージ・リストに追加されたことを確認します。
    ```
    ibmcloud fn package list
    ```
    {: pre}

    出力:
    ```
    packages
    /myOrg_mySpace/assistant-v1                        private
    ```
    {: screen}

4. 作成した {{site.data.keyword.conversationshort}} インスタンスの資格情報をパッケージにバインドします。
    ```
    ibmcloud fn service bind conversation assistant-v1
    ```
    {: pre}

    出力例:
    ```
    Credentials 'Credentials-1' from 'conversation' service instance 'Watson Assistant (formerly Conversation)-8h' bound to 'assistant-v1'.
    ```
    {: screen}

5. パッケージが {{site.data.keyword.conversationshort}} サービス・インスタンス資格情報を使用して構成されていることを確認します。
    ```
    ibmcloud fn package get assistant-v1 parameters
    ```
    {: pre}

    出力例:
    ```
    ok: got package assistant-v1, displaying field parameters
    [
      {
        "key": "__bx_creds",
        "value": {
          "conversation": {
            "credentials": "Credentials-1",
            "instance": "Watson Assistant (formerly Conversation)-8h",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/assistant/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### {{site.data.keyword.openwhisk_short}} UI からのインストール
{: #conversation_ui}

1. {{site.data.keyword.openwhisk_short}} コンソールで、[「作成」ページ ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/create) に移動します。

2. **「Cloud Foundry 組織」**リストと**「Cloud Foundry スペース」**リストを使用して、{{site.data.keyword.cos_short}} パッケージのインストール先の名前空間を選択します。名前空間は、組織名とスペース名の組み合わせから形成されます。

3. **「パッケージのインストール (Install Packages)」**をクリックします。

4. **「Watson」**パッケージ・グループをクリックします。

5. **「Watson Assistant」**パッケージをクリックします。

5. **「インストール (Install)」**をクリックします。 

6. パッケージのインストール後に「アクション」ページにリダイレクトされ、**assistant-v1** という名前の新規パッケージを検索できます。

7. **assistant-v1** パッケージで「アクション」を使用するには、サービス資格情報をアクションにバインドする必要があります。
  * サービス資格情報をパッケージ内のすべてのアクションにバインドするには、上にリストされている CLI の手順のステップ 5 と 6 に従います。 
  * サービス資格情報を個々のアクションにバインドするには、UI で以下のステップを実行します。**注**: 使用するアクションごとに以下のステップを実行する必要があります。
    1. 使用する **assistant-v1** パッケージのアクションをクリックします。そのアクションの詳細ページが開きます。 
    2. 左側のナビゲーションで、**「パラメーター」**セクションをクリックします。 
    3. 新しい**パラメーター**を入力します。key には `__bx_creds` を入力します。value には、前に作成したサービス・インスタンスのサービス資格情報 JSON オブジェクトを貼り付けます。

## {{site.data.keyword.conversationshort}} パッケージの使用
{: #usage}

このパッケージのアクションを使用するには、次の形式でコマンドを実行します。

```
ibmcloud fn action invoke assistant-v1/<action_name> -b -p <param name> <param>
```
{: pre}

すべてのアクションに、YYYY-MM-DD 形式の version パラメーターが必要です。API が後方非互換の方法で変更された場合、新しいバージョン日付がリリースされます。[API 参照](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html#versioning)で詳細を確認してください。

このパッケージの機能は、Watson Assistant の現行バージョンである 2018-07-10 を使用します。`list-workspaces` アクションを試行します。
```
ibmcloud fn action invoke assistant-v1/list-workspaces -b -p version 2018-07-10
```
{: pre}
