# rspec_capybara_selenium_fill_in

fill_in で`q_name_cont` にsearch_word が入力されてない

```Ruby
Capybara::ElementNotFound:
       Unable to find field "q_name_cont" that is not disabled
```

```Ruby
it "feed から検索ワードに該当する結果が表示されること" do
  search_word = "東京"
  create(:destination, name: search_word, user: user)
  fill_in "q_name_cont", with: search_word
  click_button "Search"
  expect(page).to have_css "h1", text: "\"#{search_word}\" Search Results : 1"
  within(".destinations") do
    expect(page).to have_css "li", count: 1
  end
end
```

## 参照

[Selenium の locator とうまくつきあうための話](https://qiita.com/okitan/items/cdf8809405821e057609)

[Seleniumのcss locatorの話](https://qiita.com/okitan/items/eb8f8e253d0811777215)

## fill_in での入力先の指定方法の優先度

```HTML
# フォームの例
<label for="q_keyword_refind">キーワード</label>
<input placeholder="Refine Search for Destination..." class="form-control font-awesome" id="destination_keyword_search" type="search" name="q[name_or_spot_or_address_cont]" />
```

指定方法一覧

```text
label: キーワード
name: q[name_or_spot_or_address_cont]
CSS selector: .form-control, .font-awesome, #destination_keyword_search
xpath: class('form_control'), id('destination_keyword_search')
```

優先度

1. name
2. CSS selector
3. label
4. xpath

* name が画面の変更に強くCSS やJS の影響を受けにくい
* CSS selector は可読性が高い
