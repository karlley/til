# heroku_task_scheduler

heroku をスリープ状態にさせないようにする

## 参照

[Herokuを無料でフル活用する方法 \| 資格マフィア](https://shikaku-mafia.com/heroku-free-plan/)

[Herokuでスリープ防止のためにスケジューラを作成する \- Qiita](https://qiita.com/kyabetsuda/items/391044601a113f73667d)

[herokuを24時間稼働させる設定 \- エンジニアになりたい肉体労働者](https://yukitoku-sw.hatenablog.com/entry/2020/02/04/225151)

[英語での住所の書き方がスラスラわかる7つのステップ](http://eigo-box.jp/others/address/)

[Herokuにクレジットカードを登録 \- くたくたじゅうよん](https://scrapbox.io/takker/Heroku%E3%81%AB%E3%82%AF%E3%83%AC%E3%82%B8%E3%83%83%E3%83%88%E3%82%AB%E3%83%BC%E3%83%89%E3%82%92%E7%99%BB%E9%8C%B2)

[アカウントの確認 \| Heroku Dev Center](https://devcenter.heroku.com/ja/articles/account-verification)

[curlコマンドでちょこっとHTTPリクエストを試すだけの記事 \- Qiita](https://qiita.com/akane_kato/items/34b408336f4ec372b139)

[ログ記録 \| Heroku Dev Center](https://devcenter.heroku.com/ja/articles/logging)

## ポイント

* heroku にクレジットカードを登録するとフリープランの550dynoに+450dyno が追加される
* Heroku Scheduler で定期的にアプリを立ち上げることでもっさり感を無くす
* Heroku Scheduler を使用するにはクレカの登録が必要
* `curl` コマンドを使いアプリへのアクセスを定期実行することでheroku がスリープするのを防止する

## Heroku Scheduler を使用したアプリの定期実行

1. Heroku Scheduler のアドオン追加

```
$ heroku addons:create scheduler:standard
Creating scheduler:standard on ⬢ karlley-departure... free
 To manage scheduled jobs run:
 heroku addons:open scheduler

Created scheduler-spherical-10282
Use heroku addons:docs scheduler to view documentation
```

2. ブラウザでSceduler の設定画面を開く

```
$ heroku addons:open schedule
```

3. `create job` をクリック
4. `Schedule` に実行する時間の間隔、`Run Command` に実行するコマンドを入力

`Run Command` に`curl https://karlley-departure.herokuapp.com/` を入力

5. Scheduler の動作を確認

```
# heroku のログに[scheduler.プロセスNo.]の文字が表示されているか確認
$ heroku logs -t
.
2021-10-20T03:36:01.281452+00:00 app[scheduler.3455]:

# heroku から出力されるログに絞り込み
$ heroku logs --source heroku
.
2021-10-20T04:52:07.150289+00:00 heroku[scheduler.3887]: Starting process with command `curl https://karlley-departure.herokuapp.com/`
```
