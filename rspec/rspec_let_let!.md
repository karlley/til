# rspec_let_let!

rspec で使用する`let` と`let!` の違い

## 参照

[RSpecのletを使うのはどんなときか？（翻訳） \- Qiita](https://qiita.com/jnchito/items/cdd9eef2ed193267c651)

[使えるRSpec入門・その1「RSpecの基本的な構文や便利な機能を理解する」 \- Qiita](https://qiita.com/jnchito/items/42193d066bd61c740612#%E9%81%85%E5%BB%B6%E8%A9%95%E4%BE%A1%E3%81%95%E3%82%8C%E3%82%8B-let-%E3%81%A8%E4%BA%8B%E5%89%8D%E3%81%AB%E5%AE%9F%E8%A1%8C%E3%81%95%E3%82%8C%E3%82%8B-let)

[RSpec の letとlet\!とbeforeの挙動と実行される順番 \- Qiita](https://qiita.com/hirotakasasaki/items/fa3b131e27f5d0694c2c)

[RSpecを書く時に心がけたい3つの指針 – PSYENCE:MEDIA](https://tech.recruit-mp.co.jp/server-side/post-7289/)

[RSpec \- letとlet\!の挙動の違いがわかりません\(requestspecでエラーが発生する\)｜teratail](https://teratail.com/questions/306025)

[willnet/rspec\-style\-guide: 可読性の高いテストコードを書くためのお作法集](https://github.com/willnet/rspec-style-guide#before%E3%81%A8letlet%E3%81%AE%E4%BD%BF%E3%81%84%E5%88%86%E3%81%91)

[rspecを読みやすくメンテしやすく書くために](https://zenn.dev/yuji_developer/articles/52cc0e356b3748)


## ポイント

* `let` は呼ばれた後に実行(遅延評価)
* `let!` は呼ばれる前に実行(事前評価)
* `let!` は`before` の前にデータが必要な場合に使用
* 基本的には`let` を使用して、事前処理が必要な場合のみ`let!` を使用
* `let`、`let!` をspec ファイルの上部に固めて書くよりもテスト毎に分けて書いた方が可読性が上がる

## let を書く位置

* context 毎に記述する
* describe の外に記述しない
* ファイルを読む時に上下しない事を意識
* DRY よりも可読性を重視

```Ruby
  describe "users#index" do
    context "管理者ユーザーの場合" do
      before do
        login_for_system(admin_user)
        create_list(:user, 30)
        visit users_path
      end

      let(:admin_user) { create(:user, :admin) }

      it "ページネーション機能, 自分以外のユーザーの削除ボタンが表示される" do
        expect(page).to have_css "div.pagination"
        User.paginate(page: 1).each do |u|
          expect(page).to have_link u.name, href: user_path(u)
          expect(page).to have_content "#{u.name} | 削除" unless u == admin_user
        end
      end
    end
```

## 記述のポイント

1. `context`: 〇〇の場合を記述
2. `before`: `context` の状態を作るための記述
3. `let`: テスト対象になるオブジェクト
4. `it`: `context` から続けて読んで違和感のないテスト名にする
