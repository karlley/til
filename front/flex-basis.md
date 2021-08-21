# flex-basis

flexbox 使用時に要素の幅を決める

## 参照

[flex\-basis \- CSS: カスケーディングスタイルシート \| MDN](https://developer.mozilla.org/ja/docs/Web/CSS/flex-basis)

[flex\-basisとwidthの違い なぜどうして？](https://kanasys.com/tech/830)

[flex\-basisとは？widthとの違いや効かないときの対処を解説 \| ウェブカツ公式BLOG](https://webukatu.com/wordpress/blog/18947/)

## ポイント

* `width`、`height` で上書き不可
* 縦並び/横並びを決める`flex-direction` の値で`flex-basis` の値が縦幅/横幅の値に切り替わる
* 孫要素には影響ない仕様のようだが孫要素の幅指定が上手くいかなかったので`width` を使用した
* 優先度がかなり強いのでできる事なら使わない方がいいかも？


