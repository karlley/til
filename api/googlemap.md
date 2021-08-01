# googlemap

rails でGoogleMap を表示

## 公式ドキュメント

[Overview  \|  Maps JavaScript API  \|  Google Developers](https://developers.google.com/maps/documentation/javascript/overview?hl=ja)

## 参照

Gem gmaps4rails 使用

[【Rails5】gmaps4rails \+ geocoder で GoogleMapを表示させる \| RemoNote](https://remonote.jp/rails-googlemap)j

[RailsでGoogleMapを表示させる\(gem 'gmaps4rails'の使い方\) \- Qiita](https://qiita.com/nakanoyoshiki/items/af9f37e9d2653518d6b0)

Gem gmaps4rails 使用しない

[Rails5でGoogleMapを表示させる \- Waku×2 プログラミング生活](https://ywatanab.hatenablog.com/entry/2018/10/08/025925)

[Rails5でGoogleMapを表示してみるまで \- Qiita](https://qiita.com/tiara/items/4a1c98418917a0e74cbb)

[【Rails】住所を入力すると自動で地図表示される方法\(Google Maps API\) \- Qiita](https://qiita.com/ryo_1241/items/bdb4babc3fc81eae3b50)

[Rails5でGoogleMapを表示 \- Qiita](https://qiita.com/enzen/items/9a919a75ebf0a34e7b91)

[\[DAY1\]オリジナルアプリ作成　〜RailsでGoogleMap利用検証〜｜Taku｜note](https://note.com/daddy0055/n/nddbe8da38bbc)

[【Rails6 / Google Map API】初学者向け！Ruby on Railsで簡単にGoogle Map APIの導入する \- Qiita](https://qiita.com/nagaseToya/items/e49977efb686ed05eadb)

[Rails5でGoogleMapを表示させる \- Waku×2 プログラミング生活](https://ywatanab.hatenablog.com/entry/2018/10/08/025925)

[google map API でrailsアプリにmapを描画 \- Fumiro Yoshihara’s Blog](https://yshrfmru.hatenablog.com/entry/2018/10/18/011749)

[Maps JavaScript API\+Google Maps Geocoding APIで住所から座標を求める方法 \| ProgramMemo](http://program-memo.com/archives/495)

[Google Maps API の使い方 利用方法 / Web Design Leaves](https://www.webdesignleaves.com/pr/plugins/googlemap_01.html)


## Gem, ツール

* geocoder: model で使用、地名からジオコーディングしてくれる(経度緯度を出力)
[alexreisner/geocoder: Complete Ruby geocoding solution\.](https://github.com/alexreisner/geocoder)

* gmaps4rails: view で使用、rails での地図表示を簡単にしてくれる, underscore.js が必要
[apneadiving/Google\-Maps\-for\-Rails: Enables easy Google map \+ overlays creation in Ruby apps](https://github.com/apneadiving/Google-Maps-for-Rails)

* underscore.js: JavaScript ライブラリ, 拡張して便利にしてくれる
[Underscore\.js](https://underscorejs.org/)

## ジオコーディングとは

* 地名や住所から経度緯度を取得すること
* ジオコーディングの方法は複数有る
* Google Geocoding API: クライアントサイド
* Gem geocoder: サーバサイド, controller を使用, DB を使って何かする場合に便利

## GoogleMap 描画の流れ

1. controller show @maker に座標, infowindow の情報を格納
2. view show Gmaps インスタンスを生成 > handler に代入
3. view show handler を元にMap を作成
4. view show Map 作成時の引数で@maker をjson に変換しmaker に代入

```Ruby
# controller show
def show
@destination = Destination.find(params[:id])
# GoogleMap 表示用のマーカーを作成
@marker = Gmaps4rails.build_markers(@destination) do |destination, marker|
marker.lat(destination.latitude)
marker.lng(destination.longitude)
marker.infowindow render_to_string(partial: "destinations/map_infowindow", locals: { destination: destination })
end
```

```JavaScript
# view show
<script type="text/javascript">
handler = Gmaps.build('Google');
handler.buildMap({ provider: { scrollwheel: false }, internal: {id: 'map'}}, function(){
markers = handler.addMarkers(<%= raw @marker.to_json %>);
handler.bounds.extendWith(markers);
handler.fitMapToBounds();
handler.getMap().setZoom(16);
});
</script>
```

## 実装手順

1. Gem `gmaps4rails`, `geocoder`, `dotenv-rails` 追加
2. 座標、住所用カラム追加
3. 地図表示用JS 追加
4. model に座標取得の設定追加
5. controller に`gmaps4rails` のJS 処理追加
6. view に地図表示部 追加
7. `geocoder` config ファイル追加


5. Chrome 拡張で動作確認

### 1. Gem `gmaps4rails`, `geocoder` 追加

```Ruby
# Gemfile
gem 'gmaps4rails'
gem 'geocoder'
gem 'dotenv-rails'
```

### 2. 座標、住所用カラム追加

```Shell
> rails g migration AddColumnToDestination address:string latitude:float longitude:float
> rails db:migrate
```
### 3. 地図表示用JS 追加

1. application.html.erb でGoogle Map 表示用のパーシャル(_google_map.html.erb) を作成
2. google_map.html.erb にJS の読み込み設定(API Key はGem dotenv-rails でGit 管理下から除外する)
3. gmap4rails でunderscore.js が必要なのでjs ファイルを作成し設置
4. application.js で必要なJS ファイルの読み込み設定を追加

* 読み込み設定ファイルはパーシャルに切り分ける必要がある
* Bootstrap などのフレームワーク使用の際にJS の読み込み設定が変更になる場合がある
* 使用するGem, API のドキュメントを参考にして最新の情報で実装する

```JS
// app/assets/javascripts/application.js
//= require underscore
//= require gmaps/google
```

```Ruby
# app/views/layouts/_google_map.html.erb
<script defer src="https://maps.googleapis.com/maps/api/js?key=<%= ENV["MAPS_JAVASCRIPT_API_KEY"] %>"></script>
<%# need to use gmap4rails %>
<script src="//cdn.rawgit.com/mahnunchik/markerclustererplus/master/dist/markerclusterer.min.js"></script>
<script src='//cdn.rawgit.com/printercu/google-maps-utility-library-v3-read-only/master/infobox/src/infobox_packed.js' type='text/javascript'></script>
```

### 4. model に座標取得の設定追加

```Ruby
# app/models/destination.rb
  # geocoder でaddress から経度, 緯度を取得する
  geocoded_by :address
  after_validation :geocode
```

### 5. controller に`gmaps4rails` のJS 処理追加

```Ruby
# app/controllers/destinations_controller.rb
def show
  @destination = Destination.find(params[:id])
  @marker = Gmaps4rails.build_markers(@destination) do |destination, marker|
    marker.lat(destination.latitude)
    marker.lng(destination.longitude)
    marker.infowindow render_to_string(partial: "destinations/map_infowindow", locals: { destination: destination })
  end
end

  private

  def destination_params
    params.require(:destination).permit(:name, :description, :address, :latitude, :longitude, :country, :picture)
  end
```

### 6. view に地図表示部 追加

```Ruby
# app/views/destinations/show.html.erb
<div id="map"></div>

      <script>
        handler = Gmaps.build("Google");
        handler.buildMap({ provider: { scrollwheel: false }, internal: {id: "map"}}, function(){
          markers = handler.addMarkers(<%= raw @marker.to_json %>);
          handler.bounds.extendWith(markers);
          handler.fitMapToBounds();
          handler.getMap().setZoom(16);
        });
      </script>
```

### 7. `geocoder` config ファイル追加

ローカルな地名から緯度, 経度を生成する為の設定

```Ruby
# config/initializers/geocoder.rb
Geocoder.configure(
  # Geocoding options
  # timeout: 3,                 # geocoding service timeout (secs)
  # lookup: :nominatim,         # name of geocoding service (symbol)
  # ip_lookup: :ipinfo_io,      # name of IP address geocoding service (symbol)
  # language: :en,              # ISO-639 language code
  # use_https: false,           # use HTTPS for lookup requests? (if supported)
  # http_proxy: nil,            # HTTP proxy server (user:pass@host:port)
  # https_proxy: nil,           # HTTPS proxy server (user:pass@host:port)
  # api_key: nil,               # API key for geocoding service
  # cache: nil,                 # cache object (must respond to #[], #[]=, and #del)
  # cache_prefix: 'geocoder:',  # prefix (string) to use for all cache keys

  # Exceptions that should not be rescued by default
  # (if you want to implement custom error handling);
  # supports SocketError and Timeout::Error
  # always_raise: [],

  # Calculation options
  # units: :mi,                 # :km for kilometers or :mi for miles
  # distances: :linear          # :spherical or :linear


  # street address geocoding service (default :nominatim)
  lookup: :google,

  # IP address geocoding service (default :ipinfo_io)
  # ip_lookup: :maxmind,

  # to use an API key:
  api_key: "GEOCODING_API_KEY",

  # geocoding service request timeout, in seconds (default 3):
  timeout: 5,

  # set default units to kilometers:
  units: :km,
)
```

## Chrome 拡張機能で動作確認

Google Maps Platform API Checker

[Google Maps Platform API Checker \- Chrome ウェブストア](https://chrome.google.com/webstore/detail/google-maps-platform-api/mlikepnkghhlnkgeejmlkfeheihlehne)

## script タグの defer

JS の読み込みタイミングを指定する記述

[<script> タグに async / defer を付けた場合のタイミング \- Qiita](https://qiita.com/phanect/items/82c85ea4b8f9c373d684)

## API とGem を使用する注意点

* API 公式の方法とGem 公式の方法を混同しないこと
* どこからがGem の領域なのかAPI の使用なのかを理解して実装する

## infowindow リファクタリングについて

controller でinfowindow の情報を取得後、パーシャル化したinfowindow を地図上に表示する

* render_to_string は表示結果を文字列として取得する
* render_to_string のlocals オプションで変数を指定する

[忘れがちなrenderメソッドの使い方まとめ \[Rails\] \- Qiita](https://qiita.com/hayashino/items/c2a4e7d3edbdcce3cd2a)

[Rails4 で render partial 部分テンプレートに変数を渡す\(locals option\)を使う時の注意点 \- Qiita](https://qiita.com/mm36/items/2d89aa3b77e740e671c2)

## コンソールで座標取得

```Shell
$ dest = Destination.new(name: "福岡", user_id: 1, spot: "ドーム", country: "japan")
$ dest.geocode #座標取得
$ dest.save
$ dest.add_addres #座標から住所を取得
$ dest.save #更新
```

## GoogleMap が新規投稿時に表示されない

* 新規投稿後のリダイレクト時にGoogleMap が表示されない
* index に戻り再度show ページに戻るとGoogleMap が表示される
* GoogleMap が表示/非常時に関わらずConsole にエラー

```Console
# Console のエラー表示
Uncaught ReferenceError: google is not defined
  at eval (eval at <anonymous> (infobox_packed.js:1), <anonymous>:1:1066)
  at infobox_packed.js:1
primitives.self-5b8a3a670839d76c06f9877827daa0d739b27f8d6ebd346d7fc6006819755e65.js?body=1:5 Uncaught ReferenceError: google is not defined
  at Object.Gmaps.Google.Primitives (primitives.self-5b8a3a670839d76c06f9877827daa0d739b27f8d6ebd346d7fc6006819755e65.js?body=1:5)
  at Handler.Gmaps.Objects.Handler.Handler.setPrimitives (handler.self-2f220cab5c91a054a6ee7f5b4c1eaab6edd160062ee3709c81714c18a6fbefa3.js?body=1:122)
  at new Handler (handler.self-2f220cab5c91a054a6ee7f5b4c1eaab6edd160062ee3709c81714c18a6fbefa3.js?body=1:8)
  at Object.build (base.self-8dd1d1a22a7ff3bf4faa70885ea5f452367e7249102c52508c66d85ca396ee8b.js?body=1:9)
  at 73:137
```

### 参照

[Overview  \|  Maps JavaScript API  \|  Google Developers](https://developers.google.com/maps/documentation/javascript/overview#Loading_the_Maps_API)

[apneadiving/Google\-Maps\-for\-Rails: Enables easy Google map \+ overlays creation in Ruby apps](https://github.com/apneadiving/Google-Maps-for-Rails#requirements-)

[スクリプトの非同期読み込み\(async, deferの違い\) \- わくわくBank](https://www.wakuwakubank.com/posts/614-javascript-async-defer/)

[Google Maps API throws "Uncaught ReferenceError: google is not defined" only when using AJAX \- Stack Overflow](https://stackoverflow.com/questions/14229695/google-maps-api-throws-uncaught-referenceerror-google-is-not-defined-only-whe)
j
### 原因

* js の遅延読み込み設定がおかしかった
* GoogleMaps API に対してGem gmap4rails がどのように作用しているか理解していなかった

```Html
# _google_map.html.erb
# GoogleMaps ドキュメント
<script defer
src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&callback=initMap">
</script>

# gmaps4rails を使用した場合の記述
<script src="https://maps.googleapis.com/maps/api/js?key=<%= ENV["MAPS_JAVASCRIPT_API_KEY"] %>"></script>
```

* `defer` は`callback` で指定された関数を呼び出すタイミングを指定している
* `callback` はgmap4rails を使う場合は記述が異なる為不要
* `callback` が不要なので`defer` の記述も不要になる


## 表示速度の改善

<https://developers-jp.googleblog.com/2015/10/google-maps-api.html>

* gem gmap4rail を使用している為、未実施
* gem を使わない状態に変更してから修正するといいかもしれない

## RSpec 画像判定

地図のピンの表示をテストする為に期待する画像の出力の判定方法

```Ruby
it "ピンが表示されていること", js: true do
expect(page).to have_selector ("img[src$='spotlight-poi2_hdpi.png']")
end
```

`src$=` でsrc 属性に以降の文字列を含むimg タグの有無を判定する

## Google Maps API request spec

API は下記の内容のテストはrequest spec で行う予定だったが中止

* contoller とmodel の流れのテスト
* ステータスコード, レスポンスボディのテスト
* Capybara は使わない
* HTTPメソッド(GET, POST, DELETE等) を使う

中止した理由は以下の通り

* GoogleMap からのjson でのレスポンスは無い
* GoogleMap からのレスポンスは地図そのものだと考える(地図オブジェクト自体がレスポンス)
* レスポンスがhtml なのでjson.parse が使えない(そもそもjson が返ってきていない)
* API のレスポンスである地図自体がJS で描画されているものなのでCapybara を使いブラウザをシュミレートしないとテストできない
