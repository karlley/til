# ajax_check_point

Ajax が正常に動かない場合のチェック項目

## 参照

[【Rails】 remote: \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/remote-true)

## チェック項目

* 出力されるHTML
* アクション発動時のRails のloｊ
* リクエストの形式(HTML/js)
* リクエストの送信内容

### form_for タグから出力されるHTML を確認
```

# create
<form class="new_relationship" id="new_relationship" action="/relationships" accept-charset="UTF-8" data-remote="true" method="post"><input name="utf8" type="hidden" value="&#x2713;" /><input type="hidden" name="authenticity_token" value="PM8P5wZesl9Ka+90zKNtzLxWcp5bFwW6wSO/jbSa4FrZplvMcTjFWGbGCM54dH+a94zuEH8lji/+CKKyONxqXA==" />
<div><input type="hidden" name="followed_id" id="followed_id" value="144" /></div>
<input type="submit" name="commit" value="フォローする" class="follow-button btn" data-disable-with="フォローする" />
</form>

# destroy
<form class="edit_relationship" id="edit_relationship_278" action="/relationships/278" accept-charset="UTF-8" data-remote="true" method="post"><input name="utf8" type="hidden" value="&#x2713;" /><input type="hidden" name="_method" value="delete" /><input type="hidden" name="authenticity_token" value="i99G0SMPNz+EWhgqPtHHqhwOViQLJkYBTQAnEN8MCVceujiRzrGMU7vEOFY5kZH5ewA1X4RhBxd3nTCdfgqjAw==" />
<input type="submit" name="commit" value="フォロー中" class="unfollow-button btn btn-primary" data-disable-with="フォロー中" />
</form>
```

### create/destory アクションのRails のlog チェック

