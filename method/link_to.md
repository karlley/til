# link_to

リンクを生成する

## 参照

[【Rails】 \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/link_to)

[\[Rails\] link\_toのリンク先を別タブで表示させたい \- Qiita](https://qiita.com/Kazuhiro_Mimaki/items/b3f2d718d2bae9083290)

## ポイント

* `href:` は必要ない
* 外部、内部ともにリンク作成は可能
* 別タブで開くには`target: :_blank` オプションを追加
* 別タブはセキュリティ的に危険なので`rel: "noopener noreferrer"` を付けた方が良い
