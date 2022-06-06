# image_handling

画像の取り扱いについて

## 参照

[Webサイトの画像の取り扱いでやってしまう10の間違い \| DevelopersIO](https://dev.classmethod.jp/articles/top_10_mistakes_in_handling_website_images_and_how_to_solve_them/)

[JPEG圧縮の仕組みと、おすすめ圧縮ツール５選。 \| WWWクリエイターズ](https://www-creators.com/archives/2322)

[JPEG圧縮・軽量化ツール9選｜画像圧縮後の容量と画質を比較 \| Naifix](https://naifix.com/jpeg-lossy-compression/)

[Macで画像（写真）のサイズや解像度を変更する方法 \- IT便利帳](https://itbenricho.com/mac-image-size-kaizodo-henko.html)

## ポイント

* ブラウザ側で無駄にリサイズしない
* できるだけリサイズしないでいいように画像サイズを揃える
* jpeg のファイル品質は最低50% でok
* jpeg のファイル品質は最高でも85% に抑える
* png よりもjpeg を使用した方がファイルサイズが小さくなる
* png は圧縮したファイルを使用する
* ボタン等は画像ではなくCSS で実装
* 画像のダウンロード時間を削減するだけでもユーザビリティは向上する
* できるだけ同じサイトでダウンロードした方が画像サイズが揃いやすい

## 流れ

1. 画像サイズを使用するサイズに合わせてリサイズ
2. リサイズ後の画像を圧縮ツールで50 - 60% 程度に圧縮
3. CSS で必要な部分のみリサイズ
