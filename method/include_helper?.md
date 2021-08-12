# include_helper?

controller にhelper がinclude されているか確認

## 参照

[クラス内にモジュールがincludeされているか確認する方法 \- Qiita](https://qiita.com/k-o-u/items/e460b44137e9cec193cc)

[Module\#include? \(Ruby 3\.0\.0 リファレンスマニュアル\)](https://docs.ruby-lang.org/ja/latest/method/Module/i/include=3f.html)

## 確認方法

rails console でinclude を確認する

```Shell
# HelperName モジュールがModelName モデルにinclude されているか確認
> ModelName.include?(HelperName)
true or false

# ModelName モデルにinclude されているmodule を確認
> ModelName.include_modules
[HelperName1, HelperName2]
```
