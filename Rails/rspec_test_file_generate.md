# rspec_test_file_generate

RSpec のテストファイルを生成

```Shell
# model spec
# 単数形
$ bin/rails g rspec:model user
```

```Shell
# controller spec
# 複数形
$ bin/rails g rspec:controller users
```

```Shell
# request spec
# test 名は任意でok
$ rails g rspec:request users_api
```

```Shell
# system spec
# 複数形
$ rails g rspec:system users
```
