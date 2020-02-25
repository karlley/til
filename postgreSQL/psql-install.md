# postgreSQL

学習の為にmacローカルにbrewでインストール

## install

```
$ brew update
$ brew install postgreSQL
$ psql --version
psql (PostgreSQL) 12.2
```

## path setting

```
$ which psql
/usr/local/bin/psql
$ export PATH="/usr/local/Cellar/postgresql/12.2/bin:$PATH" >> ~/.zshrc
$ which psql
/usr/local/Cellar/postgresql/12.2/bin/psql
```

brew でインストールした場合はPATH設定は不要なのかもしれない
(/usr/local/bin/..でokなのかも)

## 起動確認

