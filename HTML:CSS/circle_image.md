# circle_image

画像を丸く切り抜く

## 参照

[object\-fit:cover;](https://blog.ver001.com/css-image-border-radius-object-fit/)

[1行追加でOK！CSSだけで画像をトリミングできる「object\-fit」プロパティー \| Webクリエイターボックス](https://www.webcreatorbox.com/tech/object-fit)

## 実装

```Ruby
#  修正前
<%= link_to gravatar_for(destination.user, size: 50), destination.user %>
```

* a タグにclass を付与する為に`link_to` をループにする
* `border-radius` を使い画像を円形に切り抜く
* `object-fit` で画像を中心で切り抜く

```Ruby
# 修正後
<%= link_to destination.user, class: "user-icon" do %>
  <%= gravatar_for(destination.user, size: 50) %>
<% end %>
```

```CSS
.user-icon img {
  width: 50px;
  height: 50px;
  object-fit:cover;
  border-radius: 50%;
}
```
