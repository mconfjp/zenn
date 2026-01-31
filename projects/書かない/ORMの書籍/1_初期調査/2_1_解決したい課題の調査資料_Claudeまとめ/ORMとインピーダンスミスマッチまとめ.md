# ORM（Object-Relational Mapping）の歴史に関する調査まとめ

## 1. ORMが解決しようとした根本的な問題

### 1.1 インピーダンスミスマッチ - 二つの世界の根本的な違い

**オブジェクト指向の世界とリレーショナルの世界の違い：**

| オブジェクト指向 | リレーショナル |
|-----------------|--------------|
| データと振る舞いが一体化 | データは表形式 |
| 継承・ポリモーフィズム | 集合論ベース |
| オブジェクトアイデンティティ | 主キーでデータを識別 |
| オブジェクト参照でナビゲーション | JOINでデータを結合 |

### 1.2 インピーダンスミスマッチとして実際に議論されてきた問題

**Ted Neward "The Vietnam of Computer Science" (2004) での主要な問題：**[^1]

1. **Object Identity vs Primary Key**
   - オブジェクト：メモリアドレスベースの同一性
   - RDB：値ベースの同一性（主キー）

2. **Inheritance（継承）の表現**
   - Single Table / Class Table / Concrete Table の3戦略
   - それぞれトレードオフが存在

3. **Association（関連）の表現**
   - 双方向関連の維持
   - 多対多関連の中間テーブル

4. **Granularity（粒度）**
   - Value Object（住所など）の扱い
   - Embedded vs 別テーブル

5. **グラフ構造 vs 集合構造**
   - オブジェクト：任意のグラフ構造（循環参照も可能）
   - リレーショナル：本質的に集合とその関係

### 1.3 開発生産性の問題

インピーダンスミスマッチが原因で発生する問題：

1. **マッピングコード地獄**
   - 手動でオブジェクト↔行の変換
   - 関連オブジェクトの読み込み管理
   - NULL値の処理

2. **パフォーマンスチューニングの複雑さ**
   - N+1問題（関連オブジェクトの遅延読み込み）
   - 過剰なJOIN
   - キャッシュ戦略

3. **メンテナンス性の低下**
   - スキーマ変更時の影響範囲が広い
   - ビジネスロジックとデータアクセスの分離が難しい

## 2. なぜOODBやODBCでは解決できなかったのか

### 2.1 OODB（オブジェクト指向データベース）の限界

**"The Object-Oriented Database System Manifesto" (Atkinson et al., 1989) より：**[^2]

OODBが満たすべき13の必須要件が提示されたが、実際には以下の理由で失敗：

1. **クエリ言語の問題**
   - SQLほど成熟したクエリ言語がない
   - 各ベンダーが独自のクエリ言語を作成
   - 標準化の失敗（ODMG標準は普及せず）

2. **レガシーシステムとの非互換性**
   - 既存RDBMSデータとの相互運用が困難
   - 移行コストが高すぎる
   - DBAsの再教育が必要

3. **パフォーマンスと最適化**
   - RDBMSの数十年にわたる最適化技術に勝てない
   - 複雑なオブジェクトグラフのナビゲーションが遅い
   - インデックス技術が未成熟

### 2.2 ODBCの限界

**単なるデータベースアクセスAPIの限界：**

1. **インピーダンスミスマッチを解決しない**
   - SQLを書く必要がある
   - 手動でオブジェクトとテーブル行をマッピング
   - 型安全性がない

2. **開発生産性の問題**
   ```c
   // ODBCの典型的なコード（面倒！）
   SQLExecDirect(stmt, "SELECT * FROM users", SQL_NTS);
   SQLBindCol(stmt, 1, SQL_C_CHAR, name, sizeof(name), &nameLen);
   SQLBindCol(stmt, 2, SQL_C_LONG, &age, 0, &ageLen);
   ```

3. **オブジェクトモデルとの統合なし**
   - 継承のサポートなし
   - 関連のナビゲーションが手動
   - オブジェクトアイデンティティの管理が困難

### 2.3 OOPとRDBの綱引き - 誰が犠牲になったか

- **OODB**：「RDBを犠牲にしてOOPに全振り」
  - データベースをオブジェクト指向的に作り替え
  - RDBMSの強みを全て失った
  
- **ODBC**：「OOPを犠牲にして現状維持」
  - RDBはそのまま、開発者が頑張って解決
  - 開発生産性が犠牲に
  
- **ORM**：「どっちも少しずつ犠牲にして妥協」
  - RDBの基本は維持しつつOOPの概念をサポート
  - 完璧じゃないけど実用的なバランス

## 3. ORMが選ばれた理由

**Ted Neward "The Vietnam of Computer Science" (2004)より：**[^1]

> "Object/Relational Mapping is the Vietnam of Computer Science, it represents a quagmire which starts well, gets more complicated as time passes, and before long entraps its users in a commitment that has no clear demarcation point, no clear win conditions, and no clear exit strategy."

それでもORMが選ばれた理由：

1. **実用的な妥協案**
   - RDBMSの信頼性を維持
   - オブジェクト指向プログラミングの利点も活用
   - 段階的な導入が可能

2. **既存インフラの活用**
   - 成熟したRDBMS技術
   - 豊富なDBツールとエコシステム
   - DBAの既存スキルを活用可能

3. **開発生産性の向上**
   - ボイラープレートコードの削減
   - 型安全性
   - オブジェクト指向的なデータアクセス

## 4. リレーショナル指向プログラミングの試み

### 4.1 歴史的な試み

1. **埋め込みSQL（1970年代後半〜）**
   - COBOL、C、PL/Iなどにリレーショナル操作を埋め込む
   - プリコンパイラが必要

2. **4GL（第4世代言語）- 1980年代**
   - Informix-4GL、Oracle Forms、Progress 4GL
   - リレーショナル操作を言語の一部として統合

3. **Tutorial D - 1990年代〜**
   - Date & Darwenの"The Third Manifesto"（1995）で提案[^3]
   - 純粋なリレーショナルモデルに基づく言語

4. **LINQ - 2007年〜**
   - MicrosoftがC#/VBにリレーショナル演算を統合
   - OOPとリレーショナルの融合を試みた

### 4.2 リレーショナル指向言語が主流にならなかった理由

1. OOPの圧倒的な人気
2. アプリケーションロジックの複雑さへの対応不足
3. 学習曲線の問題
4. エコシステムの不足

## 5. 現代のトレンド

### 5.1 関数型プログラミングとリレーショナルモデルの相性

**相性が良い理由：**

1. 両方とも数学的基礎がしっかり
2. 不変性（Immutability）の重視
3. 合成可能性（Composability）

**具体例：**
- ScalaのSlick：型安全なSQL
- Quill：コンパイル時にSQLに変換

### 5.2 GraphQLの位置づけ

**リレーショナル的な側面：**
- クライアントが必要なデータ形状を宣言的に指定
- JOINに相当する操作を宣言的に記述

**ORMとの関係：**
- GraphQLバックエンドでORMがよく使われる
- 「クエリ言語の現代的な再解釈」として成功

---

[^1]: Ted Neward, "The Vietnam of Computer Science", 2004
[^2]: Atkinson et al., "The Object-Oriented Database System Manifesto", 1989
[^3]: Date & Darwen, "The Third Manifesto", 1995
