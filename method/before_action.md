# before_action

`before_action` を使用して各コントローラの処理の前に違う処理を実行する

## 参照

[【Rails】 before\_actionの使い方を徹底解説！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/before_action)

[before\_action \| Railsドキュメント](https://railsdoc.com/page/before_action)

## 使用例

* applications_controller#before_method をusers#index アクションの前に実行する
* ログインしているユーザにのみユーザ一覧を表示したい

```Ruby
# applications_controller.rb

before_action :before_method

def before_method
  ログインの確認処理など
end
```

```Ruby
def index
  ユーザ一覧
end
```

## before_action の実行を制限する

* オプション無しでは全コントローラ、全アクションで`before_action` が実行される
* オプションを付与することで`before_action` の実行を制限できる

1. `:only`	実行するアクション
2. `:except`	実行しないアクション
3. `:if`	実行する条件を指定
4. `:unless`	実行されない条件を指定

```Ruby
# before_method をnew, edit アクションでのみ実行する

before_action :before_method, only: [:new :edit]
```
