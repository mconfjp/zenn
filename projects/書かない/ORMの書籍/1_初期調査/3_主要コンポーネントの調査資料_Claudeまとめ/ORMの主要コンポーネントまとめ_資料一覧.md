# ORM主要コンポーネント調査 - 参考資料のまとめ

## 1. ORM全般の学術論文・研究資料

### Giving Meaning to Enterprise Architectures: Architecture Principles with ORM and ORC
- **リンク**: https://www.researchgate.net/publication/225595101_Giving_Meaning_to_Enterprise_Architectures_Architecture_Principles_with_ORM_and_ORC
- **内容**: ORM（Object Role Modeling）とObject Role Calculus（ORC）を用いたアーキテクチャ原則の形式化に関する研究。ORM/ORCによるスキーマの定義、半形式的な自然言語による表現などを含む。

### ORM general, relational, and non-relational high-level architecture | Download Scientific Diagram
- **リンク**: https://www.researchgate.net/figure/ORM-general-relational-and-non-relational-high-level-architecture_fig1_315386248
- **内容**: ORMの一般的なアーキテクチャ、リレーショナルおよび非リレーショナルの高レベルアーキテクチャ図。クラウド環境でのデータ分散管理やセキュリティに関する研究。

### ORM and component-based architecture - Software Engineering Stack Exchange
- **リンク**: https://softwareengineering.stackexchange.com/questions/155710/orm-and-component-based-architecture
- **内容**: コンポーネントベースのアーキテクチャにおけるORMの使用に関する議論。データアクセス層とビジネスロジックの結合、ORMのリレーショナルマッピング機能の制限について。

## 2. Hibernate関連資料

### Hibernate Architecture | GeeksforGeeks
- **リンク**: https://www.geeksforgeeks.org/hibernate-architecture/
- **内容**: Hibernateアーキテクチャの概要。SessionFactory、Session、第一レベルキャッシュ、第二レベルキャッシュなどのコアコンポーネントの説明。

### Hibernate Architecture
- **リンク**: https://www.tutorialspoint.com/hibernate/hibernate_architecture.htm
- **内容**: Hibernateのレイヤードアーキテクチャと主要クラスの詳細説明。データベース接続、クラスマッピング、SessionFactory、Sessionなどのコンポーネント。

### Hibernate ORM 5.4.33.Final User Guide
- **リンク**: https://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html
- **内容**: Hibernate 5.4の公式ユーザーガイド。Session、Transaction、ドメインモデル、型システム、マッピングメタデータなどの詳細なドキュメント。

### Hibernate Second-Level Cache | Baeldung
- **リンク**: https://www.baeldung.com/hibernate-second-level-cache
- **内容**: Hibernateの第二レベルキャッシュの実装と設定方法。SessionFactory-scopedキャッシュの動作原理とEhcacheとの統合。

### Chapter 2. Architecture
- **リンク**: https://docs.jboss.org/hibernate/orm/3.3/reference/en-US/html/architecture.html
- **内容**: Hibernate 3.3のアーキテクチャドキュメント。SessionFactory、Session、Transaction、エンティティ状態の定義などの詳細。

## 3. ActiveRecord関連資料

### Active Record Basics — Ruby on Rails Guides
- **リンク**: https://guides.rubyonrails.org/active_record_basics.html
- **内容**: Ruby on RailsのActive Recordの公式ガイド。MVC、ORM、Active Recordパターン、命名規則、CRUD操作などの基本概念。

### Active record pattern - Wikipedia
- **リンク**: https://en.wikipedia.org/wiki/Active_record_pattern
- **内容**: Active Recordパターンの定義とMartin Fowlerによる概念説明。データベーステーブルとクラスのマッピング、基本的な操作インターフェース。

### The Active Record Design Pattern
- **リンク**: https://researchhubs.com/post/computing/web-application/the-active-record-design-pattern.html
- **内容**: Active Recordデザインパターンの詳細説明。Rubyモジュールによる実装、データアクセス、CRUDオペレーションについて。

