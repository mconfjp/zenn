# ORM主要製品の歴史的調査 - 参考資料のまとめ

## 1. EOF (Enterprise Objects Framework) 関連資料

### [1] Apple Developer Documentation: "Enterprise Objects Framework Developer's Guide" (1997)
- **リンク**: 
	- https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_5/EOFDevelopersGuide/EOFDeveloperGuide.pdf
	- https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_4.0/System/Documentation/Developer/EnterpriseObjects/Guide/EOFDevGuide.pdf
- **内容**: EOFの公式開発者ガイド。オブジェクト永続化の概念、モデリングツールの使用方法、フレームワークのアーキテクチャを詳細に説明。

### [2] Brad Cox, "Planning the Software Industrial Revolution" IEEE Software, Nov 1990
- **リンク**: https://ieeexplore.ieee.org/document/60585
- **内容**: オブジェクト指向プログラミングの工業化についての論文。EOFの思想的背景となる、ソフトウェア部品化の概念を提唱。

## 2. TopLink 関連資料

### [3] Shaun Smith and Doug Clarke, "TopLink: An Object-Relational Persistence Architecture" OOPSLA '97
- **リンク**: https://dl.acm.org/doi/10.1145/263698.263841
- **内容**: TopLinkのアーキテクチャを説明した学術論文。オブジェクト-リレーショナルマッピングの技術的課題と解決策を詳述。

### [4] Linda DeMichiel et al., "JSR 220: Enterprise JavaBeans 3.0" (JPA仕様)
- **リンク**: https://jcp.org/en/jsr/detail?id=220
- **内容**: Java Persistence API の仕様書。TopLinkが参照実装として採用され、業界標準となった。

## 3. Hibernate 関連資料

### [5] Christian Bauer and Gavin King, "Hibernate in Action" (2004), Manning Publications
- **ISBN**: 193239415X
- **内容**: Hibernateの創設者による実践的な解説書。フレームワークの設計思想と使用方法を包括的に説明。

### [6] Linda DeMichiel, "JSR 317: Java Persistence 2.0"
- **リンク**: https://jcp.org/en/jsr/detail?id=317
- **内容**: JPA 2.0仕様書。HibernateがJPA標準に与えた影響が反映された内容。

### Gavin King, "The Hibernate Reference Documentation" (2003)
- **リンク**: https://docs.jboss.org/hibernate/orm/3.3/reference/en-US/html/
- **内容**: Hibernate 3.3の公式リファレンスドキュメント。

## 4. ActiveRecord 関連資料

### [7] Martin Fowler, "Patterns of Enterprise Application Architecture" (2002)
- **リンク**: https://www.martinfowler.com/eaaCatalog/activeRecord.html
- **内容**: Active Recordパターンの定義。データベースの行とオブジェクトを1対1で対応させる設計パターンを説明。

### [8] Rails Guides, "Active Record Basics"
- **リンク**: https://guides.rubyonrails.org/active_record_basics.html
- **内容**: Ruby on RailsのActiveRecord公式ガイド。フレームワークの基本概念と使用方法を解説。

## 5. SQLAlchemy 関連資料

### [9] Michael Bayer, "SQLAlchemy - The Database Toolkit for Python" (2012) - PyCon講演
- **リンク**: https://www.youtube.com/watch?v=woKYyhLCcnU
- **内容**: SQLAlchemyの創設者による講演。設計思想とPythonコミュニティでの位置づけを説明。

### [10] Jason Myers and Rick Copeland, "Essential SQLAlchemy" (2015), O'Reilly Media
- **ISBN**: 978-1491916469
- **内容**: SQLAlchemyの包括的な解説書。基本から高度な使用法まで網羅。

### SQLAlchemy Documentation
- **リンク**: https://docs.sqlalchemy.org/
- **内容**: 公式ドキュメント。APIリファレンスとチュートリアル。

## 6. Doctrine 関連資料

### [11] Roman S. Borschel, Guilherme Blanco, "Doctrine 2: Enterprise Persistence Layer for PHP" (2010)
- **リンク**: https://www.doctrine-project.org/projects/doctrine-orm/en/2.7/index.html
- **内容**: Doctrine 2の詳細なドキュメント。Data Mapperパターンの実装を解説。

### Jonathan H. Wage, "Doctrine 2 ORM Documentation"
- **リンク**: https://www.doctrine-project.org/projects/doctrine-orm/en/2.7/index.html
- **内容**: Doctrine 2の公式ドキュメント。Entity Manager、DQLなどの核心概念を説明。

## 7. Entity Framework 関連資料

### [12] Julia Lerman, "Programming Entity Framework" (2010), O'Reilly Media
- **ISBN**: 978-0596807269
- **内容**: Entity Frameworkの包括的な解説書。Code First、Database Firstアプローチを詳説。

### [13] ADO.NET Team Blog, "Introducing Entity Framework" (2008)
- **リンク**: https://devblogs.microsoft.com/dotnet/introducing-entity-framework/
- **内容**: Entity Frameworkの初期発表ブログ。設計目標と.NETエコシステムでの位置づけを説明。

### Microsoft Documentation, "Entity Framework Documentation"
- **リンク**: https://docs.microsoft.com/en-us/ef/
- **内容**: 公式ドキュメント。チュートリアルとAPIリファレンス。

## 8. Sequelize 関連資料

