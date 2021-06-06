# initial_inherit_unset_revert

CSS のスタイルの継承で使用するプロパティ

## 参照

[CSS値の「initial」「inherit」「unset」「revert」の違い](https://qiita.com/h-naito/items/3027f92dde68899159c7)

## ポイント

1. `initial`: 初期値にリセット
2. `inherit`: スタイルを親要素から継承, 親要素に指定がない場合はおそらく`initial` と同様の動作
3. `unset`: プロパティをリセット後、再度セットする
4. `revert`: ユーザーエージェントのスタイルをセットする
