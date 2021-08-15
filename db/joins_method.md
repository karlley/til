# joins_method

テーブルを結合してDB 上のデータを検索する`join` メソッドについて

## 参照

[【Rails】 joinsメソッドのテーブル結合からネストまでの解説書 \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/joins)

[Rails における内部結合、外部結合まとめ \- Qiita](https://qiita.com/yuyasat/items/c2ad37b5a24a58ee3d30)

[Active Record クエリインターフェイス \- Railsガイド](https://railsguides.jp/active_record_querying.html)

## ポイント

* モデル間で関連付いているデータのみ抽出して結合
* 親テーブル > 子テーブルの形でのみ結合が可能
* 結合元になるテーブルのレコードを取得
* 関連するモデルのレコードは`select` で取得
* 条件にマッチしない場合は空の`ActiveRecord::Relation` が返り値
* N+1 が起こりやすいので注意

## SQL

```Shell
# rails console でテーブル結合
> モデル名.joins(:関連名)
```

```SQL
# 発行されるSQL
SELECT カラム名 FROM テーブル名 INNER JOIN 結合するテーブル名 ON 結合条件
```

has_many: destinations, belongs_to: country の場合

条件: Country テーブル の`id` がDestinations テーブルの`country_id` に含まれているデータのみ結合

```Shell
> Country.joins(:destinations)
  SELECT "countries".* FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id"
```

## 使用目的別

* 結合のみ: `joins`
* 取得したいカラムを指定: `joins` + `select(カラム)`
* 親テーブルに条件追加: `joins` + `where(親テーブルのカラム: 条件)`
* 子テーブルに条件追加: `joins` + `where(子テーブル: {子テーブルのカラム: 条件}) `

```Shell
# 結合のみ
> Country.joins(:destinations)
  SELECT "countries".* FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id"

# 取得したいカラムを指定
# countries テーブルの全カラム、destinations テーブルのid
> Country.joins(:destinations).select("countries.*, destinations.id")
  SELECT countries.*, destinations.id FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id"

# 親テーブルに条件追加
# countries テーブルの全カラム, destinatons テーブルのid
# destinations テーブルのregion カラムに"オセアニア"が含まれているdestinations テーブルのid
> Country.joins(:destinations).select("countries.*, destinations.id").where(region: "オセアニア")
  SELECT countries.*, destinations.id FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id" WHERE "countries"."region" = $1  [["region", "オセアニア"]]

# 条件を複数追加
# destinations テーブルのregion カラムに"オセアニア"、"中東"が含まれているdestinations テーブルのid
> Country.joins(:destinations).select("countries.*, destinations.id").where(region: ["オセアニア", "中東"])
  SELECT countries.*, destinations.id FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id" WHERE "countries"."region" IN ($1, $2)  [["region", "オセアニア"], ["region", "中東"]]

# 子テーブルに条件追加
# destinations テーブルのseason カラムに"7" が含まれる
> Country.joins(:destinations).where(destinations: { season: 7 })

# 子テーブルに条件追加、countries テーブルのregionとdestinations テーブルのseason テーブルを結合
# destinations テーブルのseason カラムに"7" が含まれる、countries テーブルのregionとdestinations テーブルのseason テーブルを結合
> Country.joins(:destinations).select("countries.region,destinations.season").where(destinations: { season: 7 })
  SELECT countries.region,destinations.season FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id" WHERE "destinations"."season" = $1  [["season", 7]]

# 親テーブル、子テーブルに条件を追加
# countries テーブルのregion カラムに"アジア"、destinations テーブルのseason カラムに"7" が含まれる
> Country.joins(:destinations).where(region: "アジア", destinations: { season: "7" })
  SELECT "countries".* FROM "countries" INNER JOIN "destinations" ON "destinations"."country_id" = "countries"."id" WHERE "countries"."region" = $1 AND "destinations"."season" = $2  [["region", "アジア"], ["season", 7]]
```
