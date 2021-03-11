# start again

中断した作業を再スタートしやすい仕組みについて

## 目的

* 理解できる箇所を先に片付ける事で完成度を早く上げる
* 作業を滞らせない事でモチベダウン防止

## ポイント

* 未解決部分別に作業メモを残しを検索しやすくする
* 作業メモをテンプレ化して再スタート時に状況の把握を素早くする

## ツール

* vscode 拡張機能 todo highlight
* vim `Rg` コマンド

## 流れ

中断/再スタートで行動をフレームワーク化

### 1. 中断

自己解決, 他者解決で3時間経っても解決しない場合に中断と判断する

1. 作業メモを作成
2. 作業途中のコードに`DEBUG/FIXME` コメントを追加する

### 2. 再スタート

外部の協力でのヒントが回答された, 自己解決できそうな場合の行動

1. 作業メモに解決できそうな方法を追記
2. vscode todo highlight で`DEBUG/FIXME` を検索して問題箇所をリスト化([todo highlight 検索方法](https://github.com/karlley/dotfiles/blob/master/VSCode/todo_highlight.md))
3. vim `Rg` コマンドで`DEBUG/FIXME` を検索して解決を試みる([vim Rg 検索方法](https://github.com/karlley/dotfiles/blob/master/Vim/fzf-vim.md))
4. 解決した場合は作成した作業メモ, 外部からの回答を元にアウトプットを作成する
