# enum_i18n

enum の日本語化

## 参照

[rails での enumと、日本語化のやり方 \- Qiita](https://qiita.com/floatbottom/items/14ce2ceeab857a1f0d43)

## ポイント

i18n を使用して`カラム名_i18n` でyml ファイルから日本語を取得する

## 実装

enum を使用した値の表示が日本語化されていないので修正

```Ruby
# destination_controller.rb
  def show
    @destination = Destination.find(params[:id])
    .
  end
```

```yml
# ja.yml
---
ja:
  activerecord:
  .
  enums:
    destination:
      expense:
        till_50000: ￥10,000 ~ ￥50,000
        till_100000: ￥50,000 ~ ￥100,000
        till_200000: ￥100,000 ~ ￥200,000
        till_300000: ￥200,000 ~ ￥300,000
        till_500000: ￥300,000 ~ ￥500,000
        till_700000: ￥500,000 ~ ￥700,000
        till_1000000: ￥700,000 ~ ￥1000,000
        over_1000000: ￥1000,000 ~
```

```Ruby
# 修正前
<p class="destination-show-expense"><i class="fas fa-calculator"></i> <%= @destination.expense %></p>

# 修正後
<p class="destination-show-expense"><i class="fas fa-calculator"></i> <%= @destination.expense_i18n %></p>
```


