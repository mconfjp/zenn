
# 整理
- やりたいこと
	- 作りたいテーブルがあって、modelを作ってDBに適用させるところまで
- やり方予想
	- `php artisan model:create Post -m`
	- `php artisan migrate`
	- これでできるんだっけか
- もうちょい細かく
	- model作成と同時にmigrationファイルを作るケース
		- けどカラム全くないから、あんまない
	- model作成してからカラム追加して、対象テーブルに対してmigrationファイルを初めて作るケース
	- 既存のmodelにAlterかけてmigrationファイルを作るケース
	- 他のテーブルとのリレーション
		- 追加
		- 削除
		- 関係を変えた？
	- 

# 一旦読む

## make:modelした後にmigrationファイルを作る場合
https://zenn.dev/nenenemo/articles/10a3605c037ab6

## 公式
https://readouble.com/laravel/11.x/ja/migrations.html
> マイグレーションはデータベースのバージョン管理のようなもので
- そうか。単にAlterを自動生成してくれるやつだと思ってたけど、どのAlterがどの修正で加わったのかが分かれば、migrationが楽になるのね
> Laravelがマイグレーションの順序を決定できるようにするタイムスタンプを含めています。
- これ、けどローカルで作った時のものだからチーム開発するとズレるよな...？


> `Schema`[ファサード](https://readouble.com/laravel/11.x/ja/facades.html)は
- ファサードってどこからでも呼び出せる楽なやつみたいな認識だけどそうなのかな
> Laravelがマイグレーション名からテーブル名を決定できる場合、Laravelは生成するマイグレーションファイルへ指定したテーブル名を事前に埋め込みます。
- あっそうなんすか！？
	- 要検証🐙
> 生成するマイグレーションのカスタムパスを指定する
- はい。そういう時もありますかね
> note Note: マイグレーションのスタブは[スタブのリソース公開](https://readouble.com/laravel/11.x/ja/artisan.html#stub-customization)を使用してカスタマイズできます
- スタブとは🐙
> 必要に応じて、マイグレーションを単一のSQLファイルに「圧縮」できます
- これ一年に一回とかやってもいいかもね、直近以外だと基本的にまざってても問題ないだろし
	- 🐙
> このコマンドを実行すると、Laravelはアプリケーションの`database/schema`ディレクトリへ、「スキーマ」ファイルを書き出します。
- マイグレーションファイルと

### マイグレーションの実行
- これで実行結果が見える
	- `php artisan migrate --pretend`
	- クソ便利やないか
- `php artisan migrate --isolated`
	- これはちょっとわからない。ECS使ってるケースとかでよくあると思うけど、一つを実行したらmigrationsテーブルが更新されるから大丈夫じゃないの？　まあ違うんやろな、同時にやったらあかんもんな
- マイグレーション強性
	- 強制的にやれる。破壊的なクエリを投げる時に確認のプロンプトが入るため
### マイグレーションのロールバック
- `php artisan migrate:rollback`
	- これで最後のバッチを戻せる
	- マイグレーションは複数ファイルのファイルがあれば、それを一つのものとして扱うっぽい
- `php artisan migrate:rollback --step=5`
	- これで対象の数を指定できる。バッチ単位じゃなくってファイル単位かな？
	- そう
- `--batch=2`とかでバッチ単位指定もできる
	- これは絶対的なバッチ番号でやってる
- これでも`--pretend`指定できる
- `php artisan migrate:reset`
	- これで全部戻せる
	- これやるとバッチの単位がまとまっちゃうな
	- バッチの単位は流してる環境によってズレるね
- `php artisan migrate:refresh`
	- これはロールバックして改めてマイグレーションをまとめたやつ
	- --stepも指定できる
- `php artisan migrate:fresh`
	- 全消ししてやり直す
- 疑問
	- ファイル指定でマイグレーションできないの？
		- `--path[=PATH]                The path(s) to the migrations files to be executed (multiple values allowed)`

### 全てのコマンドオプション
```bash
➜  el-learning git:(feature/add-single-model) ✗ docker-compose exec app php artisan migrate --help   
Description:
  Run the database migrations

Usage:
  migrate [options]

Options:
      --database[=DATABASE]        The database connection to use
      --force                      Force the operation to run when in production
      --path[=PATH]                The path(s) to the migrations files to be executed (multiple values allowed)
      --realpath                   Indicate any provided migration file paths are pre-resolved absolute paths
      --schema-path[=SCHEMA-PATH]  The path to a schema dump file
      --pretend                    Dump the SQL queries that would be run
      --seed                       Indicates if the seed task should be re-run
      --seeder[=SEEDER]            The class name of the root seeder
      --step                       Force the migrations to be run so they can be rolled back individually
      --graceful                   Return a successful exit code even if an error occurs
      --isolated[=ISOLATED]        Do not run the command if another instance of the command is already running [default: false]
  -h, --help                       Display help for the given command. When no command is given display help for the list command
      --silent                     Do not output any message
  -q, --quiet                      Only errors are displayed. All other output is suppressed
  -V, --version                    Display this application version
      --ansi|--no-ansi             Force (or disable --no-ansi) ANSI output
  -n, --no-interaction             Do not ask any interactive question
      --env[=ENV]                  The environment the command should run under
  -v|vv|vvv, --verbose             Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
```

- 面白そうなもの
	- `--step`
		- 全てのマイグレーションをひとつずつ実行できる。ので、ロールバックする時とかに指定できる
	- 
### DB上でのマイグレーションテーブル
- 基本的に`php artisan migrate:status`と同じものが入っている
	- リセットした時はマイグレーションテーブルからは物理的に削除される
	- 実行済みとか未実行とかはない




## 🐙疑問🐙
- `php artisan make:migration migration_file_name`
	- このmigration_file_nameに意味はあるのか
		- →なさそう。慣習としてcreate_posts_tableみたいにしてるっぽい
		- いやちょっとは反映させてくれてた
- migrationテーブルとの関係性
- migrationファイルの中身は自分で作るだけ？
	- どうやって作るの？
		- 使える構文一覧
	- これ、任意のクエリ流せるか？　例えばUpdateとか
		- 簡易なバッチ機能になり得る？　limit500とかかければ少しずつ流せるよね
- 🐙マイグレーション実行時に流れているクエリを見てみたい。一般クエリログ有効にしてローカルでログ吐かせればいいか
- Blueprintってなんだ
