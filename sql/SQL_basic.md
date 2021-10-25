# SQL

SQL の基礎

## 参照

[MySQL入門 基礎編 \(全27回\) \- プログラミングならドットインストール](https://dotinstall.com/lessons/basic_mysql_beginner)

## 基礎、型指定

* 処理の末尾に`;`を付ける
* コメントアウトは`--`、`#`、`/* comment */` で記述
* ENUM 型はインデックス番号でも選択肢の値を指定可能
* ENUM 型は1つの値を選択、SET 型は複数の値を選択可能
* SET型で複数の値を指定する場合はスペースを入れない(`Value1,Value2`)
* ENUM、SET 型で指定された以外の値を選択すると`truncated` エラーが発生する
* SET 型での複数の値をインデックス番号で指定する場合は`2^0, 2^1, 2^2, ...` のように指定するインデックス番号の合計で指定する

```SQL
categories SET('Gadget', 'Game', 'Business')

# Gadget、Game を指定
1 + 2 = 3 を指定

# Gadget、Business を指定
1 + 3 = 4 を指定

# Game, Business を指定
2 + 3 = 5 を指定
```

* BOOL 型は`TRUE`、`FALSE`、`0`、`1` で指定
* DATETIME 型で`NOW()` で現在時刻を指定
* レコードの中に空の値がある場合は`NULL` になる
* 型指定で`NOT NULL` で空データを弾く
* 型指定で`DEFAULT` を指定することで空データをデフォルト値に指定できる(`likes INT DEFAULT 0`)
* 型指定で`CHECK` を指定することで値に制限をかけられる(`likes INT CHECK (likes >= 0 AND likes <= 100)`)
* 型指定で`UNIQUE` を指定することで重複する値に制限をかけられる(`message VARCHAR(140) UNIQUE`)
* `PRIMARY KEY` で特定のカラムをそのテーブルの主キーに指定できる(空データと重複を防げる)(`PRIMARY KEY (id)`)
* `PRIMARY KEY` を指定した特定のカラムに`AUTO_INCREMENT` を付けることで自動でインデックス番号を付与できる(`id INT NOT NULL AUTO_INCREMENT`)

## レコード抽出

* `WHERE` でデータの絞り込み(`SELECT * FROM posts WHERE likes >= 10;`)
* `BETWEEN` で絞り込みの範囲指定が可能(`SELECT * FROM posts WHERE likes BETWEEN 10 AND 20;`)
* `NOT BETWEEN` で絞り込みの範囲指定を反転
* `IN` で指定した値を含むデータの抽出が可能(`SELECT * FROM posts WHERE likes IN (4, 12);`)
* `NOT IN` で`OR` の抽出条件の反転
* `%` で0文字以上の文字、`_` で任意の1文字を表現できる(`SELECT * FROM posts WHERE message LIKE 't%';`)(`SELECT * FROM posts WHERE message LIKE '__a%';`)
* `BINARY` で大文字小文字の識別ができる
* `\`でエスケープすることで特定の記号の抽出が可能(`SELECT * FROM posts WHERE message LIKE '%\%%';`)
* `WHERE` で条件から抽出する場合は`NULL` は除外される
* `IS NULL` を抽出条件に追加することで`NULL` の値も抽出可能(`SELECT * FROM posts WHERE likes != 12 OR likes IS NULL;`)
* `IS NOT NULL` で`NULL` 以外のレコードを抽出(`SELECT * FROM posts WHERE likes != 12 OR likes IS NOT NULL;`)

## レコード並び替え

* `ORDER BY` で並び替え(`SELECT * FROM posts ORDER BY likes;`)
* `ORDER BY` + `DESC` で逆順で並び替え(`SELECT * FROM posts ORDER BY likes DESC;`)
* `LIMIT` で抽出件数の指定(`SELECT * FROM posts ORDER BY likes LIMIT 3;`)
* `OFFSET` で指定した値の先頭のレコードを除外する(`SELECT * FROM posts ORDER BY likes, message LIMIT 3 OFFSET 2;`)

## 数値関数

* 四則演算(`+ - * /`) で計算できる(`SELECT likes * 500 / 3 FROM posts;`)
* `AS` でから見出し名を変更できる(`SELECT likes * 500 / 3 AS bonus FROM posts;`)
* `FLOOR` で切り捨て
* `CEIL` で切り上げ
* `ROUND` で四捨五入

```SQL
SELECT
  likes * 500 / 3 AS bonus,
  FLOOR(likes * 500 /3) AS floor,
  CEIL(likes * 500 /3) AS ceil,
  ROUND(likes * 500 /3) AS round
FROM posts;
```

## 文字列関数

* `SUBSTRING` で文字列抽出(`SELECT message, SUBSTRING(message, 3) AS substring FROM posts;`)
* `CONCAT` で文字列連結(`SELECT CONCAT(message, ' - ', likes) AS concat FROM posts;`)
* `LENGTH` で文字数の抽出(`SELECT message, LENGTH(message) AS length FROM posts;`)
* 日本語の文字数の抽出には`CHAR_LENGTH` を使用(`SELECT message, CHAR_LENGTH(message) AS char_length FROM posts;`)
* 日本語の文字列抽出には`SUBSTRING`で問題ない

## 日時関数

* 日付の抽出には`YEAE`、`MONTH`、`DAY` を使用

```SQL
SELECT created, YEAR(created) AS year FROM posts;
SELECT created, MONTH(created) AS month FROM posts;
SELECT created, DAY(created) AS day FROM posts;
```

* `DATE_FORMAT` で指定したフォーマットで日時の表示

```SQL
SELECT
  created,
  DATE_FORMAT(created, '%M %D %Y, %W') AS date
FROM
  posts;
```

* `DATE_ADD` で指定した経過日時を表示できる

```SQL
SELECT
  created,
  DATE_ADD(created, INTERVAL 7 DAY) AS next_week
FROM
  posts;
```

* `DATEDIFF` で２つの指定した日時の差分を表示

```SQL
SELECT
  created,
  NOW(),
  DATEDIFF(created, NOW()) AS date_diff
FROM
  posts;
```

## レコード更新、削除

* `UPDATE` と`SET` を使用してレコードの更新が可能(`UPDATE posts SET likes = likes + 5 WHERE likes >= 10;`)
* 複数のカラムの更新を同時に行うには`,`で区切る

```SQL
UPDATE
  posts
SET
  likes = likes + 5,
  message = UPPER(message)
WHERE
  likes >= 10;
SELECT * FROM posts;
```

* `DELETE` でレコードの削除(`DELETE FROM posts WHERE likes < 10;`)
* `DELETE` で削除したレコードのインデックス番号は新しいレコードで使われることはない
* `TRUNCATE` でテーブルごと削除した場合はインデックス番号は最初から振り当てられる
* `ON UPDATE` で自動でカラムのデータを更新する
* `SLEEP` でSQL の処理を一時的に中断できる(`SELECT SLEEP(3);`)

## テーブル変更

* `DESC` でテーブルを表示(`DESC posts;`)
* `ALTER TABLE`と`ADD` でカラムを追加可能(`ALTER TABLE posts ADD author VARCHAR(255);`)
* `AFTER` でカラムの追加位置を指定(`ALTER TABLE posts ADD author VARCHAR(255) AFTER id;`)
* `FIRST` で最初の行にカラムを追加できる
* `DROP` でカラムを削除(`ALTER TABLE posts DROP message;`)
* `CHANGE` でカラムの型を変更(`ALTER TABLE posts CHANGE likes points INT;`)
* `RENAME` でカラム名変更(`ALTER TABLE posts RENAME messages;`)
