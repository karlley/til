# flexbox_nth_child_disable

flexbox でn番目の要素のflex化を無効化する

## 参照

[CSS \- flexボックスを使用するときに一部のボックスは横並びにさせない｜teratail](https://teratail.com/questions/109182)

[flexboxでnth\-childを使って特定番目だけカラム数を変える指定 \- tanabota（タナボタ）｜tanabota（タナボタ）](https://tanabota.blog/2019/04/11/flexbox-nth-child-column/)

## ポイント

* `nth-child` を使いn番目の要素に`width: 100%` を指定
* n番目からm番目まで指定したい場合は`:nth-child(-n+m)` で指定可能

## 実装


