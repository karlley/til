# faker_gem

factorybot でインスタンス作成時のダミーデータを自動生成してくれるgem

## 参照

[faker\-ruby/faker: A library for generating fake data such as names, addresses, and phone numbers\.](https://github.com/faker-ruby/faker)

## コンソールでダミーデータを作成

コンソールでのダミーデータの作成方法

```Shell
$ rails console

# 1 - 249 までのランダムな数字を生ｊ
> country = Faker::Number.between(from: 1, to: 249)
123
```

