# rails_s_daemon

バックグランド(デーモン化)でサーバ起動

## 参照

[【Rails】rails serverコマンド\(rails \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/rails-s)

## 手順

バックグランドでサーバ起動

```
$ rails s -d
```

サーバ停止

1. port_number で起動しているPIDを調べる
2. PID を指定して`kill`

```
$ lsof -i:port_number
$ kill -9 PID
```
