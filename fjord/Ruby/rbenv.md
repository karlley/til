# rbenv

Ruby のバージョン管理ツール

## rbenb を使ったRuby のインストール

安定版の最新のRuby をインストールする手順

1. インストールするバージョンの確認
2. Homebrew、Command line tools のインストール確認
3. rbenv、ruby-build のインストール
4. rbenv のパス設定
5. rbenv にRuby 3.0.3をインストール
6. rbenv にインストールしたRuby のバージョンを設定
7. benv-doctor、バージョンを確認

### 1. インストールするバージョンの確認

安定版の最新のRuby のバージョンを確認する

[ダウンロード](https://www.ruby-lang.org/ja/downloads/)

[Rails Girls \- Japanese](https://railsgirls.jp/install)

安定版を上記URL から確認(2021/12/1時点で`3.0.3`)

### 2. Homebrew、Command line tools のインストール確認

インストールに必要なツールがインストールされているか確認

* Homebrew
* Command line tools(Xcode がインストールされている場合は不要)

```Shell
# Homebrew のインストール確認
$ brew --version
Homebrew 3.3.6

# Command lint tools の確認
$ brew --config
...
CLT: 11.0.0.33.17 # Commmand line tools のバージョン
Xcode: N/A # Xcode はインストールしていない
```

### 3. rbenv、ruby-build のインストール

Ruby のバージョン管理に必要なツールをインストール

* rbenv
* ruby-build

```Shell
# rbenv、ruby-build のインストール
$ brew install rbenv ruby-build

$ rbenv -v
rbenv 1.1.2

$ ruby-build --version
ruby-build 20200115
```

### 4. rbenv のパス設定

* system のRuby からrbenv を使用したRuby に切り替える為にパス設定を追加
* `/usr/bin/ruby` から`/Users/○○/.rbenv/shims/ruby` にパスを変更

```Shell
# パス設定前のパス確認
$ which ruby
/usr/bin/ruby # mac OS 付属のRuby(system)

# パス設定
$ echo 'eval "$(rbenv init -)"' >> ~/.zshrc

# .zshrc を再読み込みして設定を反映
$ source ~/.zshrc

$ which ruby
/Users/karlley/.rbenv/shims/ruby
```

### 5. rbenv にRubyをインストール

* rbenv に安定版の最新のRuby をインストール
* `rbenv install --list` が古い場合は`brew rbenv upgrade` でrbenv を更新する

```Shell
# 現在のRuby のバージョンを確認
$ ruby -v
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-darwin19]

# rbenv にインストールされているバージョンを確認
$ rbenv versions
  system
  2.5.7
* 2.6.5 (set by /Users/karlley/.rbenv/version)

# rbenv に3.0.3 があるか確認
$ rbenv install --list
3.0.3 が無い

# 3.0.3 が無いのでrbenv をupgrade
$ brew rbenv upgrade

# rbenv に3.0.3 があるか再度確認
$ rbenv install --list
...
3.0.3 # upgrade したことで追加される

# rbenv に3.0.3 をインストール
$ rbenv install 3.0.3

# rbenv に3.0.3 が追加されたか確認
$ rbenv versions
  system
  2.5.7
* 2.6.5 (set by /Users/karlley/.rbenv/version)
  3.0.3 # 追加されている
```

### 6. Ruby のバージョンを切替

* rbenv にインストールした安定版の最新のRuby にバージョンを切り替える
* rbenvにインストールされたバージョン、マシンに設定されたバージョンの両方を確認する

```Shell
# マシンで使用する現在のバージョンを確認
$ ruby -v
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-darwin19]

# rbenv にインストールされたバージョンを確認
$ rbenv versions
  system
  2.5.7
* 2.6.5 (set by /Users/karlley/.rbenv/version) # 3.0.3 に切り替わっていない
  3.0.3

# マシンで使用するバージョンを指定
$ rbenv global 3.0.3

# マシンで使用するバージョンの切替を確認
$ rbenv versions
  system
  2.5.7
  2.6.5
* 3.0.3 (set by /Users/karlley/.rbenv/version) # 3.0.3 に切り替わっている

# マシンで使用するバージョンの切替を確認
$ ruby -v
ruby 3.0.3p157 (2021-11-24 revision 3fb7d2cadc) [x86_64-darwin19]
```

### 7. rbenv-doctor を確認

正常に動作するのか`rbenv-doctor` を確認

```Shell
# rbenv-doctor の確認
$ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash
Checking for `rbenv' in PATH: /usr/local/bin/rbenv
Checking for rbenv shims in PATH: OK
Checking `rbenv install' support: /usr/local/bin/rbenv-install (ruby-build 20211124)
Counting installed Ruby versions: 3 versions
Checking RubyGems settings: OK
Auditing installed plugins: OK
```

## ruby-build とは

* `rbenv install` コマンドを使えるようにするツール
* rbenv同様、Rubyのインストールに必要
* rbenv のプラグインとして使用

[ruby\-buildとrbenvのプラグイン機構](https://mogulla3.tech/articles/2020-12-30-01)

## rbenv init - について

rbenv の設定時に行う`rbenv init -` が何をやっているのか

* 環境変数の設定、シェルスクリプトの設定を行う
* `rbenv rehash` を内部的に行ってるので自分で行う必要はない

[rbenv rehashをちゃんと理解する](https://mogulla3.tech/articles/2020-12-29-01)

[rbenv \+ ruby\-build はどうやって動いているのか \- takatoshiono's blog](https://takatoshiono.hatenablog.com/entry/2015/01/09/012040)

> 1. ~/.rbenv/shims を環境変数PATHの先頭に追加する
> 2. コマンドの補完用のシェルスクリプトを読み込む
> 3. shimのrehash、すなわち rbenv rehash の実行
> 4. sh dispatcherをインストールする

## rbenv のglobal とlocal

rbenv でRuby のバージョンを指定する際の`global` と`local` の違い

[rbenv \| global と local と \.ruby\-version の微妙な関係 \- Qiita](https://qiita.com/Yinaura/items/0b021984bb21ae77816d)

[ちゃんと理解するrbenv : \(3\) バージョン切り替えの仕組みを理解する](https://mogulla3.tech/articles/2021-07-10-understanding-rbenv-3)

* `global`: マシン全体で適応されるRuby のバージョン
* `local`: 特定のディレクトリのみに適応されるRuby のバージョン
* `local` は`global` よりも優先度が高い
* `~/.rbenv/version` や`.ruby-version` を生成することによりバージョンを指定している
