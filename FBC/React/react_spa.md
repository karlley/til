# React SPA

## Create React App

https://create-react-app.dev/docs/getting-started

```sh
npx create-react-app react-memo-app
cd react-memo-app
npm start
```

以下エラーが発生

```sh
Failed to compile.

Module not found: Error: Can't resolve 'web-vitals' in '/Users/karlley/workspace/...'
ERROR in ./src/reportWebVitals.js 5:4-24
Module not found: Error: Can't resolve 'web-vitals' in '/Users/karlley/workspace/...'
```

`web-bitals` を手動でインストールすると解消

```sh
npm install web-vitals
```

## 不要ファイル削除

- `/public`
  - 静的リソースが配置される
  - 画像、htmlなどビルドされないファイルが配置される
- `/build`
  - ビルド後のファイルが配置される
  - `npm run build` するとビルド後のファイルが配置される

## eslint、prettierのセットアップ

### eslint

Reactは`react-scripts` npmにeslintは含まれているため、インストール/設定は不要

```sh
npx eslint .
```

設定ファイル`.eslintrc` or `.eslintrc.json` は追加設定を行わない場合は不要っぽい

### prettier

prettierはReactに含まれていないのでインストールが必要
https://prettier.io/docs/install

```sh
npm install --save-dev --save-exact prettier
# 設定ファイル作成
node --eval "fs.writeFileSync('.prettierrc','{}\n')"
npx prettier .
```

設定ファイル`.prettierrc` or `./prettierrc.json` は追加設定を行わない場合は不要っぽい

### フォーマットのコマンドの設定

package.jsonにスクリプトを設定していない場合は以下コマンド

```sh
npx prettier --check ./src
npx prettier --write ./src
npx eslint ./src
npx eslint --fix ./src
```

package.jsonにスクリプトを指定している場合は`npm` で実行する

```json
// package.json
{
  "scripts": {
    "fix": "prettier --write ./src && eslint --fix ./src",
    "lint": "prettier --check ./src && eslint ./src"
  },
```

```sh
npm run fix
npm run lint
```

- npm runの動作
  - `npm run <スクリプト名>` はpackage.jsonのscripts 内のコマンドを実行
- npxの動作
  - `npx <コマンド名>` でローカル or グローバルにインストールされていないコマンドを一時的に実行
  - `npx lint` のような使い方はできない（lint というスクリプトは存在しない）
  - prettierやeslintのような実行可能なパッケージは`npx` で直接実行可能
