# no_ransack_error

複数キーワードでの検索機能を追加した際のエラー

## エラー内容

`ActionView::Template::Error - No Ransack::Search object was provided to search_form_for!:`

存在すると思って書かれた変数あるいはメソッドが単に存在しなかった場合のエラー

## 参照

[「No Ransack::Search object was provided to search_form_for!」エラーを解決したい](https://teratail.com/questions/207533)

[No Ransack::Search object was provided to search_form_for](https://stackoverflow.com/questions/21351680/no-ransacksearch-object-was-provided-to-search-form-for)

[[Rails]ransackで詳細検索フォームを作成したメモ書き](https://laptrinhx.com/rails-ransackde-xiang-xi-jian-suofomuwo-zuo-chengshitamemo-shuki-3107655379/)


## 原因

`search_form_for` の`@q` がセットされていない

## 解決策

* `@q` をセットする
* `logged_in?` と`have_search_word?` の分岐に注意する

分岐のパターン

```text
* ログイン済, keyword有, keyword複数
求める値/動作: @q, @destinations, @search_word

* ログイン済, keyword有, keyword単体
@求める値/動作: q, @destinations, @search_word

* ログイン済, keyword無
求める値/動作: @q

* ログイン未
求める値/動作: ログイン画面にリダイレクト
```

旧コード(複数キーワード未対応)

```Ruby
def set_search
  if logged_in?
    @search_word = params[:q][:name_cont] if params[:q]
    @q = current_user.feed.paginate(page: params[:page], per_page: 5).ransack(params[:q])
    @destinations = @q.result(distinct: true)
end
```

修正前

```Ruby
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
  end
end
```

修正後

```Ruby
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
  end
end
```
