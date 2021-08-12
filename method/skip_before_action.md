# skip_before_action

`before_action` で呼び出しているメソッドを特定のアクションで無効化する

## 参照

[【Rails】 before\_actionの使い方を徹底解説！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/before_action)

[Action Controller の概要 \- Railsガイド](https://railsguides.jp/action_controller_overview.html#%E3%83%95%E3%82%A3%E3%83%AB%E3%82%BF)

[Railsでskip\_before\_actionにてifとonly\(except\)を併用できない \- Qiita](https://qiita.com/ash1day/items/58a617b344fd8a45eb30)

## ポイント

* 複数のフィルタ定義をしても最後に定義されたフィルタが適応される
* `if:` でさらに絞り込みを追加できる
