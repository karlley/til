# enum_error_20210522

`enum` を使用した際のエラーの対策

## 参照

[【初心者向け】form\_with, haml, enumを使ってselectによるプルダウンリストを作った話\[Rails\] \- Qiita](https://qiita.com/tanutanu/items/1bb5f12ac8ae90e71352)

[【初心者向け】i18nを利用して、enumのf\.selectオプションを日本語化する\[Rails\] \- Qiita](https://qiita.com/tanutanu/items/d44a92425188a4489ec6)

[rails での enumと、日本語化のやり方 \- Qiita](https://qiita.com/floatbottom/items/14ce2ceeab857a1f0d43)

[【Rails】enum\_helpを用いてi18n対応セレクトボックスを作成 \- Qiita](https://qiita.com/aiandrox/items/3fedb826dd870929c3f7)

[【Rails5】initializers/locale\.rbを使ったWebアプリの多言語化設定について、駆け出しVimmerが説明してみた](https://zenn.dev/yukihaga/articles/83a8f1e07ed193)

[Railsのenumで範囲外の値を渡すとバリデーションするより先にArgumentErrorが発生してしまう。 \- Qiita](https://qiita.com/snyt45/items/faf846413d103c68adf9)

[f\.selectで生成されるoptionタグを改変する \- Qiita](https://qiita.com/ATORA1992/items/fc6217412c5bc004ac6c)

## 状態

* セレクトボックスのvalue, text が同一ではない
* create するとバリデーションエラー
* strong_parameter で強制的にint 型に変換している

```Ruby
# app/controllers/destinations_controller.rb
# options_for_select を使用、セレクトボックス用の配列の指定がおかしい

<%= form_with model: @destination, local: true do |f| %>
.
  <div class="form-group">
    <%= f.label :expense %><span class="input-need"> *必須</span>
    <%= f.select :expense, options_for_select(Destination.expenses, @destination.expense_before_type_cast), { include_blank: "費用を選択" }, { class: "form-control", id: "destination_expense" } %>
  </div>
.
<% end %>
```

```Ruby
# app/models/destination.rb
# バリデーションがenum に設定した以外の値でも許可してしまっている

  validates :expense, presense: true
```

```Ruby
# app/controllers/destinations_controller.rb
# :expense を強制的にint 型に変換

private

  def destination_params
    # :expense をenum 定義しているのでInt 型に変換して追加
    params.require(:destination).permit(:name, :description, :spot, :latitude, :longitude, :address, :country_id, :picture, :season, :experience, :airline_id, :food, :region_id).merge(expense: params[:destination][:expense].to_i)
  end
```

binding.pry でexpense のクラスをを確認

```Ruby
# destinations_controller.rb
def create
  binding.pry
  @destination = current_user.destinations.build(destination_params)
  if @destination.save
    @destination.add_address
    flash[:success] = "Destination added!"
    redirect_to destination_path(@destination)
  else
    render 'destinations/new'
  end
end
```

Integer 型が渡っている

```
[1] pry(#<DestinationsController>)> destination_params
Unpermitted parameter: :expense
{
           "name" => "test",
    "description" => "",
           "spot" => "",
        "country" => "1",
         "season" => "1",
     "experience" => "",
        "airline" => "",
           "food" => "",
        "expense" => 0
}
[2] pry(#<DestinationsController>)> status = destination_params[:expense]
Unpermitted parameter: :expense
0
[3] pry(#<DestinationsController>)> status.class
Integer < Numeric
```

## 原因

* strong_parameter をパスする為にenum のインデックスをvalue としてを強制的にint 型に変換しているが、そもそもenum のkey をvalue として保存する事自体が間違い
* `create`、`update` アクションの`render` で再描画後に`@regions` と`@airlines` がnil になっていた

## 対策

* strong_parameter のint 型への変換を削除
* destinations テーブルexpence カラムに`default:0`、`null: false` オプションを追加して空データの保存を防止
* バリデーションに`inclusion` を使いenum 定義以外の値の保存を防止
* `options_for_selet` セレクトボックス生成用の配列は`Destination.expenses.keys` を`map` を使い生成
* 生成されたセレクトボックスのtext の値をgem `enum_help` を使い日本語化
* edit 時にcreate 時の値を保持して`selected` にする
* `render` の前に`@regions` と`@airline` に値を保持する

strong_parameter のint 型への変換を削除

```Ruby
# app/contollers/destinations_controller.rb

  def destination_params
    params.require(:destination).permit(:name, :description, :spot, :latitude, :longitude, :address, :country_id, :picture, :season, :experience, :airline_id, :food, :region_id, :expense)
  end
```

expense カラムにオプション追加

```Ruby
# migration ファイル

class AddNotnullAndDefaultToExpenseOnDestinations < ActiveRecord::Migration[5.2]
  def change
    change_column :destinations, :expense, :integer, default: 0, null: false
  end
end
```

expense カラムにオプション追加後

```Ruby
# app/db/schema.rb

.
  create_table "destinations", force: :cascade do |t|
    t.integer "expense", default: 0, null: false
  end
.
```

* enum 用バリデーション追加
* enum 定義の下部にinclusion を使ったバリデーションを追加

```Ruby
# app/models/destination.rb


  # 費用をenum で管理する
  enum expense: {
    till_50000: 1,
    till_100000: 2,
    till_200000: 3,
    till_300000: 4,
    till_500000: 5,
    till_700000: 6,
    till_1000000: 7,
    over_1000000: 8,
  }
  validates :expense, inclusion: { in: Destination.expenses.keys }
.
```

gem `enum_help` 追加

```Ruby
# Gemfile

gem 'enum_help'
```

ロケールファイルを修正

```yml
# config/locales/ja.yml

.
  enums:
    destination:
      expense:
        till_50000: ￥10,000 ~ ￥50,000,
        till_100000: ￥50,000 ~ ￥100,000,
        till_200000: ￥100,000 ~ ￥200,000,
        till_300000: ￥200,000 ~ ￥300,000,
        till_500000: ￥300,000 ~ ￥500,000,
        till_700000: ￥500,000 ~ ￥700,000,
        till_1000000: ￥700,000 ~ ￥1000,000,
        over_1000000: ￥1000,000 ~,
.
```

* 日本語化されているかコンソールで確認
* `テーブル名.カラム名複数形_i18n` でenum 定義のvalue, key を出力

```Ruby
$ rails console
> Destination.expenses_i18n
{
      "till_50000" => "￥10,000 ~ ￥50,000,",
     "till_100000" => "￥50,000 ~ ￥100,000,",
     "till_200000" => "￥100,000 ~ ￥200,000,",
     "till_300000" => "￥200,000 ~ ￥300,000,",
     "till_500000" => "￥300,000 ~ ￥500,000,",
     "till_700000" => "￥500,000 ~ ￥700,000,",
    "till_1000000" => "￥700,000 ~ ￥1000,000,",
    "over_1000000" => "￥1000,000 ~,"
}
```

* セレクトボックス生成
* `config/locales/ja.yml` に合わせてexpenses のkey を`I18n.t("enums.destination.expense.#{k}")` で翻訳して配列化する

```Ruby
# app/views/destinations/_destination_form.rb

.
  <div class="form-group">
    <%= f.label :expense %><span class="input-need"> *必須</span>
    # 下記を修正
    <%= f.select :expense, options_for_select(Destination.expenses.keys.map{|k| [k, k]}), { include_blank: "費用を選択" }, class: 'form-control', id: 'destination_expense' %>

    # 修正後
    <%= f.select :expense, options_for_select(Destination.expenses.keys.map{|k| [I18n.t("enums.destination.expense.#{k}"), k]}), { include_blank: "費用を選択" }, class: 'form-control', id: 'destination_expense' %>
  </div>
.
```

* create 時の値を保持して`selected` にする
* `options_for_select` の第一引数に選択済みの値として`@destination.expense` を追加

```Ruby
# app/views/destinations/_destination_form.rb

  <div class="form-group">
    <%= f.label :expense %><span class="input-need"> *必須</span>
    <%= f.select :expense, options_for_select(Destination.expenses.keys.map{|k| [I18n.t("enums.destination.expense.#{k}"), k]}, @destination.expense), { include_blank: "費用を選択" }, class: 'form-control', id: 'destination_expense' %>
  </div>
```

`render` での再描画の直前`@regions` と`@airline` の値を保持する

```Ruby
  def new
    @destination = Destination.new
    # 地域選択の初期値
    @regions = Country.where(ancestry: nil)
    @airlines = Airline.all
  end

  def create
    @destination = current_user.destinations.build(destination_params)
    if @destination.save
      @destination.add_address
      flash[:success] = "Destination added!"
      redirect_to destination_path(@destination)
    else
      @regions = Country.where(ancestry: nil)
      # 地域選択済みで国一覧を取得
      if destination_params[:region_id] != ""
        @countries = Country.find(destination_params[:region_id].to_i).children
      end
      @airlines = Airline.all
      render 'new'
    end
  end

  def edit
    @destination = Destination.find(params[:id])
    @regions = Country.where(ancestry: nil)
    selected_region = Country.find_by(id: @destination.country_id).parent
    @countries = selected_region.children
    @airlines = Airline.all
  end

  def update
    @destination = Destination.find(params[:id])
    if @destination.update_attributes(destination_params)
      @destination.update_address
      flash[:success] = "Destination updated!"
      redirect_to @destination
    else
      @regions = Country.where(ancestry: nil)
      # 地域選択済みで国一覧を取得
      if destination_params[:region_id] != ""
        @countries = Country.find(destination_params[:region_id].to_i).children
      end
      @airlines = Airline.all
      render 'edit'
    end
  end
```
