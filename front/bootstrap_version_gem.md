# bootstrap_version_gem

rails のBootstrap Gem のバージョンによる違い

## 公式

[twbs/bootstrap\-sass: Official Sass port of Bootstrap 2 and 3\.](https://github.com/twbs/bootstrap-sass)

[twbs/bootstrap\-rubygem: Bootstrap 4 rubygem for Rails / Sprockets / Hanami / etc](https://github.com/twbs/bootstrap-rubygem)

## 参照

[Railsアプリで Bootstrap 4 を利用する \- Qiita](https://qiita.com/NaokiIshimura/items/c8db09daefff5c11dadf)

## 使用するGem

使用するBootstrap のバージョンによってGem が違う

* Bootstrap~3: bootstrap-sass
* Bootstrap 4: bootstrap

```Ruby
# Bootstrap3
# sass-rails gem はデフォルトで入ってるはず
gem 'bootstrap-sass',
gem 'sass-rails'
```

```Ruby
# Bootstrap4
# 4はまだ使ったことがない
gem 'bootstrap',
gem 'jquery-rails'
```

## バージョン別の違い

* Rails6 以降はwebpaker を使って導入
* Rails5 以前はアセットパイプラインを使用して導入

## 読み込みファイルについて

SCSS 拡張子のファイルでBootstrap を読み込む

* デフォルトではapplication.scss
* カスタムCSS の場合はcustom.scss

```CSS
# SCSS ファイル
# bootstrap-sass gem を使う為の記述
@import "bootstrap-sprockets";
@import "bootstrap";
```

* bootstrap のcss系 ファイルが複数ある場合は拡張子が必要
* bootstrap テンプレートを使用する場合など

```CSS
# assets/stylesheets/bootstrap.scss とassets/stylesheets/bootstrap.css の2つのファイルがある場合
# bootstrap.scss をコンパイルファイルに含みたい
@import "bootstrap-sprockets";
@import "bootstrap.scss";
```
