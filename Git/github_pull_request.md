# github_pull_request

GitHub でのプルリク作成, Local マージまでの流れ

## 参考URL

[【GitHub】Milestone, Issue, Pull Requestを関連付けて扱う \- Qiita](https://qiita.com/kodai_0122/items/18f7faa80f0302244c51)

[【実践編①】ポートフォリオ作成のロードマップ〜RSpec、Rubocop導入〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/n109e4aff8643?magazine_key=mbce36d0b3160)

## 流れ

リモート側(GitHub) にローカル側(PC) で作業した差分をアップロードする

1. GitHub でissue 作成
2. Local のmaster ブランチから作業用ブランチ作成
3. 作業用ブランチに切り替え
4. 作業用ブランチで 1commit したらpush してGitHub でpull request 作成
5. GitHub でpull request とissue を紐付け
6. 作業ブランチで追加commit を積む
7. GitHub で作業ブランチをmaster ブランチにmerge
    1. GitHub のpull request ページFiles changed からコードの差分確認
    2. GitHub のpull request ページConversation でMerge pull request
8. Local の作業ブランチをmaster ブランチにmerge
    1. master ブランチにチェックアウト
    2. 作業ブランチをmaster ブランチにmerge
9. Local からmaster ブランチから作業ブランチを作成の繰り返し

## コマンド

Local からmaster ブランチから作業用ブランチ作成

```Shell
# ブランチ作成,切り替えを同時実行
$ git checkout -b branch_name

# 既存ブランチへの切り替え
$ git checkout branch_name
```

作業ブランチでpull request 用リモートブランチ作成

```Shell
# add & commit
$ git add .
$ git commit -m "commit message"

# push & リモートブランチ作成
$ git push --set-upstream origin branch_name
$ git push -u origin branch_name
```

Local の作業ブランチをmaster ブランチにmerge

```Shell
# master ブランチに切り替え
$ git checkout master

# branch_name ブランチをmaster ブランチにmerge
$ git merge branch_name
```
