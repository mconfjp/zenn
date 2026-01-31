---
title: Laravelのマイグレーション機能をちゃんと知る
emoji: 🌊
type: tech
topics:
  - PHP
  - Laravel
  - データベース
published: true
publication_name: levtech
---
# これはなに
![](https://storage.googleapis.com/zenn-user-upload/785488a7191f-20251015.png)
こんにちは、レバテック開発部のもりたです。
今回はLaravelのマイグレーション機能についてまとめます。これはアプリケーションコード上でデータベースのテーブル修正やカラム修正を管理する機能です。もりたは仕事上でもLaravelを使うんですが、マイグレーションについてちゃんと理解してなかったため、今回まとめてみました。

:::details 構成
## 構成
今回の構成は以下の通りです。
- 概要
	- マイグレーションの流れ
	- Eloquentとマイグレーションの関係
- マイグレーションファイルの作成方法
- マイグレーションファイルの書き方
- マイグレーションの実行
:::
それでは参りましょう。

# マイグレーションの概要
## マイグレーションの流れ
![](https://storage.googleapis.com/zenn-user-upload/3a16d511d7f9-20251015.png)

Laravelのマイグレーション機能とは、本来SQLを書いてテーブル^[スキーマ定義と呼びたいんですが、馴染みのない人もいそうなのでこの記事ではテーブル定義、カラム定義で通します]の定義変更を行わなければならないものを、PHPのコード上で管理しデータベースに反映することができる機能のことです。
修正内容はマイグレーションファイルというものに記録されて、その情報を元にデータベースへの反映が実行されます。
作業の流れは以下の３ステップです。

- 「マイグレーションファイルの生成」
- 「マイグレーションファイルを書く」
- 「マイグレーション実行」

マイグレーションファイルを用意し、その中にテーブル定義変更を記述し、実行する。それだけです。
## Eloquentとマイグレーションの関係性
マイグレーション機能と似たような仕組みとして、Eloquentが存在します。
EloquentはLaravelに標準搭載されているORマッパーであり、こちらもPHPコードでのデータベース操作を可能にします。両者は共通して以下のようなコンポーネントを利用しています。

- DBConnection: データベースとの接続を確立する
- QueryBuilder: PHPコードからSQLを組み立てる
- Grammar: SQLの方言を吸収する

Eloquentとマイグレーション機能ははPHPコードからSQLを生成するという似た目的を持ち、共通したコンポーネントを利用しています。しかし、生成するSQLに着目すると両者の違いが見えてきます。
まずマイグレーションはテーブル定義を操作することを目的にしており、生成されるSQLはDDL^[SQLのうち、テーブルの作成やカラムの追加など、テーブル定義を変更するクエリを指す]です。一方でEloquentは保存されたデータを操作することが目的であり、生成されるSQLはDML^[SQLのうち、SELECTやUPDATE、DELETEなどデータそのものを操作するクエリを指す]です。PHPコードからSQLを組み立てる際に利用するQueryBuilderについても区別されており、マイグレーションはDDL向けのSchemaBuilderを、EloquentではDML向けのQueryBuilderを利用しています。

注意したいのはEloquentとマイグレーションはお互いに依存関係を持たないという点です。
例えば`User`というEloquent Modelを作成した上で`Users`というテーブルを作成するマイグレーションファイルを生成したとしても、`User`Modelのプロパティがマイグレーションファイルに反映される.......なんてことは起こりません。
# マイグレーションファイルの生成
![](https://storage.googleapis.com/zenn-user-upload/aec8141d44f1-20251015.png)
ここからは「マイグレーションの流れ」で説明した作業手順に沿って解説します。まずはマイグレーションファイルの生成です。
## 素の実行
生成は以下のコマンドで実行できます。
`php artisan make:migration <migration_file_name>`
このコマンドを打つと、`database/migrations`配下に`yyyy_mm_dd_xxxxxx_migration_file_name.php`という形式でファイルが生成されます。生成されるファイルは以下の通りです。
```php app/database/migrations/yyyy_mm_dd_xxxxxx_migration_file_name.php
// app/database/migrations/yyyy_mm_dd_xxxxxx_migration_file_name.php
<?php
  
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
  
return new class extends Migration
{
	/**
	* Run the migrations.
	*/
	public function up(): void
	{
		//
	}
  
	/**
	* Reverse the migrations.
	*/
	public function down(): void
	{
		//
	}
};
```
とてもシンプルなファイルが生成されました。対象テーブルの指定もなく、`up`と`down`という関数が置かれているだけです。マイグレーションコマンド実行時に指定した`migration_file_name`はファイル名に使われており、その他には影響を及ぼしていません。

ただ、オプションやマイグレーションファイル名の命名規則を守ることで、最初から色々と書かれた状態のファイルを生成できます^[しかし、実際に自動生成される内容はほんとに少しなので、何も考えずにマイグレーションファイルを生成して手で書いてしまってもいいと思います]。
続けて、オプションと命名規則の解説をします。

## オプション
### `--table=<テーブル名>`
`--table`オプションをつけて実行すると、`--table=<テーブル名>`で指定したテーブルへのマイグレーションになります。具体的には以下の通り、shemaの指定が加わります。

```php
// `php artisan make:migration <migration_file_name> --table=dummies`

	/**
	* Run the migrations.
	*/
	public function up(): void
	{
		Schema::table('dummies', function (Blueprint $table) {
		//
		});
	}
	 
	/**
	* Reverse the migrations.
	*/
	public function down(): void
	{
		Schema::table('dummies', function (Blueprint $table) {
		//
		});
	}
	 
```
### `--create=<テーブル名>`
こちらも同様で、`--create=<テーブル名>`で指定したテーブルへのマイグレーションになります。ただ今回は新規作成であることを明示的に表現しているため、shemaの指定と`id`、`timestamps`が最初から記載されています。

```php
// `php artisan make:migration <migration_file_name> --create=dummies`

	/**
	* Run the migrations.
	*/
	public function up(): void
	{
		Schema::create('dummies', function (Blueprint $table) {
		$table->id();
		$table->timestamps();
		});
	}
	 
	/**
	* Reverse the migrations.
	*/
	public function down(): void
	{
		Schema::dropIfExists('dummy');
	}
```

:::details その他のオプション
### その他のオプション
その他のオプションは以下の通りです。`--path`ならギリ使うかなあというくらいだと思います。

``` bash
$ docker-compose exec app php artisan make:migration --help                     
Description:
  Create a new migration file

Usage:
  make:migration [options] [--] <name>

Arguments:
  name                   The name of the migration

Options:
      --create[=CREATE]  The table to be created
      --table[=TABLE]    The table to migrate
      --path[=PATH]      The location where the migration file should be created
      --realpath         Indicate any provided migration file paths are pre-resolved absolute paths
      --fullpath         Output the full path of the migration (Deprecated)
  -h, --help             Display help for the given command. When no command is given display help for the list command
      --silent           Do not output any message
  -q, --quiet            Only errors are displayed. All other output is suppressed
  -V, --version          Display this application version
      --ansi|--no-ansi   Force (or disable --no-ansi) ANSI output
  -n, --no-interaction   Do not ask any interactive question
      --env[=ENV]        The environment the command should run under
  -v|vv|vvv, --verbose   Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

```
:::

## マイグレーションファイル名からの推論
生成コマンドに与えるファイル名を一定の命名規則で実行すると、実行したいDDLの種類がマイグレーションファイル名から推測され、適切なコードが生成されます。
可能な記述は以下の通りです。

```bash
// テーブル作成
php artisan make:migration create_<テーブル名>_table
// カラム追加
php artisan make:migration add_<カラム名>_to_<テーブル名>_table
php artisan make:migration add_columns_to_<テーブル名>_table
// カラム削除
php artisan make:migration remove_<カラム名>_from_<テーブル名>_table
php artisan make:migration remove_columns_from_<テーブル名>_table

// こんな書き方もできる
php artisan make:migration "add <カラム名> to <テーブル名>"
// 最後のtableは抜いてもいい
php artisan make:migration create_<テーブル名>
```


:::details 実行して作成されるもの
実行して作成されるものについては以下に列挙します。いちいち書いてると量が多いのでアコーディオンにしています。
### 作成: `create_<テーブル名>_table`
```php
public function up(): void
{
	Schema::create('dummies', function (Blueprint $table) {
	$table->id();
	$table->timestamps();
	});
}
public function down(): void
{
	Schema::dropIfExists('dummies');
}
```
### カラム追加: `add_<カラム名>_to_<テーブル名>_table`
```php
public function up(): void
{
	Schema::table('dummies', function (Blueprint $table) {
		//
	});
}
```
特にカラムの追加コードを書いてくれるわけではないですが、`Schema::table()`を呼び出すように変わっています。複数カラム追加の場合も同様です。

### カラム削除: `add_<カラム名>_to_<テーブル名>_table`
```php
public function up(): void
{
	Schema::table('dummies', function (Blueprint $table) {
		//
	});
}
```
カラム追加の時と同様のファイルが生成されます。

:::
# マイグレーションファイルを書く
![](https://storage.googleapis.com/zenn-user-upload/3ad6b66eeded-20251015.png)
マイグレーションファイルを生成できたので、次は中身を書いていきましょう。例を挙げると、こんな感じのファイルを書くことになります。最初に構造から把握していきます。

```php
<?php
  
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
  
return new class extends Migration
{
	/**
	* Run the migrations.
	*/
	public function up(): void
	{
		Schema::create('posts', function (Blueprint $table) {
			$table->id();
			$table->string('title');
			$table->text('content');
			$table->timestamps();
		});
	}
  
	/**
	* Reverse the migrations.
	*/
	public function down(): void
	{
		Schema::dropIfExists('posts');
	}
};
```

マイグレーションファイルの構造として着目してほしいのは以下の３点です。

- `up`
	- マイグレーションが実行されたときに実行してほしいものをかく
- `down`
	- マイグレーションを取り消したい時の振る舞いを定義する
- `Schema::`
	- `up`や`down`の中身で、DDLに相当するところ。この例では`CREATE TABLE`と`DROP TABLE`に対応する関数が書かれている

`up`に実行したいDDLを記載し、`down`にそれを打ち消すDDLを書くだけです。つまり、DDLに当たるPHPコードさえ書けてしまえばマイグレーションファイルの中身は書けてしまいます。
ではどのようにDDL部分を記述すれば良いか見ていきましょう。
## 代表的なマイグレーション構文^[マイグレーション構文というのはいま即興で考えたワードです]
代表的なDDLについて、マイグレーションファイル内での書き方を整理していきましょう。
### テーブル作成: `CREATE TABLE`
SQLにおける`CREATE TABLE`構文にあたるものです。`Schema::create()`関数を利用し、関数内に対象テーブルに追加したいカラムの情報を書きます。具体的には以下の通り記載します。

```php
Schema::create('users', function (Blueprint $table) {
	$table->id();
	$table->string('name');
	$table->string('email');
	$table->timestamps();
});
```

カラムの型情報については全て書いていくと長いため、以下の公式サイトを参照してください。
https://readouble.com/laravel/11.x/ja/migrations.html#columns

### テーブル更新: `ALTER TABLE`
続いて、`ALTER TABLE`に対応するものです。SQLの`ALTER TABLE`では`ALTER TABLE <テーブル名> ADD <カラム名> <カラムの型>;`という構文を使い、修正したいときは`ADD`の代わりに`MODIFY`が出てきたり、削除したいときは`DROP`が出てきたりします。

その考え方はマイグレーションファイルでも同じで、`Schema::table()`関数を利用し、その中で`change`や`drop`など、様々な操作が可能です^[カラム追加は特に関数などなく、作成したいカラムを定義するだけです（例：`$table->string('postnum');`）。記載漏れなのか意図的なのかわかりにくいから`->add()`とかできないのかなーと思ったんですが、実は`$table->string('postnum')->add()`;という構文でも実行できるみたいです。ただしこれは偶然そうなっているだけ。ここは冒頭に説明したGrammarクラスに関わる箇所のようで、追いかけていくと今回の記事のスコープから外れそうだったため調査は打ち切りました]。

```php
Schema::table('dummies', function (Blueprint $table) {
	// カラム追加
	$table->string('postnum');
	// カラム定義の変更
	$table->string('name', 50)->change();  
	// カラムのリネーム
	$table->renameColumn('from', 'to');
	// カラムの削除
	$table->dropColumn('votes');
});
```
参考：https://readouble.com/laravel/11.x/ja/migrations.html#modifying-columns

なおこの際に幾つかの**修飾子**をつけることができます。例えば特定のカラムの後ろにこのカラムを入れたい、といった指定が可能です。詳しくは以下を参照してください。
https://readouble.com/laravel/11.x/ja/migrations.html#column-modifiers

### テーブル削除: `DROP TABLE`
最後はテーブルごと削除する`DROP TABLE`です。これはマイグレーションを巻き戻す`down`メソッド内で利用することが多いですね。以下の例は、単純に`DROP`するだけではなく、対象のテーブルが存在する場合のみ`DROP`する構文です。

```php
Schema::dropIfExists('users');
```

### インデックス操作＆外部キー
その他、インデックス操作と外部キーの設定方法については以下のページを参照してください。
- インデックス
	- https://readouble.com/laravel/11.x/ja/migrations.html#indexes
- 外部キー
	- https://readouble.com/laravel/11.x/ja/migrations.html#foreign-key-constraints

# マイグレーションの実行
![](https://storage.googleapis.com/zenn-user-upload/890825d3dcd1-20251015.png)
## 概要
先ほどまでの章でマイグレーションファイルを生成し、修正内容を定義するところまで説明しました。「マイグレーションの実行」では、マイグレーションファイルの定義を元に、データベースにテーブル定義を反映させていきます。
ここで重要なのがマイグレーションの**バッチとステップ**という概念、そして`migrations`**テーブル**です。

### マイグレーションのバッチとステップ
マイグレーションファイルを生成し、マイグレーション実行することで、修正内容がデータベースに反映されます。ではこの時、複数のマイグレーションファイルが存在している場合、どこまでが反映されるでしょうか？
答えは「まだデータベースに反映されていないマイグレーションファイル**全て**が反映される」です。マイグレーション機能では、この一度のマイグレーション実行で反映されたマイグレーションをバッチという単位で管理しています。
それに対して、ファイルひとつひとつの実行単位をステップと呼びます。
マイグレーションの実行ではこれらを区別して考えることが重要です。

### `migrations`テーブル
次に大切なのが`migrations`テーブルの存在です。
先ほど「まだデータベースに反映されていないマイグレーションファイル全てが反映される」と書きましたが、ではどのようにマイグレーションファイルの反映状態を管理しているのでしょうか？　その答えが`migrations`テーブルです。
`migrations`テーブルは以下のようなテーブル構成です。

| id  | migration                            | batch |
| --- | ------------------------------------ | ----- |
| 1   | 0001_01_01_000000_create_users_table | 1     |

管理の方法は至ってシンプル。上記のテーブル構成のように、マイグレーションファイルのうち実行ずみのファイルがmigrationsテーブルに保存されます。`migration`カラムはマイグレーションファイルの名前と対応しているため、実行後のマイグレーションファイルの名前を変更すると、そのファイルは未実行とみなされてしまいます^[それやってローカル環境をぶっ壊したことがあります]。

## 実行コマンド
概要の説明が終わったので、実際のコマンドを確認していきます。
基本的なコマンドは以下のとおりです。

`php artisan migrate`

上記を基本に、派生コマンドがあったり、オプションを追加することができます。
### 派生コマンド
主要な派生コマンドは以下のとおりです。

- `php artisan migrate:state`
	- マイグレーションファイルの反映状況を一覧する
- `php artisan migrate:rollback`
	- 直近のバッチをロールバックする
- `php artisan migrate:reset`
	- 直近のバッチをロールバックし、改めてマイグレーションを実行し直す
- `php artisan migrate:refresh`
	- 全マイグレーションをロールバックしてから、マイグレーションを実行する
- `php artisan migrate:fresh`
	- 全テーブルを削除して、マイグレーションを実行する^[すでに実行ずみのマイグレーションファイルも実行されます]

### オプション
そして主要なオプションは以下のとおりです。

- `--path`
	- 流したいマイグレーションファイルのパスを指定できる
- `--database`
	- DDL実行対象のデータベースを選択できる。configファイルで設定したものの中から選択する形になる
- `--pretend`
	- 実際に実行されるクエリを見ることができる
- `--step`
	- マイグレーションファイルをバッチにまとめず個別に流す
	- ロールバック時に付記する場合はロールバックしたいstepの数を指定できる

:::details その他の知識
# その他の知識
## イベント
マイグレーションの終わりや始まりをLaravelのイベントとしてキャッチすることが可能です。イベントの詳細は以下のドキュメントを参照ください。
https://readouble.com/laravel/11.x/ja/migrations.html#events
## スキーマのDump
`php artisan schema:dump`コマンドにより、データベーススキーマのDumpを取ることができます。作成されたファイルは`database/schema`ディレクトリに保存されます。
このコマンドは裏でデータベースのコマンドラインクライアントのDumpコマンドを打っているので、コマンドラインクライアントが入ってない環境だと例外を吐きます。
:::

# おわりに
![](https://storage.googleapis.com/zenn-user-upload/63592fd0b66b-20251015.png)
以上、マイグレーションについてのまとめでした。
割と長いこと勘で書いてきたので、今回まとめられて良かったです。EloquentやQueryBuilderについてもまたまとめたいなと思っています。

## 参考文献
- [マイグレーション - Laravel公式ドキュメント](https://readouble.com/laravel/11.x/ja/migrations.html)
- [テーブルの生成 - Laravel公式ドキュメント](https://readouble.com/laravel/11.x/ja/migrations.html#creating-tables)
	- `Schema`ファサードを利用したDDLの表現。これは色々とメソッドがあって追いきれないので、この記事で紹介したのは一部にとどめました。詳しくは公式だったり、あとはコードを読んでみてください
	- ちなみにコードは`laravel_app/vendor/laravel/framework/src/Illuminate/Database/Schema/Builder.php`に記載があります
- [Laravel入門者が マイグレーション処理の流れを クソ丁寧に追ったら、追いきれなくなった話](https://qiita.com/don-bu-rakko/items/8a74ab90677d83d507e1)
	- タイトルの通りマイグレーション処理のコードリーディングをしている記事。Builderのコードがどこにあるのかなどを解説しています
- [【保存版】Laravel Migration Rollbackの全手順と5つの失敗しない方法](https://dexall.co.jp/articles/?p=2652)
	- ロールバックに焦点を当てた記事です