# pull_request

> 仕事のプロジェクトの場合、チームメンバーにコミット権限付与し、自分のアカウント以下にフォークするのではなく、本家リポジトリに別ブランチを作って作業を行い、main ブランチに対してPRを送る場合が多いです。

* Fork 式とリポジトリ共有式の2通りの方法がある
* master(main) ブランチへのpush は基本的に行わない

## pull request のメリット

* コードレビューを習慣化できる
* レビュー、マージ作業をタスク化して記録できる
* ソースコードの明確化

## pull request を出す際の注意点

[初心者がRailsプロジェクトへの初PRする前に見るチェックリスト \- komagataのブログ](https://docs.komagata.org/5676)

* File Changes を確認
* lint を通す
* ファイル末尾に改行を入れる
* 分かりやすいコミットログ
* ルールに沿ったコミットログ
* git クライアントとGitHub のメールアドレスを揃える
* 不要なファイルをコミットしない
* gem のgroup を適切な読み込みにする
* 機密情報は環境変数を使って管理
* 不要な依存gem をコミットしない
* メソッドは読みやすく短く書く
* テストしてからレビュー依頼
* 理解していないコードはコミットしない

## コミットメッセージの注意点

[コミットメッセージの書き方\. 適切な情報を残そう \| by risacan \| Medium](https://medium.com/@risacan/%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8%E3%81%AE%E6%9B%B8%E3%81%8D%E6%96%B9-64aeadd92057)

* 日本語、英語は統一する
* 自分だけでなく他人が見て分かり易いメッセージを意識
* なぜ(Why)コードを変更したのかを伝える
* コミットメッセージの2行を空行にする
* 有意義なメッセージ、ポジティブなメッセージを残す

## pull request の内容をローカルで確認

```
$ git fetch        # 変更を取得
$ git branch -a        # ブランチを確認
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/update-readme
$ git checkout -b update-readme origin/update-readme # チェックアウト

# ここから動作確認
```

## pull request のテンプレート設定

[リポジトリ用のプルリクエストテンプレートの作成 \- GitHub Docs](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

[\.github/pull\_request\_template\.md](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

1. GitHub 上で`Add file` からリポジトリのルートに`.github/pull_request_template.md` を作成
2. 作成したテンプレートの内容を追記
3. ページ下部の`Commit new file` から`デフォルトブランチ` にコミットする

* テンプレートはデフォルトブランチ(`main` や`master`)に設置する必要がある
* テンプレートの設置ディレクトリは`.github/`、`docs`、`プロジェクトのルート` が選べる

## 進行中(WIP)のPR作成

進行中(WIP)のPull Request をドラフトとして作成することで不用意なマージ等を防止する

[プルリクエストのステージの変更 \- GitHub Docs](https://docs.github.com/ja/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/changing-the-stage-of-a-pull-request)

[WIPの代わりにDraft Pull Requestを利用する \(GitHub\) \- Qiita](https://qiita.com/tatane616/items/13da1b6797a7b871ad58)

* PR作成時に`Create draft pull request` でドラフトとしてPR を作成する
* Open にしたPR をドラフトに変更する場合は該当PRのページで`Reviewers` > `Convert to draft` をクリックする
* ドラフトのPRには左上部に`Draft` がグレーのボタンで表示される
* ドラフトからOpen のPRに変更したい場合は該当PR のページ下部の`Ready for review` をクリックする

## pull request にレビュー前にコメントを追加

[心がけている PR の書き方とコードレビューについて](https://zenn.dev/ckona/articles/my-codereview)

[GitHub – Pull Requestをレビュー【コメントの書き方と修正依頼】 \| Howpon\[ハウポン\]](https://howpon.com/6351)

* PR 画面でコード横の`+`をクリックして行毎のコメントを追加、`Start your review` をクリック
* 行毎のコメントが終わったら右上の`Finish your review` をクリック
* PR 自体に対するコメントを追加して`Submit your review` をクリック
* PR 画面に遷移後にコメントが反映されているか確認
