# readonly_disabled

フォーム等で編集させたくない箇所をグレーアウトして編集できなくする

## 参照

[HTML 要素の disabled 属性と readonly 属性の違いと正しい使い方 \- A Memorandum](https://blog1.mammb.com/entry/2020/01/05/090000)

[【HTML】<input>タグのreadonly属性とdisabled属性の違い \- 小さなことからこつこつと。](https://bonoponz.hatenablog.com/entry/2020/11/12/%E3%80%90HTML%E3%80%91%3Cinput%3E%E3%82%BF%E3%82%B0%E3%81%AEreadonly%E5%B1%9E%E6%80%A7%E3%81%A8disabled%E5%B1%9E%E6%80%A7%E3%81%AE%E9%81%95%E3%81%84)

[Ruby on Rails 5 \- 条件によりtext\_fieldにreadonlyオプションを付与したい｜teratail](https://teratail.com/questions/169215)

[Capybaraチートシート \- Qiita](https://qiita.com/morrr/items/0e24251c049180218db4#%E8%A6%81%E7%B4%A0%E3%81%AE%E7%8A%B6%E6%85%8B%E3%82%92%E7%A2%BA%E8%AA%8D%E3%81%99%E3%82%8Bcapybaranodeelement)

## ポイント

* `readonly`は編集できないがパラメータの送信は可能
* `disabled` は編集、送信どちらも不可
* DB やmodel で使用する`readonly` とは別

## 実装

ゲストユーザーの場合のみdisabled 属性を追加

```Ruby
<%= form_for(@user) do |f| %>
  <div class="form-group">
    <%= f.label :name %>
    <%= f.text_field :name, class: 'form-control', id: 'user_name', disabled: guest_user? %>
  </div>
<% end %>
```

## テスト

`disabled` 属性が付与されているかテスト

1. `find` でテスト対象の要素を取得
2. `取得した要素名.disabled?` で状態をテスト

```Ruby
it "ユーザー名とメールアドレスが編集できない" do
  user_name = find "#user_name"
  user_email = find "#user_email"
  # readonly 属性の有無をテスト
  expect(user_name.disabled?).to be_truthy
  expect(user_email.disabled?).to be_truthy
end
```

