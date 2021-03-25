# dotenv-rails

API Key などを別ファイルにする事でセキュリティを高めるgem

## 参照

[bkeepers/dotenv: A Ruby gem to load environment variables from \`\.env\`\.](https://github.com/bkeepers/dotenv)

[【Rails】dotenv\-railsの導入方法と使い方を理解して環境 \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/dotenv-rails)

[Railsで使える環境変数を管理できるgem\(dotenv\-rails\)や\.envの導入方法 \- Qiita](https://qiita.com/ryosuketter/items/ceb592dc6b23a20e51b5)

## 使い方

1. gem dotenv-rails インストール
2. .env ファイル作成
3. rails c で環境変数確認
4. .gitignore 設定
5. アプリで読み込み設定

dotenv-rails インストール

```Ruby
gem 'dotenv-rails'
```

アプリのトップディレクトリに`.env` ファイルを作成

```Ruby
# 環境変数を.env ファイルに追加
MAPS_JAVASCRIPT_API_KEY = 'api_key'
```

コンソールで環境変数が設定されているか確認

```Shell
$ rails c
> ENV['MAPS_JAVASCRIPT_API_KEY']
'api_key'
```

`.env` ファイルをGit 管理下から除外する

```Git
# .gitignore
/.env
```

git 管理下から`.env` ファイルが除外されているか確認

```Shell
$ git status --ignored
.
Ignored files:
.env
```
