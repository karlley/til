# ransack_actionview_template error

ransack での`ActionView::Template::Error`

正しいテンプレートが読み込めていない系のエラー

## 原因予想

* @destinations に正しい値が渡っていない
* `have_search_word?` がfalse の場合の`ransack` メソッドの引数`(params[:q])` がおかしい

## 修正前/修正後

* @destinations がnil
* destinations::index 使用

```Ruby
# 修正前
# feed から検索条件に該当する行き先を検索
def set_search
  if logged_in?
    if have_search_word?
      # 検索ワードからスペース区切りで配列を作成
      search_word = params[:q][:name_or_spot_or_address_cont].split(/[\p{blank}\s]+/)
      # 検索ワードの数だけ検索ワードをkey にしたハッシュを作成
      grouping_hash = search_word.reduce({}) { |hash, word| hash.merge(word => { name_or_spot_or_address_cont: word }) }
      # ユーザのフィードから検索
      @q = current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: grouping_hash })
      # 検索結果(distinct: true で重複除外)
      @destinations = @q.result(distinct: true)
      # view h1 表示用にインスタンス変数化
      @search_word = search_word
    end
    # ransack メソッドを使って@q を取得しないとNo Ransack::Search エラーが出る
    @q = current_user.feed.paginate(page: params[:page], per_page: 5).ransack(params[:q])
    @destinations = @q.result(distinct: true)
  end
end
```

```Ruby
# 修正前
# feed から検索条件に該当する行き先を検索
def set_search
  if logged_in?
    if have_search_word?
      # 検索ワードからスペース区切りで配列を作成
      search_word = params[:q][:name_or_spot_or_address_cont].split(/[\p{blank}\s]+/)
      # 検索ワードの数だけ検索ワードをkey にしたハッシュを作成
      grouping_hash = search_word.reduce({}) { |hash, word| hash.merge(word => { name_or_spot_or_address_cont: word }) }
    end
      # ユーザのフィードから検索
      @q = current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: grouping_hash })
      # 検索結果(distinct: true で重複除外)
      @destinations = @q.result(distinct: true)
      # view h1 表示用にインスタンス変数化
      @search_word = search_word
  end
end
```

* `have_search_word?` がtrue での処理を検索用ハッシュ生成処理のみに絞った
* `ransack` メソッドの引数の`(params[:q])` が間違っていた
* 重複していた処理を排除した

## 状況把握

`have_search_word?` のtrue/false でのparams の値

```Ruby
# search_word 無
> params.permit!
{
    "controller" => "static_pages",
        "action" => "home"
}
```

```Ruby
# search_word 有
> params.permit!
{
          "utf8" => "✓",
             "q" => {
        "name_or_spot_or_address_cont" => "福岡",
                          "country_eq" => "",
                          "expense_eq" => "",
                           "season_eq" => ""
    },
        "commit" => "Search",
    "controller" => "destinations",
        "action" => "index"
}
# @search_word が配列になっている
> @search_word.class
Array < Object
```

`grouping_hash` の値

```Ruby
# search_word "福岡 日本"
> grouping_hash
{
    "福岡" => {
        :name_or_spot_or_address_cont => "福岡"
    },
    "日本" => {
        :name_or_spot_or_address_cont => "日本"
    }
}
```

`ransack` メソッドの引数の違いでの比較

```Ruby
# ransack(params[:q])
# search_word "福岡 日本"
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack(params[:q])
Ransack::Search<class: Destination, base: Grouping <conditions: [Condition <attributes: ["name", "spot", "address"], predicate: cont, combinator: or, values: ["福岡 日本"]>], combinator: and>>
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack(params[:q]).result
  Destination Load (2.1ms)  SELECT  "destinations".* FROM "destinations" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) AND (("destinations"."name" ILIKE '%福岡 タワー%' OR "destinations"."spot" ILIKE '%福岡 タワー%') OR "destinations"."address" ILIKE '%福岡 タワー%') ORDER BY "destinations"."created_at" DESC LIMIT $1 OFFSET $2  [["LIMIT", 5], ["OFFSET", 0]]
  ↳ /Users/karlley/.pryrc:24
[]
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack(params[:q]).result.to_sql
"SELECT  \"destinations\".* FROM \"destinations\" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) AND ((\"destinations\".\"name\" ILIKE '%福岡 タワー%' OR \"destinations\".\"spot\" ILIKE '%福岡 タワー%') OR \"destinations\".\"address\" ILIKE '%福岡 タワー%') ORDER BY \"destinations\".\"created_at\" DESC LIMIT 5 OFFSET 0"
```

