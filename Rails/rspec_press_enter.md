# rspec_press_enter

RSpec のsystem spec 実行時にselenium でEnter キーの押下をシュミレートする

## 環境

* macOS 10.15.6
* Ruby 2.5.7
* Rails 5.2.3
* rspec-rails 4.0.1
* capybara 3.32.2
* selenium-webdriver 3.142.7

## 参照

[Keyboard :: Documentation for Selenium](https://www.selenium.dev/documentation/en/webdriver/keyboard/)

[send\_keys\(special\)\-Ruby](https://www.seleniumqref.com/api/ruby/element_set/Ruby_special_send_keys.html)

[ruby on rails \- How to simulate pressing Enter in Rspec \- Stack Overflow](https://stackoverflow.com/questions/17542185/how-to-simulate-pressing-enter-in-rspec)

[使えるRSpec入門・その4「どんなブラウザ操作も自由自在！逆引きCapybara大辞典」 \- Qiita](https://qiita.com/jnchito/items/607f956263c38a5fec24)

## 目標

検索実行ボタンの無い検索バーでテキスト入力後にEnter キーを押して検索を実行する動作をシュミレートしたい

## 解決策

`send_keys` メソッドを使う。

下記のような検索フォームに検索テキストとして`keyword` を入力後にEnter キー押下した際の検索結果のテストを想定しています。

```HTML
<form>
  <input placeholder='Search' id="keyword_search">
</form>
```

```Ruby
it '検索結果のテスト', js:true do
  # 検索フォームにkeyword を入力
  fill_in 'Search , with: 'keyword'

  # テキスト入力後のEnter キー押下
  find("#keyword_search").send_keys :return

  # 検索結果の検証
  expect(page).to have_content 'keyword'
end
```

## ポイント

selenium 以外のヘッドレスブラウザとの情報が色々と混ざってしまったので整理

* `:return` は`:enter` でも可
* `:return` は小文字でないとNG
* `find("#keyword_search").native.send_keys :return` ではNG, `.native` はseleniumでは不要

## 学び

Poltergeist, capybara-webkit 等の他のヘッドレスブラウザ毎に微妙に記述が異なるので使用する環境できちんと調べる事が重要
