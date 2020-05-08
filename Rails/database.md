# DB

DB 作成フロー

## 1. rails db:create

- database.yml を元にDB を作成
- `rails new` でdatabase.yml は自動作成される

## 2. rails db:migrate

- migrationファイルを元にDB にテーブルを作成
- `rails g model` でmigrationファイルは自動作成される
- migrationファイルは1度のmigrate で使い切り
- DB のテーブル等を変更する場合はその都度新しいmigration ファイルが必要

## 3. schema.rb

- `rails db:migrate` の実行結果が反映されるファイル
- ファイルの有無はアプリの起動に関係ない