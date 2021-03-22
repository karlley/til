# resize_image_fit_parent

親要素に合わせて画像をトリミング/リサイズ

## 参照

[サイズがバラバラな画像をレスポンシブで縦横比を揃えて表示させる \- Qiita](https://qiita.com/becolomochi/items/265a7f940a1c809f5ba7)

## 実装

```Ruby
# 修正前
  <span class="picture">
    <%= link_to((image_tag destination.picture.thumb200.url), destination_path(destination.id), class: "destination-picture") if destination.picture.url.present? %>
  </span>
```

* 画像のフレームをインライン要素からブロック要素に変更
* div に`destination-picture-frame` を追加

```Ruby
# 修正後
<%= link_to destination_path(destination.id) do %>
  <div class="portfolio-entry destination-picture-frame">
    <%= image_tag(destination.picture.thumb200.url) if destination.picture.url.present? %>
  </div>
<% end %>
```

```CSS
# 親要素のサイズに合うようにトリミング
.destination-picture-frame {
  position: relative;
  overflow: hidden;
  padding-top: 60%;
}

.destination-picture-frame img {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```







