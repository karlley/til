# rails_action_name

view, controller 等でアクション毎で分岐させる

## 参照

[アクションだけ異なるフォームを条件分岐してパーシャル化する方法｜エラーは資産](https://yuta-blog.tokyo/partial-bunki/)

[\[Rails 4\.x\] ViewからController名やAction名を参照する方法 \- Qiita](https://qiita.com/colorrabbit/items/ef91170d9b5c266ad416)

## ポイント

* view, controller でアクション名を取得可能
* `controller.action_name` の`controller` は省略可能

## 実装

users#followingアクション、users#followersアクション でh1の表示を分岐させる

```Ruby
# users#follow

  <% if controller.action_name == "following" %>
    <h1>Following / <%= link_to "Followers", followers_user_path %></h1>
  <% elsif controller.action_name == "followers" %>
    <h1><%= link_to "Followings", following_user_path %> / Followers</h1>
  <% end %>
```
