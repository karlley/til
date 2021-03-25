# git flow

個人開発でのgit の流れ

## 参照

[【GitHub】Milestone, Issue, Pull Requestを関連付けて扱う \- Qiita](https://qiita.com/kodai_0122/items/18f7faa80f0302244c51)

[【実践編①】ポートフォリオ作成のロードマップ〜RSpec、Rubocop導入〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/n109e4aff8643)

## 流れ

PR 用コミット作成

```Shell
$ git diff
$ git status
$ git add .
$ git commit -m "message"
$ git push --set-upstream origin branch_name(git push -u origin branch_name と同じ)
```

PR 作成

```
1. GitHub にbranch_name でブランチが作成される
2. GitHub Compare & pull request
3. PR Title, 該当するIssue 番号(Close #) を入力(PR名とIssue名は同じでok)
4. GitHub Create pull request
5. Milestone 設定
6. commit を積む
```

リモート側をMerge

```
1. GitHub Files changed ページで変更内容確認
2. GitHub Conversation ページ Merge pull request
3. GitHub Confume merge(コメントはPR名と同じでok)
```

ローカル側もMerge

```Shell
$ git checkout master
$ git merge repo_name
```

## Heroku を使用してる場合の追加作業

model を追加した場合はmigrate 実行

```Shell
$ git push heroku master
$ heroku run rails db:migrate
```

サンプルデータ再生成 + DB リセット

```Shell
$ git push heroku master
$ heroku pg:reset DATABASE
$ heroku run rails db:migrate
$ heroku run rails db:seed
$ heroku restart
# heroku pg:reset DATABASE で警告が出る
# app名か--confirm app名 をオプションに付けて実行すればok
❯ heroku pg:reset DATABASE
 ▸    WARNING: Destructive action
 ▸    postgresql-cylindrical-22088 will lose all of its data
 ▸
 ▸    To proceed, type karlley-departure or re-run this command with --confirm karlley-departure

> karlley-departure
Resetting postgresql-cylindrical-22088... done
```
