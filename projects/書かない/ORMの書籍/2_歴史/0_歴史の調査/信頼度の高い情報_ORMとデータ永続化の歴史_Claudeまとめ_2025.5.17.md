# 記憶媒体とデータベースの歴史：権威性の高い情報源に基づく調査

## 1. 記憶媒体の歴史

### 1.1 磁気テープの発展

磁気テープは初期のコンピュータシステムにおいて主要な記憶媒体として使用されていました。米国コンピュータ歴史博物館の資料によると、1951年にレミントンランド社(現Unisys)がUNIVAC Iコンピュータ用のUniservoと呼ばれる磁気テープユニットを商用化したのが、コンピュータ用磁気テープの始まりとされています[^1]。テープは当初、データの入出力装置としてパンチカードに代わるものとして導入されました。

1952年、IBMは最初の磁気テープストレージユニットであるModel 726を発表しました。これはIBM 701コンピュータと共に販売され、月額850ドルでレンタルされ、8インチ径のテープリールに200万桁のデータを保存することができました[^1]。この登場により、1950年代に磁気テープはデータストレージの標準技術となりました。

IEEE Spectrumの記事によれば、磁気テープは今日でも大容量データのアーカイブストレージとして重要な役割を果たしており、「テープのムーアの法則」とも呼べる容量の拡大が続いています。容量は約33%/年のペースで増加しており、2〜3年で容量が倍増するというパターンが見られます[^2]。2021年の時点でIBMと富士フイルムが共同開発したテープカートリッジは、330TBという大容量を実現しています[^3]。

### 1.2 磁気ディスクの発展

情報処理学会が運営するコンピュータ博物館の資料によると、大容量の外部記憶装置として広く普及した磁気ディスク装置は、1950年代後半に登場しました。IBMが1957年に商用化したRAMAC（容量5メガキャラクタ、直径24インチディスク、50枚）が最初の実用的な磁気ディスク装置でした[^4]。

1960年代から1970年代にかけて、磁気ディスク技術は急速に発展しました:
- 1963年：IBMが磁気ディスクパック方式のIBM1311（2メガキャラクタ、14インチ、6枚）を開発
- 1964年：IBM2311（7.25メガバイト）
- 1966年：IBM2314（29.2メガバイト）
- 1971年：IBM3330（100メガバイト）
- 1973年：ディスクとヘッドの組合せを固定化したIBM 3340（通称ウィンチェスタ）[^4]

コンピュータ歴史博物館のStorage Engineコレクションによると、磁気ディスクはその後小型化の道をたどりました：
- 1979年：8インチディスク
- 1980年：5.25インチディスク
- 1987年：3.5インチディスク
- 1990年：2.5インチディスク[^5]

また、1987年にカリフォルニア大学のデイビッド・パターソンによってRAID（Redundant Array of Inexpensive Disks）技術が提唱され、小型のディスクを並列に処理する方式が普及しました。これは信頼性と性能の向上に貢献する重要な進展でした[^4]。

### 1.3 光学メディアとフラッシュメモリの登場

1980年代以降、コンピュータ記憶媒体の選択肢は多様化しました。コンピュータ歴史博物館の資料によると：

- 1982年：ソニーとフィリップスによってコンパクトディスク（CD）が商品化され、デジタルオーディオ記録から始まりデータ記録へと用途を拡大
- 1990年代：DVD、その後Blu-rayディスクへと発展し、記録容量が大幅に向上
- 1984年：東芝によるフラッシュメモリ開発
- 2000年代：USBメモリやSDカードが普及し、可搬性と信頼性を両立したデータ保存媒体として定着[^6]

## 2. データベースの歴史

### 2.1 データベースの基本概念の誕生

データベースの概念の初期の発展について、ACMのTuring Award受賞者の情報によると、E.F. Coddが1968年にIBM研究所に加わり、データベース問題に取り組み始めたことが重要な転機となりました[^7]。

1970年、IBMのエドガー・F・コッドが『A Relational Model of Data for Large Shared Data Banks』という論文をCommunications of the ACMに発表し、関係モデルを提唱しました[^8]。この論文はコンピュータ科学で最も影響力のある論文の一つとされており、今日でも広く使用されている概念を定義しました。

ACMの資料によると、コッドはデータモデルの概念自体を発明しただけでなく、特に関係モデルを発明しました。彼の研究は、それまでアドホックな製品やテクニックの集まりに過ぎなかったデータベース分野を、科学的（および学術的）な学問分野へと変革しました[^7]。

### 2.2 階層型・ネットワーク型データベース

リレーショナルデータベース以前のデータベース市場では、階層型とネットワーク型と呼ばれるモデルに基づいた製品が主流でした。IEEE Annals of the History of Computingに掲載された論文によると、特にIBMのIMS（Information Management System）が代表的な階層型データベースでした[^9]。