```Ruby
# ransack({ combinator: 'or', groupings: grouping_hash })
# search_word "福岡 タワー"
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: grouping_hash })
Ransack::Search<class: Destination, base: Grouping <combinator: or>
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: grouping_hash }).result
  Destination Load (2.5ms)  SELECT  "destinations".* FROM "destinations" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) AND ((("destinations"."name" ILIKE '%福岡%' OR "destinations"."spot" ILIKE '%福岡%') OR "destinations"."address" ILIKE '%福岡%') OR (("destinations"."name" ILIKE '%タワー%' OR "destinations"."spot" ILIKE '%タワー%') OR "destinations"."address" ILIKE '%タワー%')) ORDER BY "destinations"."created_at" DESC LIMIT $1 OFFSET $2  [["LIMIT", 5], ["OFFSET", 0]]
  ↳ /Users/karlley/.pryrc:24
+----+------+---------+-------------+---------+---------------------------+---------------------------+---------------------+--------+------------+-----------+-----------------------------------------------+-----------------------+--------+------------+---------+--------+
| id | name | country | description | user_id | created_at                | updated_at                | picture             | spot   | latitude   | longitude | address                                       | expense               | season | experience | airline | food   |
+----+------+---------+-------------+---------+---------------------------+---------------------------+---------------------+--------+------------+-----------+-----------------------------------------------+-----------------------+--------+------------+---------+--------+
| 11 | 福岡 | 153     |             | 1       | 2021-01-30 05:33:03 +0900 | 2021-01-31 04:38:05 +0900 | /images/default.png | タワー | 33.5932846 | 130.35151 | 日本、〒814-0001 福岡県福岡市早良区百道浜...  | ￥200,000 ~ ￥300,000 | 5      | 観光       | 4       | もつ鍋 |
+----+------+---------+-------------+---------+---------------------------+---------------------------+---------------------+--------+------------+-----------+-----------------------------------------------+-----------------------+--------+------------+---------+--------+
1 row in set
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: grouping_hash }).result.to_sql
"SELECT  \"destinations\".* FROM \"destinations\" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) AND (((\"destinations\".\"name\" ILIKE '%福岡%' OR \"destinations\".\"spot\" ILIKE '%福岡%') OR \"destinations\".\"address\" ILIKE '%福岡%') OR ((\"destinations\".\"name\" ILIKE '%タワー%' OR \"destinations\".\"spot\" ILIKE '%タワー%') OR \"destinations\".\"address\" ILIKE '%タワー%')) ORDER BY \"destinations\".\"created_at\" DESC LIMIT 5 OFFSET 0"
```

`search_word` が未入力の場合の検索結果(全件取得)

