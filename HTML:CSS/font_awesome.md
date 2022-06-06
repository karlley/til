# font_awesome

font awesome の使い方

* Font Awesome バージョン 5.9 以前はCDN, ダウンロードで読み込む
* Font Awesome バージョン 5.9 以降はアカウント登録が必須, 読み込みはJS

## 参照

[Webアイコンフォントの【Font Awesome】がアカウント登録必須になったので、使い方をおさらいしました \| Webクリエイターボックス](https://www.webcreatorbox.com/webinfo/font-awesome)

## 手順

1. [Font Awesome](https://fontawesome.com/) でアカウント作成
2. アカウント作成後に発行されるKit ID を含むKit Code をhead タグ内に貼り付ける
3. [Icons \| Font Awesome](https://fontawesome.com/icons?d=gallery&p=2) で使用したいアイコンを検索
4. 生成されたHTML を使用する場所に貼り付ける

```Ruby
# view/layouts/_rails_default.html.erb
.
<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
# これを貼り付け
<script src="https://kit.fontawesome.com/1259488a6b.js" crossorigin="anonymous"></script>
```

```HTML
# 貼り付けるHTML
<i class="far fa-heart"></i>
```
