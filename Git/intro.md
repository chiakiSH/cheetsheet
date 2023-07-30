# git

- [git](#git)
  - [TODO](#todo)
  - [概要](#概要)
  - [用語](#用語)
    - [リポジトリ(repository)](#リポジトリrepository)
    - [分散型バージョン管理システム(DVCS, Distributed Version Control System)](#分散型バージョン管理システムdvcs-distributed-version-control-system)
    - [git ホスティングサービス](#git-ホスティングサービス)
  - [リポジトリの内容](#リポジトリの内容)
    - [オブジェクト](#オブジェクト)
    - [参照](#参照)
    - [index](#index)
    - [HEAD](#head)
  - [git コマンド](#git-コマンド)
    - [git init](#git-init)
    - [git clone](#git-clone)
    - [git add](#git-add)
    - [git commit](#git-commit)
    - [git checkout](#git-checkout)
    - [git merge](#git-merge)
    - [git config](#git-config)

---

## TODO

- [refspec?](https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-Refspec)
- [packfile?](https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-Packfile)
-

## 概要

**バージョン管理システム**(VCS, Version Control System)とは, 何らかのファイル・ディレクトリのバージョンを管理するためのシステムのことである. VCS により,

- いつ
- 誰が
- どのような意図で
- どのように

ファイル・ディレクトリに変更を加えたかが明らかとなる. 大規模なプロジェクトにおいては, 多くの開発者が様々な改変を当該ファイル・ディレクトリに加えるため, バージョン管理システムがなければ一貫して開発を行うことは難しい.

**git** は現在最も普及している VCS であり, 数多のコードが git 管理されている. 本稿では git について扱う.

## 用語

#### リポジトリ(repository)

VCS の文脈では, バージョン管理をしたい当該ファイル・ディレクトリの, 過去の変更履歴やその際のコメントなどの包括的な情報が入っている, データの集合体のこと. リポジトリが実際にどのような構造をしているかは個々の VCS の実装に依存するが, git では, `.git`という名前のディレクトリにより管理される.

#### 分散型バージョン管理システム(DVCS, Distributed Version Control System)

VCS のうち, (通常 1 つの)サーバと, 多数のクライアントが存在して, その双方がリポジトリを持っているような形態をもつものをいう.
厳密にいえば, 分散型バージョン管理システムにおいては, (分散型の名の通り)サーバは存在しなくてもよいのだが, 今日の通常の開発ではサーバが存在することが普通である.
サーバ側のリポジトリを**リモートリポジトリ**(remote repository)といい, クライアント側の個々のリポジトリをそれぞれ**ローカルリポジトリ**(local repository)という. 基本的には, 次のような開発が想定されている.

1. まず開発者は, リモートリポジトリを自分の手元にコピーすることで, ローカルリポジトリを作成する.
2. 開発者は作業することでローカルリポジトリに変更を加える. すると, リモートリポジトリとローカルリポジトリの内容に差異が生じる.
3. 開発者は変化したローカルリポジトリの内容をリモートリポジトリに送信する. すると, リモートリポジトリには自分の作業内容が反映されることになる.
4. 多数の開発者が同じことを行う.
5. 他の開発者が行ったリモートリポジトリに対する変更を自身のローカルリポジトリに取り込むことで, 他の開発者の進捗を共有する.
6. 2.に戻る.

VCS は大きく分けて集中型と分散型に分類され, 集中型ではリモートリポジトリしか存在せず, 全員が直接リモートリポジトリを編集することになる. これに対する分散型のメリットとしては,

- リモートリポジトリとの通信以外の操作は, ローカルリポジトリ上で自己完結的に行える.
- リモートリポジトリに変更を送信する前のいくつかのバージョンをローカルリポジトリに保持できる.

などがある.

git は分散型バージョン管理システムである.

#### git ホスティングサービス

リモートリポジトリを管理するために, git ホスティングサービスを利用することがよくある. git ホスティングサービスには,

- **GitHub**
- GitLab
- Bitbucket
- Azure DevOps

などが知られている(他にもいろいろある). git ホスティングサービスを使うことで, サーバ側に git 単独では存在しない機能を追加してくれる. 例えば,

- **プルリクエスト**(PR, Pull Request)
  自分が送信した変更を, リモートリポジトリ側に反映してほしいという提案のこと. 直ちに反映せずにプルリクエストという手順を踏むことで, 統制の取れた開発やコードレビューの機会が提供される.
- Wiki, issues の共有
  既知のバグや情報の共有, 今後のロードマップの共有などができる.
- **CI/CD**(Continuous Integration/Continuous Delivery)
  自動的にテストを行い, 現在のリポジトリがバグを含んでいないかを検証する.
- 設定
  開発チーム以外がリポジトリを破壊しないようにセキュリティを設定したり, 開発者に権限を設定することで開発の統制をとる.

といった機能は git 単独では利用することができないが, git ホスティングサービスにより提供されることがある.

## リポジトリの内容

`.git`ディレクトリの中身で, 重要なものは以下の 4 つ.

- objects ディレクトリ
  git 管理されているデータのすべてのコンテンツが格納される
- refs ディレクトリ
  オブジェクトを指す参照が格納される
- index ファイル
  ステージングエリアの情報が格納される
- HEAD ファイル
  現在チェックアウトしているブランチの情報が格納される

#### オブジェクト

blob オブジェクト：ファイルの内容のみ
tree オブジェクト：
commit オブジェクト：
tag オブジェクト：

`git cat-file -t (SHA-1)`... オブジェクトタイプが見れる. blob, tree など...
`git cat-file -p (SHA-1)`... オブジェクトの情報を整形して読める

- blob: ファイルの中身
  ```
  some code here
  ```
- tree: その中に含まれるディレクトリ構造
  ```
  040000 tree 0423ed44b4eb8e514736e7f59510a75b154a6b31    folder
  100644 blob e32092a83f837140c08e85a60ef16a6b2a208986    test.txt
  ```
- commit: ディレクトリのスナップショットを示す tree オブジェクト, 親 commit オブジェクト, コミットの情報, コミットメッセージ

  ```
  tree 4768aa8b08a25a6e9e2bd52d51791a8234e3845b
  parent 761eb7068362b3f6ee4e3161c6f446de30a41ba2
  author mochix2 <50793071+mochix2@users.noreply.github.com> 1690656983 +0900
  committer mochix2 <50793071+mochix2@users.noreply.github.com> 1690656983 +0900

  add .vscode to gitignore
  ```

- tag: 任意のオブジェクトに対する, 注釈つきタグの具体的内容

  ```
  object 0423ed44b4eb8e514736e7f59510a75b154a6b31
  type tree
  tag nice_tag
  tagger mochix2 <50793071+mochix2@users.noreply.github.com> 1690659118 +0900

  really nice tag
  ```

```
hash-object: ファイルからblobを作る
cat-file: オブジェクトを見る
update-index: （ステージングエリアを表す）indexを更新
write-tree: indexからtreeを作る
read-tree: treeからステージングエリアを呼び出す(mergeが起こる？)
commit-tree: treeからcommitオブジェクトをつくる

```

オブジェクトは, 適切な方法により元のファイル内容を加工し, SHA-1 が計算され, zlib により圧縮されてバイナリとなる.

#### 参照

参照とは, 単に SHA-1 が 1 行入っており, それにより特定のオブジェクトを指すことができるファイルのことである. 参照には

- ブランチ
  `.git/refs/heads/(branch_name)`という名前のファイルとして格納されており, ブランチの先頭の commit オブジェクトを指す.
- タグ
  `.git/refs/tags/(tag_name)`という名前のファイルとして格納されており, commit オブジェクトか tag オブジェクトを指す.
- リモート
  `.git/refs/remotes/(remote_name)/(branch_name)`という名前のファイルとして格納されており, 最後にリモートと通信したときのリモートブランチが指していた commit オブジェクトを指す.
  の 3 種類がある.

#### index

現在のステージングエリアにある
`git ls-files --stage`により,

#### HEAD

現在作業中のブランチに対する「シンボリック参照」である.
以下のような形式で, ブランチ名が平文で書いてある.

```
ref: refs/heads/master
```

## git コマンド

ヘルプは`git <verb> --help`で確認可能.

#### git init

そのディレクトリを git 管理下におく. 具体的には`.git`ディレクトリを作成する.

```
git init
```

#### git clone

既存の git リポジトリにアクセスして, コピーを取得する.

```
git clone https://github.com/xxxxxx/yyyyyy
```

#### git add

適切に blob, tree オブジェクトを作り保存
index に登録

#### git commit

#### git checkout

#### git merge

#### git config

git インストール時に設定すべき項目を設定しておく.
項目一覧は`git config --list`で確認可能.

```
git config --global user.name "Nobita Nobi"
git config --global user.email "nobita@gmail.com"
git config --global core.editor "C:/Program Files/... code.exe"
```
