# image_tag

画像を表示する

## 参照

[【Rails】 image\_tagを使って簡単に画像を表示させよう！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/image_tag)

[【Rails】image\_tagの基本とデフォルト画像の設定方法 \- Qiita](https://qiita.com/Toshimatu/items/a6382efd410fe5544406)

[image\_tag \| Railsドキュメント](https://railsdoc.com/page/image_tag)

## 画像ファイルのディレクトリと読込

* `app/assets/images/`: ファイル名の前に`/`は不要 `image_name.jpg`
* `public`: ファイル名の前に`/` が必要 `/image_name.jpg`

## ポイント

* `image_tag` のオプションの`size` は画像を読み込む際のサイズを指定するだけ
* 画像サイズのスタイリングは最終的にCSS で微調整する
* `alt` 属性をオプションで付与できる
