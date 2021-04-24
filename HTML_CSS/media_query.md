# media_query

メディアクエリでスタイルを分岐させる

## 参照

[<meta name="viewport" content="width=device\-width,initial\-scale=1">
](https://qiita.com/ryounagaoka/items/045b2808a5ed43f96607)

[スマホ用のサービスの開発環境を整える【Rails】 \- Qiita](https://qiita.com/Hassan/items/2fd4150bbdd6fcd92afc)

[【CSS】スマホで Media Queries が効かないときの対処法 \- Qiita](https://qiita.com/noraworld/items/10926dfa6c88f3afdbb3)

## 手順

1. `<head>` タグにviewport 設定追加
2. css, sass ファイルに分岐させたいスタイルを追加

```Ruby
# app/view/layouts/_rails_default.html.erb

<%= csrf_meta_tags %>
<%= csp_meta_tag %>

# viewport 設定追記↓
# 多分css 系のファイルより先に読み込む必要がある
<meta name="viewport" content="width=device-width,initial-scale=1">
<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
<%# Font Awesome 読み込み %>
<script src="https://kit.fontawesome.com/1259488a6b.js" crossorigin="anonymous"></script>
```

```CSS
# custom.scss

# 768px までのスタイルを追記
@media screen and (max-width: 768px) {
  .jumbotron-extend {
    height: 100%;
    h1 {
      font-size: 40px;
    }
  }
}
```

## 何を優先したスタイルを当てるか

スタイルの優先方法が2種類ある

* モバイルファースト
* PC ファースト

どちらを優先するかによって記述する順番を変える必要がある


```
# モバイルファースト

@media screen and (max-width: 500px) {
  /* スマホ用のスタイル */
}
@media screen and (max-width: 1200px) {
  /* PC用のスタイル */
}
```

```
# PC ファースト

全体のスタイル

@media screen and (max-width: 500px) {
  /* スマホ用のスタイル */
}
```

## min/max

min: ~まで
max: ~から

```CSS
@media screen and (min-width:770px){
画面幅 770px ~ ∞ 以上のスタイル
style
}

@media screen and (max-width:770px){
画面幅 0 ~ 770px 以下のスタイル
}
```
