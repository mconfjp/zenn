---
title: MacでRancherDesktop使ってたらunhandled auxillary vector type 29でDockerが起動しない
emoji: 🍕
type: tech
topics:
  - docker
  - macos
  - rosetta
publication_name: "levtech"
published: false

---
## これはなに
こんにちは、レバテック開発部のもりたです。
わたしの所属するチームではRancherDesktopというDockerのツールを使ってローカル環境を構築しています。ただ、昨日ローカル環境のvolumeを削除したところDockerが立ち上がらなくなってしまいました。

``rosetta error: unhandled auxillary vector type 29``

エラーコードを見る感じ、原因は割と深い場所にありそうです。VMとかなんも知らん人間なのでちょっと苦手意識があったのですが、これを機に整理してみました。

:::details 構成
### 構成

- 前提：やさしい用語解説
- 問題
- 解決
- おわりに

エラー解決記事なので問題と解決を書いて終わり！　としたかったんですが、色々と理解していないところが多すぎたので、前提となる知識を整理するコーナーを設けさせていただきました。
:::

## 前提
今回の問題を解決するにはわたしの知識が足りなさすぎたので、色々と前提を整理しました。整理したのは大きく二つ。「機械語の変換」と「Docker（とか）の仕組み」です。

### 機械語の変換
前提の前提になりますが、プログラムのコードは実行前に機械語に変換されCPUによって実行されます。この機械語はどのCPUでも共通のものが使えるわけではなく、CPUのアーキテクチャによって命令の種類が異なります。
以下は例。

```
x86_64（Intel/AMD）の命令
10110000 01100001  ← "Aを表示しろ"

arm64（Apple Silicon）の命令
11010010 10000010  ← "Aを表示しろ"
```

こんな感じで、同じ処理でもビット列は全く異なります。そのため、x86_64向けにコンパイルされたバイナリを、arm64 CPU上で動かそうとしてもクラッシュしてしまいます。

### Docker（とか）の仕組み
続いて、わたしたちの普段利用するDocker（とか）の環境がどう動いているかの整理です。
以下の図はわたしがこれまでに500回くらい見たはずなのに全ての理解を放棄してきたがために理解できてなかった図です。

```
物理的なMac（ハードウェア）
└── macOS
    └── Apple Virtualization Framework（仮想ハードウェアを作る仕組み）
        └── 仮想CPU・仮想メモリ・仮想ディスク ← VM（箱）
            └── Alpine Linux ← VMの中で動くOS
                └── Docker
                    └── コンテナ
```

みなさんも見たことがあるんじゃないでしょうか。よくわかるDockerの仕組み、とかでコンテナって便利だなみたいなキャプションと共に解説され、そうだね〜〜と思いつつ特に理解してないみたいな、そういうアレじゃないでしょうか。
急に7つも構成要素が出てきて困っちゃうと思うのですが、今回注目したいのは**VM**と**Alpine Linux**の部分です。


:::details なぜmacOSでDockerを動かすのにLinuxが必要か
#### なぜmacOSでDockerを動かすのにLinuxが必要か

実はDockerはLinuxのカーネル機能に依存しています。macOSにはそのカーネル機能がないため、macOS上でDockerを動かすには間にLinux環境を挟む必要があります。それが仮想マシン（VM）です。
:::

### 変換が必要なケース
で、わたしのローカル環境で利用していたDocker FileはLinux x86_64を指定していました。

```dockerfile
FROM --platform=linux/x86_64 amazonlinux:2023
```

本番環境（AWS等）がIntel/AMD（x86_64）サーバーで動いているため、ローカルでも同じx86_64イメージを使っている...という具合です。ただし、Apple Silicon MacのCPUがarm64です。x86_64のDockerイメージをそのまま実行することはできません。となった時に、どこかで変換が必要になります。

では、どこで変換することができるでしょうか。先ほどの図で考えてみましょう。

```
物理的なMac（ハードウェア）
└── macOS
    └── Apple Virtualization Framework（仮想ハードウェアを作る仕組み）
        └── 仮想CPU・仮想メモリ・仮想ディスク ← VM（箱）
            └── Alpine Linux ← VMの中で動くOS
                └── Docker
                    └── コンテナ
```