```Ruby
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: nil })
Ransack::Search<class: Destination, base: Grouping <combinator: or>>
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: nil }).result
  CACHE Destination Load (0.0ms)  SELECT  "destinations".* FROM "destinations" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) ORDER BY "destinations"."created_at" DESC LIMIT $1 OFFSET $2  [["LIMIT", 5], ["OFFSET", 0]]
  ↳ /Users/karlley/.pryrc:24
+----+------------------+---------+------------------+---------+------------------+------------------+------------------+-----------------+------------------+------------------+------------------+------------------+--------+------------------+---------+------------------+
| id | name             | country | description      | user_id | created_at       | updated_at       | picture          | spot            | latitude         | longitude        | address          | expense          | season | experience       | airline | food             |
+----+------------------+---------+------------------+---------+------------------+------------------+------------------+-----------------+------------------+------------------+------------------+------------------+--------+------------------+---------+------------------+
| 11 | 福岡             | 153     |                  | 1       | 2021-01-30 05... | 2021-01-31 04... | /images/defau... | タワー          | 33.5932846       | 130.35151        | 日本、〒814-0... | ￥200,000 ~ ...  | 5      | 観光             | 4       | もつ鍋           |
| 10 | Tamimouth        | 107     | This is Faker... | 1       | 2021-01-26 11... | 2021-01-28 05... | /images/defau... | Hartmann Drive  | 16.3575019107... | 157.297381554... | Suite 798 973... | ￥200,000 ~ ...  | 12     | Your Experience! | 13      | Pappardelle a... |
| 9  | Bartolettifurt   | 50      | This is Faker... | 1       | 2021-01-26 11... | 2021-01-26 11... | /images/defau... | Shanna Drive    | 75.0395353325... | 95.5757776587... | Apt. 147 364 ... | ￥700,000 ~ ...  | 2      | Your Experience! | 5       | Pierogi          |
| 8  | Schaeferside     | 135     | This is Faker... | 1       | 2021-01-26 11... | 2021-01-26 11... | /images/defau... | Rivka Extension | 35.6741807999... | 139.7499491      | 77621 Emory P... | ￥300,000 ~ ...  | 2      | Your Experience! | 63      | Tuna Sashimi     |
| 7  | Port Frankieland | 80      | This is Faker... | 1       | 2021-01-26 11... | 2021-01-26 11... | /images/defau... | Waelchi Village | 3.09176227928... | -99.116044011... | Suite 176 146... | ￥200,000 ~ ...  | 1      | Your Experience! | 40      | Pasta with To... |
+----+------------------+---------+------------------+---------+------------------+------------------+------------------+-----------------+------------------+------------------+------------------+------------------+--------+------------------+---------+------------------+
5 rows in set
> current_user.feed.paginate(page: params[:page], per_page: 5).ransack({ combinator: 'or', groupings: nil }).result.to_sql
"SELECT  \"destinations\".* FROM \"destinations\" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) ORDER BY \"destinations\".\"created_at\" DESC LIMIT 5 OFFSET 0"
[15] pry(#<DestinationsController>)>
```

`ransack` メソッドの引数の違いでのSQL の比較

```SQL
# (params[:q])
"SELECT  \"destinations\".* FROM \"destinations\" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) AND ((\"destinations\".\"name\" ILIKE '%福岡 タワー%' OR \"destinations\".\"spot\" ILIKE '%福岡 タワー%') OR \"destinations\".\"address\" ILIKE '%福岡 タワー%') ORDER BY \"destinations\".\"created_at\" DESC LIMIT 5 OFFSET 0"

# ({ combinator: 'or', groupings: grouping_hash })
"SELECT  \"destinations\".* FROM \"destinations\" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) AND (((\"destinations\".\"name\" ILIKE '%福岡%' OR \"destinations\".\"spot\" ILIKE '%福岡%') OR \"destinations\".\"address\" ILIKE '%福岡%') OR ((\"destinations\".\"name\" ILIKE '%タワー%' OR \"destinations\".\"spot\" ILIKE '%タワー%') OR \"destinations\".\"address\" ILIKE '%タワー%')) ORDER BY \"destinations\".\"created_at\" DESC LIMIT 5 OFFSET 0"

# ({ combinator: 'or', groupings: nil })
"SELECT  \"destinations\".* FROM \"destinations\" WHERE (user_id IN (SELECT followed_id FROM relationships WHERE follower_id = 1) OR user_id = 1) ORDER BY \"destinations\".\"created_at\" DESC LIMIT 5 OFFSET 0"
```

* 検索ワードが入らない場合はsearch_word は`[]` になり、grouping_hashは`{}` になる
* @q はransack メソッドの引数のgroupings が空なので結果的に全投稿が代入される
* `have_search_word?` での分岐は`search_word.reduce` でnilエラーが出るので必要
* `ransack` メソッドの引数を`(params[:q])` にするとsearch_word 毎にカラムから検索されない
* `have_search_word?` がfalse の場合は`ransack` メソッドの引数は`({ combinator: 'or', groupings: nil })` になり全件取得できる
