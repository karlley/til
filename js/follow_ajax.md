# follow_ajax

follow 機能をAjax 化

## 参照

[Railsでユーザーフォロー機能を実装する（Ajax使うよ）① \- Qiita](https://qiita.com/tanutanu/items/a4a33554cb0e873720e9)

[Railsでユーザーフォロー機能を実装する（Ajax使うよ）② \- Qiita](https://qiita.com/tanutanu/items/5f41ece86e9d2b70bae4)

[ユーザーフォロー機能の実装 \- Hit the books\!\!](https://ud-ike.hatenablog.com/entry/2020/12/28/190830)

[Railsで remote: true と js\.erbを使って簡単にAjax\(非同期通信\)を実装しよう！\(いいね機能のデモ付\) \- Qiita](https://qiita.com/motoki0208/items/45211df065e0c037d032)

[【Rails】 remote: \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/remote-true)

[jQueryで要素のテキストやHTMLを変更するtext\(\)とhtml\(\)の使い方 \| キノコログ](https://kinocolog.com/jquery_text_html/)

[Ruby on Rails \- Rails AjaxでPatialを切り替える方法について｜teratail](https://teratail.com/questions/32771)

## 処理の流れ

1. 動的に変化させたい要素の親要素にjs 処理用のid を振る
2. ボタン等の動的に非同期DB 処理を分岐させたい要素はパーシャル化してrender で再描画する事で表示を切り替える
3. テキストや数値などの動的な描画は変化させる要素にid を振り、js(jQuery) で再描画する

## ポイント

* パーシャルではインスタンス変数はローカル変数に代入して使用する
* パーシャルからパーシャルを呼び出す場合は明示的にローカル変数を記述して可読性を上げる
* インスタンス変数名を明確にする事で複雑な処理の可読性を上げる
