---
title: TiDBのMySQL互換モード（AUTO_ID_CACHE=1）切り替えをこうやりました
emoji: 🍕
type: tech
topics:
  - database
  - TiDB
  - DDL
publication_name: "levtech"
published: true

---
## これはなに
こんにちはレバテック開発部のもりたです。
最近、AWS AuroraからTiDBへの移行プロジェクトの担当をしています。その中でTiDBをMySQL互換モードに切り替える機会があったので、その作業手順をメモしておきたいと思います。
なおMySQL互換モードについての解説は以下の公式ページを参照ください。（簡単にいうと、TiDBはAUTO INCREMENTのIDが単調増分しないのですが、MySQL互換モードにするとMySQLみたいに増えてくれるやつです）
https://docs.pingcap.com/ja/tidb/stable/auto-increment/#mysql-compatibility-mode


:::details 前提:テーブル構成
### 前提：テーブル構成
こんなテーブル構成で話を進めます。今回は通常モードからMySQL互換モードにすると仮定してみましょう。

```SQL
-- 親テーブル: departments (部署)
CREATE TABLE departments (
    dept_id INT NOT NULL,  -- 部署ID（主キー）
    dept_name VARCHAR(100) NOT NULL, -- 部署名
    PRIMARY KEY (dept_id)
);

-- 子テーブル: employees (従業員)
CREATE TABLE employees (
    employee_id INT NOT NULL,  -- 従業員ID（主キー）
    employee_name VARCHAR(100) NOT NULL, -- 従業員名
    dept_id INT NOT NULL, -- 部署ID（外部キー: departmentsテーブルを参照）

    PRIMARY KEY (employee_id),
    FOREIGN KEY (dept_id) 
        REFERENCES departments (dept_id)
);
```
:::


## やり方
以下の手順で進めました。
- 対象テーブルを移し替えたいテーブルを用意する
- DUMPしたデータを入れる
- 切り替えクエリで切り替え！！

MySQL互換モードを設定した新テーブルを用意して、旧テーブルと入れ替える感じです。

### 対象テーブルを移し替えたいテーブルを用意する
CTEATE TABLEコマンドを用意し、そこに`AUTO_ID_CACHE=1`を設定するだけです。
こんな感じ。^[既存テーブルのCREATE TABLEコマンドを用意するのが一番だるいかも。もりたはSQLクライアントにMySQL Workbentchを使ってて、既存のテーブルのCREATE TABLEをサクッと取れる機能があるので、それ使いました]
```SQL
-- 親テーブル: departments (部署)
CREATE TABLE departments_tmp (
    dept_id INT NOT NULL,  -- 部署ID（主キー）
    dept_name VARCHAR(100) NOT NULL, -- 部署名
    PRIMARY KEY (dept_id)
)AUTO_ID_CACHE=1 -- MySQL互換モードの指定;

-- 子テーブル: employees (従業員)
CREATE TABLE employees_tmp (
    employee_id INT NOT NULL,  -- 従業員ID（主キー）
    employee_name VARCHAR(100) NOT NULL, -- 従業員名
    dept_id INT NOT NULL, -- 部署ID（外部キー: departmentsテーブルを参照）

    PRIMARY KEY (employee_id),
    FOREIGN KEY (dept_id) 
        REFERENCES departments_tmp (dept_id) -- 参照先はtmp。じゃないとこける
)AUTO_ID_CACHE=1 -- MySQL互換モードの指定;
```

### DUMPしたデータを入れる
次にデータをDUMPしてきて入れ直します。
DUMPはお使いのSQLクライアントでいい感じにやる機能があればそれを使うのがいいと思います。SQLクライアントが気を利かせて、色々と設定のケアをしてくれます。
以下は一例で、文字コードが変にならないようにしてくれています。

例）
```SQL
-- MySQL dump 10.13  Distrib 8.0.36, for macos14 (arm64)
--
-- Host: 127.0.0.1    Database: dev_ltlink
-- ------------------------------------------------------
-- Server version	8.0.11-TiDB-v8.1.1

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!50503 SET NAMES utf8 */;
```

### 切り替えクエリで切り替え！！
最後。以下のクエリでMySQL互換モードを指定した新テーブルに入れ替えました。

```SQL
/*--------------------------------------------------------
ここからきりかえ
--------------------------------------------------------*/
-- 外部キーチェックを一時的に無効にする
SET FOREIGN_KEY_CHECKS = 0;
-- テーブル名を一括で変更する（アトミックな操作）
RENAME TABLE 
    departments TO departments_old,      -- 既存の親テーブルを旧名に退避
    departments_tmp TO departments,      -- 新しい親テーブルを正式名に変更
    departments_old TO departments_tmp;  -- 退避させた旧テーブルを一時テーブル名に移動 (任意)

-- 外部キーチェックを戻す
SET FOREIGN_KEY_CHECKS = 1;
```

大切なのは、
- 外部キーチェックの無効化
- RENAMEをアトミックに実行する
になります。^[まあ本番作業とかだとどうせメンテ入れるからどっちでもいいんですけど、テスト環境とかでメンテ入れたくないけどなるべく面倒なことを起こさずに実施したい時とかは、これで実施するといいと思います。]


## おわりに
なんかテーブルの入れ替えって絶対めんどいでしょって思ってたんですが、思ったよりは簡単でした。
まあこれはデータ量とかに依存するんだろうな〜〜。