# Git/GitHub

Git: 分散型バージョン管理システム

## 基礎

* リモートリポジトリ: 複数人で共有するリポジトリ
* ローカルリポジトリ: 自分だけが利用するリポジトリ、リモートリポジトリに修正点を追加する
* コミット: ファイルの修正点の差分を時系列順で記録したもの
* インデックス: コミット前の状態を一時的に記録した状態
* `clone`: リモートリポジトリの内容をローカルリポジトリにダウンロードすること
* `pull`: リモートリポジトリから最新の変更履歴をローカルリポジトリにmerge すること
* `fetch`: リモートリポジトリから最新の変更履歴をローカルリポジトリにFETCH_HEAD として取得すること
* `stash`: ファイルの変更内容を一時的に対比させること
* `fast-forward`: ポインタを前に進める、早送り
* `tag`: コミットに分かりやすい名前をつけるもの
* `amend`: 直前のコミットに対してファイルの追加、コメントの修正を行う
* `revert`: 指定したコミットを打ち消すコミットを作る
* `reset`: 指定したコミットを捨てる
* `cherry-pick`: 別ブランチから指定したコミットを取り込む
* `rebase -i`: コミットの書き換え、コメント追加、統合を行う
* `squash`: `merge` のオプションとして追加することでmerge 対象ブランチのコミットを1つのコミットにまとめて`merge` する

## HEAD

使用ブランチの先頭を指すポインタ

