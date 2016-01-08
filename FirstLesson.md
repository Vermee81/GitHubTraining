Git ドリル 第1回
===========

初期設定
---------
1. ユーザー名とメールアドレスを設定

    まずは自分の名前とメールアドレスを設定しましょう。
    ```
    git config --global user.name "Taro Yamada"
    git config --global user.email "yamada@example.net"
    ```
    ちなみに--globalオプションをつけて修正される設定ファイルは
    ~/.gitconfigに保存されます。
    世界中の人の画面に表示されることを意識して
    （日本語フォントが入っていない環境を意識して）、ユーザー名は英語にしましょう。

2. 確認する

    ```
    git config --global -l
    例:
    $ git config --global -l
    user.name=Vermee81
    user.email=***[秘密]***
    core.excludesfile=/Users/[秘密]/.gitignore
    core.editor=/usr/bin/vim
    color.ui=true
    ```

3. やり直したいときは設定したときと同じコマンドを入力しましょう


練習環境の作成
--------------
作るディレクトリは以下の３つ
共用リポジトリ
太郎さんのリポジトリ
花子さんのリポジトリ

1. 練習用のディレクトリ作成

    ```
    mkdir [好きなディレクトリ名]
    cd [好きなディレクトリ名]
    例:
    mkdir 20150711_training
    cd 20150711_training
    ```

2. 共用リポジトリ作成（仮想リモートリポジトリ）
    ここではkyoyuu.gitというディレクトリを共用リポジトリとします。
    このリポジトリ名を前提に進めていくので、同じ名前にすることをおすすめします。
    ```
    mkdir kyoyuu.git
    cd kyoyuu.git
    git init --bare --share
    例:
    $ git init --bare --share
    Initialized empty shared Git repository in /Users/[秘密]/Documents/workspace/20150711_training/kyoyuu.git/
    ```

3. 太郎さんのリポジトリ作成
    ```
    cd .. //練習用ディレクトリ直下に戻る
    mkdir -p Taro/kyoyuu
    cd Taro/kyoyuu
    git init //リポジトリの作成
    git config user.name "Taro Yamada" //太郎用にユーザー名を設定
    git config user.email "taro_yamada@example.com" //太郎用にemailを設定
    git remote add origin [[共用リポジトリまでのパス]]
    ```
    確認
    ```
    git remote -v
    ```
    修正したいとき
    ```
    git remote set-url origin [[共用リポジトリまでのパス]]
    ```

4. 花子さんのリポジトリ作成
    ```
    cd ../../ //練習用ディレクトリ直下に戻る
    mkdir -p Hanako/kyoyuu
    cd Hanako/kyoyuu
    git init //リポジトリの作成
    git config user.name "Hanako Suzuki" //花子用にユーザー名を設定
    git config user.email "hanako_suzuki@example.com" //花子用にemailを設定
    git remote add origin [[共用リポジトリまでのパス]]
    ```
    確認
    ```
    git remote -v
    ```

はじめてのコミット
------------
シナリオ：仲の良い太郎さんと花子さんは、
一緒にホームページを作ることにしました。
初心者の2人が最初に思いついたのはHello World.
2人とも自分の名前が表示されるトップページを作ることにしました。


太郎さんのリポジトリ直下にindex.html
というファイル名で、中身は以下のようなファイルを作ってください。

```
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="utf-8" />
<title>トップページ</title>
</head>
<body>
Hello 太郎
</body>
</html>
```
リポジトリの状態を確認してみましょう
```
git status
例:
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

  index.html

nothing added to commit but untracked files present (use "git add" to track)
```
太郎さんはindex.htmlファイルをブラウザで開いて確認しました。「Hello 太郎」と表示されていたので、コミットすることにします。

