# render_partial_locals

`render` メソッドでパーシャルを使用する際の注意点

## 参照

[partialではインスタンス変数を参照しない方がいい \- Qiita](https://qiita.com/mom0tomo/items/e1e3fd29729b2d112a48)

[【パーシャル】インスタンス変数の直接参照ではなく、localsで値を渡す \- fv17の日記 \- Coding Every Day](https://forest-valley17.hatenablog.com/entry/2018/10/15/104936)

[レイアウトとレンダリング \- Railsガイド](https://railsguides.jp/layouts_and_rendering.html#%E3%83%91%E3%83%BC%E3%82%B7%E3%83%A3%E3%83%AB%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B)

## ポイント

* `render` でパーシャルを表示する場合のインスタンス変数の参照はcontroller との結合が上がり再利用性が低下するのでローカル変数を渡して参照する
* ローカル変数名を変更したい場合は`as:` オプションを使用する
* `collection` を使用したループ処理で`render` でパーシャルを表示する場合も`as:` オプションでローカル変数名を変更できる
* `collection` を使用したループ処理で`locals` オプションでローカル変数を設定した場合はレンダリング中のどのパーシャルにも任意の名前のローカル変数を使用できる
* パーシャルからパーシャルを表示する場合はパーシャルを通してローカル変数を渡しているわけではない、最初の`render` の`locals` の値を2番目のパーシャルに渡す必要がある
* 基本的にはインスタンス変数はコントローラで生成した変数にアクセスするためのもの
