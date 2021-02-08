# rails action view template error

存在すると思って書かれた変数あるいはメソッドが存在しなかった場合のエラー

`ActionView::Template::Error`

## 参考URL

[「No Ransack::Search object was provided to search_form_for!」エラーを解決したい](https://teratail.com/questions/207533)

## 原因

* controller, view の処理の流れを意識して組めていない
* 各view で必要な@変数をどのcontroller から取得しているか理解していない

```Ruby
# モデル毎の処理
view: posts/index.html.erb
controller: posts_controller.rb
method: posts_controller::index

# 共通テンプレートの処理
view: application.html.erb
controller: application_controller.rb
method: application_controller に定義されたメソッド
```

## 対策

1. 命名規則に沿ってview, controller のファイル名を修正する
2. `before_action` を使ってどのcontroller が呼ばれても@変数を返す処理が走るようにする

```Ruby
# application_controller.rb
# set_search をどのcontroller からも使えるようにする
  before_action :set_search

  def set_search
    @search = User.ransack(params[:q])
    @users = @search.result
  end
```
