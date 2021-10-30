# github_cannot_push

`git push -u origin repo_name` ができなくなった

## 参照

[gitでPlease make sure you have the correct access rights and the repository exists\. が出た時の対処法 \- Qiita](https://qiita.com/GakuNaitou/items/81dbbd3ea6211af71648)

[GitHubヘルプを参考にSSHキーの設定を行ってみた \| DevelopersIO](https://dev.classmethod.jp/articles/github-ssh-setting/)

[GithubにSSH接続できるようにする \- くま's Tech系Blog](https://kumaskun.hatenablog.com/entry/2019/10/20/225335)

## 状態

* 突然`git push` が通らなくなった
* HTTPS でGitHub へ接続していた
* SSH での接続設定はしていない
* GitHub へのアクセストークンは設定済み

## 原因と予想

* SSH での接続でも秘密鍵の設定がおかしくなり接続できなくなる事があるらしい
* HTTPS での接続が上手くいかないのが直接的な原因

## 対策

* SSH での接続には切り替えない
* HTTPS で接続の再設定を行う

[dotfiles/git\_parsonal\_access\_token\.md at master · karlley/dotfiles](https://github.com/karlley/dotfiles/blob/master/Git/git_parsonal_access_token.md)

