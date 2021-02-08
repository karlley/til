# fzf-vim

vim でfzf を使う

<https://github.com/junegunn/fzf.vim>

## install

```.vimrc
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
```

## how to using

https://wonderwall.hatenablog.com/entry/2017/10/07/220000

https://kashewnuts.github.io/2018/12/02/bp_advent_calender.html

https://arimasou16.com/blog/2018/11/02/00268/

https://techracho.bpsinc.jp/jhonda/2019_12_24/85173

https://liginc.co.jp/469142

## setting

* NerdTree をファイルオープン時に閉じる設定を追加

## how to using

* `ctrl + F` :Files + preview
* `ctrl + G` :GFiles?
* `ctrl + R` :Rg + preview
* `ctrl + H` :History
* `ctrl + B` :Buffers
* FZF起動後に `ctrl + T` タブで開く
* FZF起動後に `ctrl + X` 上下スプリットで開く
* FZF起動後に `ctrl + V` 左右スプリットで開く
* `:Commands` 設定されているコマンド一覧
* `:Maps` 設定されているショートカット一覧
