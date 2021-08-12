# model_and_helper_method

model メソッドとhelper メソッドの使い分け、違いについて

## 参照

[モデルに定義するメソッド、ヘルパーに定義するメソッドの違い \- Qiita](https://qiita.com/s_tatsuki/items/9a4b40f29f047efe5a94)

[Ruby \- 【Rails】モデルからヘルパーに定義したメソッドを呼び出す｜teratail](https://teratail.com/questions/42571)

[Ruby on Rails 5 \- Rails　モデルに定義するメソッド、ヘルパーに定義するメソッドの違いがいまいち分からない｜teratail](https://teratail.com/questions/187649)

[【Ruby入門】private と protected の使い方まとめ【メソッドのアクセス制御】 \| 初心者向け完全無料プログラミング入門](https://26gram.com/private-protected-in-ruby)

[Ruby の private と protected 。歴史と使い分け \- Qiita](https://qiita.com/tbpgr/items/6f1c0c7b77218f74c63e)

## ポイント

* controller はルーティングから処理の支持を出すだけに絞る
* view での処理はhelper に切り出す
* DB 関連の処理はmodel メソッドを使用

## 特徴/用途

model method

* 用途: そのモデルに関する処理、直接DB からデータを取得する処理
* 呼び出し: `Object.method_name`

helper method

* 用途: view にパラメータを渡す処理
* 呼び出し: `<%= method_name %>`、`method_name`

## controller private method

用途は少し異なるがコントローラの処理を分ける場合に使用する

controller private method

* 用途: クラス内でしか呼び出したくない処理、セキュリティ関連？
* 呼び出し: `Object.method_name`
