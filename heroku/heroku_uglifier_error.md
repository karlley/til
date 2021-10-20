# heroku _uglifier_error

heroku デプロイ時に ``` Uglifier::Error: Unexpected character '`' ```が発生

## 参照

[lautis/uglifier: Ruby wrapper for UglifyJS JavaScript compressor\.](https://github.com/lautis/uglifier)

[Uglifier::Error: Unexpected character '\`' 【Railsデプロイエラー】 \- 箱のプログラミング日記。](https://www.y-hakopro.com/entry/2020/04/22/142921)

## 状況

* js ファイルに` ` ` を使った記述を追加
* local ではマージまで完了(エラー無し)

```Shell
$ git push heroku master
.
remote:        Uglifier::Error: Unexpected character '`'
remote:        --
remote:         16870
remote:         16871   App.cable = ActionCable.createConsumer();
remote:         16872
remote:         16873 }).call(this);
remote:         16874 // gem turbolinks でready イベントが発火しないのでturbolinks:load
remote:         16875 $(document).on('turbolinks:load', function () {
remote:         16876   // 国名のセレクトボックスのoption 作成
remote:         16877   function appendOption(country) {
remote:            =>     var html = `<option value="${country.id}">${country.country_name}</option>`;
remote:         16879     return html;
remote:         16880   }
remote:         16881   // 国名のセレクトボックス用HTML
remote:         16882   function appendCountriesSelectBox(insertHTML) {
remote:         16883     var countriesSelectBoxHtml = '';
remote:         16884     countriesSelectBoxHtml = `<div class="form-group" id="country_select_box">
remote:         16885                                 <label for="destination_country_id">国</label>
remote:         16886                                 <span class="input-need"> *必須</span>
.
remote:  !     Precompiling assets failed.
remote:  !
remote:  !     Push rejected, failed to compile Ruby app.
remote:
remote:  !     Push failed
remote: Verifying deploy...
remote:
remote: !	Push rejected to karlley-departure.
remote:
To https://git.heroku.com/karlley-departure.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'https://git.heroku.com/karlley-departure.git'
```
## 原因

* Rails 標準ライブラリの中のJS 軽量化ライブラリ`Uglifier` がES5 までの文法規格までしかサポートしていない
* assets のコンパイルエラーでデプロイできていない

## 対策

`config/environments/production.rb` のUglifier の記述を公式の通りに修正

```
# 公式での対策法

# これを
config.assets.js_compressor = :uglifier

# 下記に修正
config.assets.js_compressor = Uglifier.new(harmony: true)
```

```Ruby
# coufig/environments/production.rb

.
  # Compress JavaScripts and CSS.
  # heroku デプロイ時にuglifier error が出るので修正
  # config.assets.js_compressor = :uglifier
  config.assets.js_compressor = Uglifier.new(harmony: true)
  # config.assets.css_compressor = :sass
```