リポジトリの管理対象にするために、ステージ領域に追加します。
```
git add index.html
```
太郎さんのリポジトリの状態を確認してください。
```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   index.html

```
ではリポジトリに記録するために、コミットしてください。
```
git commit -m "ZZZ"
```
リポジトリの状態を確認してください。
```
$ git status
On branch master
nothing to commit, working directory clean
```
リポジトリの記録を見てみましょう。
```
git log
例:
$ git log
commit 1931c96f7753b90c2dd53741ac421369b0a27685
Author: Taro Yamada <taro_yamada@example.com>
Date:   Sat Jul 11 22:46:35 2015 +0900

    ZZZ
```
眠かったので、思わず自分の心をコミットメッセージに反映してしまいました。
こんなのがリポジトリの記録に残るなんて、永遠の恥です。
花子さんにも笑われてしまいます。
コメントを直しましょう。
直前のコミットメッセージを修正するには以下のようにします。
```
git commit --amend
```
viベースのエディタが起動して次のような画面が表示されます。
```
ZZZ

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Jul 11 22:46:35 2015 +0900
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   new file:   index.html
#
```
コミットメッセージのフォーマットは運用にもよりますが、
原則は1行目にコミットする変更内容の要約を*1行*で書きます。
2行目はブランク
3行目に変更した理由や詳細な内容を書きます。

Qiitaの次の投稿がとても参考になります。
http://qiita.com/itosho/items/9565c6ad2ffc24c09364
では次のように修正しましょう。
```
[add]自分の名前を表示する

html5でHello 太郎と表示しました
```
リポジトリの記録を確認してみましょう。
```
$ git log
commit 266164bdeaa74bb9dd82e24ba78b7429d3fba3dd
Author: Taro Yamada <taro_yamada@example.com>
Date:   Sat Jul 11 22:46:35 2015 +0900

    [add]自分の名前を表示する

    html5でHello 太郎と表示しました
```

ドリル1-1
----------
花子さんのリポジトリで以下の操作をしてください。

1. 花子さんのリポジトリの直下にindex.htmlというファイルを作成してください。
    ファイルの中身は以下のようにしてください。

    ```
    <!DOCTYPE html>
    <html lang="ja">
    <head>
    <meta charset="utf-8" />
    <title>トップページ</title>
    </head>
    <body>
    <h1>Hello 太郎と花子</h1>
    </body>
    </html>
    ```

2. リポジトリの状態を確認してください
3. index.htmlをステージ領域に追加してください
4. `git commit`と実行して、コミットしてください。
コメントは適当でかまいません。
5. `git commit --amend`と実行してコミットのコメントを修正してください。
どんな修正でもかまいません。
6. リポジトリの状態を確認して、ステージ領域に何も残っていないことを確認してください。

    ```
    On branch master
    nothing to commit, working directory clean
    ```

歴史をきざむ
------------
シナリオ：太郎さんは、トップページを見て物足りなさを感じました。
「もうちょっと、目立たせよう。」

ドリル1-2
--------
1. index.htmlを以下のように修正してください。

    ```
    <!DOCTYPE html>
    <html lang="ja">
    <head>
    <meta charset="utf-8" />
    <title>トップページ</title>
    </head>
    <body>
    <h1>Hello 太郎</h1>
    </body>
    </html>
    ```

2. リポジトリの状態を確認してください。以下のようになっていることを確認してください。

    ```
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   index.html
    no changes added to commit (use "git add" and/or "git commit -a")
    ```

3. `git diff`と入力して差分を確認してください。
4. ブラウザで確認してください。
5. index.htmlをステージ領域に追加してください。
6. `git commit`と実行して、コミットしてください。
コメントは適当でかまいません。
7. リポジトリの状態を確認して、ステージ領域に何も残っていないことを確認してください。
8. `git log --oneline`と実行して履歴を確認してください

直前の過去からやり直したい
-----------
シナリオ：
太郎は、思いました。
「h1タグは目立ちすぎじゃないか。」
自分の名前を太字で強調するくらいにしておこう。
h1で囲んだ記録は花子に見られたくない。
こんなのがリポジトリの記録に残るなんて、永遠の恥です。
花子さんにも笑われてしまいます。

1. 以下のように太郎さんのindex.htmlを修正してください。

    ```
    <!DOCTYPE html>
    <html lang="ja">
    <head>
    <meta charset="utf-8" />
    <title>トップページ</title>
    </head>
    <body>
    <b>Hello 太郎</b>
    </body>
    </html>
    ```

2. ブラウザで動作確認した後、`git add index.html`を実行してください
3. `git commit --amend`でコミットしてください。コメントは判別できれば適当でいいです。
4. `git log --oneline`でリポジトリの記録を確認してください

    ```
    $ git log --oneline
    774fe59 [modify] メッセージをbタグで囲む
    ee214c6 [add]自分の名前を表示する
    ```


