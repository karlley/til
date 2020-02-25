## bundle exec

bundler で管理しているGem を使用してコマンドを実行したい場合に使用

```
$ bundle exec command_name
```

`rails new` での使用例

```
#システムで使用しているrails を使用してnew
$ rails new app_name
#プロジェクト配下のGemfile で管理しているBundlerでnew
$ bundle exec rails new app_name
```

## bundle init

- rails アプリを作成時は自動的にGemfile が生成される
- rails を使用しない場合(Rubyアプリ等)の場合は手動で`bundle init` しGemfile を生成する必要がある
- `gem install` でインストールしたgem はbundler で管理しているgem とは違い常に読み込まれる

## --path vender/bundle

- rails アプリを作成時にgem をアプリ配下のvender/bundle にgem をインストールする
- 指定しない場合は`gem list` でLOCAL GEM にインストールされる
- `bundle install --path vender/vundle` でインストールするとGemfile.lock, .bundle/config が生成される
- Gemfile.lock にはインストール済みのgem が書き出される

```:Gemfile.lock
GEM
  remote: https://rubygems.org/
  specs:
    actioncable (6.0.2.1)
.
RUBY VERSION
   ruby 2.6.5p114

BUNDLED WITH
   2.1.4
```

- .bundle/config には`--path` で指定したディレクトリが書き出される

```:.bundle/config
---
BUNDLE_PATH: "vender/bundle"
```

## イメージ

- `gem install` , `bundle install` したgem は全てLOCAL GEM にインストールされる(インストールされただけ)
- Gemfile を使用する場合はLOCAL GEM からGemfile に記載されたgem をプロジェクト毎に取得し、使用する
- gem 自体はRuby のバージョンに紐付いている

```
ruby ver.A > ver.A でインストールしたgem を使用
ruby ver.B > ver.B でインストールしたgem を使用
```

## local or global

Gemfile, .bundle/ 配下で下記コマンドを実行

```
# global install gems
$ gem list
# local install gems
$ bundle exec gem list
```

## 結局どの方法でgem を管理すればいいのか

- bundler, irb-tools など競合しない, バージョンを気にしないgem は`gem install` 
- それ以外はプロジェクト毎に`bundle install`
- bundle ですべて管理したかったらLOCAL GEM を全て削除し、bundle 配下に置くことは可能なのでとりあえず気にしないで進める