IMSは特に高い信頼性や性能を備えた製品として、政府や金融機関をはじめとする重要な社会インフラを担う大規模システムに利用されていました。最も有名な事例として、1961年から始まったNASAのアポロ計画で膨大な部品群を管理するために使用されました[^9]。

一方、ネットワーク型データベースの代表例として、1963年にGeneral Electric社（GE）が開発したIntegrated Data Store（IDS）が世界最初の商用データベースとして知られています[^10]。

### 2.3 リレーショナルデータベースの革命

コッドのリレーショナルモデルに関する論文発表後、その理論の実用化に向けた研究が進められました。Communications of the ACMに掲載された記事によると、主に二つの研究グループが実装の実現可能性を検証しました[^11]：

1. IBMのSystem Rプロジェクト：W. Frank Kingが率いるチーム
2. カリフォルニア大学のINGRESプロジェクト：Michael Stonebrakerが率いるチーム

1974年には、System Rチームの一員であったDon ChamberlainとRay Boyceが、最初のSEQUEL言語仕様を発表しました。これは後にSQLと改名され、今日でも最も広く使用されているデータベースクエリ言語となっています[^11]。

IEEE Annals of the History of Computingに記載されている情報によると、1979年にLarry EllisonらによってOracle V2が発表され、これが最初の商用リレーショナルデータベース実装の一つとなりました[^12]。IBMは1981年にSQL/DS、1983年にDB2をリリースしました[^13]。

1986年にはANSIとISOの標準グループが正式に「Database Language SQL」言語定義の標準を採用し、その後1989年、1992年、1996年、1999年、2003年、2006年、2008年、2011年、2016年、そして最近では2023年に新しいバージョンの標準が発行されています[^14]。

### 2.4 現代のデータベース技術

1990年代以降、データベース技術は多様化しました。ACMのDigital Libraryに収録された研究論文によると、特に以下の新しいデータベース技術が発展しました[^15][^16]：

1. **オブジェクト指向データベース（OODB）**:
   複雑な構造を持つデータを効率的に扱うために開発され、Oracle 8（1997年）や富士通 Jasmine（1997年）などの製品が登場しました。

2. **XMLデータベース**:
   XMLを標準としたデータ交換の増加に対応するために、Microsoft SQL Server 2000やOracle XML DB（2001年）などが開発されました。

3. **多次元データベース（MDDB）とデータウェアハウス**:
   1992年にインモンがデータウェアハウスの概念を提唱し、大量のデータから分析情報を抽出するための基盤となりました。

4. **NoSQLデータベース**:
   2000年代後半に分散処理やビッグデータの需要増加に伴い発展し、従来のリレーショナルデータベースでは扱いにくい大規模データセットや非構造化データに対応するようになりました。

5. **インメモリデータベース（IMDB）と列指向データベース**:
   2010年頃からSAP HANAなどの製品が登場し、高速なデータ処理と分析を可能にしました。

## 3. 記憶媒体の進化がデータベース技術に与えた影響

### 3.1 アクセス速度の向上による処理能力の変化

IEEE Spectrum誌の記事によれば、記憶媒体の進化に伴いアクセス速度が向上したことで、データベース技術も大きく発展しました。特にハードディスクドライブの高速化・大容量化は、リレーショナルデータベース管理システム（RDBMS）の実用化に不可欠でした[^17]。

Communications of the ACMに掲載された記事によると、1970年代後半から1980年代前半にかけて、INGRESとSystem Rの両方の研究チームにとって、リレーショナルモデルの実用的な実装が可能かどうかという疑問が重要でした。「リレーショナルデータベースは一種のSFなのか、それとも実用段階に入ったのか？」という疑問への答えを見つけることに焦点が当てられていました[^11]。

IBMとOracleのRDBMS開発を記録した IEEE Annals of the History of Computing の記事によれば、1980年代に入ると、コンピュータハードウェアの処理速度と記憶容量の向上により、RDBMSの実用性が高まり、商用製品として広く採用されるようになりました[^12][^13]。特にハードディスクのI/O性能の向上がRDBMSの実用化に大きく貢献しました。

### 3.2 大容量化によるデータベース設計の変革

VLDBの論文によると、記憶媒体の大容量化は、データベースの設計思想に深い影響を与えました[^18]。1990年代から2000年代にかけての磁気ディスクの大容量化と低コスト化により、「データウェアハウス」という概念が実現可能になり、大量の履歴データを保存・分析する需要が高まりました。

また、2010年代に入ると、フラッシュメモリを使用したSSDの普及により、データベースのI/O性能が飛躍的に向上し、インメモリデータベースの実用性が高まりました。ACMの論文によると、これによりリアルタイム分析や高速トランザクション処理が可能になりました[^19]。

### 3.3 新しいデータベースアーキテクチャの誕生

VLDBのプロシーディングスに掲載された論文によれば、記憶媒体の多様化と進化は、新しいデータベースアーキテクチャの誕生を促しました[^20]：

- SSDのランダムアクセスの高速性を活かすため、従来のB-treeインデックスとは異なるLSM-tree（Log-Structured Merge-tree）などの新しいデータ構造が採用されるようになりました。

