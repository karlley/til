# naming

メソッド/モデル の命名について

## 参照

[モデルやメソッドに名前を付けるときは英語の品詞に気をつけよう \- Qiita](https://qiita.com/jnchito/items/459d58ba652bf4763820)

[【プログラミング】メソッド名や関数名などを英語で命名する際に参考にしているQiitaの記事5選 – monaga\.site](https://monaga.site/naming-english-when-programming/)

[Railsモデルのメソッドの命名について \- Qiita](https://qiita.com/qpSHiNqp/items/7bcb0492c777a488ceba)

[英語 名詞の種類（不可算名詞とは？）｜スタディサプリ大学受験講座](https://studysapuri.jp/course/entrance-exam/lineup/subject/english00007.html)

[【英語】数えられない名詞”不可算名詞“って何? \| 勉強の悩み・疑問を解消！小中高生のための勉強サポートサイト｜SHUEI勉強LABO](https://www.shuei-yobiko.co.jp/labo/jh-english-10/)

## model 名

* 名詞(◯Payment ✕Pay)
* 形容詞 + 名詞(◯AttachedFile ✕AttachFile)
* 形容詞が無い場合は過去分詞形を使う(Attached)
* 名詞 + 名詞(UserGroup)
* 動詞 + 名詞だとメソッドに見えてしまうのでNG

## method 名

* 動詞(◯activate ✕active)
* 動詞 + 名詞(○ ticket.notify_expiration ✕ticket.notify_expire)
* 動詞 + 動詞は文法的にNG

動詞で始める系、動詞で始めない系の2系統が存在する

### 動詞で始める系

* 処理を実行する系のメソッド

### 動詞で始めない系

* プロパティ系(一定の値を返すだけ)は動詞で始めなくてOK(full_name)
* 真偽値を返すメソッドは動詞で始めなくてOK(action_required?)
* データ型を変換するメソッドは動詞ではじめない(to_date)
* 英文そのままをメソッドにする場合(member_count_should_not_exceed)

## 不可算名詞を使わない

* ルーティング、モデル、コントローラ等で複数/単数を使い分けるRails には不向き
* 使用する場合は違う言い回しを選択(◯something required ✕need something)

## 可算名詞/不可算名詞

名詞の種類

### 可算名詞

user, post, country

* 数えられる名詞
* 複数形で`s` を付ける

### 不可算名詞

coffee, rain, bread, money, infomation, news

* 数えられない名詞
* 複数形で`s` が付かない
* much やlittle で量を表現
* 複数形で単語自体が変わる

## 調べ方

下記のキーワードを単語と合わせて検索

```
# payment の定義をググる
payment define
```

* 定義: `define`
* 動詞: `verb`
* 名詞: `noun`
* 形容詞: `adjective`
* 複数形: `plural`
* 可算名詞/不可算名詞: `countable uncountable`
* 対義語: `opposite`

## 参考サイト

[英和辞典・和英辞典 \- Weblio辞書](https://ejje.weblio.jp/)

[Synonyms and Antonyms of Words \| Thesaurus\.com](https://www.thesaurus.com/)

[和英辞典・自動翻訳だけじゃダメ！もっといい英語名を見つけるためのTips集 \- Qiita](https://qiita.com/jnchito/items/3815a755b889b64a1840)

[英語力をアップさせる知見がいっぱい！「Rubyistのための英語勉強会」を開催しました \- give IT a try](https://blog.jnito.com/entry/2015/09/02/120620)

[短い時間で十分伝わるレベルの英文を書くための5つのポイント \- give IT a try](https://blog.jnito.com/entry/2013/07/05/072858)

