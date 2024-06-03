# js nvm node

nvmを使用してnodeをインストールする

## nvm

- Node Version Manager 
- Node.jsのバージョン管理ツール
- `nvm current` でアクティブになっているNode.jsのバージョンを出力する

### nvmのコマンド

```sh
$ nvm ls # カレントバージョン
$ nvm ls-remote # インストール可能バージョン
$ nvm install バージョン # バージョン指定でインストール
$ nvm install node # 最新バージョンをインストール
$ nvm install --lts # 最新のLTSバージョンをインストール
$ nvm uninstall バージョン # バージョン指定でアンインストール
$ nvm current # 使用中のバージョン
```

- nvmでインストールしたバージョンが自動でカレントバージョンになる

## LTSバージョン

- Long Term Support版
- 長期サポート版
- Node.jsの偶数のバージョンのこと
- 30カ月間は重要なバグ修正が行われることが保障されている
- 対義語としてCURRENTバージョンがある(最新版だが安定性が保障されていない)

## .nvmrc

- nvmのせっていファイル
- `.nvmrc` ファイルでディレクトリ毎のNode.jsのバージョン切替が可能になる
- Rubyのrbenvの`.ruby-version` ファイルと近い
- `.nvmrc`を使った2つのバージョンの切替方法
  + `nvm user` コマンド
  + シェルに設定が必要

## Macにnvmを使用してnodeをインストールする

- 以下の順番でインストールルする
  - homebrew -> nodebrew -> nvm -> node

```sh
$ brew -v
Homebrew 4.3.2
$ brew install nodebrew 
$ nodebrew -v
nodebrew 1.2.0
$ nvm ls # カレントバージョン
       v16.13.1
->     v18.17.1
...
$ nvm ls-remote # インストール可能バージョン
...
      v20.14.0   (Latest LTS: Iron)
$ nvm install --lts # 最新のLTSバージョンをインストール
$ nvm ls
       v16.13.1
       v18.17.1
->     v20.14.0 # 最新のLTSバージョン
```

## nvmのアンインストール

アンインストールするには以下の2つを行う

- ルートディレクトリの`.nvm/` を削除
- インストール時に追加された`.bashrc` 内の記述の削除

## nvmを使って手動でバージョン切替

以下を行うと手動でバージョン切替ができる

- バージョンを切り替えたいディレクトリに`.nvmrc` を作成
- `nvm use` コマンドを実行

[nvm\-sh/nvm: Node Version Manager \- POSIX\-compliant bash script to manage multiple active node\.js versions](https://github.com/nvm-sh/nvm#nvmrc)

例) `project_name/`のカレントバージョンを`v18.17.0` から`v16.13.1` に切り替える     

```sh
$ cd project_name
$ nvm current # プロジェクトのカレントバージョン
v18.17.0
$ echo "16.13.1" > .nvmrc # .nvmrcを作成
$ cat .nvmrc #ファイル確認
16.13.1
$ nvm install # .nvmrcのバージョンをインストール
$ nvm use # 16.13.1にカレントバージョンを切替
```

以下の方法でも切替可能

```sh
$ nvm use node # 最新バージョンをカレントバージョンに切替
$ nvm use --lts # 最新のLTS をカレントバージョンに切替
$ nvm use バージョン # 指定したバージョンをカレントバージョンに切替
```

## nvmを使って自動でバージョン切替

シェルに`.nvmrc` を自動読み込むする設定を追加

- `~/.zshrc` に設定を追加
  - 使用するシェルによって貼り付けるスクリプトが異なるので注意
- `.nvmrc` の存在を確認し、存在する場合は自動で読み込むことで切り替えを行う

手順

1. カレントディレクトリの`.zshrc` に[公式](https://github.com/nvm-sh/nvm#bash)のスクリプトを貼り付け
2. ` source ~/.zshrc` で再読み込み
3. `.nvmrc` が存在するディレクトリに移動する下記のように表示されバージョンが切り替わる

```sh
$ cd project_name 
Now using node v16.13.1 (npm v8.1.2)
```

## 参照

[WSL2で nvm による NodeJS のインストールとバージョン管理](https://zenn.dev/keijiek/articles/4976559b876090)

[karlley/fbc\_js\_nvm\_node: nvmを使ってNode\.jsをインストールする](https://github.com/karlley/fbc_js_nvm_node)