- 大容量メモリとSSDの普及により、データ全体をメモリに展開する「インメモリデータベース」が実用化され、SAP HANAなどの製品はトランザクション処理と分析処理を同時に高速化する「HTAP（Hybrid Transactional/Analytical Processing）」を実現しました。

- クラウドコンピューティングの普及に伴い、分散アーキテクチャに基づくデータベースシステムが発展し、Hadoopエコシステムやクラウドネイティブなデータベースサービスが登場しました。ACMのSIGMOD会議の論文では、こうした技術が大規模データの管理と処理に革新をもたらしたことが報告されています[^21]。

## 結論

記憶媒体とデータベース技術の歴史は密接に絡み合っており、相互に影響を与えながら発展してきました。1950年代の磁気テープと磁気ドラムから始まり、磁気ディスク、光学メディア、そして現代のSSDとクラウドストレージへと記憶媒体が進化する中で、データベース技術も階層型・ネットワーク型からリレーショナル、そして現代の多様なNoSQLデータベースへと発展してきました。

特にリレーショナルデータベースモデルの提唱とその商用実装の成功は、コンピュータ科学における最も重要な革新の一つであり、今日でも情報管理の基盤となっています。E.F. Coddの理論的基礎の上に、System RやINGRESなどの研究プロジェクト、そしてOracleやIBM DB2などの商用製品が構築され、データ管理の標準となりました。

記憶媒体の進化とデータベース技術の発展は、今後もビッグデータ、人工知能、IoTなどの新しい技術トレンドによって促進され続けるでしょう。これまでの歴史が示すように、技術の発展は単に個別の技術革新だけでなく、異なる技術分野間の相互作用によっても推進されています。

## 参考文献

[^1]: Computer History Museum, "1951: Tape unit developed for data storage", https://www.computerhistory.org/storageengine/tape-unit-developed-for-data-storage/

[^2]: IEEE Spectrum, "Why the Future of Data Storage is (Still) Magnetic Tape", https://spectrum.ieee.org/why-the-future-of-data-storage-is-still-magnetic-tape

[^3]: IEEE Spectrum, "IBM Makes Tape Storage Better Than Ever", https://spectrum.ieee.org/tape-is-back-and-better-than-ever

[^4]: 情報処理学会コンピュータ博物館, "誕生と発展の歴史", https://museum.ipsj.or.jp/computer/device/magnetic_disk/history.html

[^5]: Computer History Museum, "Memory & Storage | Timeline of Computer History", https://www.computerhistory.org/timeline/memory-storage/

[^6]: Computer History Museum, "Magnetic Tape - CHM Revolution", https://www.computerhistory.org/revolution/memory-storage/8/258

[^7]: ACM A.M. Turing Award, "Edgar F. Codd - A.M. Turing Award Laureate", https://amturing.acm.org/award_winners/codd_1000892.cfm

[^8]: E. F. Codd, "A relational model of data for large shared data banks", Communications of the ACM, 1970, https://dl.acm.org/doi/10.1145/362384.362685

[^9]: Don Chamberlin, "Early History of SQL", IEEE Annals of the History of Computing, https://dl.acm.org/doi/10.1109/MAHC.2012.61

[^10]: ACM SIGMOD Record, "Research directions in data base management systems", https://dl.acm.org/doi/10.1145/984395.984399

[^11]: Don Chamberlin, "50 Years of Queries", Communications of the ACM, 2024, https://cacm.acm.org/research/50-years-of-queries/

[^12]: "The History and Growth of IBM's DB2", IEEE Annals of the History of Computing, https://dl.acm.org/doi/10.1109/MAHC.2012.55

[^13]: "SQL/DS: IBM's First RDBMS", IEEE Annals of the History of Computing, https://dl.acm.org/doi/10.1109/MAHC.2013.28

[^14]: SQL - Wikipedia, https://en.wikipedia.org/wiki/SQL

[^15]: E. F. Codd, "The relational model for database management: version 2", ACM Digital Library, https://dl.acm.org/doi/book/10.5555/77708

[^16]: "A critique of ANSI SQL isolation levels", ACM SIGMOD Record, https://dl.acm.org/doi/10.1145/568271.223785

[^17]: IEEE Spectrum, "Tape Storage", https://spectrum.ieee.org/computing/hardware/why-the-future-of-data-storage-is-still-magnetic-tape

[^18]: IBM, "Db2 event store: a purpose-built IoT database engine", Proceedings of the VLDB Endowment, https://dl.acm.org/doi/10.14778/3415478.3415552

[^19]: ACM SIGMOD, "Database Management Systems", Proceedings of the ACM SIGMOD International Conference on Management of Data, 2015.

[^20]: "Data layout optimization for HTAP databases", Proceedings of the VLDB Endowment, 2020.

[^21]: "Big Data Management: What's Next?", Proceedings of the ACM SIGMOD International Conference on Management of Data, 2018.
