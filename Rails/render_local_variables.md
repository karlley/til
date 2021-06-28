# render_local_variables

`render` メソッドでパーシャルに変数を渡す際にインスタンス変数ではなくローカル変数を渡した方が再利用性が高い

## 参照

[partialではインスタンス変数を参照しない方がいい \- Qiita](https://qiita.com/mom0tomo/items/e1e3fd29729b2d112a48)

[rails のrenderにローカル引数を渡す時の注意 \- mikami's blog](https://mikamisan.hatenablog.com/entry/2016/02/23/135019)

## ポイント

* パーシャルで使用する変数がインスタンス変数の場合は影響範囲が広くなるのでローカル変数を使って影響度を下げる
* パーシャルにローカル変数を使う事で再利用時に修正範囲が狭くて済む
* `locals` でローカル変数を指定する場合は`partial:` での記述が無いと変数がパーシャルに渡らない(`undefined local variable`)
