# heroku_environment_variable

heroku 内での環境変数について

## 参照

[\[heroku\] 環境変数の操作 \- Qiita](https://qiita.com/colorrabbit/items/18db3c97734f32ebdfde)

[Heroku \- Herokuデプロイ時\.envの管理について｜teratail](https://teratail.com/questions/75408)

## ポイント

* ローカルの`.zshrc` に環境変数を書いて読み込むような方法は無い
* heroku の環境変数の設定はコンソール/ブラウザ どちらから設定する必要有り
* GoogleMap API の環境変数はセットしたのか覚えてない？

## heroku-config

`heroku-config` というheroku add on を使用すればローカルの`.env` をheroku に自動設定できるらしい

[Herokuで本番環境の環境変数\(config vars\)を\.envファイルで設定する \- dackdive's blog](https://dackdive.hateblo.jp/entry/2016/01/26/121900)

[\[Heroku\] \.envファイルで環境変数を設定・管理する \| エンジニアもどきの技術メモ](https://e-tec-memo.herokuapp.com/article/277/)

[xavdid/heroku\-config: \[Utility\] Push and pull heroku environment variables to your local env](https://github.com/xavdid/heroku-config)
