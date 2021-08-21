# googlefonts

rails にGoogleFonts を使用

## 参照

[RailsでのWebフォント（Google Fonts）の使い方 \- Qiita](https://qiita.com/koki_73/items/ca1e4965dd6848167a2d)

[font\-familyの書き方まとめ：CSSでフォント種類を指定しよう](https://saruwakakun.com/html-css/basic/font-family-how-to)

[Webフォントを使う時はbold時のfont\-weightを読み込んだ上で扱う \- Qiita](https://qiita.com/aktuehr/items/a2567aa7cc708cd9a4bd)

[そろそろWebフォントのFOUT問題・Web Vitals対策をがっちりと検証しておこう\(SEO・読み込み方法・サブセット化・ユーザビリティなど\) ｜ ノースディテール](https://www.northdetail.co.jp/blog/1324/)

[Google Fonts を非同期で読み込みサイトスピードを高速化 – FirstLayout](https://firstlayout.net/fast-display-even-with-google-fonts/)

[Google Fontsを使う限りPage Speedは上がらない \| tm23forest\.com](https://tm23forest.com/contents/google-fonts-and-page-speed-insights-opinion)

## ポイント

* `@import` はscss ファイルでないと適応されない(css ファイルはNG)
* コンパイル後のHTML ファイルには@import のurl は出力されない
* font-weight の違う同一フォントを使用する場合は必要な太さのみインポートする
* web フォント特有の問題としてページスピードやチラツキなどの問題有り
* ページスピードやチラツキなどを抑制する方法がある

## 導入

1. GoogleFonts でフォントを選択
2. `Select This Style` をクリックして`@import` 用のソースを表示
3. scss ファイルにフォント読込、フォントスタイルを追加

Roboto(100, 300, italic), Noto(100, 300) を使用する場合

```CSS
//custom.scss
//インポート
@import "bootstrap-sprockets";
@import "bootstrap";
//下記追加
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@100;300&family=Roboto:ital,wght@0,100;0,300;1,100&display=swap');
```

```SCSS
//custom.scss
//font 指定
body {
font-family: 'Roboto', 'Noto Sans JP', sans-serif;
}

//font-weihgt 指定
h1, h2, h3, h4, h5, h6 {
  line-height: 1;
  font-weight: 300;
}
```
