---
copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: access policies, iam, roles, functions

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



# アクセス・ポリシーの設定
{: #iam}

## {{site.data.keyword.openwhisk_short}} への IAM 役割のマッピング
{: #user-roles}

{{site.data.keyword.openwhisk_short}} では、名前空間とは、IAM の役割とポリシーを利用したアクセス管理に使用できる {{site.data.keyword.cloud_notm}} リソースのことです。名前空間に対して設定したポリシーはすべて、名前空間に含まれる {{site.data.keyword.openwhisk_short}} のエンティティー (アクションやトリガーなど) にも適用されます。
{: shortdesc}

{{site.data.keyword.openwhisk_short}} では、プラットフォーム管理役割とサービス管理役割の両方を使用します。 名前空間を作成できるユーザーに関するポリシーをプラットフォーム・レベルで設定し、名前空間自体とのやりとりを管理するためにサービス役割を使用することができます。

IAM の主要な概念について詳しくは、[IAM の資料](/docs/iam?topic=iam-iamconcepts#iamconcepts)を確認してください。
{: tip}

</br>

### プラットフォーム管理の役割

下記の表は、プラットフォーム管理役割にマップされたアクションの詳細を示しています。 ユーザーは、プラットフォーム管理役割を使用して、プラットフォーム・レベルでサービス・リソースに対するタスクを実行できます。 例えば、サービスに対するユーザー・アクセス権限の割り当て、サービス ID の作成または削除、インスタンスの作成、アプリケーションへのインスタンスのバインドなどを実行できます。
{: shortdesc}

リソース・アクセス・ポリシーの割り当て、編集、確認、または削除の方法について詳しくは、[IAM アクセス権限の管理](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)を参照してください。
{: tip}

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
      <td>ユーザーは名前空間を作成できます。 サービスの作成中に `service id` 操作と `apikey lock` 操作を実行するには、管理者の役割が必要です。</td>
    </tr>
  </tbody>
</table>

サービスをプロビジョンする必要がないので、サービスを操作するために必要なプラットフォームの役割は、エディターの役割だけです。その他の役割について詳しくは、[プラットフォーム管理の役割](/docs/iam?topic=iam-userroles)を確認してください。

</br>

### サービス固有の役割
{: #service_specific_roles}

サービス固有の役割により、特定のサービス内における、アクセス・ポリシーの適用範囲が決まります。 {{site.data.keyword.openwhisk_short}} の場合、それらの役割の適用によって、ユーザーがサービス使用のために行える操作 (UI へのアクセスや API 呼び出しの実行など) を制御できます。
{: shortdesc}

許可は他の許可をベースに構築されています。例えば、`ライター`役割で実行できる操作はすべて、`管理者`役割でも実行できます。 ただし、`管理者`役割には、その他の権限が追加されています。 各役割に付与される一般的な許可については、[サービス・アクセスの役割](/docs/iam?topic=iam-userroles)を確認してください。

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

</br>

### CLI によるポリシーの設定
{: #cli-set}

IAM ベースの名前空間内のリソース (アクションなど) に IAM ベースのサービスへのアクセス権限を付与するには、そのリソースが含まれている名前空間に関する IAM アクセス・ポリシーを作成できます。

```
ibmcloud iam service-policy-create <namespace_service_ID> --roles <IAM_role1,IAM_role2> --service-name <other_service_type> --service-instance <other_service_GUID>
```
{: pre}

<table>
  <thead>
    <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> <code>ibmcloud iam service-policy-create</code> コマンドのコンポーネントの説明</th>
  </thead>
  <tbody>
    <tr>
      <td><code>&lt;namespace_service_ID&gt;</code></td>
      <td>名前空間のサービス ID。すべてのサービス ID を参照するには、<code>ibmcloud iam service-ids</code> を実行します。</td>
    </tr>
    <tr>
      <td>`--roles` <code>&lt;IAM_role&gt;</code></td>
      <td>アクションでターゲット・サービスを使用するために必要な IAM サービス・アクセス役割のタイプ。他のサービスのサポートされている役割を確認するには、<code>ibmcloud iam roles --service SERVICE_NAME</code> を実行します。詳しくは、[IAM アクセス権限](/docs/iam?topic=iam-userroles#service-access-roles)を参照してください。</td>
    </tr>
    <tr>
      <td>`--service-name` <code>&lt;other_service_type&gt;</code></td>
      <td>他の {{site.data.keyword.cloud_notm}} サービス・タイプの名前。</td>
    </tr>
    <tr>
      <td>`--service-instance` <code>&lt;other_service_GUID&gt;</code></td>
      <td>アクションに付与しようとしているアクセス権限の対象である他のサービス・インスタンスの GUID。このサービス・インスタンスの GUID を取得するには、<code>ibmcloud resource service-instance &lt;other_service_instance_name&gt;</code> を実行します。</td>
    </tr>
  </tbody>
</table>

</br>

**次のステップ**
サービス資格情報の管理について詳しくは、[Manage service credentials for serverless applications](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external} ブログを参照してください。



