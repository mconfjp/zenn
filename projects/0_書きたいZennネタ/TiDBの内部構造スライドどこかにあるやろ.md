# 調べる

いい感じに集まりました！TiDB内部構造に関する日本語スライドをまとめます。ほぼ全部 PingCAP Japan の公式教育コンテンツで、Speaker Deck にあります。

---

## 🗂️ TiDB内部構造 日本語スライド集

### 🏗️ アーキテクチャ全体

**① TiDBが生まれた背景 そして、TiDB Serverlessへ** https://speakerdeck.com/pingcap0315/tidbgasheng-maretabei-jing-sosite-tidb-serverlesshe TiDB / TiKV / PD / TiFlash の役割、Region の仕組み、Serverless アーキテクチャまで幅広くカバー。全体像をつかむのにまずこれ。
[[TiDBの内部構造関連の資料を読む]]

**② 最近たまに見かけるTiDBってなんだ？ (Findy)** https://speakerdeck.com/pingcap0315/zui-jin-tamanijian-kakerutidbtutenanda-findy 入門寄りだけど、アーキテクチャ図や各コンポーネントの説明が丁寧。

**③ MySQLインターフェースNewSQL TiDBの実力とは** https://speakerdeck.com/pingcap0315/mysqlintahuesunewsql-tidbnoshi-li-toha アーキテクチャ図に加えて、Google Percolatorによるスナップショット分離、Two Phase Commitの話まで踏み込んでいる。

---

### 🔑 TiKV（ストレージ層）

**④ TiKV - Raftによるデータ整合性の実現** https://speakerdeck.com/pingcap0315/tikv-raftniyorudetazheng-he-xing-noshi-xian Raft Consensus Algorithm / ログレプリケーション / リーダー選挙 の仕組みをじっくり解説。

**⑤ TiKV - トランザクション & MVCC**（スライドはロック競合のページから参照可） https://speakerdeck.com/pingcap0315/tidbdenorotukujing-he-notoraburusiyuteingu MVCCの実現方法、楽観ロック/悲観ロックの違い、prewrite/commitフローなど。

---

### 🧮 SQL実行の仕組み

**⑥ TiDBでのSQL実行の仕組み** https://speakerdeck.com/pingcap0315/newsql-wu-liao-toreningu-tidbdenosqlshi-xing-noshi-zu-mi DML/DDLがどう実行されるか、実行計画、オプティマイザーヒントまでカバー。

**⑦ TiDBでのインデックス設計（KVマッピングルール）** https://speakerdeck.com/pingcap0315/tidb-denoindetukusushe-ji テーブルデータ・インデックスがKVペアにどうマッピングされるか。内部構造的に重要なトピック。

---

### 📊 HTAP / TiFlash（分析系）

**⑧ TiFlashの紹介** https://speakerdeck.com/pingcap0315/tiflashnoshao-jie TiKVからのリアルタイム同期、MPPモード、カラムナストアとしての内部動作。

---

### 🧪 ユーザー事例（内部構造の理解に役立つ）

**⑨ LINEによる大規模ログデータ管理の挑戦** https://speakerdeck.com/pingcap0315/linezhu-shi-hui-she-tidbniyoruda-gui-mo-rogudetaguan-li-notiao-zhan-siyadeingunasideda-liang-insatowosabakuniha-2023-tidb-user-day-setusiyon 実際のクラスタ構成やパフォーマンス検証を通じて、内部の挙動が具体的に見える。

---

### 📖 スライド以外のオマケ（Zenn記事）

- **TiKVにおけるトランザクションとMVCCの話（RocksDB含む）** https://zenn.dev/koiping/articles/1e538994bf7fb7 RocksDB、MVCC、CFの仕組みを日本語でしっかり書いてある記事。スライドじゃないけど補足資料として◎

---

④→①→⑥→⑤の順で読むと、ストレージ→全体像→SQL実行→トランザクションと段階的に理解が深まりやすいと思います！