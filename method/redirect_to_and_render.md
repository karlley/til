# redirect_to_and_render

`redirect_to` と`render` の使い分け

## 参照

[【Rails】renderとredirect\_toの違いと使い分け \- Qiita](https://qiita.com/morikuma709/items/e9146465df2d8a094d78)

## 比較

### 記述

* `redirect_to`: URL、メソッドを指定
* `render`: view ファイルを指定

```Ruby
redirect_to destination_path(@destination)

render 'new'
```

### 動作

* `redirect_to`: HTTPリクエストを受けてcontroller でアクション実行
* `render`: 指定したview ファイルを表示するのみ

```
・render: controller > view
・redirect_to: controller > URL > route > controller > view
```

### 使い分け

* `redirect_to`: controller で処理した後にview を表示させたい(データの削除、更新)
* `render`: データを保持したまま次のview を表示させたい(バリデーションエラー、ログイン)

## ポイント

`render` で再描画してもアクション自体はrender 前のアクションのままなのでrender 後にnil エラーになりやすい

[JavaScript \- 【rails 】controllerでエラー処理をした後、renderでテンプレ表示。jsが読み込まれない・・・。｜teratail](https://teratail.com/questions/49673)

[Formに入力した値を維持したままリロードする方法 \- Qiita](https://qiita.com/seiya1121/items/cf6b44fae757f6300ada)

>renderはnewアクションのテンプレート(new.html.slim)をrenderしているだけなので、action_nameはcreateのままなのです。

```Ruby
# create 後にnew にrender する例

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
      # nil エラー防止の為に@regions、@airline をrender 前にセットする
      @regions = Country.where(ancestry: nil)
      # 地域選択済みで国一覧を取得
      if destination_params[:region_id] != ""
        @countries = Country.find(destination_params[:region_id].to_i).children
      end
      @airlines = Airline.all
      render 'new'
    end
```