[【やっとわかった！】gitのHEAD^とHEAD~の違い \- Qiita](https://qiita.com/chihiro/items/d551c14cb9764454e0b9)

* `~`: ~世代前のコミットを指定
  * `HEAD`:   最新コミット
  * `HEAD~`:  1つ前のコミット
  * `HEAD~~`: 2つ前のコミット
  * `HEAD~2`: 2つ前のコミット
* `^`: 親コミットを指定
  * `HEAD^`: 1つ目の親コミット
  * `HEAD^^`: 1つ目の親の親コミット
  * `HEAD^2`: 2つ目の親コミット

## merge/rebase

`merge` と`rebase` の違い

[よく分かる！git rebaseとmergeの違いと使い分け \| WWWクリエイターズ](https://www-creators.com/archives/1943)

[git mergeとrebaseの違い。ブランチを統合する方法。 \- Qiita](https://qiita.com/shizen-shin/items/cfad2fdf9c405fce70e5)

* `merge`
  * main とtopic ブランチを統合して新しいコミットを作成
  * topic ブランチのコミット履歴は残る
  * main から作業を行う
* `rebase`
  * コミット履歴の移動、削除を行う
  * `merge` 前に履歴をmain に移動する目的等で使用
  * topic から作業を行う

## Fast-Forward/Non Fast-Forward

Fast-ForwardとNon Fast-Forward の違いについて

[fast\-forwardマージから理解するgit rebase \- Qiita](https://qiita.com/vsanna/items/451b42f886c599a16a55)

[【git】分かりやすく！mergeは「合流」、rebaseは「付け替え」\! \- NullNote](https://nullnote.com/web/git/merge_rebase/)

* `Fast-Forward merge`
  * 分岐元のmain ブランチが分岐後に修正が行われていなかった場合、分岐先のtopic ブランチの先頭コミットまでmain ブランチのポインタを進める
  * merge コミットは作られない
* `Non Fast-Forward merge`
  * 分岐元のmain ブランチが分岐後にも修正が進められた状態で、分岐先のtopic ブランチとのmerge コミットを作成して`merge` する
  * merge コミットを作成
  * merge コミット作成時にコンフリクトが起こる場合がある

## pull の動作の違い

状況による`pull`の動作の違いについて

* ローカルブランチに変更がない: Fast-Forward merge が行われる
* ローカルブランチに変更がある、コンフリクト無し: merge コミットが作られる
* ローカルブランチに変更がある、コンフリクト有り: コンフリクト解消、手動で`merge`

## pull/fetch

pull とfetch の動作の違いについて

* `pull`
  * リモートリポジトリの変更履歴をローカルリポジトリに`merge`
  * 内部的な動作は`fetch + merge`
* `fetch`
  * リモートリポジトリの変更履歴をローカルリポジトリの`FETCH_HEAD` ブランチとして取得
  * fetch 後に取得した変更履歴をローカルリポジトリのmain に統合するにはFETCH_HEAD ブランチの`merge`、もしくは再度`pull` する

## reset

`reset` について

* `soft`: コミットだけを無かったことにする、インデックスはそのまま
* `mixed`: 変更したインデックスを元の状態に戻す
* `hard`: コミットを完全に無かったことにする、インデックスを元の状態に戻す

`reset` 前のコミットは`ORIG_HEAD` で参照できる

```
# ORIG_HEAD にreset してreset 前の状態に戻す
$ git reset --hard ORIG_HEAD
```

## rebase

`rebase` について

[git rebase についてまとめてみた \- Qiita](https://qiita.com/KTakata/items/d33185fc0457c08654a5)

[git pull と git pull –rebase の違いって？図を交えて説明します！ – KRAY Inc\.](https://kray.jp/blog/git-pull-rebase/)

* 過去の変更履歴を改変する
* チーム開発時はpush している内容を変更することになるので注意が必要
* 対象コミットを選択後、オプションにより変更の動作を決める

```
# HEAD から4つ目までのコミットを表示、修正したい対象コミットへの動作を選択する
$ git rebase -i HEAD~4
```

## stash

ファイルの変更を一時的に退避させる

* `git stash`: インデックスに追加(`git add` 済み)されたファイルの変更を退避
* `git stash -u`: 未追跡のファイル(`git add` されていない)、無視されたファイルも含めて変更を退避
* `git stash list`: 退避させた変更のリストを表示
* `git stash drop`: 退避リストを削除
* `git stash pop`: 退避から変更を復元、退避リストを削除
* `git stash apply`: 退避した変更を現在のブランチに復元

## プルリクエスト

ローカルリポジトリでの他の開発者に伝えること

1. 対象ソースをclone
2. 作業用ブランチ作成
3. 修正
4. push
5. プルリクエスト作成
6. プルリクエストのレビュー/フィードバック
7. レビュー/フィードバックに対する修正
8. merge
9. プルリクエストをclose

ネットワークグラフ: GitHub 上でルート/フォークブランチのブランチ履歴が見れるページ

[リポジトリ間の接続を理解する \- GitHub Docs](https://docs.github.com/ja/repositories/viewing-activity-and-data-for-your-repository/understanding-connections-between-repositories)

* public リポジトリ or GitHub pro アカウントでないと見れない
* リポジトリのトップページ > Insights > Network

## Fork/Clone

対象リポジトリをFork してローカルにClone、Fork 元のリポジトリをPull できるように設定する

1. 対象リポジトリのFork ボタン
2. 自分のGitHub アカウントにFork したリポジトリが作成される
3. Fork したリポジトリのHTTPS のURL をコピー
4. ローカルでFork したリポジトリのHTTPS のURL を使いClone

```
# 作業用ディレクトリに移動
$ cd workspace

# Fork したリポジトリをClone
$ git clone https://github.com/karlley/patchwork.git
```

5. Fork 元のリポジトリから最新をPull できるようにremote を設定(upstream という名前で追加)

```
# Clone したディレクトリに移動
$ cd patchwork

# Fork 元のremote を追加
$ git remote add upstream https://github.com/jlord/patchwork.git
```

## GitHub Pages

ブランチにあるファイルを自動的に静的なWebサイトとしてホストしてくれる機能

```
# GitHub pages のURL
http://githubusername.github.io/repositoryname
```

## Collaborators に他のユーザーを追加

Collaborators: GitHubのユーザーでかつ誰かのリポジトリにアクセスして編集することが許された権限を持つ人たち

[コラボレーターを個人リポジトリに招待する \- GitHub Docs](https://docs.github.com/ja/account-and-profile/setting-up-and-managing-your-github-user-account/managing-access-to-your-personal-repositories/inviting-collaborators-to-a-personal-repository)

[Githubでコラボレーターを追加する方法 \- Qiita](https://qiita.com/negisys/items/52207c4837a8129df063)

1. Fork した自分のリポジトリの`Settings`
2. `Manage access` タブ
3. `Add people` クリック
4. Collaborator に追加する`reporobo` を検索して追加

## ローカルブランチがリモートブランチより遅れている

状況: 作業ブランチをリモートのmain ブランチにマージ後、ローカルのmain ブランチに切り替えた場合に発生した
原因: ローカルのmain ブランチがリモートのmain ブランチから遅れている
対策: リモートのmain ブランチにマージ後、ローカルのmain ブランチで`git pull` で早送りすることで`HEAD` が先頭に追いつく
今後: 作業ブランチを作成する前にローカルのmain ブランチで`git pull` することで最新の状態から作業ブランチを作成できる

```
# main ブランチに切り替えた際のログ
Your branch is behind 'origin/main' by 6 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```

## 間違ってpush したコミットを打ち消す

`revert` を使って打ち消しコミットを作成してリモートに`push` する

[【gitコマンド】いまさらのrevert \- Qiita](https://qiita.com/chihiro/items/2fa827d0eac98109e7ee)

* 打ち消しコミットが追加される
* `push` してもコンフリクトしない
* 履歴が残るので安全
* 履歴に不要な打ち消しコミットが追加されてしまう

```Shell
# 直前のコミットを打ち消す
$ git revert HEAD

# 2つ前のコミットを打ち消す
$ git revert HEAD~2 or git revert HEAD~~

# コミットID のコミットを打ち消す
$ git revert コミットID
```