機能別にブランチを作成
---------------
シナリオ：
太郎は思いました。
「自分の名前だけが表示されるトップページなんて面白くない。他の人の名前も表示したい。」
PCを見るとそこにはGoogleの画面がうつっており、右上に「太郎」と表示されていた。
「そうだ、ログイン画面を作ろう」

ブランチは、別々の開発を並行して行うために使用します。
ここでは、
太郎さんはログイン機能の開発
花子さんはトップページの開発
なので、太郎さんはログイン機能を開発するためのブランチを作成します。
まずは`git branch`を実行して、ブランチの一覧を見ましょう。
```
$ git branch
* master
```
masterしかありません。
現在のブランチを起点に新しいブランチを作りましょう。
`git branch [新しいブランチ名] [起点ブランチ名]`で作成します。
[起点ブランチ名]を省略すると現在のブランチが起点になります。
`git branch looogin_screen`と実行して、ブランチを確認しましょう。
```
$ git branch
  looogin_screen
* master
```
先頭に＊がついているのが現在見ているブランチです。
ブランチを移動してみましょう。
```
$ git checkout looogin_screen
Switched to branch 'looogin_screen'
$ git branch
* looogin_screen
  master
```
うっかり「o」をたくさんつけてしまいました。
ブランチを移動する前に名前を変たり消したりして遊んでみましょう。
ブランチの名前を変更するときは、
`git branch -m [古いブランチ名] [新しいブランチ名]`
```
$ git branch -m looogin_screen login_screen
$ git branch
* login_screen
  master
```
ブランチを消してみましょう
```
$ git checkout master //まずはmasterブランチへ移動
Switched to branch 'master'
$ git branch -d login_screen
Deleted branch login_screen (was b8c5246).
$ git branch
* master
```

ドリル 1-3
----------
太郎さんのリポジトリで以下の操作をしてください

1. login_screenという名前のブランチを作成してください
2. ブランチを確認してください。
3. login_screenブランチへ移動してください。
4. 余裕のある人は`git help checkout`を見て、
ブランチの作成と作成したブランチへの移動を同時に行うオプションを探してください
5. さらに余裕があったら、login_screenブランチを削除してから、
４で見つけたオプションでブランチの作成と移動を１つのコマンドで行ってください。


login_sceenブランチへ移動（チェックアウト）してください。
```
$ git checkout login_screen
Switched to branch 'login_screen'
$ git branch
* login_screen
  master
```
ログイン画面用のhtmlファイル(login.html)を作成してください
```
<!DOCTYPE html>
<html lang="ja">
    <head>
        <meta charset="utf-8" />
        <title>ログイン画面</title>
    </head>
    <body>
        <div id="form">
            <p class="form-title">Login</p>
            <form action="post">
                <p>Username</p>
                <p class="username"><input type="text" name="username" maxlength="32" autocomplete="OFF" /></p>
                <p>Password</p>
                <p class="password"><input type="password" name="password" autocomplete="OFF" /></p>
                <p class="submit"><input type="submit" value="Enter" /></p>
            </form>
        </div>
    </body>
</html>
```

ドリル1-4
----------
1. リポジトリの状態を確認してください
2. ブラウザでlogin.htmlに表示くずれがないか確認してください
3. login.htmlをステージ領域に追加してください
4. login.htmlをコミットしてください。

```
$ git status
On branch login_screen
Untracked files:
  (use "git add <file>..." to include in what will be committed)
login.html
nothing added to commit but untracked files present (use "git add" to track)
$ git add login.html
$ git commit
[login_screen bca0dc0] [add] ログイン画面の追加
 1 file changed, 19 insertions(+)
 create mode 100644 login.html
$ git log --oneline
bca0dc0 [add] ログイン画面の追加
b8c5246 [modify] メッセージをbタグで囲む
378cedf [add] 自分の名前を表示する
```

ドリル1-5
----------
1. ログイン画面の先頭にある「Login」(9行目にあります)を「ログイン画面」に変更してください
2. リポジトリの状態を確認してください
3. ブラウザでlogin.htmlに表示くずれがないか確認してください
4. login.htmlをステージ領域に追加してください
5. login.htmlをコミットしてください。