### [14] Sequelize Documentation
- **リンク**: https://sequelize.org/master/
- **内容**: 公式ドキュメント。Node.jsでのORM使用方法、Promise/async-awaitとの統合を説明。

### Tom Dykstra, "Sequelize: Getting Started" (2016)
- **リンク**: https://sequelize.org/master/manual/getting-started.html
- **内容**: Sequelizeの入門ガイド。基本的な使用方法とベストプラクティス。

## 9. Dapper 関連資料

### [15] Sam Saffron, "Introducing Dapper" (2011)
- **リンク**: https://samsaffron.com/archive/2011/03/30/How+I+learned+to+stop+worrying+and+write+my+own+ORM
- **内容**: Dapper開発の動機とマイクロORMの概念を説明したブログ記事。

### [16] Stack Overflow Blog, "Dapper, a Simple Object Mapper for .NET"
- **リンク**: https://stackoverflow.blog/2011/06/13/dapper-a-simple-object-mapper-for-net/
- **内容**: Stack OverflowでのDapper採用事例とパフォーマンス特性の説明。

### Dapper GitHub Repository
- **リンク**: https://github.com/DapperLib/Dapper
- **内容**: ソースコードとドキュメント。使用例とベンチマーク結果。

## 10. GORM 関連資料

### [17] GORM Documentation
- **リンク**: https://gorm.io/docs/
- **内容**: 公式ドキュメント。Go言語でのORM使用方法、タグベースのマッピングを解説。

### Go GitHub Issue Discussion on ORMs
- **リンク**: https://github.com/golang/go/issues/15213
- **内容**: Go言語コミュニティでのORM必要性についての議論。

## 11. Diesel 関連資料

### [18] Sean Griffin, "Diesel: A Safe, Extensible ORM and Query Builder for Rust" (2018) - RustConf
- **リンク**: https://www.youtube.com/watch?v=qUe-OQNxc_M
- **内容**: Dieselの設計思想とRustの型システム活用方法についての講演。

### [19] Diesel Documentation
- **リンク**: http://diesel.rs/
- **内容**: 公式ドキュメント。コンパイル時型安全性の実現方法を詳説。

### "Type-safe SQL in Rust" (2017) - Rust Blog Post
- **リンク**: https://blog.rust-lang.org/2017/05/15/rust-and-postgres.html
- **内容**: Rustの型システムを活用したSQL操作の安全性について解説。

## 12. TypeORM 関連資料

### [20] TypeORM Documentation
- **リンク**: https://typeorm.io/
- **内容**: 公式ドキュメント。デコレータベースのマッピング、Active RecordとData Mapperパターンの実装を解説。

### [21] Umed Khudoiberdiev, "TypeORM - Amazing ORM for TypeScript and JavaScript" (2017)
- **リンク**: https://medium.com/@umed.khudoiberdiev/typeorm-amazing-orm-for-typescript-and-javascript-es7-es6-es5-843b8db5c50c
- **内容**: TypeORM創設者によるフレームワーク紹介記事。設計思想と特徴を説明。

## 13. Prisma 関連資料

### [22] Johannes Schickling, "Prisma: Next-generation ORM for Node.js and TypeScript" (2020) - GraphQL Conf
- **リンク**: https://www.youtube.com/watch?v=RDpCSNaAdwM
- **内容**: Prismaの設計思想と次世代ORMアプローチについての講演。

### [23] Prisma Documentation
- **リンク**: https://www.prisma.io/docs/
- **内容**: 公式ドキュメント。Prismaスキーマ言語、型安全なクライアント生成を解説。

### "Modern Database Access for TypeScript & Node.js" (2020) - Prisma Blog
- **リンク**: https://www.prisma.io/blog/modern-database-access-for-typescript-node-js-introducing-prisma-client-421d9d0d36ae
- **内容**: Prismaクライアントの紹介記事。型安全性と開発者体験の向上を説明。

## その他の関連資料

### Oracle TopLink Developer's Guide 11g Release 1 (11.1.1)
- **リンク**: https://docs.oracle.com/cd/E13224_01/wlw/docs103/guide/toplink/index.html
- **内容**: TopLinkの詳細な技術ドキュメント。キャッシュ管理、クラスタ対応などを解説。

### NeXT Computer, Inc., "Enterprise Objects Framework: A Second Generation Object-Relational Persistence Framework" (1994)
- **内容**: EOFの技術白書。オブジェクト永続化フレームワークの第二世代としての位置づけを説明。

### Sascha Depold, "Sequelize - The Node.js ORM" (2014) - Node.js Conference講演
- **内容**: SequelizeのNode.jsエコシステムでの役割と非同期プログラミングとの統合を説明。

### Jinzhu, "GORM - The Fantastic ORM Library for Golang" (2016) - GopherCon China
- **内容**: GORMの設計思想とGo言語の特性を活かした実装についての講演。

### Benjamin Eberlei, "Persistence in PHP with Doctrine ORM" (2013), SitePoint
- **内容**: Doctrineの実践的な使用方法とPHP開発での活用例を解説。

### David Heinemeier Hansson, "Active Record - Object-Relational Mapping Put on Rails" (2004) - RailsConf講演
- **内容**: ActiveRecordの設計思想とRailsフレームワークでの統合を説明。

### "Building Node.js Apps with TypeORM" (2018) - LogRocket Blog
- **内容**: TypeORMを使用したNode.jsアプリケーション開発の実践ガイド。