```
# create
Started DELETE "/relationships/278" for ::1 at 2021-07-05 10:03:28 +0900
Processing by RelationshipsController#destroy as JS
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"i99G0SMPNz+EWhgqPtHHqhwOViQLJkYBTQAnEN8MCVceujiRzrGMU7vEOFY5kZH5ewA1X4RhBxd3nTCdfgqjAw==", "commit"=>"フォロー中", "id"=>"278"}
  User Load (0.4ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = $1 LIMIT $2  [["id", 1], ["LIMIT", 1]]
  ↳ app/helpers/sessions_helper.rb:25
  Destination Load (0.5ms)  SELECT  DISTINCT "destinations".* FROM "destinations" ORDER BY "destinations"."created_at" DESC LIMIT $1 OFFSET $2  [["LIMIT", 12], ["OFFSET", 0]]
  ↳ app/controllers/application_controller.rb:36
  Rendered destinations/_map_infowindow.html.erb (0.8ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.3ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.3ms)
  Rendered destinations/_map_infowindow.html.erb (0.4ms)
  Rendered destinations/_map_infowindow.html.erb (0.6ms)
  Relationship Load (10.5ms)  SELECT  "relationships".* FROM "relationships" WHERE "relationships"."id" = $1 LIMIT $2  [["id", 278], ["LIMIT", 1]]
  ↳ app/controllers/relationships_controller.rb:14
  User Load (28.0ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = $1 LIMIT $2  [["id", 144], ["LIMIT", 1]]
  ↳ app/controllers/relationships_controller.rb:14
  Relationship Load (1.4ms)  SELECT  "relationships".* FROM "relationships" WHERE "relationships"."follower_id" = $1 AND "relationships"."followed_id" = $2 LIMIT $3  [["follower_id", 1], ["followed_id", 144], ["LIMIT", 1]]
  ↳ app/models/user.rb:57
   (1.4ms)  BEGIN
  ↳ app/models/user.rb:57
  Relationship Destroy (1.0ms)  DELETE FROM "relationships" WHERE "relationships"."id" = $1  [["id", 278]]
  ↳ app/models/user.rb:57
   (2.4ms)  COMMIT
  ↳ app/models/user.rb:57
  Rendering relationships/destroy.js.erb
  Rendered users/_follow.html.erb (11.1ms)
   (4.8ms)  SELECT COUNT(*) FROM "users" INNER JOIN "relationships" ON "users"."id" = "relationships"."follower_id" WHERE "relationships"."followed_id" = $1  [["followed_id", 144]]
  ↳ app/views/relationships/destroy.js.erb:2
  Rendered relationships/destroy.js.erb (66.3ms)
Completed 200 OK in 938ms (Views: 155.0ms | ActiveRecord: 50.4ms)

# destroy
Started POST "/relationships" for ::1 at 2021-07-05 10:04:11 +0900
Processing by RelationshipsController#create as JS
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"demYQo5EJMJbIqzqvpaMeWua6bXUz7IVngnds6rfY52QgMxp+SJTxXePS1AKQZ4vIEB1O/D9OYChIsCMJpnpmw==", "followed_id"=>"144", "commit"=>"フォローする"}
  User Load (2.9ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = $1 LIMIT $2  [["id", 1], ["LIMIT", 1]]
  ↳ app/helpers/sessions_helper.rb:25
  Destination Load (0.5ms)  SELECT  DISTINCT "destinations".* FROM "destinations" ORDER BY "destinations"."created_at" DESC LIMIT $1 OFFSET $2  [["LIMIT", 12], ["OFFSET", 0]]
  ↳ app/controllers/application_controller.rb:36
  Rendered destinations/_map_infowindow.html.erb (1.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.3ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.1ms)
  Rendered destinations/_map_infowindow.html.erb (0.2ms)
  Rendered destinations/_map_infowindow.html.erb (0.3ms)
  User Load (0.3ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = $1 LIMIT $2  [["id", 144], ["LIMIT", 1]]
  ↳ app/controllers/relationships_controller.rb:5
   (0.1ms)  BEGIN
  ↳ app/models/user.rb:52
  CACHE User Load (0.0ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = $1 LIMIT $2  [["id", 1], ["LIMIT", 1]]
  ↳ app/models/user.rb:52
  Relationship Create (0.8ms)  INSERT INTO "relationships" ("follower_id", "followed_id", "created_at", "updated_at") VALUES ($1, $2, $3, $4) RETURNING "id"  [["follower_id", 1], ["followed_id", 144], ["created_at", "2021-07-05 01:04:12.255042"], ["updated_at", "2021-07-05 01:04:12.255042"]]
  ↳ app/models/user.rb:52
   (5.9ms)  COMMIT
  ↳ app/models/user.rb:52
  Rendering relationships/create.js.erb
  Relationship Load (0.3ms)  SELECT  "relationships".* FROM "relationships" WHERE "relationships"."follower_id" = $1 AND "relationships"."followed_id" = $2 LIMIT $3  [["follower_id", 1], ["followed_id", 144], ["LIMIT", 1]]
  ↳ app/views/users/_unfollow.html.erb:1
  Rendered users/_unfollow.html.erb (2.4ms)
   (0.6ms)  SELECT COUNT(*) FROM "users" INNER JOIN "relationships" ON "users"."id" = "relationships"."follower_id" WHERE "relationships"."followed_id" = $1  [["followed_id", 144]]
  ↳ app/views/relationships/create.js.erb:2
  Rendered relationships/create.js.erb (8.1ms)
Completed 200 OK in 558ms (Views: 31.3ms | ActiveRecord: 11.4ms)
```

### form_for タグで送信されるリクエストの形式のチェック

* `form_for`, `form_tag` は`remote: true` が必要
* `form_with` はデフォルトが非同期通信対応なので`remote: true` は不要
* `request.format` コマンドで確認可能

```Ruby
# relationships_controller.rb
# create/destory アクション毎にbinding.pry を挿入して値を確認

def create
binding.pry
.
end
```

```Shell
# create
> request.format
#<Mime::Type:0x00007f9c8c5038b0 @synonyms=["application/javascript", "application/x-javascript"], @symbol=:js, @string="text/javascript", @hash=4531546923536184601>

# destroy
> request.format
#<Mime::Type:0x00007fc7732f9a30 @synonyms=["application/javascript", "application/x-javascript"], @symbol=:js, @string="text/javascript", @hash=-3171290800069540651>
```

### js.erb からリクエストの送信のチェック

* js.erb ファイルに`console.log` でリクエストの内容をチェック
* アクションを実行してdev ツールのconsole タブで値を確認

```
# create.js.erb
console.log("create.js.erb を呼び出し");
$("#follow_form").html("<%= escape_javascript(render('users/unfollow')) %>");
$("#followers").html('<%= @user.followers.count %>');

# destroy.js.erb
console.log("destroy.js.erb を呼び出し");
$("#follow_form").html("<%= escape_javascript(render('users/follow')) %>");
$("#followers").html('<%= @user.followers.count %>');
```