```
$ git status
On branch login_screen
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
modified:   login.html
no changes added to commit (use "git add" and/or "git commit -a")
$ git add login.html
$ git commit
neocomplete does not work this version of Vim.
It requires "if_lua" enabled Vim(7.3.885 or above).
Press ENTER or type command to continue
[login_screen 6b46fd7] [modify] ページ先頭のタイトルを日本語に変更
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git log --oneline
6b46fd7 [modify] ページ先頭のタイトルを日本語に変更
2c7f392 [add] ログイン画面の追加
b8c5246 [modify] メッセージをbタグで囲む
378cedf [add] 自分の名前を表示する
```

結合するときは慎重に
-------------------

マスターブランチに結合しましょう。
gitでブランチを統合するときによく使うコマンドは２つあります。
`git merge`と`git rebase`です。
どちらを使うかは、gitの運用によるところもあります。
２種類の結合を試してみましょう。

まずは、かなり違和感がありますが、
`git rebase`を試してみましょう。
rebaseは2つのブランチに分かれていた履歴が1つになります。
```
$ git checkout master
Switched to branch 'master'
$ git log --oneline
217de6d [modify] メッセージをbタグで囲む
fd127c4 [add] 自分の名前を表示する
$ git rebase login_screen
First, rewinding head to replay your work on top of it...
Fast-forwarded master to login_screen.
$ git log --oneline
2640714 [modify] ログイン画面の見出しを日本語に変更
095806f [add] ログイン画面を作成
217de6d [modify] メッセージをbタグで囲む
fd127c4 [add] 自分の名前を表示する
$ git log --graph
* commit 26407140166064ef86dd2c496e14380e0050d0c4
| Author: Taro Yamada <taro_yamada@example.com>
| Date:   Wed Jul 29 09:40:01 2015 +0900
|
|     [modify] ログイン画面の見出しを日本語に変更
|
|     Loginをログイン画面に変更する
|  
* commit 095806f0f21b07831d222f10ff44bb228e59b6ea
| Author: Taro Yamada <taro_yamada@example.com>
| Date:   Wed Jul 29 09:32:15 2015 +0900
|
|     [add] ログイン画面を作成
|
|     ログイン画面[login.html]というファイルを作成する
|  
* commit 217de6db7e6199bfcec0ad04bcfb8aa737fa07ea
| Author: Taro Yamada <taro_yamada@example.com>
| Date:   Wed Jul 22 09:19:04 2015 +0900
|
|     [modify] メッセージをbタグで囲む
|
|     bタグでHello太郎を囲む
|  
* commit fd127c4d3b7c282e8e512787ee20c0f2bd7119f6
  Author: Taro Yamada <taro_yamada@example.com>
  Date:   Wed Jul 15 09:38:39 2015 +0900

      [add] 自分の名前を表示する

      html5でHello 太郎と表示する
```
1つの履歴になっていてある意味見やすいのですが、
もっとブランチが複数にわかれていて、
それぞれの人が異なるブランチで作業していたらどうでしょうか？
git rebase はリポジトリのツリーの履歴を大きく変更するので、
かなり危険な操作という意識をもってください。
ローカル環境に影響する範囲ではコメント履歴を綺麗にする目的でよく使います。

mergeコマンドを試したいので、戻しましょう。

`git log --oneline`と入力してください。

```
$ git log --oneline
2640714 [modify] ログイン画面の見出しを日本語に変更
095806f [add] ログイン画面を作成
217de6d [modify] メッセージをbタグで囲む
fd127c4 [add] 自分の名前を表示する
```

「[modify] メッセージをbタグで囲む」の状態まで戻すときは行頭のハッシュ値をコピーして
`git reset --hard [ハッシュ値]`と入力してください。

```
$ git reset --hard 217de6d
HEAD is now at 217de6d [modify] メッセージをbタグで囲む
$ git log --oneline
217de6d [modify] メッセージをbタグで囲む
fd127c4 [add] 自分の名前を表示する
```
戻りました。

次はmasterブランチに結合した後も、git logで
ログイン画面用のブランチ（トピックブランチ）で行ったコミットであることを、
わかりやすく明確にしたいので、mergeコマンドを使います。