### The Active Record Pattern | Clark Feusier
- **リンク**: https://clarkfeusier.com/the-active-record-pattern-orm-rails-active-record
- **内容**: Active Recordパターン、ORM、Rails ActiveRecordの概要。オブジェクトとリレーションのマッピング方法の説明。

## 4. Entity Framework関連資料

### DbContext Lifetime, Configuration, and Initialization - EF Core | Microsoft Learn
- **リンク**: https://learn.microsoft.com/en-us/ef/core/dbcontext-configuration/
- **内容**: DbContextのライフサイクル、設定、初期化に関する公式ドキュメント。Unit of Work、依存性注入、設定パターンなど。

### 1. Introducing the DbContext API - Programming Entity Framework: DbContext [Book]
- **リンク**: https://www.oreilly.com/library/view/programming-entity-framework/9781449331825/ch01.html
- **内容**: DbContext APIの導入と基本概念。ObjectContextとの比較、簡略化されたAPI、クエリと保存操作の詳細。

### DbContext in Entity Framework Core
- **リンク**: https://www.entityframeworktutorial.net/efcore/entity-framework-core-dbcontext.aspx
- **内容**: EF CoreにおけるDbContextの詳細説明。DbSet、OnConfiguring、基本的な使用方法のチュートリアル。

### DbContext Class (Microsoft.EntityFrameworkCore) | Microsoft Learn
- **リンク**: https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext?view=efcore-9.0
- **内容**: DbContextクラスの公式APIリファレンス。Unit of WorkとRepositoryパターンの実装、APIの詳細仕様。

## 5. Prisma関連資料

### Prisma | Simplify working and interacting with databases
- **リンク**: https://www.prisma.io
- **内容**: Prismaの公式ウェブサイト。高性能なORM、インテリジェントなキャッシング、グローバルエッジキャッシュ、サーバーレスアーキテクチャの特徴。

### GitHub - prisma/prisma: Next-generation ORM for Node.js & TypeScript
- **リンク**: https://github.com/prisma/prisma
- **内容**: PrismaのGitHubリポジトリ。Prisma Client、Prisma Migrate、Prisma Studioの実装とドキュメント。

### GitHub - prisma/prisma-engines: 🚂 Engine components of Prisma ORM
- **リンク**: https://github.com/prisma/prisma-engines
- **内容**: Prisma ORMのエンジンコンポーネント。Query compiler、Schema engine、Prisma Formatなどのコアエンジンの実装。

### Engines | Prisma Documentation
- **リンク**: https://www.prisma.io/docs/orm/more/under-the-hood/engines
- **内容**: Prismaエンジンの詳細ドキュメント。Query engine、Node-APIライブラリ、バイナリ実装、キャッシュプール管理について。

### What is Prisma and Why Do We Need Another ORM? | Nearform
- **リンク**: https://commerce.nearform.com/blog/2021/prisma-orm/
- **内容**: Prismaの設計思想と従来のORMとの違い。カスタムSDL、マイグレーション、型安全性、コード生成アプローチの説明。

## 6. Diesel（Rust）関連資料

### Diesel is a Safe, Extensible ORM and Query Builder for Rust
- **リンク**: https://diesel.rs/
- **内容**: Dieselの公式ウェブサイト。安全性、拡張性、抽象化、コンパイル時のクエリ検証などの特徴。

### diesel - Rust
- **リンク**: https://docs.rs/diesel/
- **内容**: DieselのRust公式ドキュメント。APIリファレンス、クエリビルダー、型システム、バックエンド統合の詳細。

### QueryDsl in diesel::query_dsl - Rust
- **リンク**: https://docs.diesel.rs/2.0.x/diesel/query_dsl/trait.QueryDsl.html
- **内容**: QueryDslトレイトのドキュメント。Select文の構築方法、フィルタリング、ソート、グループ化などのクエリメソッド。

