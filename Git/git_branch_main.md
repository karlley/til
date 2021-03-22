# git_branch_main

git のメインリポジトリ名の`master` を`main` に変更する

* git version 2.28 以降が必要
* git のバージョンアップにはbrew 経由のgit が必要

## 参照

[Gitのデフォルト・ブランチ名を変更する方法 \| Rriver](https://parashuto.com/rriver/tools/change-git-default-branch-name)

## 設定追加

デフォルトで作成されるリポジトリのメインブランチ名を`master` から`main` に変更

```Shell
$ git config --global init.defaultBranch main
```

```Git
# .gitconfig に下記が追加される
[init]
	defaultBranch = main
```

## GitHub のリポジトリ名変更

202010/10 からデフォルトブランチ名が`main`名が`main` に変更されている?
