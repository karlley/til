# rails test

## テスト開始までの手順

- `bin/rails test` でテストが動くか確認
- controllerのテストをコメントアウト or 削除
  - コントローラーのテストは一般的には不要
- 自分で追加したコントローラー以外のメソッドに対してテストを書く
  - メソッドが追加されていないモデルのテストは不要

## テストユニットの書き方

### テストファイル

テスト作成時に以下のようなテンプレートファイルが作成される

```ruby
require "test_helper"

class ArticleTest < ActiveSupport::TestCase
  test "the truth" do
    assert true
  end
end
```

- `ActiveSupport::TestCase` を継承したクラスにテストを書く
- `test_helper` をrequireしているので全テストファイルで`~/test/helpers/test_helper.rb` が読み込まれる

### assert

代表的なアサーションの書き方

```ruby
# 実効値がtrueを期待
asset(実際の値)

# 実効値がfalseを期待
assert_not(実行値)

# 実効値が期待値であることを期待 
assert_equal(期待値, 実行値)

# 実効値が期待値でないことを期待 
assert_not_equal(期待値, 実行値)
```

- `期待値、実効値` の順で並んでいることに注意
  - [assertEqualsやassert\_equalの引数はなぜ expected, actual の順なのか、調べてみた \#Ruby \- Qiita](https://qiita.com/jnchito/items/a8a16a8242889b41d8ec)
- [Rails テスティングガイド \- Railsガイド](https://railsguides.jp/testing.html#%E5%88%A9%E7%94%A8%E5%8F%AF%E8%83%BD%E3%81%AA%E3%82%A2%E3%82%B5%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3)

## モデルテスト

1. モデルのテストファイルを作成
  - [Rails テスティングガイド \- Railsガイド](https://railsguides.jp/testing.html#%E3%83%A2%E3%83%87%E3%83%AB%E3%83%86%E3%82%B9%E3%83%88)
  - テストファイル
  - フィクスチャ
  - モデルファイルを自動生成している場合は存在しているかも
2. `bin/rails test` でテストの動作を確認
3. コントローラーの全テストファイルをコメントアウト
4. 全フィクスチャをコメントアウト
5. `bin/rails test` でエラーが発生しないことを確認

```sh
# テストファイル作成
$ rails g test_unit:model user

# モデルテスト実行
$ bin/rails test
```

テスト実行時に以下のエラーが発生

```
ActiveRecord::NotNullViolation: RuntimeError: NOT NULL constraint failed: report_mentions.mention_to_id
```

fixtureが不正なためコメントアウト or 削除した

## DBに保存する系のテストのポイント

- レコード作成は`create!` で作成失敗時に例外を送出する

## テスト作成時のポイント

- emailは`@example.com` で作成する
- 関連付いているモデルのidのカラムは`xxx_id` で作成されている 
  - fixtureでのカラム名は`xxx` だけでok
  - 関連カラムは`null: false` に指定されている

## システムテスト

```sh
# テスト実行
$ bin/rails test:system
```

- 実行するとChromeが動き出す
- `set up` にログイン等の事前設定を記述する
- 失敗したテストのスクショが`~/tmp/screenshots/` ディレクトリに保存される

### ボタンのクリック

クリックする対象によってメソッドを変える必要がある

- `click_link`: aタグを探す
- `click_button`: buttonタグを探す
- `click_on`: aタグ、buttonタグを探す

### フォームへの入力

- `fill_in '項目名', with: '入力する値'` 

### 行数を指定してテストを実行

- システムテストの場合も`bin/rails test` で実行(`:system` は不要)
- テストファイルのディレクトリは`test/` ディレクトリ以降を指定 

```sh
# 行数を指定してテストを実行
$ bin/rails test テストファイルのディレクトリ:行数

# 例) 日報のシステムテストの10行目のみ実行
$ bin/rails test test/system/reports_test.rb:10
```
