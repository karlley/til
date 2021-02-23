# git_merge_push

作業ブランチからmaster へのmerge, heroku のseed 作成まで

## 参考URL

[【実践編①】ポートフォリオ作成のロードマップ〜RSpec、Rubocop導入〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/n109e4aff8643?magazine_key=mbce36d0b3160)

[git pushがreject（拒否）されたときの対処法 \- Qiita](https://qiita.com/Takao_/items/5e563d5ea61d2829e497)

## 手順

コミットからheroku の初期データ導入まで

```Shell
$ git add -A
$ git commit -m "commit message"
$ git checkout master
$ git merge branch_name
$ git push
$ git push heroku master
$ heroku pg:reset DATABASE
$ heroku run rails db:migrate
$ heroku run rails db:seed​
```

* `git push heroku master` の中身は`git push heroku local_master_branch:remote_master_branch`
* `git push heroku master` はmaster ブランチで作業ブランチをmerge してから行う
* `git push` がリジェクトされた時は`git pull` 等でHEAD を合わせる
