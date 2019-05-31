---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-10"

keywords: iam, access managment, roles, service roles, policies, access

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
{:download: .download}


# アクセス権限の管理
{: #iam}

{{site.data.keyword.openwhisk}} は、Identity and Access Management (IAM) をサポートしています。 名前空間などのリソースに対して、IAM ポリシーを定義できるようになりました。
{: shortdesc}

</br>

## {{site.data.keyword.openwhisk_short}} への IAM 役割のマッピング
{: #user-roles}

{{site.data.keyword.openwhisk_short}} では、名前空間が、IAM の役割とポリシーを利用してアクセス管理を実施できる {{site.data.keyword.Bluemix_notm}} リソースと見なされます。名前空間に対して設定したポリシーはすべて、名前空間に含まれる {{site.data.keyword.openwhisk_short}} のエンティティー (アクションやトリガーなど) にも適用されます。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} では、プラットフォーム管理役割とサービス管理役割の両方を使用します。名前空間を作成できるユーザーに関するポリシーをプラットフォーム・レベルで設定するとともに、名前空間自体とのやりとりを管理するためにサービス役割を使用することができます。

IAM の主要な概念について詳しくは、[IAM の資料](/docs/iam?topic=iam-iamconcepts#iamconcepts)を確認してください。
{: tip}

</br>

### プラットフォーム管理の役割

下記の表は、プラットフォーム管理役割にマップされたアクションの詳細を示しています。 ユーザーは、プラットフォーム管理役割を使用して、プラットフォーム・レベルでサービス・リソースに対するタスクを実行できます。 例えば、サービスに対するユーザー・アクセス権限の割り当て、サービス ID の作成または削除、インスタンスの作成、アプリケーションへのインスタンスのバインドなどを実行できます。
{: shortdesc}

<table>
  <thead>
    <tr>
      <th>プラットフォームの役割</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>管理者</td>
      <td>ユーザーは名前空間を作成できます。</td>
    </tr>
  </tbody>
</table>

サービスを操作するには、プラットフォーム管理における管理者役割が必要です。 役割について詳しくは、[プラットフォーム管理の役割](/docs/iam?topic=iam-userroles)を確認してください。

</br>

### サービス固有の役割
{: #service_specific_roles}

サービス固有の役割により、特定のサービス内における、アクセス・ポリシーの適用範囲が決まります。 {{site.data.keyword.openwhisk_short}} の場合、それらの役割の適用によって、ユーザーがサービス使用のために行える操作 (UI へのアクセスや API 呼び出しの実行など) を制御できます。
{: shortdesc}


許可は他の許可をベースに構築されていることに留意する必要があります。 例えば、`ライター`役割で実行できる操作はすべて、`管理者`役割でも実行できます。 ただし、`管理者`役割には、その他の権限が追加されています。 各役割に付与される一般的な許可については、[サービス・アクセスの役割](/docs/iam?topic=iam-userroles)を確認してください。

各操作の実行に必要な役割については、以下の表を確認してください。

<table><caption>各役割で実行可能な操作</caption>
  <tr>
    <th style="width:150px">アクション</th>
    <th style="width:2500px">説明</th>
    <th style="width:50px">リーダー</th>
    <th style="width:50px">ライター</th>
    <th style="width:50px">管理者</th>
  </tr>
  <tr>
    <td><code>functions.namespaces.update</code></td>
    <td>名前空間を更新します。</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.delete</code></td>
    <td>名前空間を削除します。</td>
    <td></td>
    <td></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.namespaces.read</code></td>
    <td>使用可能な名前空間を表示します。</td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.create</code></td>
    <td>Functions 名前空間内にエンティティー (アクションなど) を作成します。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.update</code></td>
    <td>Functions 名前空間内のエンティティー (パッケージなど) を更新します。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.delete</code></td>
    <td>Functions 名前空間からエンティティー (トリガーなど) を削除します。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.read</code></td>
    <td>名前空間内にある使用可能なエンティティー (ルールなど) を表示します。</td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>functions.entities.activate</code></td>
    <td>名前空間内のエンティティー (アクションなど) をアクティブにします。</td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
</table>

UI でユーザー役割を割り当てる方法については、[IAM アクセス権限の管理](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)を参照してください。

</br>


## 名前空間用の IAM アクセス・ポリシーの設定
{: #set-iam}

名前空間またはその中にあるエンティティーを管理する場合は、上記の[サービス固有の役割](#service_specific_roles)を使用して他のユーザーにアクセス権限を付与できます。名前空間の作成中に、名前空間と機能ユーザー ID を表すサービス ID が作成されます。デフォルトでは、機能ユーザー ID にリーダー役割が割り当てられます。リーダーは、名前空間のエンティティーを読み取り、アクションを呼び出すことができます。トリガーがアクションを呼び出すときにはリーダー役割が使用されます。インバウンド・トラフィックを制御するために、アクションを呼び出すリーダー役割などのアクセス権限を他のユーザーに割り当てることができます。
{: shortdesc}

リソース・アクセス・ポリシーの割り当て、編集、確認、または削除の方法については、[IAM アクセス権限の管理](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)を参照してください。
{: tip}




</br>
</br>

## 名前空間から他のリソースへのアクセス
{: #namespace-access}

通常、アクションが呼び出す他の {{site.data.keyword.Bluemix_notm}} リソースとサービスには、適切な認証が必要になります。
それらのサービスが IAM 対応で IAM トークンを受け入れる場合は、名前空間の機能 ID をアウトバウンド通信に利用できます。
[IAM アクセス権限の管理](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)で説明しているように、名前空間ごとに、名前空間を表すサービス ID が作成されます。IAM ポリシー管理を使用して適切な役割を割り当てることで、このサービス ID に他のサービスおよびリソースに対するアクセス権限を付与できます。

実行時に、{{site.data.keyword.openwhisk_short}} は名前空間のサービス ID の API キーを、環境変数 `__OW_IAM_NAMESPACE_API_KEY` の値としてアクション・コードに渡します。アクション・コードでは、この API キーを使用して IAM トークンを生成します。 サポートされる大多数の {{site.data.keyword.openwhisk_short}} SDK (Cloudant、{{site.data.keyword.watson}}、{{site.data.keyword.cos_full_notm}} など) は、IAM API キーそのものを認証に使用します。REST API を使用するその他の IAM 管理のサービスまたはリソースは、IAM API キーから生成されたトークンで認証を行えます。詳しくは、[ユーザーまたはサービス ID の IAM アクセス・トークンの作成](/apidocs/iam-identity-token-api#create-an-iam-access-token-for-a-user-or-service-i)を参照してください。

API キーとトークンの組み合わせについて詳しくは、[IAM の資料](/docs/iam?topic=iam-iamapikeysforservices)を参照してください。

