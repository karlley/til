# update_address

update_address メソッドのrspec がパスしない(安定しない)

## 状態

```Ruby
# destinations_controller.rb
  def update
    @destination = Destination.find(params[:id])
    if @destination.update_attributes(destination_params)
      @destination.update_address
      flash[:success] = "Destination updated!"
      redirect_to @destination
    else
      render 'edit'
    end
  end
```

```Ruby
# destination.rb
  # geocoder で使用する文字列を生成
  def address_keyword
    # 国名が選択されている場合は国番号から国名を検索
    if country.present?
      country_name = Country.find_by(id: country).country_name
      [name, country_name, spot].compact.join(', ')
    end
  end

  # geocode 後の座標からaddress を追加する
  def add_address
    if latitude && longitude.present?
      self.address = Geocoder.search([latitude, longitude]).first.address
      save
    end
  end

  # 座標が更新された場合のみgeocode してaddress を更新する
  def update_address
    # if will_save_change_to_attribute?("latitude") || will_save_change_to_attribute?("longitude")
    if saved_change_to_latitude? || saved_change_to_longitude?
      self.address = Geocoder.search([latitude, longitude]).first.address
      update!(address: address)
    end
  end
```

```Ruby
# model/destination_spec.rb
RSpec.describe Destination, type: :model do
  let!(:destination) { create(:destination) }

  context "バリデーション" do

  context "update_address メソッド" do
    it "座標の更新があった場合にaddress カラムを更新できること" do
      destination = create(:destination)
      destination.latitude = 35.658584
      destination.longitude = 139.7454316
      destination.update_address
      expect(destination.address).to eq "日本、〒105-0011 東京都港区芝公園４丁目２−８"
    end

    it "座標の更新が無い場合はメソッドをパスすること" do
      destination = create(:destination)
      destination.update_address
      expect(destination.update_address).to eq nil
    end
  end
end
```

## 仮説

1. address_keyword メソッドで住所, 座標を生成する文字列が正しく生成されていない
2. `if saved_change_to_latitude? || saved_change_to_longitude?` の判定がおかしい
3. save メソッドが効かずレコードが更新されていない
4. Geocoder が正常に動作せず座標が取得できていない
5. 文字数バリデーションエラーでレコードが更新されていない

## 状況調査

1. address_keyword メソッドで住所, 座標を生成する文字列が正しく生成されていない

```Shell
# address_keyword の動作確認
$ rails console
> destination = Destination.first
> destination.address_keyword
"福岡, 153, ドーム"
# address_keyowrd で返る文字列の国名が国番号になっている
```

2. `if saved_change_to_latitude? || saved_change_to_longitude?` の判定がおかしい

```Shell
# if saved_change_to_latitude? || saved_change_to_longitude? 動作確認
$ rails console
> destination = Destination.first
> destination.saved_change_to_latitude?
false
> destination.saved_change_to_longitude?
false
> destination.latitude = 1234
> destination.longitude = 1234
> destination.saved_change_to_latitude?
false
> destination.saved_change_to_longitude?
false
# レコードを更新してもtrue が返らない

# will_save_change_to_attribute? メソッドを使用
> destination.will_save_change_to_attribute?("latitude")
true
> destination.will_save_change_to_attribute?("longitude")
true
```

3. save メソッドが効かずレコードが更新されていない ok > できればupdata メソッドの方が良い
4. Geocoder が正常に動作せず座標が取得できていない ok > add_address 実行後に座標がログで出力されている
5. 文字数バリデーションエラーでレコードが更新されていない ok > validation を修正してもspec はパスしなかった

## 解決策

1. address_keyword メソッドで住所, 座標を生成する文字列が正しく生成されていない

```Ruby
# 国番号から国名を取得 > 文字列連結するように修正
   def address_keyword
-    [name, country, spot].compact.join(', ')
+    # 国名が選択されている場合は国番号から国名を検索
+    if country.present?
+      country_name = Country.find_by(id: country).country_name
+      [name, country_name, spot].compact.join(', ')
+    end
   end
```

2. `if saved_change_to_latitude? || saved_change_to_longitude?` の判定がおかしい

```Ruby
# save_change_to_latitude? > will_save_change_to_attribute?("latitude) に修正
   def update_address
-    if saved_change_to_latitude? || saved_change_to_longitude?
+    if will_save_change_to_attribute?("latitude") || will_save_change_to_attribute?("longitude")
       self.address = Geocoder.search([latitude, longitude]).first.address
-      save
+      update!(address: address)
     end
```

3. save メソッドが効かずレコードが更新されていない > update! メソッドに変更
4. Geocoder が正常に動作せず座標が取得できていない > そのまま
5. 文字数バリデーションエラーでレコードが更新されていない > そのまま

テストの記述がおかしかったので修正

* FactoryBot でupdate メソッドを使えない
* instance_name.column_name で値を更新(save の直前)してレコードの書き換えをテストする
* destination が呼ばれた時点でFactoryBot でインスタンスが生成されるので`destination = create(:destination)` は省略できる

```Ruby
# update_address メソッドのテストを修正
   context "update_address メソッド" do
     it "座標の更新があった場合にaddress カラムを更新できること" do
-      destination.update(latitude: 35.658584, longitude: 139.7454316)
+      destination = create(:destination)
+      destination.latitude = 35.658584
+      destination.longitude = 139.7454316
       destination.update_address
       expect(destination.address).to eq "日本、〒105-0011 東京都港区芝公園４丁目２−８"
     end

     it "座標の更新が無い場合はメソッドをパスすること" do
-      destination.update(name: "edit name", spot: "edit spot", country: "edit country")
+      destination = create(:destination)
       destination.update_address
       expect(destination.update_address).to eq nil
     end
```
