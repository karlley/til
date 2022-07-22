# PRGパターン

[PRGパターンとは何か \- 理系学生日記](https://kiririmode.hatenablog.jp/entry/20160618/1466241269)

[新人Webエンジニア必須？の知識「PRGパターン」について](https://zenn.dev/imah/articles/3d186a6462ecc8)

> POSTメソッドでリクエストを受け取った場合、そのメソッドの処理の終わりに必ずRedirectし、処理後の結果はGetメソッドで呼び出せる画面に返しましょう

* Post/Redirect/Get の略
* 二重サブミット、セキュリティ対策を目的としたデザインパターン
* POST 後にリロード、または「戻る」を押した時に二重でPOSTされるのを防止できる
* 同じ情報のページのURLが複数存在するのを防止できる

# Sinatra

> 最小の労力でRubyによるWebアプリケーションを手早く作るためのDSLです。

* DSL: Domain-Specific Language、特定のクラスの問題に対して最適化、抽象化されたプログラミング言語

# Sinatra のインストールと起動

* Ruby 3.0.0 以降の場合は`webrick` gem が必要
* ルートパスは`http://localhost:4567/`
* `sinatra-contrib` gem でリロード不要で変更を反映する

```
# 作業ディレクトリ作成
$ mkdir try_sinatra
$ cd try_sinatra

# bundler 追加(Gemfile が追加される)
$ bundle init

# Gemfile に下記のgem を追記
# sinatra, webrick のインストール
$ bundle install
```

```
# Gemfile

# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

gem "sinatra" # 追記
gem "sinatra-contrib" # 追記
gem "webrick" # 追記
```

Ruby ファイル作成

* `sinatra-contrib` gem を使えるようにする為に`'require sinatra/reloader'` が必要

```
$ touch app.rb
```

```
# app.rb

# frozen_string_literal: true

require 'sinatra'
require 'sinatra/reloader'

get '/' do
  'hello'
end
```

sinatra 起動

* bundler を使用しているので`bundle exec` が必要

```
$ bundle exec ruby app.rbb
[2022-05-13 06:45:38] INFO  WEBrick 1.7.0
[2022-05-13 06:45:38] INFO  ruby 3.0.3 (2021-11-24) [arm64-darwin21]
== Sinatra (v2.2.0) has taken the stage on 4567 for development with backup from WEBrick
[2022-05-13 06:45:38] INFO  WEBrick::HTTPServer#start: pid=30899 port=4567
```

`http://localhost:4567/` にアクセスして`hello` と表示されればok

# Sinatra のポイント

* `public` ディレクトリのファイルは`/` 以下にマッピングされる
* `public` ディレクトリにcss, js 等のファイルを設置する
* メインのRuby ファイルに`HTTPメソッド`、`リソースのパス`、`表示するリソースファイル名` を記述する
* `views/` ディレクトリに表示するリソースファイルを設置する
* メインのRuby ファイルに定義したインスタンス変数をerb で呼び出せる
* `views/layout.erb` でview の共通レイアウトのファイルを定義できる
* メインのRuby ファイルに`helpers` として共通の処理を記述できる
* `GET`、`POST` 以外のHTTPメソッドを使う場合は`method_override` が必要な場合がある

# File.open

```Ruby
File.open('ファイル名', 'モード')
```

* `File.new` とほぼ同じだが、ブロックで使う時には`File. open` は自動で`close` される点が異なる
* 読込(`r`)、新規書き込み(`w`)、追記書き込み(`a`)などのモードがある

# JSON.load

```Ruby
JSON.load(文字列)
```

* 内部的に`JSON.parse` が実行されている
* `JSON.parse` はJSONの文字列しか読み込めない
* `JSON.load` はJSON 以外の文字列も読込可能

# JSON ファイルからのデータ読込

1. `require 'json'` でJSONモジュールを読み込む
2. `File.open` でファイルを開く
3. `JSON.load` でJSONをRuby型ハッシュに変換

```Ruby
# main.rb

require 'json'

# JSONを読み込みRuby型ハッシュに変換
ruby_hash = File.open('data.json') { |json| JSON.load(json)} 
=> [{"ID"=>1, "title"=>"memo_1", "content"=>"memo_1 text"}, {"ID"=>2, "title"=>"memo_2", "content"=>"memo_2 text"}]

# 読込データの確認
puts ruby_hash
{"memos"=>[{"id"=>1, "title"=>"Title", "content"=>"Content"}, {"id"=>2, "title"=>"Title 2", "content"=>"Content 2"}]}
=> nil
```

```JSON
// data.json

{
  "memos": [
    {
      "id": 1,
      "title": "Title",
      "content": "Content"
    },
    {
      "id": 2,
      "title": "Title 2",
      "content": "Content 2"
    }
  ]
}
```

# JSON ファイルへのデータ書込み

1. `File.open` でファイルを開く
2. `JSON.load` JSONをRuby型ハッシュに変換
3. Ruby型ハッシュにデータを追加
4. `File.open` を書き込みモードでファイルを開く
5. `JSON.dump` でデータをJSONに変換

```Ruby
# main.rb

require 'json'

# JSONからRuby型ハッシュを作成
ruby_hash = File.open('/data.json', 'r') { |json| JSON.load(json) }

# 値の更新
ruby_hash["user"] = "user_2"

# 配列内のハッシュに値を追加
ruby_hash["memos"] << { "id": 2, "title": "Title 2", "content": "Content 2" }

# Ruby型ハッシュからJSONに変換
File.open('data.json', 'w') { |json| JSON.dump(ruby_hash, json)}
```

```JSON
// db/data.json

{
  "user": "user_1",
  "memos": [
    {
      "id": 1,
      "title": "Title",
      "content": "Content"
    }
  ]
}
```

データ書き込み後

```JSON
// db/data.json

{
  "user": "user_2",
  "memos": [
    {
      "id": 1,
      "title": "Title",
      "content": "Content"
    },
    {
      "id": 2,
      "title": "Title 2",
      "content": "Content 2"
    }
  ]
}
```

参照: [RubyでJSONファイルを更新する \- No Solution for Life](https://masuyama13.hatenablog.com/entry/2020/06/16/193153)

# Sinatra でURLから値を取得する際の`*` は展開するとString オブジェクトとして展開される

```ruby
# test.rb

get '/test/*' do |s|
  text = s.class # URIの*のオブジェクトを調べる
  "URLの「*」は #{text} オブジェクトとして展開される"  # オブジェクト名を出力
end
```

`http://localhost:4567/test/text` へのアクセス時の表示

```
URLの「*」は String オブジェクトとして展開される
```

# Integer オブジェクトのnext の反対はpred

[class Integer \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/Integer.html#I_PRED)

* `next`: self から次の整数を返す
* `pred`: self から`-1` した整数を返す

```ruby
int = 2
int.next
=> 3
int.pred
=> 1

```

# input、textarea タグに初期値を設定する

* `input`: `value` 属性に初期値を指定する
* `textarea`: `textarea` 要素の中に初期値を指定する

```html
<form>
  <input value="inputの初期値">
</form>

<form>
  <textarea>textareaの初期値</textarea>
</form>
```

# JSON.load とJSON.parse の使い分け

[RubyでJSONを読み込む \- No Solution for Life](https://masuyama13.hatenablog.com/entry/2020/06/15/183142)

引数によって使い分ける

* `JSON.load`: 引数がJSONファイル
* `JSON.parse`: 引数がJSON文字列

```ruby
# ./main.rb
require 'json'

# JSON.load
file = File.open('./db/memos.json')
memos_data = JSON.load(file)
p memos_data
=> {"memos"=>[{"id"=>1, "title"=>"Title 1", "content"=>"Content 1"}, {"id"=>2, "title"=>"Title 2", "content"=>"Content 2"}]}

# JSON.parse
string = File.read('./db/memos.json')
memos_data = JSON.parse(string)
p memos_data
=> {"memos"=>[{"id"=>1, "title"=>"Title 1", "content"=>"Content 1"}, {"id"=>2, "title"=>"Title 2", "content"=>"Content 2"}]
```

```json
// ./db/memos.json

{
  "memos": [
    {
      "id": 1,
      "title": "Title 1",
      "content": "Content 1"
    },
    {
      "id": 2,
      "title": "Title 2",
      "content": "Content 2"
    }
  ]
}
```

# JSON ファイルを読み込んでシンボル型のキーに変換する

[JSON\.\#load \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/JSON/m/load.html)

[JSON\.\#parse \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/JSON/m/parse.html)

[JSONファイルの値を取り出したい。（おまけ：binding\.irbの使い方 \| FJORD BOOT CAMP（フィヨルドブートキャンプ）](https://bootcamp.fjord.jp/questions/934)

* `JSON.load`、`JSON.parse` 共に`symbolize_names` オプションを`true` にする
* `JSON.load` のみ追加で`proc` を`nil`、`create_additions` オプションを`false` にする

```json
// ./db/memos.json

{
  "memos": [
    {
      "id": 1,
      "title": "Title 1",
      "content": "Content 1"
    },
    {
      "id": 2,
      "title": "Title 2",
      "content": "Content 2"
    }
  ]
}
```

```ruby
# ./main.rb

require 'json'

# JSON.load
file = File.open('data.json')
use_json_load = JSON.load(file, nil, create_additions: false, symbolize_names: true)
p use_json_load

# JSON.parse
string = File.read('data.json')
use_json_parse = JSON.parse(string, symbolize_names: true)
p use_json_parse
```

```json
// data.json

{
  "memos": [
    {
      "id": 1,
      "title": "Title 1",
      "content": "Content 1"
    },
    {
      "id": 2,
      "title": "Title 2",
      "content": "Content 2"
    }
  ]
}
```

# form タグでサポートされていないGETとPOST以外のHTTPメソッドを指定する

`_method` パラメータを使いHTTPメソッドを指定する

* `form` タグのメソッドは`post` を指定
* `type="hidden"` を指定した`input` タグを追加
  * `value` 属性にHTTPメソッドを指定
  * `name` 属性に`_method` を指定
* `type="hidden"` を指定した`input` タグは非表示になる

```erb
<form action="/memos/<%= @memo[:id] %>" method="post">
    <input type="hidden" name="_method" value="delete">
    <input type="submit" value="削除">
</form>
```

[\[html/css\] httpのフォームでDELETEやPUTのメソッドを送る方法 \- 脳汁portal](https://portaltan.hatenablog.com/entry/2015/07/22/122031)

モジュラースタイルで記述する場合は別途`method_override` が必要になる

[Sinatra: README \(Japanese\)](http://sinatrarb.com/intro-ja.html)

# Sinatra のルーティングの* と: の違い

`:` は名前付きパラメータ、`*` はワイルドカード

* `:` の値は`params['パラメータ名']` で取得できる
* `*` の値は`[arams['splat']` で取得できる
* ブロックパラメータで値を保持できる

```ruby
# params で値を取得
get '/hello/:name' do
  # get /hello/bob の場合
  "hello,#{params['name']}!" #=> hello,bob! 
end

# ブロックパラメータで値を取得
get '/hello/:name' do |n|
  # get hello/marry の場合
  "hello, #{n}!" #=> hello, marry!
end

# params['splat'] で値を取得
get '/say/*/to/*' do
  # get /say/hello/to/world の場合
  "matched #{params['splat']}" #=> matched ["hello", "world"]
end

# ブロックパラメータで複数の値を取得
get '/download/*.*' do |path, ext|
  # get /download/path/to/file.xml の場合
  [path, ext] #=> ["path/to/file", "xml"]
end
```

通常のルーティングの場合は名前付きパラメータ(`:`) を使い、パスから複数の値を取得したい場合やワイルドカードを使いたい場合に`*` を使う選択をすると良いかもしれない。

[Sinatra: README \(Japanese\)](http://sinatrarb.com/intro-ja.html)

# SecureRandom でユニークな文字列をID にする

SecureRandomモジュールの`uuid` を使う

> UUIDとは、全世界で2つ以上のアイテムが同じ値を持つことがない一意な識別子のこと(参照: [UUID](https://e-words.jp/w/UUID.html))

[module SecureRandom \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/class/SecureRandom.html)

* `require 'securerandom'` でSecureRandomモジュールを読み込む
* `uuid` メソッドで生成された値は文字列

```ruby
> require `securerandom`
true

> id = SecureRandom.uuid
#=> "4ef81d22-ea5e-432f-9916-0287fd06dee1"

> id
#=> "4ef81d22-ea5e-432f-9916-0287fd06dee1"

> id.class
#=> String
```

# 複数のファイルを取得して作成順で並び替える

1. `Dir.glob` で指定したディレクトリ内のファイルを配列で取得
2. `File.birthtime` でファイル作成日時を取得
3. `sort_by` でファイルを並び替える

[Dir\.\[\] \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Dir/s/=5b=5d.html)

[Ruby ファイルを読み込む\(Dir\.globなど\)順番について \- Qiita](https://qiita.com/devzooiiooz/items/43da78f1c3c5c0552710)

## 指定したディレクトリ内のファイルを配列で取得

`dir` ディレクトリ内の拡張子が`json` のファイルを配列で取得する

```
# サンプルのディレクトリ構成

./
├ main.rb
└ dir/
    ├ 1.json
    ├ 2.json
    └ 3.json
```

```ruby
# main.rb

files = Dir.glob('./dir/*.json')
p files 
```

```sh
$ ruby main.rb
["./dir/1.json", "./dir/2.json", "./dir/3.json"]
```

## ファイル作成日時を取得する

`File.birthtime` で作成日時を取得

```ruby
# main.rb

files = Dir.glob('./dir/*.json')
files.each do |file|
  puts "ファイル名: #{file} 作成日時: #{File.birthtime(file)}"
end
```

ファイル名と作成日時を出力

```sh
$ ruby main.rb
"ファイル名: ./dir/1.json 作成日時: 2022-06-03 08:39:06 +0900"
"ファイル名: ./dir/2.json 作成日時: 2022-06-03 08:39:17 +0900"
"ファイル名: ./dir/3.json 作成日時: 2022-06-03 08:39:34 +0900"
```

作成日時以外にも更新日時や最終アクセス日時もFileクラスに用意されている

* 更新日時: [File.mtime](https://docs.ruby-lang.org/ja/latest/class/File.html#S_MTIME)
* 最終アクセス日時: [File.atime](https://docs.ruby-lang.org/ja/latest/class/File.html#S_ATIME)

## ファイルの並び替え

* 昇順: `sort_by`
* 降順: `sort_by` + `reverse`

[Enumerable\#sort\_by \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/sort_by.html)

[Array\#reverse \(Ruby 3\.1 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Array/i/reverse.html)

ファイルを取得して作成日時で昇順/降順でそれぞれ並び替える

```ruby
# main.rb

# 昇順
files = Dir.glob('./dir/*.json')

sort_files = files.sort_by { |file| File.birthtime(file) }
reverse_files = files.sort_by { |file| File.birthtime(file)}.reverse

puts "昇順"
sort_files.each do |file|
  puts "ファイル名: #{file} 作成日時: #{File.birthtime(file)}"
end

puts "降順"
reverse_files.each do |file|
  puts "ファイル名: #{file} 作成日時: #{File.birthtime(file)}"
end
```

## nomarize.css の導入

公式ページを元に`normarize.css` を作成し`link`タグで読み込む

1. [公式ページ](https://necolas.github.io/normalize.css/) の`Download` からコードをコピー
2. `public/` ディレクトリに`normarize.css` ファイルを作成しコードをペースト
3. `views/layout.erb` ファイルの`head` タグ内に`<link rel="stylesheet" href="/normarize.css">` を追記

独自のスタイルを当てるcssファイルよりも先に読み込まなければならないことに注意する

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <mata charset="utf-8">
    <title>メモアプリ</title>
    <link rel="stylesheet" href="/normarize.css">
    <link rel="stylesheet" href="/style.css">
  </head>
  <body>
    <h1>メモアプリ</h1>
    <%= yield %>
  </body>
</html>
```

[Normalize\.css: Make browsers render all elements more consistently\.](https://necolas.github.io/normalize.css/)

## vscodeでerbファイルをフォーマットする

vscodeの拡張機能のBeautifyを使う

### なぜBeautifyなのか

似たようなフォーマッタとしての拡張機能のPrettierがあるがerbファイルが上手くフォーマットされないらしいので

### erbファイルのフォーマット設定

デフォルトではerbファイルをHTMLとして認識してくれないので追加で設定が必要

1. vscodeで`cmd + ,` で設定が開くので`beautify` と検索
2. `Beautify: Language` の`Edit in settings.json` をクリック
3. `settings.json` が開きBeautifyのLanguage設定が追記される
4. `erb` の設定を`html` のエリアに追加する

```json
//settings.json

{
  ...
    "beautify.language": {
        "js": {
            "type": [
                "javascript",
                "json",
                "jsonc"
            ],
            "filename": [
                ".jshintrc",
                ".jsbeautifyrc"
            ]
        },
        "css": [
            "css",
            "less",
            "scss"
        ],
        "html": [
            "htm",
            "html",
            "erb" //追記する
        ]
    }
}
```

### 自動フォーマットを設定

Beautify拡張機能に限らず全てのフォーマッタの自動修正を設定する

`setting.json` に下記を追記

```json
//settings.json
{
  ...
    //ペースト時の自動フォーマット
    "editor.formatOnPaste": true,
    //入力時の自動フォーマット
    "editor.formatOnType": true,
    //保存時の自動フォーマット
    "editor.formatOnSave": true,
}
```

[Beautify \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)

[【簡単】vsCodeでRubyのerbファイルを自動フォーマットする方法 \- Qiita](https://qiita.com/Kiyo_Karl2/items/6fc6e0b57512deb1ff60)

[Visual Studio Codeのhtmlフォーマッターのメモ \- Qiita](https://qiita.com/rubytomato@github/items/d4716c41a2d15c64f791)

## HTML のドキュメント

* 公式ドキュメント: [HTML Standard](https://html.spec.whatwg.org/)

* タグについて: [HTML 要素リファレンス \- HTML: HyperText Markup Language \| MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element)

* クイックリファレンス: [スタイルシートリファレンス（ABC順）](http://www.htmq.com/style/indexa.shtml)

## HTML の迷いやすいタグ

* `article`: 独立して意味をなすもの
* `section`: 見出しとコンテンツがセットで表現できるもの、`h`タグとセットで使う
* `aside`: 余談・補足など
* `header`: メインコンテンツのヘッダー、メニューバーなど
* `main`: メインコンテンツ
* `footer`: フッター、コピーライトなど
* `div`: ひとかたまりの範囲として定義する、スタイルを目的にマークアップする場合に使う

[HTMLのタグ選びで迷うところ \- Qiita](https://qiita.com/nishinoshake/items/cf3ba7b8a5da82f4150d)

### divタグとsectionタグの使い分け

見出しとコンテンツをまとめられるなら`section`、スタイル目的で囲みたいなら`div` を使う

### アウトラインを意識したマークアップのサンプル

```html
<!DOCTYPE html>
<html>
  <head>
    <mata charset="utf-8">
    <title>タイトル</title>
    <!-- CSS等の読込 -->
  </head>
  <body>

    <header>
      <h1>タイトル</h1>
      <nav>
        <ul>
          <li><a>リンク1</a></li>
          <li><a>リンク2</a></li>
          <li><a>リンク3</a></li>
        </ul>
      </nav>
    </header>

    <main>
      <article>
        <h2>見出し1</h2>
        <p>コンテンツ1</p>
        <section>
          <h3>見出し2</h3>
          <p>コンテンツ2</p>
        </section>
      </article>
      <aside>
        <h2>関連リンク</h2>
        <ul>
          <li><a>リンク1</a></li>
          <li><a>リンク2</a></li>
          <li><a>リンク3</a></li>
        </ul>
      </aside>
    </main>

    <footer>
      <p><small>©Copyright</small></p>
    </footer>

  </body>
</html>
```

## HTMLのセクション要素にCSSを適応しても良いのか

セクション要素(`section`や`article`等)にCSSを適応してもOK。ただし以下のような縛りがある。

* アウトラインを意識したマークアップを重視するため`div`よりもセクション要素を優先
* セクション要素で囲めない要素の場合のみ`div`を使う
* セクション要素にCSSを適応する場合は`class`や`id`を付与してそれに対して指定する
* レイアウト目的(2カラム等)でCSSを適応場合もセクション要素を優先的に使う
  * セクション要素で囲める要素はセクション要素を使う
  * ボタン等のセクション要素で囲めない要素は`div`を使う

2カラムレイアウトでのセクション要素の使い方の例

```html
<body>
  <h1>見出し1</h1>
  <p>テキスト</p>

  <!-- セクション要素で囲める2カラムレイアウト -->
  <article class="flexbox">
    <section class="main">
      <h2>小見出し1</h2>
      <p>テキスト</p>
    </section>
    <section class="side">
      <h2>小見出し2</h2>
      <p>テキスト</p>
    </section>
  </article>

  <!-- セクション要素で囲めない2カラムレイアウト -->
  <nav>
    <div class="flexbox">
      <div class="main">
        <a>link_1</a>
      </div>
      <div class="side">
        <a>link_2</a>
      </div>
    </div>
  </nav>

</body>
```

```css
.flexbox {
  display: flex;
  justify-content: center;
  align-items: center;
}

.main {
  flex: 3;
}

.side {
  flex: 1;
}
```

## 注意点

* セクション要素を使ったマークアップの情報は情報量が多くどの情報が正しいのか分からない状態になってしまうので基本的には[公式ドキュメント](https://html.spec.whatwg.org/)を参照する
* `div` を多様するとアウトラインが正しくマークアップされていないことに気付きにくいので[Outliner](https://gsnedders.html5.org/outliner/)でアウトラインを確認する

[ゼロからはじめるHTML5でのサイト制作／第4回　HTML5で新しく定義された新要素「section要素」の使い方の基本をまとめよう \| デザインってオモシロイ \-MdN Design Interactive\-](https://www.mdn.co.jp/di/contents/4040/50876/)

[【html5】sectionタグについて](https://teratail.com/questions/47214)

[HTML Standard](https://html.spec.whatwg.org/)

## HTMLのフォーム内で空文字でのデータ送信を防止する

`required` を使うことでクライアント側(ブラウザ)で必須入力の項目を検証してデータを送信できる

* 必須入力にしたい要素に`required="required"`もしくは`required` 属性を追加する
* あくまでクライアント側で空文字を防止するだけなのでセキュリティを万全にするにはサーバ側で空文字が送信された場合の処理は別途必要

例)フォームの値を`/memos` にPOSTで送信する(タイトルを必須入力に指定)

```HTML
<form action="/memos" method="post">
  <label>Title</label>
  <input name="title" type="text" required="required">
  <label>Content</label>
  <textarea name="content"></textarea>
  <input type="submit" value="New Memo">
</form>
```

### 参照

[HTML Standard 日本語訳](https://momdo.github.io/html/input.html#attr-input-required)

[<input>: 入力欄 \(フォーム入力\) 要素 \- HTML: HyperText Markup Language \| MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element/input#attr-required)


## require とrequire_relative の違い

[requireとrequire\_relativeの使い分け \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/06/19/000000)


## nil? とempty? の違い

[nil?とempty?の違い \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/06/20/000000)

## XSSとその対策

[XSSとその対策 \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/06/21/000000)

## READMEの書き方

[READMEの書き方 \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/06/24/000000)

## GitHubのREADMEに画像を添付する

[GitHubのREADMEに画像を表示したい \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/06/25/000000)

## キャプチャをgifに変換する

[Macでgifアニメーションを作る恐らく一番楽な方法 \- Qiita](https://qiita.com/mikene_koko/items/b132f1e9afef82d589b3)

## メソッド名は必ず動詞で始めないといけない訳ではない

[メソッド名は必ず動詞で始めないといけない訳ではない \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/07/02/060650)

## ディレクトリ・トラバーサルとは

[ディレクトリ・トラバーサルとは \- karlley's tech blog](https://karlley.hatenablog.jp/entry/2022/07/03/082604)
