---
title: macOS向けにCouchDBの初期設定手順を説明するよ
emoji: 🛏️
type: tech
topics:
  - CouchDB
  - 分散同期型データベース
  - 初期設定
published: true
publication_name: levtech
---

# これはなに
こんにちは、レバテック開発部のもりたです。
今回はMacOS向けにHomebrewを使ったCouchDBのインストール/初期設定手順について簡単にまとめます。簡単なんですけど、[公式ドキュメント](https://docs.couchdb.org/en/stable/index.html)だと分かりにくい箇所があったり、インターネットに日本語記事がないのでね、まとめますね。

:::details 構成と前提
## 構成
- CouchDBとは
- インストール手順
## 前提
- macOS Version: Sequoia 15.xでテスト済み
- Homebrewがインストール済み
- 管理者権限でのコマンド実行が可能
:::
# CouchDBとは
- 基本的な特徴
	- IDのみ持っておりスキーマレス。好きなデータをどんどん入れられる
		- MongoDBとかと同じ
	- 追記型ストレージモデルであり、データの破損がだいぶ少ない
	- CouchDB/PouchDBという組み合わせ
		- ブラウザ上で動くPouchDBと組み合わせることで分散同期型データベースとして動かせる
- 小ネタ
	- キャッチフレーズは「リラックス」。Webインターフェースが用意されておりそれがFauxton。これは元々futonという名前だったが、2016年にReact.jsで書き直された時に名前が変わったらしい。Fauxは偽物という意味

# インストール手順
## 0. 概要
このガイドでは、macOSでApache CouchDBをHomebrewを使ってインストールし、Fauxtonでのverify機能まで正常に動作させる手順を説明します。

:::details Homebrewがいいの？
### Homebrewがいいの？
公式サイトからCouchDB.appをダウンロードするよりも、Homebrewでのインストールを推奨します。なんでかというと、公式サイトからのダウンロードでやったら、わたしの環境では立ち上がらなかったからです。
:::

## インストール手順

### 1. CouchDBのインストール
```bash
brew install couchdb
```

### 2. 管理者アカウントの設定
CouchDB 3.x系では、起動前に管理者アカウントの設定が必須です。

#### 設定ファイルの編集
```bash
vim /usr/local/etc/local.ini
```

#### 管理者アカウントの追加
ファイルに以下の内容を追加：
```ini
[admins]
admin = yourpassword
```

**重要:** 行頭にセミコロン（`;`）がある場合はコメントアウトなので削除してください。
### 3. CouchDBの起動

#### 方法1: サービスとして起動（推奨）
```bash
brew services start couchdb
```

#### 方法2: 手動起動（デバッグ用）
```bash
/usr/local/opt/couchdb/bin/couchdb
```

### 4. 動作確認
#### コマンドラインでの確認
```bash
curl http://localhost:5984/
```

正常な場合、以下のようなJSONが返されます：
```json
{
  "couchdb": "Welcome",
  "version": "3.5.0",
  "git_sha": "...",
  "uuid": "...",
  "features": [...],
  "vendor": {
    "name": "The Apache Software Foundation"
  }
}
```

#### Fauxtonでの確認
ブラウザで以下のURLにアクセス：
```
http://localhost:5984/_utils/
```

### 5. Fauxtonでのログインと諸設定

#### ログイン
- Username: `admin`
- Password: 設定ファイルで設定したパスワード

#### verify機能の実行
1. Fauxtonにログイン後、サイドバーから「Verify」を選択
2. Verify instlationを押下
3. 全ての項目が正常に実行されれば成功

#### クラスターorシングルノードの選択
立ち上げたばかりの時点では_usersなどのシステムデータベースが作られていません。
1. サイドバーのsetupより選択
	1. 今回はシングルノードを選択しました
2. データベース一覧で_usersデータベースが作成されていることを確認

#### 自動タイムアウト時間を設定
Fauxtonは10分経過すると自動ログアウトする設定になっています。セキュリティ的に結構なことですが、そのせいで思わぬエラーに引っ掛かることもあるので、ConfigurationをいじってTimeout時間を伸ばします。
- 設定
	- `Section: chttpd_auth`
	- `Name: timeout`
	- `Value: 任意の秒数。（例: 3600）`

設定後はCouchDBの再起動をしてください。
```bash
brew services restart couchdb
```

ここまで設定できていれば、初期設定としては完了です。
# おわりに
以上です。
この設定が終わってから、Dockerでインストールすればよかったなーってちょっと後悔しました。Dockerでのやり方も調べたらまたまとめます。
その他、他の人は躓かないであろうしょーもな設定ミスやグダグダインストールの様子は以下のスクラップにまとめました。

https://zenn.dev/mconfjp/scraps/f42a2b9ed46700

# 参考文献
- [CouchDB - 公式ドキュメント](https://docs.couchdb.org/en/stable/index.html)
- [ブラウザ内DBによるシングルページWebアプリの高性能化手法](https://amzn.asia/d/2f1hgnH)
	- この書籍きっかけで書いています。ただしこの書籍にはCouch DB自体の情報はそんなに載ってません
- [7つのデータベース 7つの世界](https://amzn.asia/d/7yYLfNZ)
	- いろんなデータベースの世界を見せてくれる最高な書籍
