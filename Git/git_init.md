## git 設定確認

```
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

```
$ git config --global user.name "karlley"
$ git config --global user.email karlley.jp@gmail.com
$ git config --global core.editor vim
```

## GitHub 連携

1. GitHub でリポジトリ作成(README.md は自動生成しない)
2. README.md を作成
3. commit & push

README.md 作成

```
$ cd dir
$ touch README.md
$ echo "# Header" >> README.md
```

commit & push

```
$ git add .
$ git commit -m "first commit"
$ git remote add origin https://github.com/karlley/repo.git
$ git push -u origin master
```
