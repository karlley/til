# nvm

https://github.com/nvm-sh/nvm#installing-and-updating

公式のcrulを使う方法でインストール

```sh
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

以下を`.zshrc` に貼り付け、`source ~/.zshrc` で再読み込み

```zsh
$ export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

## 自動バージョン切替え

`.nvmrc` が存在するディレクトリはバージョンが自動で切り替わるように設定

https://github.com/nvm-sh/nvm#bash

zsh用のスクリプトを`.zshrc` に貼り付け

```sh
# place this after nvm initialization!
autoload -U add-zsh-hook

load-nvmrc() {
  local nvmrc_path
  nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version
    nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$(nvm version)" ]; then
      nvm use
    fi
  elif [ -n "$(PWD=$OLDPWD nvm_find_nvmrc)" ] && [ "$(nvm version)" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}

add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

## 不要なnodeをアンインストール

自動バージョン切替えの設定をしても`system` のnodeが指定されてしまう

```sh
$ nvm ls
       v16.13.1
       v18.17.1
->       system
default -> lts/* (-> v18.17.1)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v18.17.1) (default)
stable -> 18.17 (-> v18.17.1) (default)
lts/* -> lts/hydrogen (-> v18.17.1)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.17.1
```

`homebrew` 経由での`node` をアンインストール

```sh
$ brew uninstall node
```

再度確認

- `homebrew` の`node` が無いか
- `.nvmrc` が存在するディレクトリでバージョンが切り替わるか

```sh
$ sources ~/.zshrc
nvm ls
$ nvm ls
->     v16.13.1
       v18.17.1
default -> lts/* (-> v18.17.1)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v18.17.1) (default)
stable -> 18.17 (-> v18.17.1) (default)
lts/* -> lts/hydrogen (-> v18.17.1)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.17.1
```

## npm

`nvm` を使ってNode.jsをインストールした時点で導入されてた

```sh
$ npm --version
9.6.4
```
