# hidden_field_tag

`form` タグ等のパラメータ送信時に使用する`hidden_field_tag` について

## 参照

[f\.hidden\_fieldとhidden\_field\_tagの使い方【Ruby on Rails】 \- SakuraWi \- BLog](https://sakurawi.hateblo.jp/entry/hidden_field)

[\[Rails\]hidden\_fieldとhidden\_field\_tagの違いについて！\[初心者\] \- Qiita](https://qiita.com/jackie0922youhei/items/6c4ba67db446e0c91944)

## ポイント

* フォーム入力無しでパラメータを送信する際に使用
* `hidden_field` はcontroller で生成したインスタンスを渡すのに使用、`hidden_field_tag` はmodel/controller 関係なくパラメータを送信可能
* 送信する値の変数名は`:変数名` で指定する

```Ruby
# 送信元
<%= form_for(current_user.active_relationships.build, remote: true) do |f| %>
  <% # フォロー中/フォロワーのユーザ情報、表示ページのユーザ情報をrelationships#create に送信 %>
  <div><%= hidden_field_tag :followed_id, relationship_user.id %></div>
  <div><%= hidden_field_tag :current_page_user_id, @user.id %></div>
  <%= f.submit "フォローする", class: "follow-button btn" %>
<% end %>

# 受信側
class RelationshipsController < ApplicationController
  before_action :logged_in_user

  def create
    # users/_list_follow から送信されたrelationship_user.id を:follow_idとしてフォローするユーザを取得
    @follow_user = User.find(params[:followed_id])
    current_user.follow(@follow_user)
    # user/_list_follow から送信されたcurrent_page_user.id で表示するユーザを取得
    @user = User.find(params[:current_page_user_id])
    respond_to do |format|
      # show/show-follow で分岐させる必要有り
      format.html { redirect_to @follow_user }
      format.js
    end
  end

end
```
