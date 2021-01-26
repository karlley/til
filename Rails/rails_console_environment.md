# rails console environment

`rails c` を環境別で起動する方法

## 参考URL

[【console】rails c RAILS_ENV=testは廃止予定なのでrails c -e testを使う](https://qiita.com/sukebeeeeei/items/cf0ccb6f7e8b4e8a775b)

[【Rails】envメソッドで環境確認する方法と各コマンドの環境指定方法とは？](https://pikawaka.com/rails/env)

## how to

```Shell
# development 環境で起動
$ rails console

# 起動してる環境を確認
> Rails.env
"development"
```

```Shell
# test 環境で起動
$ rails console -e test
```

```Shell
# console 内で環境を変更する
# development 環境で起動
$ rails console

# test 環境に変更
> Rails.env = "test"

# 環境を確認
> Rails.env
"test"
```
