# git flow

個人開発でのgit の流れ

## 参照

[【GitHub】Milestone, Issue, Pull Requestを関連付けて扱う \- Qiita](https://qiita.com/kodai_0122/items/18f7faa80f0302244c51)

[【実践編①】ポートフォリオ作成のロードマップ〜RSpec、Rubocop導入〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/n109e4aff8643)

[【Git】ブランチの命名規則を調べてたらIssueドリブン開発という存在を知った \- Qiita](https://qiita.com/c6tower/items/fe2aa4ecb78bef69928f)

## 流れ

### 1. Issue、ブランチ 作成

1. GitHub にタスクを元にしたIssue 作成
2. ローカルでIssue 番号を元にしたブランチ作成(`#` を`\` でエスケープしてブランチ名を入力)

```
$ git checkout -b \#number_branch_name
```

### 2. PR 用コミット作成

**ブランチ名に`#` を使用する場合は`\` でエスケープが必要**

```Shell
$ git diff
$ git status
$ git add .
$ git commit -m "message"
$ git push --set-upstream origin \#number_branch_name(git push -u origin \#number_branch_name と同じ)
```

### 3.PR 作成

```
1. GitHub にbranch_name でブランチが作成される
2. GitHub Compare & pull request
3. PR名(Issue名), コメント(Close #issue_number) を入力
4. GitHub Create pull request
5. Milestone 設定
6. commit を積む
```

### 4.リモート側をMerge

```
1. GitHub Files changed ページで変更内容確認
2. GitHub Conversation ページ Merge pull request
3. GitHub Confume merge(コメントはPR名と同じでok)
```

### 5. ローカル側もMerge

```Shell
$ git checkout master
$ git branch -l
$ git merge repo_name
```

### 6.Heroku を使用してる場合の追加作業

作業内容に応じて下記3つのどれかを実行

1. model を追加した場合はmigrate 実行

```Shell
$ git push heroku master
$ heroku run rails db:migrate
```

2. サンプルデータ再生成 + DB リセット

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

3. seed ファイルを環境別に作成している場合

```
$ git push heroku master
$ heroku pg:reset DATABASE
$ heroku run rails db:migrate
$ heroku run rails db:seed RAILS_ENV=production
$ heroku restart
```