最も簡単なのはローカル環境も本番と同じCPUを使ってしまうことですが、わたしはMacで作業がしたいのでそうはいきません。その他の主要な選択肢は以下の２つです。

- QEMU: VMレベルでの変換
- Rosetta: コンテナレベルでの変換

これらのうち、Rosettaをわたしのチームでは利用していました。これはAppleが提供するx86_64→arm64の変換レイヤーです。macOSの機能として提供されており、今回のシステムではarm64のLima VM（Alpine Linux）の中でRosettaを使い、x86_64のDockerコンテナを動かしています。

---

## 問題
ここからがわたしの遭遇したエラーの解説です。
### 何が起きたか

ことの発端はLima VMのディスクが満杯になり、Rancher Desktopが起動しなくなったことでした。ディスクがいっぱいということで一旦環境をリセットしようと考え、RancherDesktopの機能であるFactory Resetで復旧したところ、今度はDockerコンテナが起動しなくなりました。
その際のエラー文が以下です。

```
rosetta error: unhandled auxillary vector type 29
```

Rosettaのエラーです。変換の箇所でおかしなことが起きていると推測できます。

### 原因
その後の調査で以下のIssueが見つかりました。

参考Issue（未解決）: https://github.com/lima-vm/lima/issues/3592

要点は以下です。
- **AT_MINSIGSTKSZ**（auxiliary vector type 29）はLinux kernel 5.11で追加された情報で、プロセス起動時にカーネルが「シグナル処理に必要な最小スタックサイズ」をRosettaに渡す
- ただ、Linux kernel 6.13以降では、macOS 15.xのRosettaが上記の**AT_MINSIGSTKSZ**（auxiliary vector type 29）を変換できずクラッシュするようになった

## 解決方法

### 方針
というわけで解決策は2つあります。

1. Alpine Linuxのkernelを6.12以下に下げる
	1. そもそも当該Issueが起きないバージョンなら問題はない
2. macOSを新しいバージョンにあげる
	1. Rosettaのバージョンを上げれば変換できるかも？

この２つのうち、前者で対応しました。IssueがまだOpenだったのと、経緯を見る感じLinux側の問題であるように映ったための判断です。
また、これは補足情報ですが、Alpine Linuxのバージョンを下げるためにはRancher Desktopの再インストールで対応しています。ここで言及しているAlpine Linuxはアプリケーションのコンテナで指定するLinux OSのことではなく、VMの直下で動くLinuxです。Rancher Desktopの仕様上、このバージョンはRancher Desktop自体のバージョンに依存するようだったので、Rancher Desktopごと入れ替えることになりました。

### 解決

結局、Rancher Desktop v1.18.2 をインストールしました。

```
https://github.com/rancher-sandbox/rancher-desktop/releases
```

このバージョンになった理由は色々と試行錯誤があるので、以下を参照ください。
#### 罠：Rancher Desktopのバージョンを下げても最新kernelが入ってしまう
Rancher Desktopのバージョンとkernelのバージョンを比較したところ、Rancher Desktopの1.21.0がkernel 6.12だということがわかりました。

| RDバージョン | Alpine | kernel   |
| ------- | ------ | -------- |
| 1.24.0  | 3.23.0 | 6.18（NG） |
| 1.23.1  | 3.23.0 | 6.18（NG） |
| 1.21.0  | 3.22.2 | 6.12（OK） |
| 1.18.2  | 3.20.3 | 6.6（OK）  |
そこでRD 1.21.0をインストールしてFactory Resetしてみたのですが、起動直後はkernel 6.6になるものの、その後kernelが6.18にアップグレードされてしまいました。これはAlpine linuxが起動時に`apk upgrade`を実行し、kernelが最新化されてしまったことが原因...なのではないかと思っています（推測）。
その後もう一段Rancher Desktopのバージョンを下げてみたところ、事象が再発しなくなったため、一旦1.18.2を利用しています。

---

## おわりに
これでこの記事はおしまいです。
ローカル環境の構築でしくじるのって時間を無駄にしている感じがあってかなり苦手だったんですが、これを機会にちゃんと理解を深めるのも悪くはないなと思いました。
以上！