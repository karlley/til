# js linter formatter

## ESLint とは

- JavaScript の静的検証ツール
- 括弧、スペース等の統一に役立つ
- rubocop のように独自ルールを作成可能
- JSX をサポートしている
- Babel と連携すると最新の構文や型に対応可能

## ESLint のインストール

`npm`を使用して公式の方法でインストール

[Getting Started with ESLint \- ESLint \- Pluggable JavaScript Linter](https://eslint.org/docs/latest/use/getting-started#manual-set-up)

- `package.json` が必要なので`npm init` コマンドが必要
  - [npm でのパッケージのインストール時は package\.json を作っておく \- karlley](https://scrapbox.io/karlley/npm%E3%81%A7%E3%81%AE%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%99%82%E3%81%AFpackage.json%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%A6%E3%81%8A%E3%81%8F)
- 公式に記載されている `--config` オプションは付けない
- `npm init @eslint/config` コマンド実行後の質問の後にインストールされる
- インストール後は下記のファイルが追加される
  - `.eslintrc.json`
  - `node_modules`
  - `package-lock.json`
- ローカルにインストール済み
  - 公式はローカルインストール(`--global` オプション無し)
  - 基本はローカルインストール(プロジェクト毎)が良いっぽい

```sh
$ npm init # package.jsonを作成、実行していないとエラーが出る
$ npm init @eslint/config #ESLintの設定
✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · none
✔ Which framework does your project use? · none
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ What format do you want your config file to be in? · JSON
Local ESLint installation not found.
The config that you've selected requires the following dependencies:

eslint@latest
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · npm
Installing eslint@latest

added 96 packages, and audited 97 packages in 8s

23 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Successfully created .eslintrc.json file in /home/kariya/workspace/study/fbc_js
```

`npm init @eslint/config` 実行後の質問内容

```sh
How would you like to use ESLint? … #シンタックスだけで良いか
What type of modules does your project use? #使用しているモジュールを選択
Which framework does your project use? #使用しているフレームワークを選択
Does your project use TypeScript? #TypeScriptを使用しているか
Where does your code run? #実行する環境、複数選択可
What format do you want your config file to be in? #.eslintrcファイルの形式
```

インストールしても`eslint` コマンドが使える訳ではないことに注意

```sh
$ eslint
Command 'eslint' not found, but can be installed with:
sudo apt install eslint
```

インストールの確認は`npm ls` で行う

```sh
$ npm ls
fbc_eslint@1.0.0 /home/kariya/workspace/study/fbc_eslint
└── eslint@8.46.0
```

## コマンドラインで ESLint を動かす

- `npx` コマンドで eslint を実行できる
- `--fix` オプションでエラー箇所を自動修正

```sh
$ cd project_name
$ npx eslint file_name.js
#修正点が表示される

# エラー箇所を自動修正
$ npx eslint file_name.js --fix
```

## VSCode で ESLint を動かす

以下を行う

1. ESLint が動作する環境か確認
2. コマンドラインで ESLint の動作を確認
3. 拡張機能の追加
4. VSCode で ESLint の動作を確認
5. 必要であればリンターのルールを追加

### 1. ESLint が動作する環境か確認

- nvm
  - インストールされているか
- Node.js
  - プロジェクトのカレントバージョン
- npm
  - npm 経由でローカルに eslint がインストールされているか
  - 今回は`--save-dev eslint` オプション無しでローカルインストール済み

```sh
# nvm
$ nvm --version
0.35.2

# Node.js
$ nvm current
v18.17.0
$ nvm ls
       v16.13.1
->     v18.17.0
         system
default -> lts/* (-> v18.17.0)
node -> stable (-> v18.17.0) (default)
stable -> 18.17 (-> v18.17.0) (default)
iojs -> N/A (default)
unstable -> N/A (default)
lts/* -> lts/hydrogen (-> v18.17.0)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.1 (-> N/A)
lts/hydrogen -> v18.17.0
kariya@LAPTOP-MMJ98U32:~/workspace/study/fbc_eslint$

# npm
$ npm -v
9.6.7
$ npm ls
fbc_eslint@1.0.0 /home/kariya/workspace/study/fbc_eslint
└── eslint@8.46.0
```

### 2. コマンドラインで ESLint の動作を確認

動作確認用ファイルを作成して ESLint の動作を確認

```js
// test.js
// 引数のname、namがエラーで警告が出るはず
function hello(name) {
  document.body.textContent = "Hello, " + nme + "!";
}

hello("World");
```

```sh
$ cd project_directory
$ npx eslint file_name.js #以下のような警告が出ればESLintが動作している
/home/kariya/workspace/study/fbc_eslint/test.js
  1:16  error  'name' is defined but never used  no-unused-vars
  2:43  error  'nme' is not defined              no-undef

✖ 2 problems (2 errors, 0 warnings)
```

### 3. 拡張機能の追加

VSCode の ESLint 拡張機能を追加

[ESLint \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

[ESLint \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

### 4. VSCode で ESLint の動作を確認

VSCode で ESLint のリンターの警告が表示されるか確認

1. コマンドラインでの ESLint の動作確認で使用したファイルをエディタで開く
2. ファイル内に警告箇所に赤の波線が表示されるのを確認
3. VSCode のターミナル内の問題パネルに警告の詳細の表示を確認

### 5. 必要であればリンターのルールを追加

- プロジェクトのルールは`npm` で`eslint` インストール後に作成される`.eslintrc.json` が参照される
- デフォルトではルールは空になっている
- プロジェクトのルールが存在する場合は`.eslintrc.json` にルールを追加する
- `.eslintrc.json` を別のディレクトリで管理する方もあるらしい
  - [VS Code に ESLint を設定する \- Qiita](https://qiita.com/Mount/items/5f8196b891444575b7db)

## Prettier とは

- コード整形ツール
- Node.js 上で動作
  - 環境に依存しない
- JavaScript、JSX、TypeScript、HTML、CSS 等に対応
- プロジェクト内の記述を統一できる
- リンター(ESLint)では整形できない部分を補う
- ESLint と組み合わせて使用可能
  - ルールが競合する場合は無効化する設定が必要
    `eslint-conrg-prettier`
- VSCode の拡張機能は推奨されていないかも？
- `npm` でインストールする

## Prettier と ESLint の違い

- ESLint では整形できないコードを Prettier は整形できる
- 手軽に確実に整形できる

- 整形する対象
  - ESLint
    - `npx eslint --fix` はエラー箇所のみ整形(エラー以外は整形しない)
  - Prettier
    - デフォルトのスタイルに沿って全て整形
- 使用用途
  - ESLint
    - 構文チェック
  - Prettier
    - 整形ツール

[Prettier 入門 ～ ESLint との違いを理解して併用する～ \- Qiita](https://qiita.com/soarflat/items/06377f3b96964964a65d#%E4%BD%95%E6%95%85-prettier-%E3%82%92%E5%88%A9%E7%94%A8eslint-%E3%81%A8%E4%BD%B5%E7%94%A8%E3%81%99%E3%82%8B%E3%81%AE%E3%81%8B)

## Prettier のインストール

公式に沿って`npm` でインストール

[Install · Prettier](https://prettier.io/docs/en/install)

```sh
$ npm install --save-dev --save-exact prettier
$ npm ls
fbc_eslint@1.0.0 /home/kariya/workspace/study/fbc_eslint
├── eslint@8.46.0
└── prettier@3.0.1
```

インストール時のオプションの詳細

- `--save-dev`
  - 開発環境用でインストール
  - `package.json`内の`devDependencies` に追加される
- `--save-exact`:
  - インストールバージョンを明示する
  - 明示的なバージョン固定

[【npm 初心者】なんんとなく使っていた npm install の \-\-save\-dev ついて調べてみた](https://zenn.dev/hrkmtsmt/articles/5f4a0e5c79b77a)

[Node\.js プロジェクトの依存パッケージ更新戦略 \| DevelopersIO](https://dev.classmethod.jp/articles/strategies-node-project/)

[npm\-install \| npm Docs](https://docs.npmjs.com/cli/v9/commands/npm-install)

## Prettier 関連のファイル作成

フォーマットルールの`.prettierrc` を作成(中身は空)

```sh
echo {}> .prettierrc.json
```

## コマンドラインで Prettier を動かす

`npx` で`prettier` コマンドを実行できる

```sh
$ npx prettier ファイル名
$ npx prettier . --write
```

`--write` オプションで整形後に上書き

## VSCode で Prettier を動かす

prettier の VSCode 拡張機能をインストール

- エディタはローカルインストールされた Prettier を参照する
- js 以外のフォーマットもこの拡張で行ってくれる

[Editor Integration · Prettier](https://prettier.io/docs/en/editors)

[Prettier \- Code formatter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

[prettier/prettier\-vscode: Visual Studio Code extension for Prettier](https://github.com/prettier/prettier-vscode)

### Windows の WSL を使う場合

WSL を使って prettier を動かす場合はローカル VSCode から WSL 内の Ubuntu に接続した状態で行わなければフォーマットが動かなかった

- VSCode の WSL 拡張をインストール
  - [WSL \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
- ローカル VSCode から WSL 内のファイルにアクセスする
  - `ctrl + shift + p` > `wsl` と入力 > `WSLへの接続` > リモートに接続された状態になる
    - VSCode の左下に`WSL: Ubuntu` を表示されていれば接続状態

## React プロジェクトで ESLint、prettier が動くようにする

未着手: react の memo アプリのディレクトリで上記 2 つが動作するようにする

## markdownのpretteir自動フォーマットをキャンセル

settings.jsonに以下を追加

```json
    "[markdown]": {
        "editor.formatOnSave": false
    },
```

[\[VSCode\] Prettierをアップデートしたら言語単位でのフォーマット無効設定が動作しなくなったので対処した話 \| DevelopersIO](https://dev.classmethod.jp/articles/the-setting-to-disable-formatting-for-certain-languages-does-not-work-after-updating-prettier-in-vscode/)

## 参照

[Getting Started with ESLint \- ESLint \- Pluggable JavaScript Linter](https://eslint.org/docs/latest/use/getting-started#manual-set-up)

## メモ

[eslint、pretteirの実行コマンドのショートハンド \- karlley](https://scrapbox.io/karlley/eslint%E3%80%81pretteir%E3%81%AE%E5%AE%9F%E8%A1%8C%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E3%81%AE%E3%82%B7%E3%83%A7%E3%83%BC%E3%83%88%E3%83%8F%E3%83%B3%E3%83%89)
