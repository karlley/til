# gitignore

## gitignore 効かない

1. .gitignore の状態確認
2. git のキャッシュを削除
3. .gitignore の再確認

https://qiita.com/blue0513/items/1525a7f06d708e901690
https://qiita.com/fuwamaki/items/3ed021163e50beab7154

.gitignore 状態確認

```
$ git status --ignored
$ git ls-files --other --ignored --exclude-standard
```

git キャッシュ削除

```
$ git rm -r --cached . //ファイル全体キャッシュ削除
$ git rm -r --cached [ファイル名]  //ファイル指定してキャッシュ削除
```

きちんと設定されていると再度push するとignore したいファイルが削除されてpush される 
