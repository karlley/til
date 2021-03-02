# rails_asset_pipeline_error

アセットパイプラインでのエラーについて

## 参考URL

[Rails Asset Pipelineがうまくいかないときの問題の切り分けかた \- Qiita](https://qiita.com/metheglin/items/c5c756246b7afbd34ae2#comments)

## 原因

アセットパイプラインでのプリコンパイル先のファイル名に改行コード(`\n`)が入っていｊ

## 確認したこと

1. assets のファイルパス
2. プリコンパイル時の読み込みファイル

### 1. assets のファイルパス

コンパイルされる際に読み込むディレクトリが設定されているか?

* assets/stylesheets
* assets/javascript
* assets/images
* assets/fonts

```Shell
$ rails c
> Rails.application.config.assets.paths
[
    [ 0] "/Users/karlley/Workspace/departure/app/assets/config",
    [ 1] "/Users/karlley/Workspace/departure/app/assets/fonts",
    [ 2] "/Users/karlley/Workspace/departure/app/assets/images",
    [ 3] "/Users/karlley/Workspace/departure/app/assets/javascripts",
    [ 4] "/Users/karlley/Workspace/departure/app/assets/stylesheets",
    [ 5] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/gmaps4rails-2.1.2/vendor/assets/javascripts",
    [ 6] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/jquery-rails-4.3.1/vendor/assets/javascripts",
    [ 7] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/coffee-rails-4.2.2/lib/assets/javascripts",
    [ 8] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/actioncable-5.2.3/lib/assets/compiled",
    [ 9] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/activestorage-5.2.3/app/assets/javascripts",
    [10] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/actionview-5.2.3/lib/assets/compiled",
    [11] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/turbolinks-source-5.2.0/lib/assets/javascripts",
    [12] #<Pathname:/Users/karlley/Workspace/departure/node_modules>,
    [13] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/bootstrap-sass-3.4.1/assets/stylesheets",
    [14] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/bootstrap-sass-3.4.1/assets/javascripts",
    [15] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/bootstrap-sass-3.4.1/assets/fonts",
    [16] "/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/bootstrap-sass-3.4.1/assets/images"
]
```

### プリコンパイル設定の確認

* 読み込むべきファイル(css, js 等) が読み込みディレクトリに設置されているか?
* マニフェストファイル(application.scss) から読み込むべきファイルが指定されているか?

```Ruby
# 読み込むべきファイル一覧
app/assets/stylesheets/*.css
app/assets/javascripts/*.js(application.js を除く)
```

```Ruby
# プリコンパイル読み込み先
# app/config/initializers/assets.rb
Rails.application.config.assets.precompile += %w(
animate.css \
bootstrap-datepicker.min.css \
bootstrap.css \
flexslider.css \
icomoon.css \
magnific-popup.css \
owl.carousel.min.css \
owl.theme.default.min.css \
style.css \
themify-icons.css \
bootstrap-datepicker.min.js \
bootstrap.min.js \
google_map.js \
jquery.countTo.js \
jquery.easing.1.3.js \
jquery.easypiechart.min.js \
jquery.magnific-popup.min.js \
jquery.min.js \
jquery.stellar.min.js \
jquery.waypoints.min.js \
magnific-popup-options.js \
main.js \
modernizr-2.6.2.min.js \
owl.carousel.min.js \
respond.min.js)
```

```Shell
# プリコンパイルされるファイルを確認
$ rails c
> Rails.application.config.assets.precompile
[
    [ 0] #<Proc:0x00007f93bae04098@/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/sprockets-rails-3.2.1/lib/sprockets/railtie.rb:84 (lambda)>,
    [ 1] /(?:\/|\\|\A)application\.(css|js)$/,
    [ 2] "animate.css",
    [ 3] "\nbootstrap-datepicker.min.css",
    [ 4] "\nbootstrap.css",
    [ 5] "\nflexslider.css",
    [ 6] "\nicomoon.css",
    [ 7] "\nmagnific-popup.css",
    [ 8] "\nowl.carousel.min.css",
    [ 9] "\nowl.theme.default.min.css",
    [10] "\nstyle.css",
    [11] "\nthemify-icons.css",
    [12] "\nbootstrap-datepicker.min.js",
    [13] "\nbootstrap.min.js",
    [14] "\ngoogle_map.js",
    [15] "\njquery.countTo.js",
    [16] "\njquery.easing.1.3.js",
    [17] "\njquery.easypiechart.min.js",
    [18] "\njquery.magnific-popup.min.js",
    [19] "\njquery.min.js",
    [20] "\njquery.stellar.min.js",
    [21] "\njquery.waypoints.min.js",
    [22] "\nmagnific-popup-options.js",
    [23] "\nmain.js",
    [24] "\nmodernizr-2.6.2.min.js",
    [25] "\nowl.carousel.min.js",
    [26] "\nrespond.min.js"
]
```

プリコンパイルされるファイル名に改行コード(\n) が入ってしまっている

```Ruby
# 改行コード(\)を削除
# config/initializers/assets/rb
Rails.application.config.assets.precompile += %w(
animate.css
bootstrap-datepicker.min.css
bootstrap.css
flexslider.css
icomoon.css
magnific-popup.css
owl.carousel.min.css
owl.theme.default.min.css
style.css
themify-icons.css
bootstrap-datepicker.min.js
bootstrap.min.js
google_map.js
jquery.countTo.js
jquery.easing.1.3.js
jquery.easypiechart.min.js
jquery.magnific-popup.min.js
jquery.min.js
jquery.stellar.min.js
jquery.waypoints.min.js
magnific-popup-options.js
main.js
modernizr-2.6.2.min.js
owl.carousel.min.js
respond.min.js)
```

再度プリコンパイル先のファイルを確認

```Shell
$ rails c
> Rails.application.config.assets.precompile
[
    [ 0] #<Proc:0x00007fb43af39698@/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/sprockets-rails-3.2.1/lib/sprockets/railtie.rb:84 (lambda)>,
    [ 1] /(?:\/|\\|\A)application\.(css|js)$/,
    [ 2] "animate.css",
    [ 3] "bootstrap-datepicker.min.css",
    [ 4] "bootstrap.css",
    [ 5] "flexslider.css",
    [ 6] "icomoon.css",
    [ 7] "magnific-popup.css",
    [ 8] "owl.carousel.min.css",
    [ 9] "owl.theme.default.min.css",
    [10] "style.css",
    [11] "themify-icons.css",
    [12] "bootstrap-datepicker.min.js",
    [13] "bootstrap.min.js",
    [14] "google_map.js",
    [15] "jquery.countTo.js",
    [16] "jquery.easing.1.3.js",
    [17] "jquery.easypiechart.min.js",
    [18] "jquery.magnific-popup.min.js",
    [19] "jquery.min.js",
    [20] "jquery.stellar.min.js",
    [21] "jquery.waypoints.min.js",
    [22] "magnific-popup-options.js",
    [23] "main.js",
    [24] "modernizr-2.6.2.min.js",
    [25] "owl.carousel.min.js",
    [26] "respond.min.js"
]
```
