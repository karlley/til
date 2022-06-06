# selectbox_html

セレクトボックス作成時のHTML の要素について

## 参照

[<select>: HTML 選択要素 \- HTML: HyperText Markup Language \| MDN](https://developer.mozilla.org/ja/docs/Web/HTML/Element/select)

## それぞれの要素について

```HTML
# サンプル
<label for="pet-select">Choose a pet:</label>

<select name="pets" id="pet-select">
    <option value="">--Please choose an option--</option>
    <option value="dog">Dog</option>
    <option value="cat">Cat</option>
    <option value="hamster">Hamster</option>
    <option value="parrot">Parrot</option>
    <option value="spider">Spider</option>
    <option value="goldfish">Goldfish</option>
</select>
```

`label`:
* セレクトボックスの名前
* `for` でラベル名を指定することで`select` 要素のアクセシビリティ用の`id` と紐付けることができる

`select`:
* セレクトボックス
* 複数選択可の`multiple`等の固有の属性がある

`option`:
* セレクトボックスの選択肢
* 1要素につき必ず1つの`value` 属性を持つ
* `selected` 属性を付けることで画面遷移時に選択済みにする

`value`:
* サーバに送信される値
* `value` が含まれない場合は`option` で囲まれた値が送信される
