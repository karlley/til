# rubocop

rubocop について

## rubocop とは

* 規約に沿ってコードが書かれていかチェックするgem
* コーディング規約は数多く存在する

## rubocop の導入

gem でインストールする

```Shell
$ gem install rubocopo
$ rubocop -v
```

## rubocop の使い方

```Shell
# ruby のプロジェクトに移動
$ cd プロジェクト名

# 規約チェック
$ rubocop
```

## rubocop の設定

* `--auto-gen-config` オプションを付けると設定ファイル(`.rubocop_todo.yml`)を自動生成する
* `rubocop_todo.yml` を元に規約に合うようにコードを修正していく
* `rubocop.yml` をプロジェクトルートに設置して規約を細かく設定できる
* Rails 特有のルールはRails Cops に分類される

## rubocop-fjord の設定

* `.rubocop.yml` に`robocop-fjord` を読み込む
* Rails には`rails.yml` も読み込む
* `.rubocop.yml` に`inherit_from: .rubocop_todo.yml` の記述が無い場合は追記(記述が無いと修正点が読み込まれない)

[fjordllc/rubocop\-fjord: rubocop rules for fjord, Inc\.](https://github.com/fjordllc/rubocop-fjord)

Ruby プロジェクト

```yml
# .rubocop.yml 
inherit_gem:
  rubocop-fjord:
    - "config/rubocop.yml"
```

Rails プロジェクト

```yml
# .rubocop.yml
inherit_gem:
  rubocop-fjord:
    - "config/rubocop.yml"
    - "config/rails.yml"
```

## rubocop の導入と設定

Bundler でrubocop を導入後に実行

```
# .rubocop.yml、.rubocop_todo.ymlを生成
$ bundle exec rubocop --auto-gen-config

# .rubocop.yml にrubocop-fjord の読込を追記

# rubocop で構文チェック
$ bundle exec rubocop
```

rubocop-fjord の読込を追記

```
# .rubocop.yml
inherit_gem:
  rubocop-fjord:
    - "config/rubocop.yml"
```

## rubocop を使った修正の手順

1. `bundle exec rubocop` を実行すると`.rubocop_todo.yml` に修正内容が追記される
2. `.robocop_todo.yml` の修正点をコメントアウトして再度`bundle exec rubocop`
3. コメントアウトした修正点が表示される
4. `bundle exec rubocop -a` で自動修正 or 手動で修正
5. `.rubocop_todo.yml` が全てコメントアウトされるまで繰り返す

## .rubocop_todo.yml ファイルの運用について

基本的にはプロジェクト毎の運用ルールに沿うが、一般的には下記のように運用する

* `rubocop --auto-gen-config` で自動生成後、指摘事項を修正した時点で削除して良い
* どうしても `.rubocop_todo.yml` にignoreするルールを記載せざるをえなくなった場合のみファイルを残す
* `.rubocop.yml` ファイルの`inherit_from: .rubocop_todo.yml` の記述も`.rubocop_todo.yml` ファイルを削除した時点で削除する
* 予め無視するルールは`.rubocop.yml` に記載し、`rubocop_toco.yml` ファイルはリモートにpush しない

## rubocop のRubyのバージョンを指定する

* `#frozen_string_literal: true` のマジックコメントを追加しているのにrubocop で`Freeze mutable objects assigned to constants.` と表示されるのでRubyバージョンを指定する
* バージョンを指定したがエラーは消えなかった
* `#frozen_string_literal: true` は文字列オブジェクトを`freez` するだけなので定数には別途で`freez` する必要がある

[RuboCopが静的解析のRubyバージョンを決める流れ \- koicの日記](https://koic.hatenablog.com/entry/target-ruby-version-of-rubocop)

```
# .rubocop.yml

inherit_gem:
  rubocop-fjord:
    - "config/rubocop.yml"

# Ruby バージョンを指定
AllCops:
  TargetRubyVersion: 3.0.3
```