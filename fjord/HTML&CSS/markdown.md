# markdown

HTMLをより書き易くしたもの

## 参照

[Daring Fireball: Markdown Syntax Documentation](https://daringfireball.net/projects/markdown/syntax)

## ポイント

* markdownを導入されているサービス毎に方言がある
* markdownでカバーしていないHTMLは直接タグを記述する
* サービスによっては改行の`br`タグが自動挿入されるものもある
* code block内のコードの言語を` ```HTML `のように記述する事でシンタックスハイライトが効く
* inline code をコードの記述以外の用途で使うのはNG

## vscode で選択文字をタグで囲む

[VS Codeで選択範囲をHTMLタグで囲むショートカットキーを設定する \- Qiita](https://qiita.com/MToyokura/items/da4fb3477affd0278506)

1. 囲みたい文字を選択
2. `cmd + shift + p` でコマンドパレットを開く
3. `Emmet: Wrap Individual Lines with Abbreviation` を選択
4. 囲みたいタグ名を入力(複数選択の場合は`*`も入力)
5. 選択した文字が任意のタグで囲まれる
