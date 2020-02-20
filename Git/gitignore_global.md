git の管理から.DS_store を除外する

1. .gitignore_global 作成
2. .gitconfig で有効化

## .gitignore_global

```
$ cd
$ touch .gitignore_global
```

```:.gitignore_global
.DS_Store
```

## 有効化

```
$ git config --global core.excludesfile ~/.gitignore_global
$ git config --global -l
core.excludesfile=/Users/karlley/.gitignore_global
```