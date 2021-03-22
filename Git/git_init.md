# git init

`git init` するまでの設定

## git 設定確認

```Shell
$ cd dir
$ git init
$ git config -l
credential.helper=osxkeychain
user.name=karlley
user.email=karlley.jp@gmail.com
core.editor=vim
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true
```

user, email, editor が設定されていない場合はglobal で設定

```Shell
$ git config --global user.name "karlley"
$ git config --global user.email karlley.jp@gmail.com
$ git config --global core.editor vim
```

## GitHub 連携

1. GitHub でリポジトリ作成(README.md は自動生成しない)
2. README.md を作成
3. commit & push

README.md 作成

```Shell
$ cd dir
$ touch README.md
$ echo "# Header" >> README.md
```

commit, remote リポジトリの設定

```Shell
$ git add .
$ git commit -m "first commit"
$ git remote add origin https://github.com/karlley/repo.git
```

`git remote` 後に設定を確認

```Shell
$ git config -l
.
remote.origin.url=https://github.com/karlley/Learning.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
```

リモートリポジトリにpush

```Shell
$ $ git push -u origin master
```



