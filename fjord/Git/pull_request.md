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
