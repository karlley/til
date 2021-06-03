# bootstrap3_dropdown_content

Bootstrap3 でのドロップダウンでコンテンツの表示/非表示を切り替える

## 参照

[ドロップダウン ≪ コンポーネント ≪ Bootstrap3日本語リファレンス](http://bootstrap3.cyberlab.info/components/dropdowns.html)

[button要素で開閉 ≪ コラップス ≪ JavaScript ≪ Bootstrap3日本語リファレンス](http://bootstrap3.cyberlab.info/javascript/collapse-button.html#usage2)

## ポイント

* ボタン、リストを含む全要素を`dropdown` クラスで囲む
* `button` タグでFontAwesome を囲んだ場合はCSS でボタンの背景色等を消す
* Bootstrap3 のdropdown とcollapse は動作が異なる

## 実装

`dropdown-toggle` をクリックで`dropdown-menu` が開く

```Ruby
<div class="dropdown destination-show-dropdown-menu">
  <button class="dropdown-toggle destination-show-dropdown-toggle" data-toggle="dropdown">
    <i class="fas fa-ellipsis-h"></i>
  </button>
  <ul class="dropdown-menu destination-show-edit-delete" role="menu">
    <% if current_user == @destination.user %>
      <li class="destination-show-edit">
        <%= link_to "Edit", edit_destination_path(@destination) %>
      </li><!-- .destination-show-edit -->
    <% end %>
    <% if current_user.admin? || (current_user == @destination.user) %>
      <li class="destination-show-delete">
        <%= link_to "Delete", destination_path(@destination), method: :delete, data: { confirm: "Are you sure?" } %>
      </li><!-- .destination-show-delete -->
    <% end %>
  </ul><!-- .destination-show-edit-delete -->
</div><!-- .dropdown .destination-show-dropdown-menu -->
```
