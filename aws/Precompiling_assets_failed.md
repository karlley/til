# Precompiling_assets_failed.

heroku push 時の`Precompiling_assets_failed.` の解決法

## 参照

[RAILS\_ENV=production bin/rails assets:precompile](https://www.somethinggood.work/36)

[config\.fog\_provider = 'fog/aws'](https://qiita.com/wakasa51/items/8bc831d5c3915286c876)

## 方法

環境別にプリコンパイルしてエラーが出ないか確認


```Shell
# 開発環境でプリコンパイル
$ RAILS_ENV=development bin/rails assets:precompile

# 本番環境でプリコンパイル
$ RAILS_ENV=production bin/rails assets:precompile
```
