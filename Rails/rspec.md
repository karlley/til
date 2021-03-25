# rspec

rails のテストツール

[Publisher: RSpec \- Relish](https://relishapp.com/rspec)

## テストファイル作成コマンド

```Shell
# model spec
# 単数形
$ bin/rails g rspec:model user

# controller spec
# 複数形
$ bin/rails g rspec:controller users

# request spec
# test 名は任意でok
$ rails g rspec:request users_api

# system spec
# 複数形
$ rails g rspec:system users
```

## RSpec エラー時にチェックする項目

ヒューマンエラーでのエラーが多い場所

* test 環境用でseed を入れ直してなかった(CSV のデータがない)
* 初期データの確認
* validation 変更後にテストも変更したのか
* factroy の作り忘れ

## テストの種類

RSpec のテストの種類

### 1. 単体テスト

model spec

* 部品のテスト
* バリデーション
* インスタンスメソッド


### 2. 結合テスト

request spec

* contoller とmodel の流れのテスト
* ステータスコード, レスポンスボディのテスト
* Capybara は使わない
* HTTPメソッド(GET, POST, DELETE等) を使う
* controller spec が不要になる
* API のテストが可能


### 3. システムテスト

system spec

* アプリ全体を通したテスト
* feature spec の後継
* UI, E2E テスト
* Capybara を使用
* ブラウザの動きをシュミレートする
* Ajax を使用したページのテスト
