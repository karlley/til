# show_change_log

特定のファイルの変更履歴を表示

## 参照

[Gitで特定ファイルの変更履歴をみる \- Bye Bye Moore](https://shuzo-kino.hateblo.jp/entry/2014/05/22/222251)

[Gitでコミットメッセージや変更内容を検索して確認する方法 \| Rriver](https://parashuto.com/rriver/tools/git-log-s-option)

## ポイント

* ファイル毎、ディレクトリ単位での変更履歴を表示可能

## やり方

1. ファイル毎の変更履歴からコミットのハッシュを取得
2. 取得したコミットのハッシュから変更の詳細を表示

```Shell
# file_name の変更履歴を表示(コミットハッシュを取得)
$ git log -p file_name

# コミットハッシュから変更の詳細を表示
$ git show commit_hash
```