```
$ git checkout master
Switched to branch 'master'
$ git log --oneline
217de6d [modify] メッセージをbタグで囲む
fd127c4 [add] 自分の名前を表示する
$ git merge --no-ff login_screen
neocomplete does not work this version of Vim.
It requires "if_lua" enabled Vim(7.3.885 or above).
Press ENTER or type command to continue
Merge made by the 'recursive' strategy.
login.html | 19 +++++++++++++++++++
1 file changed, 19 insertions(+)
create mode 100644 login.html
$ git log --oneline
eaf9f9d Merge branch 'login_screen'
2640714 [modify] ログイン画面の見出しを日本語に変更
095806f [add] ログイン画面を作成
217de6d [modify] メッセージをbタグで囲む
fd127c4 [add] 自分の名前を表示する
```

`git log --graph`

```
*   commit 1f341cce88208bdac4c278ee3a17087a88e56d15
|\  Merge: 217de6d 2640714
| | Author: Taro Yamada <taro_yamada@example.com>
| | Date:   Wed Jul 29 12:43:00 2015 +0900
| |
| |     Merge branch 'login_screen'
| |
| * commit 26407140166064ef86dd2c496e14380e0050d0c4
| | Author: Taro Yamada <taro_yamada@example.com>
| | Date:   Wed Jul 29 09:40:01 2015 +0900
| |
| |     [modify] ログイン画面の見出しを日本語に変更
| |
| |     Loginをログイン画面に変更する
| |
| * commit 095806f0f21b07831d222f10ff44bb228e59b6ea
|/  Author: Taro Yamada <taro_yamada@example.com>
|   Date:   Wed Jul 29 09:32:15 2015 +0900
|
|       [add] ログイン画面を作成
|
|       ログイン画面[login.html]というファイルを作成する
|  
* commit 217de6db7e6199bfcec0ad04bcfb8aa737fa07ea
| Author: Taro Yamada <taro_yamada@example.com>
| Date:   Wed Jul 22 09:19:04 2015 +0900
|
|     [modify] メッセージをbタグで囲む
|
|     bタグでHello太郎を囲む
|  
* commit fd127c4d3b7c282e8e512787ee20c0f2bd7119f6
  Author: Taro Yamada <taro_yamada@example.com>
  Date:   Wed Jul 15 09:38:39 2015 +0900

      [add] 自分の名前を表示する

      html5でHello 太郎と表示する
```

rebaseとmergeの使い分けについては以下の投稿が参考になると思います。
http://powerful-code.com/blog/2012/11/merge-or-rebase/


リモートリポジトリに反映
----------------
masterブランチにチェックアウトして、
`git push origin master`と実行します

```
$ git checkout master
Switched to branch 'master'
$ git push origin master
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 1.85 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
To /秘密/kyoyuu.git
 * [new branch]      master -> master
```

シナリオ：
太郎さんはリモートリポジトリに反映したことを花子さんに伝えました。
花子さんはそれを元に修正します。


リモートリポジトリからとってくる（中央管理型）
-----------------
まずは、花子さんのリポジトリへ移動してください。

```
cd ../../Hanako/kyoyuu
```

`git fetch origin`を実行してリモートリポジトリからファイルをとってきます
```
$ git fetch origin
warning: no common commits
remote: Counting objects: 13, done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 13 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (13/13), done.
From /秘密/kyoyuu
 * [new branch]      master     -> origin/master
```
これでorigin/masterというブランチが花子さんのリポジトリにできます。
origin/masterはリモートリポジトリと結びついているブランチです。
マージするために差分を確認します。
リモートリポジトリとの差分を確認するには、
`git diff origin/master`と実行してください。
```
$ git diff origin/master
diff --git a/index.html b/index.html
index e847f89..65bbc00 100644
--- a/index.html
+++ b/index.html
@@ -5,6 +5,6 @@
diff --git a/index.html b/index.html
index e847f89..65bbc00 100644
--- a/index.html
+++ b/index.html
@@ -5,6 +5,6 @@
         <title>トップページ</title>
diff --git a/index.html b/index.html
index e847f89..65bbc00 100644
--- a/index.html
+++ b/index.html
@@ -5,6 +5,6 @@
         <title>トップページ</title>
     </head>
     <body>
-        <b>Hello 太郎</b>
+        <h1>Hello 太郎と花子</h1>
     </body>
 </html>
diff --git a/login.html b/login.html
deleted file mode 100644
index 6a7aa9b..0000000
--- a/login.html
+++ /dev/null
@@ -1,19 +0,0 @@
-<!DOCTYPE html>
-<html lang="ja">
-    <head>
-        <meta charset="utf-8" />
-        <title>ログイン画面</title>
-    </head>
-    <body>
-        <div id="form">
-            <p class="form-title">ログイン画面</p>
-            <form action="post">
-                <p>Username</p>
-                <p class="username"><input type="text" name="username" maxlength="32" autocomplete="OFF" /></p>
-                <p>Password</p>
-                <p class="password"><input type="password" name="password" autocomplete="OFF" /></p>
-                <p class="submit"><input type="submit" value="Enter" /></p>
-            </form>
-        </div>
-    </body>
-</html>
 ```
