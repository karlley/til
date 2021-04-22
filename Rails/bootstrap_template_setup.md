# rails_bootstrap_template_setup

rails にBootstrap のテンプレートを導入

* ダウンロードファイルのindex.html のheader, footer をrails のテンプレートに導入していく作業
* テンプレート公式を熟読して設定を追加すること
* **Bootstrap のバージョンに合ったテンプレートを選択すること(ファイル構成が全く違う!!)**

## 使用テーマ

Bootstrap3 対応(HTML5)テンプレート

[Air: Free HTML5 Bootstrap Template for Portfolio and Landing Pages \- FreeHTML5\.co](https://freehtml5.co/preview/?item=air-free-html5-bootstrap-template-for-portfolio-and-landing-pages)

[Bootstrap · The world's most popular mobile\-first and responsive front\-end framework\.](https://getbootstrap.com/docs/3.3/)

## 参考URL

[【実践編⑦】ポートフォリオ作成のロードマップ〜ログ、Bootstrapテンプレート〜｜こうだい＠フルリモートエンジニア｜note](https://note.com/kodai_0122/n/n2ca036c070e9#T9ff0)

[RailsでCSSの読み込む順番を制御する方法 \- Qiita](https://qiita.com/takish/items/c5f264577d2db75fd10c)

[アセットパイプライン \- Railsガイド](https://railsguides.jp/asset_pipeline.html)

[Modernizrを使ってブラウザーの機能を調べるには \- Build Insider](https://www.buildinsider.net/web/modernizr/01)

[Rails初学者がつまずきやすい「アセットパイプライン」](https://www.transnet.ne.jp/2016/02/28/rails%E5%88%9D%E5%AD%A6%E8%80%85%E3%81%8C%E3%81%A4%E3%81%BE%E3%81%9A%E3%81%8Dcolnr%E3%80%8C%E3%82%A2%E3%82%BB%E3%83%83%E3%83%88%E3%83%91%E3%82%A4%E3%83%97%E3%83%A9%E3%82%A4%E3%83%B3/)

[Railsアプリで外部提供のJavaScriptファイルを利用する \- Qiita](https://qiita.com/NaokiIshimura/items/6ee8d7e0b7a960bcba41)

[Railsでvendor/assets/javascripts以下にあるファイルを特定のページでのみ使用する \- Qiita](https://qiita.com/teitei_tk/items/bf444c6aa6398bde83bf)

[RailsのIE8~対応でやったことまとめ 2017 \- Qiita](https://qiita.com/kurechon/items/84d037c2dbe3cb876840)

[RailsでBootstrapのナビゲーションでアクティブメニューに自動でclassを付与 \- Qiita](https://qiita.com/nemochu33/items/81c375fcaacb6c7866ca)

[【Ruby on Rails】画像は public と app/assets/images のどちらに設置すべき？ \- きゃまなかのブログ](https://techblog.kyamanak.com/entry/2017/10/13/003818)

[Rails で静的コンテンツをそのまま表示する方法 \- 約束の地](https://obel.hatenablog.jp/entry/20170523/1495539693)

### css.map ファイルについて

[CSS と JS のプリプロセッサの設定  \|  Tools for Web Developers  \|  Google Developers](https://developers.google.com/web/tools/setup/setup-preprocessors?hl=ja)

* 複数のCSS ファイルのディレクトリ, 読み込み順などが記述されているマッピングファイル
* 自動生成される

## 作業内容

* テーマダウンロード
* CSS, JS, fonts, image ファイルを各フォルダに移動
* CSS, JS ファイル読み込み設定
* プリコンパイル設定

### 1. CSS, JS ファイルの移動

* ダウンロードしたファイルを下記ディレクトリに移動
* 下記ディレクトリへの移動はどのテーマでも共通

css, sass フォルダの中身のみ: `app/assets/stylesheets`

```
# 追加前
application.css
costom.scss

# 追加後
_bootstrap-compass.scss
_bootstrap-mincer.scss
_bootstrap-sprockets.scss
animate.css
application.css
bootstrap
bootstrap.css
bootstrap.css.map
bootstrap.scss
costom.scss
flexslider.css
icomoon.css
magnific-popup.css
style.css
style.css.map
style.scss
```

js フォルダの中身のみ: `app/assets/javascripts`

```
# 追加前
application.js
cable.js
channels
underscore.js

# 追加後
application.js
bootstrap.min.js
cable.js
channels
jquery.easing.1.3.js
jquery.flexslider-min.js
jquery.min.js
jquery.waypoints.min.js
main.js
modernizr-2.6.2.min.js
respond.min.js
underscore.js
```

### 2. fonts ファイルの移動

* fonts フォルダのフォントデータのみを`app/assets/fonts` に移動
* フォントデータ以外のファイルは`vendor` ディレクトリに移動

```
# app/assets/fonts
# 追加後
# glyphicons, icomoon のフォントファイルを追加
glyphicons-halflings-regular.eot
glyphicons-halflings-regular.svg
glyphicons-halflings-regular.ttf
glyphicons-halflings-regular.woff
glyphicons-halflings-regular.woff2
icomoon/
  icomoon.eot
  icomoon.svg
  icomoon.ttf
  icomoon.woff
```

`vendor` ディレクトリを`app/assets/` ディレクトリと同じ構造にして外部font, css を使えるようにする

```
# vendor/
# 追加後
assets/
  fonts/
    icomoon.eot
    icomoon.svg
    icomoon.ttf
    icomoon.woff
  stylesheets/
    style.css
```

### 3. vendor ディレクトリのgitignore 設定

`vendor` ディレクトリをGit 対象にして`assets` をアセットパイプラインで使用可能にする

```Ruby
# 修正前
/vendor

# 修正後
# Ignore gems
/vendor/bundle
/vendor/.keep
```

### 5. font パス変更

* `assets/fonts/icomoon/`, `vendor/fonts/icomoon/` に設置したicomoon フォントの読み込みパスを変更
* どちらのディレクトリも`icomoon` ディレクトリ以下にフォントを再配置したので修正が必要
* ファイルパスはおそらく`fonts` ディレクトリからの絶対パス?

* `assets/stylesheets/icomoon.css`
* `vendor/stylesheets/icomoon/style.css`

```Ruby
# assets/stylesheets/icomoon.css
# 修正前
@font-face {
    font-family: 'icomoon';
    src:    url('fonts/icomoon.eot?1oniuf');
    src:    url('fonts/icomoon.eot?1oniuf#iefix') format('embedded-opentype'),
        url('fonts/icomoon.ttf?1oniuf') format('truetype'),
        url('fonts/icomoon.woff?1oniuf') format('woff'),
        url('fonts/icomoon.svg?1oniuf#icomoon') format('svg');

# 修正後
@font-face {
    font-family: 'icomoon';
    src:    url('icomoon/icomoon.eot?1oniuf');
    src:    url('icomoon/icomoon.eot?1oniuf#iefix') format('embedded-opentype'),
        url('icomoon/icomoon.ttf?1oniuf') format('truetype'),
        url('icomoon/icomoon.woff?1oniuf') format('woff'),
        url('icomoon/icomoon.svg?1oniuf#icomoon') format('svg');
```

### 5. CSS ファイル読み込み設定

SCSS ファイルで各CSS ファイルを読み込むように設定を追加

1. `app/assets/stylesheets/application.css` をSCSS ファイルに拡張子変更(メインCSS をapplication.scss に切り替える)
2. `require_tree .`, `require_self` を読み込まないように設定
3. Bootstrap テンプレート用の読み込みを追加
4. 旧メインCSS ファイル(custom.scss) からsprocets, Bootstrap の読み込みを削除

* `*=` で読み込む, `* ` で読み込まない
* `_`で始まる, `.min` ファイル, `.map` ファイルなどの特殊ファイルは読み込み不要

```SCSS
# 修正前
 *= require_tree .
 *= require_self
 */
```

* `require`, `@import` は読み込み順も反映されるので順序を意識して記述する
* テンプレートのスタイルを当てる為に`style.css`, `style.scss` `bootstrap.scss` より下に記述してを優先
* オリジナルのスタイルを当てる為に`custom.scss` を`style.scss` より下に記述して優先
* `icomoon/style` は最後に読み込まないとエラーが出る(おそらく`vender/` 以下に配置したファイルの為)

```SCSS
// 修正後
 * require_tree .
 * require_self
 *= require animate.css // .css ファイルを読み込み
 *= require icomoon.css
 *= require bootstrap.css
 *= require flexslider.css
 *= require magnific-popup.css
 *= require style.css
 */
 @import "bootstrap-sprockets"; // bootstrap-sass gem を使う為に必要, アセットパイプラインに必要な記述の為拡張子が不要?
 @import "bootstrap.scss"; // app/assets/stylesheets/以下のscss ファイルを読み込み
 @import "style.scss"; // .scss ファイル読み込み
 @import "custom.scss"; // 独自設定していたcss の読み込み
 @import "icomoon/style"; // vendor/stylesheets/icomoon/style.css の読み込み, 拡張子無しでok?, 最後に読込しないとエラーが出る
```

bootstrap が読み込みこまれていない場合のエラー

```
It's not clear which file to import for '@import "bootstrap"'.
Candidates:
  bootstrap.css
  bootstrap.scss
Please delete or rename all but one of these files.
```

@import で読み込むSCSS ファイル を指定する

```Ruby
# bootstrap に拡張子が付いていない
@import "bootstrap-sprockets";
@import "bootstrap"; # 拡張子を付ける
@import "style.scss";
@import "costom.scss";
@import "icomoon/style";
```

旧CSS ファイル(`app/assets/stylesheets/custom.scss`)からsprockets, Bootstrap 読み込みを削除

```SCSS
// custom.scss
// application.scss でインポートするので不要
// @import "bootstrap-sprockets";
// @import "bootstrap";
```

### 6. image ファイルの移動

テーマファイルの中で使用する画像ファイルを`public/images` に移動

* 画像の配置には`app/assets/images/` と`public/images` の2つが選択できる
* テンプレートのCSS からの画像へのパスに合わせる必要がある

```Ruby
# app/publick/images/
default.png
loader.gif # これを追加した
thumb200_default.png
thumb400_default.png
```

### 7. JS ファイル読み込み設定

`app/assets/javascripts/application.js` を修正して全読み込みになっていた設定をマニュアルで読み込むように変更する

* `require tree .` をコメントアウト: 記述済みのJS ファイルのSprokects への指定を止める
* `jquery`, `bootstrap` を削除: view ファイルで直接読み込む
* `modernizr` を追加: テンプレートに同梱されているので必要?

```JavaScript
# 修正前
//= require rails-ujs
//= require jquery // Bootstrap で使用
//= require bootstrap // Bootstrap で使用
//= require activestorage
//= require turbolinks
//= require underscore
//= require gmaps/google
//= require_tree .
```

```JavaScript
# 修正後
//= require rails-ujs // デフォルト
//= require modernizr-2.6.2.min // テンプレートに同梱さていた
//= require activestorage // デフォルト
//= require turbolinks // デフォルト
//= require underscore // GoogleMap API で必要
//= require gmaps/google //GoogleMap API で必要
// require_tree .
```

### 8. プリコンパイル設定

`config/initioalizers/assets.rb` にプリコンパイルの設定を追加

* `require_tree .` をコメントアウトしたので本番環境用にプリコンパイルの設定が必要
* `app/application.js` で削除した`jquery`, `bootstrap` を含むテンプレート用CSS, JSファイルをプリコンパイルファイルに設定する
* 設定後にプリコンパイル時のassets ファイルパス, assets ファイルの読み込み一覧を確認する

プリコンパイルするCSS, JS のファイルをリスト形式で表示する

```Shell
$ cd ~/Workspace/departure/app/assets/stylesheets/
$ ls -1
.DS_Store
_bootstrap-compass.scss
_bootstrap-mincer.scss
_bootstrap-sprockets.scss
animate.css //コンパイル
application.scss
bootstrap
bootstrap.css //コンパイル
bootstrap.css.map
bootstrap.scss
custom.scss
flexslider.css //コンパイル
icomoon.css //コンパイル
magnific-popup.css //コンパイル
style.css //コンパイル
style.css.map
style.scss

$ cd ~/Workspace/departure/app/assets/javascripts/
$ ls -1
.DS_Store
application.js
bootstrap.min.js //コンパイル
cable.js //コンパイル
channels
jquery.easing.1.3.js //コンパイル
jquery.flexslider-min.js //コンパイル
jquery.min.js //コンパイル
jquery.waypoints.min.js //コンパイル
main.js //コンパイル
modernizr-2.6.2.min.js //コンパイル
respond.min.js //コンパイル
underscore.js //コンパイル
```

プリコンパイルするファイルを`config/initializers/assets.rb` に追記

```Ruby
# 修正前
Rails.application.config.assets.precompile += %w( admin.js admin.css )
```

* stylesheets ディレクトリの`.map`, `_*`, `bootstrap フォルダ`, `.sass` ファイルはコンパイルしない
* javascripts ディレクトリの`chanels フォルダ`, `applications.js`,`respond.min.js` はwindows 用なのでコンパイルしない

```Ruby
# 修正後
Rails.application.config.assets.precompile += %w(
animate.css
bootstrap.css
flexslider.css
icomoon.css
magnific-popup.css
style.css
bootstrap.min.js
cable.js
jquery.easing.1.3.js
jquery.flexslider-min.js
jquery.min.js
jquery.waypoints.min.js
main.js
modernizr-2.6.2.min.js
underscore.js
)
```

assets ファイルパスの確認

```Shell
$ rails c
> Rails.application.config.assets.paths
```

assets ファイルの読み込みファイル一覧の確認

```Shell
$ rails c
> Rails.application.config.assets.precompile
```

### IE対応用JS 読み込み設定

IE 用のJS ファイルを読み込み設定

* html5.min.js: HTML5 に対応していないIE8 以前のブラウザ対策
* respond.min.js: HTML5 に対応していないIE 以前のブラウザでのレスポンシブ表示対策
* `</script>` はHTML5 移行は省略できるらしいが分からない未省略

`app/views/layouts/_shim.html.erb` にIE 用JS 読み込み追加

```Ruby
# 修正前
<!--[if lt IE 9]>
  <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
<![endif]-->
```

```Ruby
# 修正後
<!--[if lt IE 9]>
  <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js"></script>
  <script src="js/respond.min.js"></script>
<![endif]-->
```

### 10. view ファイルでの読み込み追加

* テンプレートファイルのindex.html のheader, footer 部のCSS, JS 読み込みを追加
* footer 部にJS 読み込み専用テンプレートを作成してJS 読み込み追加

`app/views/layouts/_rails_default.html.erb` にテンプレートのindex.html を参考にのhead 部を追加

```Ruby
# 修正前
<%= csrf_meta_tags %>
<%= csp_meta_tag %>

ここにheader 部の記述を追加

<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
<script src="https://kit.fontawesome.com/84eeecacff.js" crossorigin="anonymous"></script>
```

```Ruby
# 修正後
<%= csrf_meta_tags %>
<%= csp_meta_tag %>

# ここから追記
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<title>Departure</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="description" content="旅行の行き先共有サービス" />
<meta name="keywords" content="Departure, 旅行, バックパッカー, SNS" />
<link rel="shortcut icon" href="/images/favicon.ico">

<!-- Facebook and Twitter integration -->
<meta property="og:title" content=""/>
<meta property="og:image" content=""/>
<meta property="og:url" content=""/>
<meta property="og:site_name" content=""/>
<meta property="og:description" content=""/>
<meta name="twitter:title" content="" />
<meta name="twitter:image" content="" />
<meta name="twitter:url" content="" />
<meta name="twitter:card" content="" />

<link href="https://fonts.googleapis.com/css?family=Inconsolata:400,700" rel="stylesheet">
# ここまで追記

<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
<script src="https://kit.fontawesome.com/84eeecacff.js" crossorigin="anonymous"></script>
```

JS 読み込み用に`app/views/layouts/_js.html.erb` を新規作成しテンプレートのindex.html のfooter 下部のJS を`javascript_include_tag` で読み込み

```Ruby
# app/views/layouts/_js.html.erb
# 新規追加
<!-- jQuery -->
<%= javascript_include_tag('jquery.min') %>
<!-- jQuery Easing -->
<%= javascript_include_tag('jquery.easing.1.3') %>
<!-- Bootstrap -->
<%= javascript_include_tag('bootstrap.min') %>
<!-- Waypoints -->
<%= javascript_include_tag('jquery.waypoints.min') %>
<!-- Flexslider -->
<%= javascript_include_tag('jquery.flexslider-min') %>
<!-- Main -->
<%= javascript_include_tag('main') %>
```

`app/views/layouts/application.html.erb` で`_js.html.erb` を読み込み

```Ruby
# app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
  <head>
    <title><%= full_title(yield(:title)) %></title>
    <%= render 'layouts/rails_default' %>
    <%= render 'layouts/shim' %>
    <%= render 'layouts/google_map' %>
  </head>
  <body>
    <div id="page">
      <%= render 'layouts/header' %>
      <% flash.each do |message_type, message| %>
        <%= content_tag(:div, message, class: "alert alert-#{message_type}") %>
      <% end %>
      <%= render 'layouts/search' if logged_in? %>
      <%= yield %>
    </div>
    <%= render 'layouts/footer' %>
    <%= render 'layouts/js' %><!-- ここで_js.html.erb を読み込み -->
  </body>
</html>
```

上記作業でテンプレートを使ったスタイルの変更を確認できた
