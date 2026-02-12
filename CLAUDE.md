# Zenn CLAUDE.md

技術記事・書籍執筆用のワークスペース。

## 基本方針

- **歴史的背景を重視**: 技術の「なぜ」を掘り下げる
- **深い技術分析**: 表面的な使い方だけでなく、仕組みや設計思想まで
- **Zettelkastenとの連携**: `../zettel/` の永久ノートを活用して記事を組み立てる

## よく使うコマンド

```bash
# プレビュー
npx zenn preview

# 新規作成
npx zenn new:article
npx zenn new:book

# 一覧表示
npx zenn list:articles
npx zenn list:books
```

## フォルダ構成

```
zenn/
├── articles/          ← 記事本文（.md）
├── books/             ← 本
├── projects/          ← プロジェクト管理・調査ログ
│   ├── 書いた/        ← 公開済みプロジェクト
│   └── 書かない/      ← ボツ・保留プロジェクト
└── package.json       ← Zenn CLI設定
```

## 記事のフロントマター

すべての記事は以下のフロントマターを持つ：

```yaml
---
title: "記事タイトル"
emoji: "🔖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["topic1", "topic2"]
published: true # true: 公開 / false: 下書き
---
```

## 執筆ワークフロー

1. **企画・リサーチ** → `projects/` でプロジェクト立ち上げ
2. **調査** → `../zettel/` でZettelkasten形式でノート蓄積
3. **構成** → プロジェクトディレクトリで構成メモ作成
4. **執筆** → `articles/` に記事作成
5. **公開** → フロントマターの `published: true` で公開

## 記事の特徴

- 日本語での丁寧な解説
- コード例は必要に応じて（技術深掘りがメイン）
- 折りたたみ詳細（`<details>`）で補足情報を整理
- 歴史的経緯・設計思想の説明を重視

## 重要なルール

- **絵文字は明示的に依頼されない限り使わない**（フロントマター除く）
- **`projects/` の調査ログは記事の根拠**: 記事執筆時に参照すること
- **Zettelkastenとの往復**: 記事化できそうな永久ノートを見つけたら `projects/` で企画を立てる
