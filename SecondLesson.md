Git ドリル 第2回(Gitを使った運用)
===========

中央管理型ワークフロー
-----------

中央リポジトリを設定して、そこが開発メンバー全員のpush/pull先にします。
開発メンバー作業している間に中央リポジトリが更新されている可能性があるので、pushする前にpullしてローカルのリポジトリのmasterを最新の状態にして、ローカルでmergeしてから、pushします。
**Pushする前にローカルのmasterを最新に!**

参考
-  https://www.atlassian.com/ja/git/workflows#!workflow-centralized

[<img src="https://www.atlassian.com/ja/git/workflows/pageSections/00/contentFullWidth/0/tabs/00/pageSections/09/contentFullWidth/0/content_files/file1/document/git-workflow-svn-push-local.png" style="background-color: white">](https://www.atlassian.com/ja/git/workflows/pageSections/00/contentFullWidth/0/tabs/00/pageSections/09/contentFullWidth/0/content_files/file1/document/git-workflow-svn-push-local.png)

フィーチャーブランチ
------------------


Gitflow
-----------
Vincent Driessenさんが提起したワークフローです。
ベストプラクティスのうちの１つと言われていて、おそらくどんな局面にも対応できそう。

メインブランチ: 中央リポジトリとそれをcloneした各開発者のリポジトリに存在するブランチ
- master リリースするためのブランチ
- develop 開発用ブランチ。リリース前の最新バージョン

サポートブランチ: 原則、各開発メンバーのリポジトリにだけ存在し、役目が終わったら消えるブランチ
- feature developブランチから派生し、特定の機能開発のために作成される。その特定の機能の開発が終わったらdevelopにマージされて消える。
- release developブランチから派生し、リリースのための準備をするブランチ。リリースのための準備作業中、余計なfeatureが混ざらないようにするために作る。リリースが終わればmasterとdevelopにマージされて消える。
- hotfix masterから派生し、リリース済みのサービスに障害があったときなどの緊急用ブランチ。バグを直したらmasterにマージされて消える。この修正がもれないように、developやreleaseにも適宜マージする。



[<img src="http://nvie.com/img/git-model@2x.png" style="background-color:white">](http://nvie.com/img/git-model@2x.png)


メリット
- どんな局面にも対応できるので、大人数で長期的にメンテナンスするプロジェクトに向いてそう

デメリット
- 覚えること多すぎ

参考
-  http://keijinsonyaban.blogspot.jp/2010/10/successful-git-branching-model.html

Github flow
-------------
Scott Chaconさんが、git-flowに問題提起しつつ、提案したワークフロー。
GitHubを使ったワークフロー。

[<img src="http://nicoespeon.com/assets/img/git/github-flow-branching-model.jpg" style="background-color=white">](http://nicoespeon.com/assets/img/git/github-flow-branching-model.jpg)

> - masterのものは全てリリース可能
> - 新しく何かをするときはmasterから直接ブランチを作る
> - 作成したブランチはローカルマシンにコミットして、リモートリポジトリにも同じ名前のブランチとして定期的にPushする
> - 開発が完了したらmasterへPull Requestを送る
> - Pull Requestがレビューされたらmasterにマージし、その場で本番環境にリリースする

「チーム開発実践幽門」p86 から引用

開発者が覚えることは3つ
- masterはリリース用だからさわらない
- 作業を始める前にブランチを切ってから始める
- 作業が終了したらmasterにPull Requestする

メリット
- Webサービスなどスピード重視のプロジェクトに向いている
- git-flowより覚えることが少ない

デメリット
- Continuous Integration（自動テストなど）を実現する環境が必要
- Pull Requestからマージするときの作業負荷


フォーク型ワークフロー
---------------------
Githubなどの分散型バージョン管理システムを使用したフロー。

ワークフローの流れ
- プロジェクトメンテナーが公式リポジトリを作成
- 開発者は公式リポジトリをフォーク
- 開発者はフォークしたリポジトリをクローン
- 開発者がそれぞれのリポジトリでfeatureブランチを作成して開発する
- 開発者がfeatureブランチをフォークした自分のリポジトリへ公開
- 開発者がfeatureブランチについて、プロジェクトメンテナーへpull requestを送る
- プロジェクトメンテナーが公式リポジトリに開発者のfeatureを統合する
- 他の開発者が公式リポジトリの変更を同期

<img src="http://kawaken.github.io/progit/figures/18333fig0502-tn.png" style="backgroud-color=white">(http://kawaken.github.io/progit/figures/18333fig0502-tn.png)

メリット
- 開発者は自由に開発できて、メンテナーはいつでも好きなタイミングで変更を取り込める

デメリット
- しいて言うならメンテナーがmasterへ統合するときの負担


ドリル1
--------
1. 公式リポジトリをforkしてください

2. forkした自分のリポジトリをcloneしてください

    ```
$ git clone https://github.com/Vermee81/centralized.git
Cloning into 'centralized'...
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
Checking connectivity... done.

$ git remote add upstream https://github.com/Vermee1981/centralized.git
$ git config -l
user.name=Vermee81
user.email=hrksb5029@gmail.com
core.excludesfile=/Users/hrksb/.gitignore
core.editor=/usr/bin/vim
color.ui=auto
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
remote.origin.url=https://github.com/Vermee81/centralized.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
remote.upstream.url=https://github.com/Vermee1981/centralized.git
remote.upstream.fetch=+refs/heads/*:refs/remotes/upstream/*
    ```

3. ローカルリポジトリにfeatureブランチを作成して、何かしら変更を加えてcommitしてください

    ```
$ git checkout -b init_page
$ vim index.html
$ git status
On branch init_page
Untracked files:
(use "git add <file>..." to include in what will be committed)

index.html

nothing added to commit but untracked files present (use "git add" to track)
$ git branch
 * init_page
 master

$ git add index.html
$ git commit
neocomplete does not work this version of Vim.
It requires "if_lua" enabled Vim(7.3.885 or above).
Press ENTER or type command to continue
[init_page 96ab3f6] [add] Add index.html
1 file changed, 10 insertions(+)
create mode 100644 index.html
$ git status
On branch init_page
nothing to commit, working directory clean
$ git log --oneline
96ab3f6 [add] Add index.html
a92083a Initial commit
    ```

4. 自分のリポジトリにfeatureブランチをpushしてください
    ```
$ git push origin init_page
Username for 'https://github.com': Vermee81
Password for 'https://Vermee81@github.com':
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 407 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Vermee81/centralized.git
* [new branch]      init_page -> init_page
    ```

5. メンテナーにpull requestを送ってください

6. pull requestを受け取ったメンテナーは自分のローカルリポジトリにブランチをfetchしてください。
衝突がなければmergeしてください。
  ```
$ git fetch https://github.com/Vermee81/centralized.git init_page
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 1), reused 6 (delta 1), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/Vermee81/centralized
 * branch            init_page  -> FETCH_HEAD

$ git merge --no-ff FETCH_HEAD
 neocomplete does not work this version of Vim.
 It requires "if_lua" enabled Vim(7.3.885 or above).
 Press ENTER or type command to continue
 Merge made by the 'recursive' strategy.
  index.html | 10 ++++++++++
  1 file changed, 10 insertions(+)
  create mode 100644 index.html

$ git log --graph
  *   commit 5010558ecd73a32be07c6dd637df2b1a65a8a792
  |\  Merge: a92083a 78f9050
  | | Author: Vermee81 <hrksb5029@gmail.com>
  | | Date:   Tue Aug 11 01:26:17 2015 +0900
  | |
  | |     Merge branch 'init_page' of https://github.com/Vermee81/centralized
  | |
  | * commit 78f905029c45a7002049f2422fecf17862123de0
  | | Author: Vermee81 <hrksb5029@gmail.com>
  | | Date:   Tue Aug 11 01:15:28 2015 +0900
  | |
  | |     [modify] Add language type
  | |
  | |     Add lang="ja" in html tag
  | |
  | * commit 96ab3f6d192b3fc905d85e4ef85fce5c1bd1f099
  |/  Author: Vermee81 <hrksb5029@gmail.com>
  |   Date:   Tue Aug 11 01:00:07 2015 +0900
  |
  |       [add] Add index.html
  |
  |       Create a new file for top page
  |  
  * commit a92083abff92a7ec7f1150faf1c00466fdc3be22
    Author: Vermee1981 <hrksb0918@gmail.com>
    Date:   Sun Aug 9 11:27:59 2015 +0900

        Initial commit
    ```

7. メンテナーは変更をmasterへ反映してください。
    ```
$ git push origin master
 Username for 'https://github.com': Vermee1981
 Password for 'https://Vermee1981@github.com':
 Counting objects: 7, done.
 Delta compression using up to 4 threads.
 Compressing objects: 100% (7/7), done.
 Writing objects: 100% (7/7), 927 bytes | 0 bytes/s, done.
 Total 7 (delta 1), reused 0 (delta 0)
 To https://github.com/Vermee1981/centralized.git
    a92083a..5010558  master -> master
    ```

8. 開発者は公式リポジトリの変更を自分のmasterブランチに反映してください
    ```
$ git pull upstream master
remote: Counting objects: 1, done.
remote: Total 1 (delta 0), reused 1 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), done.
From https://github.com/Vermee1981/centralized
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master
Updating 78f9050..5010558
Fast-forward
    ```

「チーム開発実践入門」で紹介していたワークフロー
> - GitHub型ワークフローを採用し、各自がForkしたリモートリポジトリをGitHub上に持つ
> - リリースは中央リポジトリのmasterから行う
> - 各開発メンバーのローカルマシンと自分のリモートリポジトリ上は自由に行っていい
> - 開発が終了したら中央のリポジトリのmasterへPull Requestを送る
> - Pull Requestのレビューが通ったらmasterにマージ
> - リリース準備の段階で中央リポジトリのmasterからリリースブランチを作成
> - リリースブランチ上でリグレッションテストやデプロイのテストなど、リリースのための準備作業を行う
> - リリース準備の作業中も開発者は新機能をmasterにPull Requestを送って良い
> - リリースが終わったらmasterにマージして、masterにタグを売ったあとリリースブランチを消す
