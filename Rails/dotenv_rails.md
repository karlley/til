# dotenv-rails

API Key などを別ファイルにする事でセキュリティを高めるgem

https://github.com/bkeepers/dotenv

https://pikawaka.com/rails/dotenv-rails

https://qiita.com/ryosuketter/items/ceb592dc6b23a20e51b5

## usage

1. Gem dotenv-rails インストール
2. .env ファイル作成
3. rails c で環境変数確認
4. .gitignore 設定
5. アプリで読み込み設定

```Ruby
# dotenv-rails インストール
gem 'dotenv-rails'
```

```Ruby
# アプリのトップディレクトリに.env ファイルを作成
# 環境変数を.env ファイルに追加
MAPS_JAVASCRIPT_API_KEY = 'api_key'
```

```Shell
# コンソールで環境変数が設定されているか確認
$ rails c
> ENV['MAPS_JAVASCRIPT_API_KEY']
'api_key'
```

```Git
# .env ファイルをGit 管理下から除外する
# .gitignore
/.env
```

```Shell
# git 管理下から.env ファイルが除外されているか確認
$ git status --ignored
.
Ignored files:
.env
```