シナリオ：
「Hello 太郎」と書かれた個所を見て、花子さんは怒りに震えました。
「太郎は自分のことしか考えていない。」
花子さんは、目を閉じて深呼吸をしてから太郎さんに連絡しました。
花子：「2人のプロジェクトだから、Hello 太郎と花子にしない？」
太郎：「うん、いいよ。」

太郎の了解を得て、修正方針が決まりました。
まずはrebaseコマンドで起点になるbranchをリモートリポジトリのorigin/masterに変更します
`git rebase origin/master`を実行してください。
```
$ git rebase origin/master
First, rewinding head to replay your work on top of it...
Applying: [add] 太郎と自分の名前を表示する
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging index.html
CONFLICT (add/add): Merge conflict in index.html
Failed to merge in the changes.
Patch failed at 0001 [add] 太郎と自分の名前を表示する
The copy of the patch that failed is found in:
   <秘密>
When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```
衝突している個所があるので、修正します。
修正したら、ステージ領域に追加してください。
```
$ git status
rebase in progress; onto b8c5246
You are currently rebasing branch 'master' on 'b8c5246'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)
Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
both added:      index.html
no changes added to commit (use "git add" and/or "git commit -a")
$ git add index.html
```
`git rebase --continue`を実行して、起点の移動作業を進めます。
```
$ git rebase --continue
Applying: [add] 太郎と自分の名前を表示する
```
履歴を確認しましょう。
```
$ git log --oneline
dc641c1 [add] 太郎と自分の名前を表示する
b84d061 Merge branch 'login_screen'
84b7e42 [modify] ページ先頭のタイトルを日本語に変更
e64d705 [add] ログイン画面を追加
2fa0c37 [modify] メッセージをbタグで囲む
16df9cb [add] 自分の名前を表示する
```
最後にリモートリポジトリに反映してください。
```
$ git push origin master
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 439 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To /秘密/kyoyuu.git
   b84d061..dc641c1  master -> master
```

もっと過去からやり直したい（そして未来へ戻りたい）
-----------------
シナリオ：
太郎は、思いました。
「何で自分はこんなに強調してるんだ。謙虚になろう。初心にかえろう。。。」

1. 太郎さんのリポジトリに移動
`cd ../../Taro/kyoyuu/`

2. `git log --oneline`を実行して最初のハッシュを確認してください

    ```
    $ git log --oneline
    b84d061 Merge branch 'login_screen'
    84b7e42 [modify] ページ先頭のタイトルを日本語に変更
    e64d705 [add] ログイン画面を追加
    2fa0c37 [modify] メッセージをbタグで囲む
    16df9cb [add] 自分の名前を表示する
    ```

それでは`git reset`コマンドを実行して過去に戻りましょう。
まずはsoftオプションで実行してください。
```
$ git reset --soft 2fa0c37
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

new file:   login.html

$ git log --oneline
2fa0c37 [modify] メッセージをbタグで囲む
16df9cb [add] 自分の名前を表示する
```
login.htmlファイルは残っています。
試しにhardオプションで実行します。
```
$ git reset --hard 2fa0c37
HEAD is now at 2fa0c37 [modify] メッセージをbタグで囲む
$ git status
On branch master
nothing to commit, working directory clean
$ git log --oneline
2fa0c37 [modify] メッセージをbタグで囲む
16df9cb [add] 自分の名前を表示する
```
せっかく作ったログインhtmlファイルが消えてしまいました。。。

何が起こったのかは次のページの解説がわかりやすいです。
http://qiita.com/shuntaro_tamura/items/db1aef9cf9d78db50ffe

やっぱり元に戻しましょう
`git reflog`と実行してください。

commit id を指定して`git reset --hard {commit_id}`を実行してください。


TODO
--------
- タグについて
