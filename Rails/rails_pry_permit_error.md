# rails pry permit error

binding.pry で値を確認中の`permit error` について

```Ruby
# エラー内容
(pry) output error: #<ActionController::UnfilteredParameters: unable to convert unpermitted parameters to hash>
```

## 原因

strong_parameter で許可されていないパラメータをハッシュに変換できないようになっている為

```Ruby
# params[:q] の中身を確認したい
pry > params[:q]
(pry) output error: #<ActionController::UnfilteredParameters: unable to convert unpermitted parameters to hash>
/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/actionpack-5.2.3/lib/action_controller/metal/strong_parameters.rb:266:in `to_h'
/Users/karlley/.rbenv/versions/2.5.7/lib/ruby/gems/2.5.0/gems/actionpack-5.2.3/lib/action_controller/metal/strong_parameters.rb:283:in `to_hash'
...
```

```Ruby
# permit の中に[:q] が含まれていない
def destination_params
  params.require(:destination).permit(:name, :description, :spot, :latitude, :longitude, :address, :country, :picture, :season, :experience, :airline, :food).merge(expense: params[:destination][:expense].to_i)
end
```

## 対策

`permit!` を使い強制的にパラメータを許可する

```Ruby
pry > params[:q].permit!
{
    "name_or_spot_or_address_cont" => "福岡 タワー",
                      "country_eq" => "",
                      "expense_eq" => "",
                       "season_eq" => ""
}
```

全パラメーターが表示される為どんなパラメータが設定されているかの確認にも使える
