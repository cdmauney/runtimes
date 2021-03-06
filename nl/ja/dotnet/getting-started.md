---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"
subcollection: "Dotnet"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# 入門チュートリアル
{: #getting_started}

* {: download} Hello World サンプル・アプリケーションが {{site.data.keyword.Bluemix}} に正常にデプロイされました。  入門として、このステップバイステップのガイドに従って作業してください。 または、<a class="xref" href="http://bluemix.net" target="_blank" title="(サンプル・コードのダウンロード)"><img class="hidden" src="../../images/btn_starter-code.svg" alt="アプリケーション・コードのダウンロード" />サンプル・コードをダウンロード</a>して、ご自身で探索してください。

この入門チュートリアルに従って、開発環境のセットアップ、ローカルおよび {{site.data.keyword.Bluemix}} でのアプリケーションのデプロイ、および {{site.data.keyword.Bluemix}} データベース・サービスのアプリケーションへの統合を行います。

## 始める前に
{: #prereqs}

以下が必要です。
* [{{site.data.keyword.Bluemix_notm}} アカウント](https://cloud.ibm.com/registration)
* [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli/reference/ibmcloud/download_cli.html)
* [Git ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://git-scm.com/downloads){: new_window}
* [.NET Core ダウンロード Web サイト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.microsoft.com/net/download/core) から .NET Core 2.2.2 SDK 2.2.104 をインストールします。

## ステップ 1: サンプル・アプリケーションを複製する
{: #clone}

最初に、サンプル・アプリケーション GitHub リポジトリーを複製します。
  ```
git clone https://github.com/IBM-Cloud/get-started-aspnet-core
  ```
  {: codeblock}


## ステップ 2: アプリケーションをローカルで実行する
{: #run_locally}

1. コマンド・ラインで、サンプル・アプリケーションがあるディレクトリーに移動します。

  ```
  cd get-started-aspnet-core/src/GetStartedDotnet
  ```
  {: codeblock}

1. 次のコマンドを実行し、アプリケーションをローカルで実行します。

  ```
dotnet restore
  ```
  {: codeblock}

  ```
dotnet run
  ```
  {: codeblock}

1. http://localhost:5000/ でアプリケーションを表示します。

## ステップ 3: デプロイメントのためにアプリケーションを準備する
{: #prepare}

{{site.data.keyword.Bluemix_notm}} にデプロイするには、manifest.yml ファイルをセットアップすると役に立つことがあります。 manifest.yml には、アプリケーションに関する基本的な情報 (名前、各インスタンス用に割り振るメモリー量、経路など) が含まれます。 `get-started-dotnet` ディレクトリーにサンプル manifest.yml ファイルが用意されています。

manifest.yml ファイルを開き、`name` を `GetStartedDotnet` からご使用のアプリケーション名 (<var class="keyword varname" data-hd-keyref="app_name">app_name</var>) に変更します。
{: download}

  ```
 applications:
 - name: GetStartedDotnet
   random-route: true
   memory: 512M
  ```
  {: codeblock}

この manifest.yml ファイル内の **`random-route: true`** は、アプリケーション用にランダムな経路を生成して、経路が他と衝突するのを回避します。  任意のホスト名を指定して、**`random-route: true`** を **`host: myChosenHostName`** に置き換えることも選択できます。
{: tip}

## ステップ 4: アプリケーションをデプロイする
{: #deploy}

{{site.data.keyword.Bluemix_notm}} CLI を使用してアプリケーションをデプロイできます。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインし、API エンドポイントを選択します。
  ```
ibmcloud login
  ```
  {: codeblock}

  フェデレーテッド・ユーザー ID をお持ちの場合は、代わりに次のコマンドを使用して、シングル・サインオン ID でログインしてください。 詳しくは、[『フェデレーテッド ID を使用したログイン』](/docs/cli/login_federated_id.html)を参照してください。
 ```
ibmcloud login --sso
  ```
  {: codeblock}

1. Cloud Foundry の組織およびスペースをターゲットにします。
  ```
ibmcloud target --cf
  ```
  {: codeblock}

  組織およびスペースがセットアップされていない場合は、[組織およびスペースの追加](/docs/account/orgs_spaces.html)を参照してください。
  {: tip}

1. **アプリケーションのルート・ディレクトリー `get-started-aspnet-core` で作業していることを確認し**、アプリケーションを {{site.data.keyword.Bluemix_notm}} にプッシュします。
  ```
ibmcloud cf push
  ```
  {: codeblock}

  これには時間がかかることがあります。 デプロイメント・プロセスでエラーが発生した場合、`ibmcloud cf logs <Your-App-Name> --recent` コマンドを使用してトラブルシューティングすることができます。

デプロイメントが完了すると、アプリケーションが実行中であることを示すメッセージが表示されます。  push コマンドの出力にリストされている URL でアプリケーションを表示します。  また、次のコマンドを発行して、アプリケーションの状況と URL を確認することもできます。
  ```
ibmcloud cf apps
  ```
  {: codeblock}

また、{{site.data.keyword.Bluemix_notm}} [リソース・リスト](https://cloud.ibm.com/resources)にアクセスしてアプリケーションを表示することもできます。

## ステップ 5: データベースを追加する
{: #add_database}

次に、{{site.data.keyword.cloudant_short_notm}} NoSQL データベースをこのアプリケーションに追加して、ローカルおよび {{site.data.keyword.Bluemix_notm}} 上で実行できるようにアプリケーションをセットアップします。

1. ブラウザーで {{site.data.keyword.Bluemix_notm}} にログインし、ダッシュボードに移動します。 **「リソースの作成」**を選択します。
1. 検索して **{{site.data.keyword.cloudant_short_notm}}** を見つけて、このサービスを選択します。
1. **「使用可能な認証方式 (Available authentication methods)」**で、**「レガシー資格情報と IAM の両方を使用する (Use both legacy credentials and IAM)」**を選択します。 他のフィールドについては、デフォルト設定のままでかまいません。 **「作成」**をクリックしてサービスを作成します。
1. ナビゲーションで、**「接続」**に移動して、**「接続の作成」**をクリックします。 アプリケーションを選択し、**「接続」**をクリックします。
1. デフォルト値を使用して、**「アプリの接続および再ステージ」**をクリックしてデータベースをアプリケーションに接続します。 プロンプトが表示されたら、**「再ステージ」**をクリックします。

   {{site.data.keyword.Bluemix_notm}} はアプリケーションを再始動し、`VCAP_SERVICES` 環境変数を使用してデータベース資格情報をアプリケーションに提供します。 アプリケーションに対してこの環境変数が使用可能なのは、アプリケーションが {{site.data.keyword.Bluemix_notm}} で実行されている場合のみです。

環境変数を使用すると、デプロイメント設定をソース・コードと分離することができます。 例えば、データベース・パスワードをハードコーディングする代わりに、それを環境変数に保管して、ソース・コードではその環境変数を参照するようにできます。
{: tip}

## ステップ 6: ローカルでデータベースを使用する
{: #use_database}

次に、このデータベースを指すようにローカル・コードを更新します。 サービスの資格情報を JSON ファイルに保管します。 このファイルは、アプリケーションがローカルで実行されている場合にのみ使用されます。 {{site.data.keyword.Bluemix_notm}} で実行されているときには、資格情報は `VCAP_SERVICES` 環境変数から読み取られます。

1. `src/GetStartedDotnet` ディレクトリー内に `vcap-local.json` ファイルを作成します。

1. 以下の JSON オブジェクトをコピーして `vcap-local.json` ファイルに貼り付け、変更を保存します。

   ```json
   {
     "services": {
       "cloudantNoSQLDB": [
        {
           "credentials": {
             "url":"CLOUDANT_DATABASE_URL"
           },
          "label": "cloudantNoSQLDB"
        }
       ]
     }
   }
   ```
   {: codeblock}

1. {{site.data.keyword.Bluemix_notm}} [リソース・リスト](https://cloud.ibm.com/resources)にアプリケーションが含まれているのを確認します。 アプリケーションの「サービス詳細」ページで、サイドバーの**「接続」**をクリックします。 {{site.data.keyword.cloudant_short_notm}} メニュー・アイコン (**&hellip;**) をクリックし、**「資格情報の表示」**を選択します。

1. 資格情報から `url` 値のみをコピーして `vcap-local.json` ファイルの `url` フィールドに貼り付けて、`CLOUDANT_DATABASE_URL` を置き換えます。

1. `get-started-aspnet-core/src/GetStartedDotnet` ディレクトリーから以下のコマンドを実行してアプリケーションを再始動します。

   ```
   dotnet run
   ```
   {: codeblock}

1. http://localhost:5000/ でブラウザー・ビューを最新表示します。 アプリケーションに入力するすべての名前がデータベースに追加されるようになります。

ローカル・アプリケーションと {{site.data.keyword.Bluemix_notm}} アプリケーションはデータベースを共有します。  `ibmcloud cf push` コマンドの出力にリストされている URL で {{site.data.keyword.Bluemix_notm}} アプリを表示してください。  いずれかのアプリケーションから追加した名前は、ブラウザーを最新表示すると両方に表示されます。

予期しない課金が発生しないように、アプリケーションを稼働中にしておく必要がない場合は停止することを忘れないでください。
{: tip}

## 次のステップ

* [チュートリアル](/docs/tutorials/index.html)
* [サンプル ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://ibm-cloud.github.io){: new_window}
* [Architecture Center ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/garage/category/architectures){: new_window}