### Getting Started with Diesel
- **リンク**: https://diesel.rs/guides/getting-started
- **内容**: Dieselの入門ガイド。環境設定、マイグレーション、スキーマ定義、基本的なCRUD操作のチュートリアル。

### A Guide to Rust ORMs in 2024 | Shuttle
- **リンク**: https://www.shuttle.dev/blog/2024/01/16/best-orm-rust
- **内容**: RustのORM比較記事。Diesel vs SeaORMの詳細な比較、特徴、長所と短所の分析。

## 7. SeaORM（Rust）関連資料

### SeaORM 🐚 An async & dynamic ORM for Rust
- **リンク**: https://www.sea-ql.org/SeaORM/
- **内容**: SeaORMの公式ウェブサイト。非同期サポート、動的なORM、リレーショナルデータベースのサポート。

### GitHub - SeaQL/sea-orm: 🐚 An async & dynamic ORM for Rust
- **リンク**: https://github.com/SeaQL/sea-orm
- **内容**: SeaORMのGitHubリポジトリ。実装、コード例、ActiveModelパターン、非同期操作のサンプル。

### Custom Active Model | SeaORM 🐚 An async & dynamic ORM for Rust
- **リンク**: https://www.sea-ql.org/SeaORM/docs/advanced-query/custom-active-model/
- **内容**: カスタムActiveModelの実装。IntoActiveModelトレイト、部分的なフィールド定義、REST APIでの使用例。

### Entity Structure | SeaORM 🐚 An async & dynamic ORM for Rust
- **リンク**: https://www.sea-ql.org/SeaORM/docs/0.11.x/generate-entity/entity-structure/
- **内容**: エンティティ構造の詳細。Model、Relation、ActiveModelBehaviorの定義、PostgreSQL配列型のサポート。

### Architecture | SeaORM 🐚 An async & dynamic ORM for Rust
- **リンク**: https://www.sea-ql.org/SeaORM/docs/0.9.x/internal-design/architecture/
- **内容**: SeaORMのアーキテクチャ設計。レイヤードアーキテクチャ、データベース非依存の設計思想、実行時設定の柔軟性。

### Rust Ecology Watch | SeaORM: To do the Rust version of ActiveRecord - Moment For Technology
- **リンク**: https://www.mo4tech.com/rust-ecology-watch-seaorm-to-do-the-rust-version-of-activerecord.html
- **内容**: SeaORMの技術的な詳細分析。Entity、ActiveModel抽象、コード生成、マクロの実装について。

## 8. その他の参考資料

### What is object-relational mapping (ORM)? – TechTarget Definition
- **リンク**: https://www.theserverside.com/definition/object-relational-mapping-ORM
- **内容**: ORMの基本的な定義と概念説明。メタデータディスクリプター、抽象化レイヤー、CRUDオペレーションについて。

### Organizational Research Methods: Sage Journals
- **リンク**: https://journals.sagepub.com/home/orm
- **内容**: 組織研究方法論の学術ジャーナル。査読付き研究論文、定量的・定性的研究方法論。

### ORM architecture | Documentation | DWKit
- **リンク**: https://dwkit.com/documentation/orm/orm-architecture/
- **内容**: DWKitのORMアーキテクチャ。動的オブジェクト、EntityModel、データモデル、フォームマッピングについて。

### ORM: Everything You Need to Know When Assessing ORM Skills
- **リンク**: https://www.alooba.com/skills/concepts/database-and-storage-systems/relational-databases/orm/
- **内容**: ORMスキルの評価ガイド。主要コンポーネント、エンティティと関係、クエリ言語、トランザクション管理についての概要。

### Analysis of Different ORM Tools for Data Access Object Tier Generation: A Brief Study
- **リンク**: https://www.academia.edu/124351645/Analysis_of_Different_ORM_Tools_for_Data_Access_Object_Tier_Generation_A_Brief_Study
- **内容**: ORMツールの比較研究。データアクセス層の生成、3層アーキテクチャ、MVCパターンにおけるORMの役割。
